# Calling Conventions

以下关键词 MUST 必须、MUST NOT 禁止
、REQUIRED 必需的、SHALL 应当、SHALL NOT 应当不、SHOULD 应该、SHOULD NOT 不应该、 RECOMMENDED 推荐、MAY 可以、OPTIONAL 可选的 依照 [RFC 2119](https://tools.ietf.org/html/rfc2119) 的叙述解读。

## 请求

> 关于 MIME 类型：[MIME types - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)  
> 常见 MIME 类型列表：[Incomplete list of MIME types - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types)

除上传文件外，所有的请求体的 MIME 类型必须是 `application/json` ，并且为合法的 JSON 字符串且满足[对应 API 的 schema](https://app.swaggerhub.com/apis/liqueurlibrazy/classmanagementsystem/) 。

例：

设 `newSchool` 为待新增的学校的 JS 对象

* 原生 Javascript 请求新增学校接口

    ``` javascript
    var xhr = new XMLHttpRequest();
    xhr.open('POST', '/school', true);
    xhr.setRequestHeader('Content-Type', 'application/json; charset=UTF-8');
    xhr.send(JSON.stringify(newSchool));

    xhr.onloadend = function () {
        // 获得新学校的ID
    };
    ```

* jQuery 请求新增学校接口

    ``` javascript
    // headerObj 包含一些请求 API 必需的字段，下同。参见下文 [API 鉴权]
    $.ajax({
        url: '/school',
        type: 'POST',
        headers: headerObj,
        contentType: 'application/json',
        data: JSON.stringify(newSchool),
        success: function(data) {
            // 获得新学校的ID
        }
    });
    ```

* 微信小程序请求新增学校接口

    ``` javascript
    // headerObj['content-type'] == 'application/json'
    // 小程序的默认 content-type 即为 application/json
    wx.request({
        url: '/school',
        header: headerObj,
        method: "POST",
        data: newSchool
        success: function (data) {
            // 获得新学校的ID
        }
    });
    ```

### `GET`

`GET` 请求用来从服务端获取资源的某种表示。 `GET` 请求应当只获取数据。

### `POST`

`POST` 请求用来向服务端提交某个资源的实体，常用于新建操作。 `POST` 常常会对服务端产生影响。

### `DELETE`

`POST` 请求用来删除服务端某个资源。

### `PUT` / `PATCH`

> 标准组曾使用了 `PATCH` 请求实现某些 API ，但由于小程序端不支持 `PATCH` 请求，故将其全部替换为 `PUT`

`PUT` 用于将服务端资源所有的表示替换为请求体，即替换服务端资源。

`PATCH` 用于部分修改服务端资源。

> 由于上述原因，并且极少用到 `PUT` 替换资源的语义，本 API 中不区分 PUT 请求与 PATCH 请求，即都按部分修改的语义处理。

## 响应

### `20x`

对于正确请求的响应的规范参见具体 API 的规范。

### `400`

表示请求中的信息有误，返回错误消息与一些辅助信息。

响应中必须包含一个 `message` 字段，值为向用户显示的可读的错误信息。若错误所在的字段明确，可以包含一个 `error` 数组，包括请求中错误所在的字段名。

例：

* 已设置账号类型的账号无法重复设置

    ``` javascript
    { "message": "您已绑定为学生，无法再次绑定为教师"}
    ```

* Web 端设置个人信息时输入了不存在的学校

    ``` javascript
    { "message": "输入的学校不存在"， "error": ["school"]}
    ```

### `401`

除 `/signin` 与 `/register` 两个 Endpoint 外，所有的 API 均需在登录后使用。若在未登录的情况下访问 API，必须返回 `HTTP 401` 状态。响应中推荐包含一个 `message` 字段，值为向用户显示的可读的错误信息。不应当在响应中包含具体的URL，具体的登录跳转由前端判断 401 状态实现。

例：

* 未登录

    ``` javascript
    { "message": "您尚未登录，请登录！"}
    ```

* 登录过期

    ``` javascript
    { "message": "登录过期，请重新登录！"}
    ```

### `403`

表示当前用户的权限不足以完成请求，返回错误消息。

响应中必须包含一个 `message` 字段，值为向用户显示的可读的错误信息。

例：

学生无法创建课程

``` javascript
{ "message": "仅老师可创建课程"}
```

### `404`

表示找不到当前请求中要求的资源，返回错误消息与一些辅助信息。

响应中必须包含一个 `message` 字段，值为向用户显示的可读的错误信息。若找不到的资源所在的字段明确，可以包含一个 `error` 数组，包括请求中错误所在的字段名。

例：

* 退课的班级不存在（即不存在ID为452的班级）

    ``` plain
    DELETE /class/452/student/7654
    ```

    响应

    ``` javascript
    { "message": "班级不存在", "error": ["classId"]}
    ```

* 存在ID为452的班级且存在ID为7654的学生，但不存在这个选课关系

    ``` plain
    DELETE /class/452/student/7654
    ```

    响应

    ``` javascript
    { "message": "选课不存在"}
    ```

### `409`

表示处理当前请求会造成冲突，返回错误消息。

响应中必须包含一个 `message` 字段，值为向用户显示的可读的错误信息。

例：

已选过同课程的课，再次选择其他班级

``` plain
POST /class/452/student
```

响应

``` javascript
{ "message": "已选过同课程的课"}
```

## 登录与鉴权

本应用的登录有密码登录、微信小程序登录、微信扫码登录三种方式，三种方式统一采用 JWT 管理用户登录状态。

### 为什么采用 Json Web Token

因为**小程序没有 Cookie**

用户登录较为常见的一种思路即为在 Cookie 中保存当前用户的 Session ID ，但在小程序中并没有 Cookie 这一概念，取而代之的是一系列数据缓存 API 。而浏览器中也有对应的数据存储（ localStorage 等）。

而在不使用 Cookie 的登录/鉴权方法中，最适合的的选择就是 **Json Web Token** 。

并且， **JWT 在 Spring 、 ASP.Net 、 小程序下有大量的现成实现** 。

### 什么是 Json Web Token

> http://www.jianshu.com/p/576dbf44b2ae

简单来说，就是你用一些方式向服务器证明你是你，服务器发一个证书证明你是你，以后每次服务器需要知道你是谁的时候就出示你是你的证书就好了。 JWT 就是这样一个证书。

### 有其他选择吗

有，但是

* 无法形成统一标准，可能导致各小组的模块无法配合
* 实现不会比 JWT 简单

### 注册

.Net 实现使用用户名和密码请求 `POST /register` 注册。注册后的绑定信息使用 `PUT /me` 实现。
J2EE 实现视每个微信 UnionId 首次登入为注册，注册后的绑定信息使用 `PUT /me` 实现。

### 密码登录

1. 用户使用手机号和密码请求 `POST /signin`
2. 服务器端验证手机号和密码组合的有效性
    * 若无效，返回 `HTTP 401`
    * 若登录信息有效，服务器端根据手机号，获取用户的详细信息生成 JWT Payload，根据配置生成 JWT Header ，根据 JWT Header 中的签名算法和服务端的密钥生成 JWT Signature，并生成最终的 Json Web Token
3. 服务端返回登录状态，包括用户ID和类型、上一步生成的 JWT 等用户信息
4. 客户端验证服务端响应
    * 若响应 `HTTP 401`，则提示用户重新输入手机号和密码
    * 若响应 `HTTP 200`，则获取响应中的 JWT 写入localStorage，并根据用户信息跳转至对应页面

### 微信小程序登录

> 微信小程序登录的参考详见 [开放接口·小程序](https://mp.weixin.qq.com/debug/wxadoc/dev/api/api-login.html)

1. 小程序调用 `wx.login()` 获取 code
2. 小程序请求 `GET /signin?state=MiniProgram&code=%JSCODE%`，其中 `%JSCODE%` 为上一步获取的 code
3. 服务端请求 `https://api.weixin.qq.com/sns/jscode2session?appid=%APPID%&secret=%SECRET%&js_code=%JSCODE%&grant_type=authorization_code`
    * 若微信返回该 code 无效，返回 `HTTP 401`
    * 若微信返回该 code 有效，则解析微信的响应，服务器端根据 `unionid` 获取用户的详细信息（若用户不存在则注册新用户），并生成最终的 Json Web Token
4. 服务端返回登录状态，包括用户ID和类型、上一步生成的 JWT 等用户信息
5. 客户端验证服务端响应
    * 若响应 `HTTP 401`，则提示用户重试
    * 若响应 `HTTP 200`，则使用 `wx.setStorage()` 存储 JWT，并根据用户信息跳转至对应页面

### 微信扫码登录

> 微信扫码登录的参考详见 [资源中心·微信开放平台](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419316505&token=&lang=zh_CN)

1. 服务器端使用服务端密钥签名当前时间生成 state
2. 网页使用上一步生成的 state 调用 `wxLogin.js` 显示二维码
3. 微信用户使用微信扫描二维码并且确认登录后，网页跳转至 `GET /signin?state=%STATE%&code=%JSCODE%`，其中 `%JSCODE%` 为微信生成的扫码登录 code ，`%STATE%` 为上一步使用的 state
4. 服务端验证 state 签名的有效性并请求 `https://api.weixin.qq.com/sns/oauth2/access_token?appid=%APPID%&secret=%SECRET%&code=%CODE%&grant_type=authorization_code`
    * 若微信返回该 code 无效，返回 `HTTP 401`
    * 若微信返回该 code 有效，则解析微信的响应，服务器端根据 `unionid` 获取用户的详细信息（若用户不存在则注册新用户），并生成最终的 Json Web Token
5. 服务端返回登录状态，包括用户ID和类型、上一步生成的 JWT 等用户信息，禁止返回未加密的 `openid`、`unionid`、`access_token`、`refresh_token`
6. 客户端验证服务端响应
    * 若响应 `HTTP 401`，则提示用户重试
    * 若响应 `HTTP 200`，则获取响应中的 JWT 写入localStorage，并根据用户信息跳转至对应页面

### API 鉴权

所有需要登录的 API 请求均必须带上 JWT 进行鉴权。

用户进入客户端后，客户端必须验证是否保存了 JWT 并检查 JWT 的有效性。

* 若 JWT 不存在或无效，跳转至登录页面
* 对于小程序，还应该额外进行 `wx.checkSession`

#### 客户端

对于需要登录的每个 API 请求，均必须将 JWT 放入 HTTP 请求的 `Authorization` 头中

``` plain
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjI3NTciLCJ0eXBlIjoic3R1ZGVudCJ9.XkD1G0YE-PmBGUmndPv1VFXHckD7lrIQ8wMjJqxBqdQ
```

例如

``` javascript
var token = window.localStorage.getItem("jwt");
$.ajax({
    type: 'GET',
    url: '/me',
    headers: {"Authorization": "Bearer " + token},
    success: function(data){
        // 显示用户详细信息
    }
});
```

``` javascript
var token = wx.getStorageSync('jwt');
wx.request({
    url: '/me',
    header: {
        Authorization: 'Bearer ' + token
    },
    success: function (data) {
        // 显示用户详细信息
    }
});
```

#### 服务端

对于需要登录的每个 API 请求，均必须验证 JWT 的有效性，并根据 JWT 获取用户的信息。

### Json Web Token 规约

#### JWT Header

本应用的 JWT 头统一采用

``` javascript
{ "alg": "HS256", "typ": "JWT" }
```

这意味着 JWT 签名算法统一为 `HMAC SHA256`。

#### JWT Payload

JWT Payload 中必须包含用户ID `id`、 用户类型 `type` 和过期时间 `sub` 三个字段。具体实现可以添加其他需要的字段，但禁止包含可能影响系统安全或用户数据安全的未加密信息。

例如

``` javascript
{
  "id": "2757",
  "type": "student",
  "exp": 1511700699
}
```

``` javascript
{
  "id": "2757",
  "type": "student",
  "name": "赖博阳"
  "exp": 1511700699
}
```

#### JWT Signature

使用 Base64 后的 Header 与 Payload 和服务端密钥组合并使用 `HMAC SHA256` 签名后得出。
