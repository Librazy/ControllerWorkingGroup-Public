# Call Sites

## Web

### Web 登录注册绑定

1. home.html
    ![home.html](images/home.html.png)
    * `登录.onclick`：  
        `POST /signin`  
        请求数据：  
        ``` javascript
        {
            "phone": "18911114514",
            "password": "qwer2345!"
        }
        ```  
        响应数据：  
        ``` javascript
        {
            "id": 3486,
            "type": "student",
            "name": "张三",
            "jwt":  "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjaWQiOiJPQTAwMDEiLCJpYXQiOjE0ODI2NTcyODQyMjF9.TeJpy936w610Vrrm+c3+RXouCA9k1AX0Bk8qURkYkdo="
        }
        ```  
2. register.html
    ![register.html](images/register.html.png)
    * `提交.onclick`：  

        > 注册分两步：根据手机号密码获取用户ID和登录状态、初始化个人信息

        `POST /register`  
        请求数据：  
        ``` javascript
        {
            "phone": "18911114514",
            "password": "qwer2345!"
        }
        ```  
        响应数据：  
        ``` javascript
        {
            "id": 3486,
            "type": "unbinded",
            "name": "",
            "jwt": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjaWQiOiJPQTAwMDEiLCJpYXQiOjE0ODI2NTcyODQyMjF9.TeJpy936w610Vrrm+c3+RXouCA9k1AX0Bk8qURkYkdo="
        }
        ```  
        `PUT /me`  
        请求数据：  
        ``` javascript
        {
            "name": "张三",
            "type": "student",
            "school": {
                "id": 32,
                "name": "厦门大学"
            },
            "gender": "male",
            "number": "24320152202333",
            "email": "xxxxx@xx.com"
        }
        ```  
        响应：HTTP 204
3. wechat.html
    ![wechat.html](images/wechat.html.png)
    * 微信扫码  
        扫码后会跳转到 `/signin?state=%STATE%&code=%JSCODE%`
        > 这里的“老师、学生”单选框并没有作用
4. bindStudent.html
    ![bindStudent.html](images/bindStudent.html.png)
    * `提交.onclick`：  
        `PUT /me`  
        请求数据：  
        ``` javascript
        {
            "name": "张三",
            "school": {
                "id": 32,
                "name": "厦门大学"
            },
            "gender": "male",
            "number": "24320152202333",
            "email": "xxxxx@xx.com"
        }
        ```  
        响应：HTTP 204
5. bindTeacher.html
    ![bindTeacher.html](images/bindTeacher.html.png)
    * `提交.onclick`：  
        `PUT /me`  
        请求数据：  
        ``` javascript
        {
            "name": "张三",
            "school": {
                "id": 32,
                "name": "厦门大学"
            },
            "gender": "male",
            "number": "24320152202333",
            "email": "xxxxx@xx.com"
        }
        ```  
        响应：HTTP 204
### Web Teacher

1. Teacher--基本信息：  
    ![Teacher-基本信息](images/Teacher-基本信息.png)
    * `window.onload`：  
        `GET /me`  
        请求数据：无  
        响应数据：包含教师基本信息的JSON，如  
        ``` javascript
        {
            "id": 3486,
            "type": "teacher",
            "name": "XXX",
            "number": "234546",
            "phone": "12345678978",
            "email": "xxxxx@xx.com",
            "gender": "male",
            "school": {
                "id": 32,
                "name": "厦门大学"
            },
            "title": "教授",
            "avatar": "/avatar/3486.png"
        }
        ```  
2. Teacher--基本信息修改
    ![Teacher-基本信息修改](images/Teacher-基本信息修改.png)
    * `window.onload`：  
        `GET /me`
    * `保存.onclick`：  
        `PATCH /me`  
        请求数据：
        ``` javascript
        {
            "name": "XXX",
            "number": "234546",
            "phone": "12345678978",
            "email": "xxxxx@xx.com",
            "gender": "male",
            "school": {
                "id": 32,
                "name": "厦门大学"
            },
            "title": "教授",
            "avatar": "/avatar/3486.png"
        }
        ```  
        响应：HTTP 204
3. Teacher--教师主页（课程信息）
    ![Teacher--教师主页（课程信息）](images/Teacher--教师主页（课程信息）.png)
    * `window.onload`：  
        `GET /course`
        请求数据：无  
        响应数据：包含当前用户相关联的课程列表
        ``` javascript
        [
            {
                "id": 1,
                "name": "OOAD",
                "numClass": 3,
                "numStudent": 60,
                "startTime": "2017-9-1",
                "endTime": "2018-1-1"
            },
            {
                "id": 2,
                "name": "J2EE",
                "numClass": 1,
                "numStudent": 60,
                "startTime": "2017-9-1",
                "endTime": "2018-1-1"
            }
        ]
        ```
    * `删除课程.onclick`：  
        `DELETE /course/{courseId}`
        请求数据：无  
        响应数据：HTTP 204
    ![Teacher--教师主页（课程信息）-修改课程](images/Teacher--教师主页（课程信息）-修改课程.png)
    * `修改课程.提交.onclick`：  
        `PUT /course/{courseId}`
        请求数据：修改后的课程信息  
        ``` javascript
        {
            "name": "OOAD",
            "description": "面向对象分析与设计",
            "startTime": "2017-09-20",
            "endTime": "2018-1-1",
            "proportions": {
                "3": 20,
                "4": 60,
                "5": 20,
                "report": 70,
                "presentation": 30
            }
        }
        ```
        响应：HTTP 204
4. Teacher--新建课程
    ![Teacher--新建课程](images/Teacher--新建课程.png)
    * `提交.onclick`：  
        `POST /course`
        请求数据：
        ``` javascript
        {
            "name": "OOAD",
            "description": "面向对象分析与设计",
            "startTime": "2017-09-20",
            "endTime": "2018-1-31",
            "proportions": {
                "3": 20,
                "4": 60,
                "5": 20,
                "report": 50,
                "presentation": 50
            }
        }
        ```
        响应数据：新课程ID
        ``` javascript
        {
            "id": 23
        }
        ```
5. 课程内页--首页：
    ![Teacher--课程内页-首页](images/Teacher--课程内页-首页.png)
    * `window.onload`：  
        `GET /course/{courseId}`  
        请求数据：无  
        响应数据：包含该课程的具体信息  
        ``` javascript
        {
            "id": 23,
            "name": "OOAD",
            "description": "面向对象的分析和设计"
        }
        ```
        `GET /course/{courseId}/class`  
        请求数据：无  
        响应数据：包含已创建的班级名称  
        ``` javascript
        [
             {
               "id": 45,
                "name": "周三1-2节"
             },
             {
                "id": 48,
                "name": "周三3-4节"
             }
        ]
        ```
        `GET /course/{courseId}/seminars`  
        请求数据：无  
        响应数据：包含已创建的讨论课名称  
        ```javascript
        [
            {
                "id": 29,
                "name": "界面原型设计",
                "description": "界面原型设计",
                "groupingMethod": "fixed",
                "startTime": "2017-09-25",
                "endTime": "2017-10-09"
            },
            {
                "id": 32,
                "name": "概要设计",
                "description": "模型层与数据库设计",
                "groupingMethod": "fixed",
                "startTime": "2017-10-10",
                "endTime": "2017-10-24"
            }
        ]
        ```
6. 课程内页--创建班级：
    ![Teacher--课程内页-创建班级](images/Teacher--课程内页-创建班级.png)
    * `window.onload`：  
        `GET /course/{courseId}`  
        见 5.  
        > 加载侧边栏，这里可以用 JavaScript 在 localStorage 内缓存课程信息，免去每次访问的重复请求，以下不再包含对侧边栏课程详情的请求
    * `提交.onclick`：  
        `POST /upload/classroster`
        请求数据：学生名单列表文件（ .xlsx 或 .csv ）  
        响应数据：  
        ```javascript
        {
            "url": "/roster/周三12班.xlsx"
        }
        ```
        `POST /course/{courseId}/class`  
        请求数据：包含班级的具体信息  
        ```javascript
        {
            "name": "周三1-2节",
            "site": "海韵212",
            "time": "周三一二节",
            "proportions": {
                "3": 10,
                "4": 60,
                "5": 30,
                "report": 50,
                "presentation": 50
            }
        }
        ```
        响应数据：HTTP 201
        ```javascript
        {"id": 45}
        ```
7. 课程内页--周三1-2节查看：
    ![Teacher--课程内页-周三1-2节查看](images/Teacher--课程内页-周三1-2节查看.png)
    * `window.onload`：  
        `GET /class/{classId}`  
        请求数据：无  
        响应数据：包含课程的具体信息
        ```javascript
        {
             "id": 23,
             "name": "周三1-2节",
             "numStudent": 120,
             "time": "周三一二节",
            "calling": true,
            "roster": "/roster/周三12班.xlsx",
            "proportions": {
                     "3": 20,
                     "4": 60,
                     "5": 20,
                     "report": 50,
                     "presentation": 50
             }
        }
        ```
8. 课程内页--周三1-2节修改：
    ![Teacher--课程内页-周三1-2节修改](images/Teacher--课程内页-周三1-2节修改.png)
    * `提交.onclick`：
        `PUT /class/{classId}`  
        请求数据：  
        ```javascript
        {
            "name": "周三1-2节",
            "site": "海韵212",
            "time": "周三 一二节",
            "proportions": {
                "3": 10,
                "4": 60,
                "5": 30,
                "report": 50,
                "presentation": 50
            }
        }
        ```
        响应：HTTP 204
9. 课程内页--创建讨论课：
    ![Teacher--课程内页-创建讨论课](images/Teacher--课程内页-创建讨论课.png)
    * `提交.onclick`：
        `POST /course/{courseId}/seminar`  
        请求数据：
        ```javascript
        {
            "name": "概要设计",
            "description": "模型层与数据库设计",
            "groupingMethod": "fixed",
            "startTime": "2017-10-10",
            "endTime": "2017-10-24",
            "proportions": {
                "3": 20,
                "4": 60,
                "5": 20,
                "report": 50,
                "presentation": 50
            }
        }
        ```
        响应数据：HTTP 201
        ```javascript
        {"id": 32}
        ```
10. 课程内页--讨论课1：
11. 课程内页--讨论课2：  
    ![Teacher--课程内页-讨论课2](images/Teacher--课程内页-讨论课2.png)
    * `window.onload`：  
        `GET /seminar/{seminarId}`  
        请求数据：无  
        响应数据：包含讨论课的基本信息
        ``` javascript
        {
            "id": 32,
            "name": "概要设计",
            "description": "模型层与数据库设计",
            "groupingMethod": "fixed",
            "startTime": "2017-10-10",
            "endTime": "2017-10-24"
        }
        ```  
        `GET /seminar/{seminarId}/topic`  
        请求数据：无  
        响应数据：包含讨论课的基本信息
        ``` javascript
        [
            {
                "id": 257,
                "name": "Domain model与模块划分",
                "description": "XXXXX",
                "groupLimit": 5,
                "groupLeft": 2
            }
        ]
        ```  
    * `删除讨论课.onclick`：
        `DELETE /seminar/{seminarId}`  
        请求数据：无  
        响应：HTTP 204
    * `删除话题.onclick`：
        `DELETE /topic/{topicId}`  
        请求数据：无  
        响应：HTTP 204
12. 课程内页--讨论课2修改：  
    ![Teacher--课程内页-讨论课2修改](images/Teacher--课程内页-讨论课2修改.png)
    * `window.onload`：  
        `GET /seminar/{seminarId}`  
    * `提交.onclick`：
        `PUT /seminar/{seminarId}`  
        请求数据：包含待修改信息的JSON或表单  
        ``` javascript
        {
            "name": "概要设计",
            "description": "模型层与数据库设计",
            "groupingMethod": "fixed",
            "startTime": "2017-10-11",
            "endTime": "2017-10-24",
            "proportions": {
                "3": 20,
                "4": 60,
                "5": 20,
                "report": 50,
                "presentation": 50
            }
        }
        ```  
        响应：HTTP 204
13. 课程内页--新讨论课：  
14. 课程内页--创建话题：  
    ![Teacher--课程内页-创建话题](images/Teacher--课程内页-创建话题.png)
    * `提交.onclick`：
        `POST /seminar/{seminarId}/topic`  
        请求数据：包含待新增信息的JSON或表单  
        ``` javascript
        {
            "name": "领域模型与模块",
            "description": "Domain model与模块划分",
            "groupLimit": 5
        }
        ```  
        响应数据：HTTP 201
        ``` javascript
        {"id": 2}
        ```
15. 课程内页--新讨论课查看话题（上课前）：  
16. 课程内页--新讨论课查看话题（上课后）：  
17. 话题A修改:  
    ![Teacher--课程内页-话题A修改](images/Teacher--课程内页-话题A修改.png)
    * `window.onload`：  
        `GET /topic/{topicId}`  
        请求数据：无  
        响应数据：包含该话题的JSON  
        ``` javascript
        {
            "id": 257,
            "name": "Domain model与模块划分",
            "description": "xxx",
            "groupLimit": 5
        }
        ```  
    * `提交.onclick`：  
        `PUT /topic/{topicId}`  
        请求数据：包含待修改信息的JSON或表单  
        ``` javascript
        {
            "name": "领域模型与模块",
            "description": "Domain model与模块划分",
            "groupLimit": 6
        }
        ```  
        响应：HTTP 204  
    * `重置.onclick`：
        `GET topic/{topicId}`  
18. 课程内页--评分：
    ![Teacher--课程内页-评分](images/Teacher--课程内页-评分.png)
    * `window.onload`：  
        `GET /seminar/{seminarId}/group`  
        获取所有小组  
        请求数据：无  
        响应数据：包含该讨论课的所有小组列表的JSON  
        ``` javascript
        [
            { "id": 257 }, { "id": 258 },
            { "id": 259 }, { "id": 260 },
            { "id": 261 }, { "id": 262 }
        ]
        ```
        对每个小组，获取小组详情  
        `GET /group/{groupId}`  
        请求数据：无  
        响应数据：包含小组ID对应的小组详情的JSON  
        ``` javascript
        {
            "id": 28,
            "leader": {
                "id": 8888,
                "name": "小红"
            },
            "members": [
                {
                    "id": 5324,
                    "name": "李四"
                },
                {
                    "id": 5678,
                    "name": "王五"
                }
            ],
            "report": "/report/xxxxx.pdf"
        }
        ```
19. 预览报告并打分：
    ![Teacher--课程内页-预览报告并打分](images/Teacher--课程内页-预览报告并打分.png)
    * `window.onload`：  
        `GET /group/{groupId}`  
        请求数据：无  
        响应数据：包含小组ID对应的小组详情的JSON  
        ``` javascript
        {
            "id": 28,
            "name": "1-A-1"
            "leader": {
                "id": 8888,
                "name": "小红"
            },
            "members": [
                {
                    "id": 5324,
                    "name": "李四"
                },
                {
                    "id": 5678,
                    "name": "王五"
                }
            ],
            "report": "/report/xxxxx.pdf"
        }
        ```
        `GET /seminar/{seminarId}/topic`  
        请求数据：无  
        响应数据：包含讨论课所有话题信息的JSON  
        ``` javascript
        [
            {
                "id": 257,
                "no": "A",
                "name": "Domain model与模块划分",
                "description": "XXXXXX"
            },
            {
                "id": 258,
                "no": "B",
                "name": "Domain model与模块划分",
                "description": "XXXXXX"
            },
            {
                "id": 259,
                "no": "C",
                "name": "Domain model与模块划分",
                "description": "XXXXXX"
            }
        ]
        ```  
    * `不知道是什么按钮界面组没画.onclick`：
        `PUT /group/{groupId}/grade`  
        请求数据：包含此小组报告分的JSON
        ``` javascript
            {"reportGrade": 4}
        ```
        响应：HTTP 204  

### Web Student

1. 基本信息  
    ![Student--基本信息](images/Student--基本信息.png)
    * `window.onload`：  
        `GET /me`  
        请求数据：无  
        响应数据：包含学生基本信息的JSON  
        ``` javascript
        {
            "id": 3486,
            "type": "student",
            "name": "张三",
            "number": "23320152202333",
            "phone": "18911114514",
            "email": "23320152202333@stu.xmu.edu.cn",
            "gender": "male",
            "school": {
                "id": 32,
                "name": "厦门大学"
            },
            "title": "",
            "avatar": "/avatar/3486.png"
        }
        ```  
        > 个人信息是否包含学院的问题目前界面组尚未回应
2. 基本信息修改  
    ![Student--基本信息修改](images/Student--基本信息修改.png)
    * `window.onload`：  
        `GET /me`  
    * `提交.onclick`：  
        `PUT /me`  
        请求数据：包含待修改信息的JSON  
        ``` javascript
        {
            "id": 3486,
            "type": "student",
            "name": "张三",
            "number": "23320152202333",
            "phone": "18911114514",
            "email": "23320152202333@stu.xmu.edu.cn",
            "gender": "female",
            "school": {
                "id": 32,
                "name": "厦门大学"
            },
            "title": "",
            "avatar": "/avatar/3486.png"
        }
        ```  
        响应数据： HTTP 204  
        > 具体哪些个人信息可修改界面组暂未确定
3. 学生主页（课程信息）
    ![Student--学生主页（课程信息）](images/Student--学生主页（课程信息）.png)
    * `window.onload`：  
        `GET /class`  
        请求数据：无  
        响应数据：包含与该学生关联的班级信息，如  
        ``` javascript
        [
            {
                "id": 23,
                "name": "周三1-2节",
                "site": "公寓405",
                "course": {
                    "id": 2,
                    "name": "OOAD",
                    "lessonsPerWeek": 6
                }
            },
            {
                "id": 42,
                "name": "一班",
                "site": "海韵202",
                "course": {
                    "id": 5,
                    "name": ".Net 平台开发",
                    "lessonsPerWeek": 2
                }
            }
        ]
        ```  
    * `退选课程.onclick`：  
        `DELETE /class/{classId}/student/{studentId}`  
        请求数据:无  
        响应：HTTP 204  
4. 选择课程（未查询）  
    ![Student--选择课程（未查询）](images/Student--选择课程（未查询）.png)
    * `window.onload`：  
        `GET /class?courseName=*&teacherName=*`  
        请求数据：无  
        响应数据：包含所有班级的信息，如  
        ``` javascript
        [
            {
                "id": 23,
                "name": "周三1-2节",
                "site": "公寓405",
                "course": {
                    "id": 2,
                    "name": "OOAD",
                    "lessonsPerWeek": 6
                }
            },
            {
                "id": 42,
                "name": "一班",
                "site": "海韵202",
                "course": {
                    "id": 5,
                    "name": ".Net 平台开发",
                    "lessonsPerWeek": 2
                }
            }
        ]
        ```  
    * `选择课程.onclick`：  
        `POST /class/{classId}/student`  
        请求数据：学生ID  
        ``` javascript
        {
            "id": 233
        }
        ```
        响应数据：HTTP 201  
        ``` javascript
        {
            "url": "/class/34/student/2757"
        }
        ```
5. 选择课程（查询后）  
    ![Student--选择课程（查询后）](images/Student--选择课程（查询后）.png)
    * `window.onload`：  
        `GET /class?courseName=ooad&teacher=邱明`  
        请求数据：无  
        响应数据：包含查询课程的信息，如  
        ``` javascript
        [
            {
                "id": 23,
                "name": "周三1-2节",
                "site": "公寓405",
                "course": {
                    "id": 2,
                    "name": "OOAD",
                    "lessonsPerWeek": 6
                }
            },
            {
                "id": 24,
                "name": "周三3-4节",
                "site": "公寓405",
                "course": {
                    "id": 2,
                    "name": "OOAD",
                    "lessonsPerWeek": 6
                }
            },
        ]
        ```
    * `选择课程.onclick`：  
        `POST /class/{classId}/student`  
6. 课程内页-首页
    ![Student--课程内页-首页](images/Student--课程内页-首页.png)
    * `window.onload`：  
        `GET /course/{courseId}`  
        请求数据：无  
        响应数据：  
        ```javascript
        {
            "id": 222,
            "name": "面向对象的分析与设计",
            "discription": "这个人很懒，什么都没有留下"
        }
        ```
        `GET /course/{courseId}/seminar`  
        请求数据：无  
        响应数据：  
        ``` javascript
        [
            {
                "id": 29,
                "name": "界面原型设计",
                "description": "界面原型设计",
                "groupingMethod": "fixed",
                "startTime": "2017-09-25",
                "endTime": "2017-10-09"
            },
            {
                "id": 32,
                "name": "概要设计",
                "description": "模型层与数据库设计",
                "groupingMethod": "fixed",
                "startTime": "2017-10-10",
                "endTime": "2017-10-24"
            }
        ]
        ```
7. 讨论课（随机分组）
    ![Student--课程内页-讨论课（随机分组）](images/Student--课程内页-讨论课（随机分组）.png)
    > TODO：图片未更新
    * `window.onload`：  
        `GET /seminar/{seminarId}`  
        请求数据：无  
        响应数据：包含讨论课相关信息的JSON  
        ``` javascript
        {
            "id": 32,
            "name": "概要设计",
            "description":"模型层与数据库设计",
            "groupingMethod": "fixed",
            "startTime": "2017-10-10",
            "endTime": "2017-10-24"
        }
        ```  
        `GET /seminar/{seminarId}/group/my`  
        [按讨论课ID获取学生所在小组](https://app.swaggerhub.com/apis/liqueurlibrazy/classmanagementsystem/1.0.0-beta.3#/seminar/getStudentGroupBySeminarId)
8. 讨论课（固定分组）
    ![Student--课程内页-讨论课（固定分组）](images/Student--课程内页-讨论课（固定分组）.png)
    * `window.onload`：  
        `GET /seminar/{seminarId}`  
        请求数据：无  
        响应数据：包含讨论课相关信息的JSON，如  
        ``` javascript
        {
          "id": 32,
          "name": "概要设计",
          "description":"模型层与数据库设计",
          "groupingMethod": "fixed",
          "startTime": "2017-10-10",
          "endTime": "2017-10-24"
        }
        ```  
        `GET /seminar/{seminarId}/topic`  
        请求数据：无  
        响应数据：话题详情列表  
        `GET /seminar/{seminarId}/group?include={studentId}`  
        请求数据：无  
        响应数据：学生所在的组的ID  
        ``` javascript
        [{
            "id": 28
        }]
        ```
    * `选择话题.onclick`：  
        `POST /group/{groupId}/topic`  
        请求数据：话题ID
        ``` javascript
        {
          "id": 23
        }
        ```
        响应数据：HTTP 201
        ``` javascript
        {
            "url": "/group/27/topic/23"
        }
        ```
    ![Student--课程内页-讨论课（固定分组）-上传报告](images/Student--课程内页-讨论课（固定分组）-上传报告.png)
    * `上传报告.提交.onclick`  
        `POST /upload/report`  
        请求数据：报告PDF文件  
        响应数据：
        ``` javascript
        {
            "url": "/upload/report/124324.pdf"
        }
        ```
        `PUT /group/{groupId}`  
        请求数据：
        ``` javascript
        {
            "report": "/upload/report/124324.pdf"
        }
        ```
        响应：HTTP 204
9. 固定分组
    ![Student--课程内页-固定分组](images/Student--课程内页-固定分组.png)
    * `window.onload`：
        `GET /class/{classId}/classgroup`  
        请求数据：无  
        响应数据：包含该组所有学生信息的JSON  
        ```javascript
        {
            "leader": {
                "id": 2757,
                "name": "张三",
                "number": "23320152202333"
            },
            "members": [
                {
                "id": 2756,
                "name": "李四",
                "number": "23320152202443"
                },
                {
                "id": 2777,
                "name": "王五",
                "number": "23320152202433"
                }
            ]
        }
        ```
10. 固定分组2
    ![Student--课程内页-固定分组2](images/Student--课程内页-固定分组2.png)
    * `window.onload`：
        `GET /class/{classId}/classgroup`  
    * `查询.onclick`：  
        `GET /class/{classId}/student?nameBeginWith=张`  
        请求数据：无  
        响应数据：得到学生列表
        ``` javascript
        [
            {
                "id": 233,
                "name": "张三",
                "number": 24320152202333
            },
            {
                "id": 245,
                "name": "张八",
                "number": 24320152202334
            }
        ]
        ```
    * `确定.onclick`：  
        `PUT /class/{classId}/classgroup/add`  
        [添加班级小组成员](https://app.swaggerhub.com/apis/liqueurlibrazy/classmanagementsystem/1.0.0-beta.3#/class/addClassGroupMember)
11. 固定分组3
    ![Student--课程内页-固定分组3](images/Student--课程内页-固定分组3.png)
    见 9
12. 查看分数
    ![Student--课程内页-查看分数](images/Student--课程内页-查看分数.png)
    * `window.onload`：
        `GET /group/{groupId}/grade`
        请求数据：无
        响应数据：得到该学生的小组分数
        ``` javascript
        {
             "presentationGrade": [
                {
                     "topicId": 257,
                     "grade": 4
                },
               {
                     "topicId": 258,
                     "grade": 5
               }
            ],
            "reportGrade": null
        }
        ```

## 小程序

### 小程序 登录注册绑定

1. LoginUI  
    ![LoginUI](images/LoginUI.png)
    * 点击登录按钮：`POST /signin`  
        请求数据:  
        ```javascript
        {
          "phone":"18911114514",
          "password":"qwer2345!"
        }
        ```
        响应数据:  
        ```javascript
        {
            "id": 3486,
            "type": "student",
            "name": "张三",
            "jwt":     "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjaWQiOiJPQTAwMDEiLCJpYXQiOjE0ODI2NTcyODQyMjF9.TeJpy936w610Vrrm+c3+RXouCA9k1AX0Bk8qURkYkdo="
        }
        ```
2. TeacherBindingUI  
    ![TeacherBindingUI](images/TeacherBindingUI.png)
    * 绑定账号：`PUT /me`  
        请求数据：  
        ```javascript
        {
            "number": "23320152202333",
            "name":"张三",
            "school": {
                "id": 32,
                "name": "厦门大学"
            }
        }
        ```
        响应：HTTP 204  
3. StudentBindingUI  
    ![StudentBindingUI](images/StudentBindingUI.png)
    * 绑定账号：`PUT /me`  
        请求数据：  
        ```javascript
        {
            "number": "23320152202333",
            "name":"张三",
            "school": {
                "id": 32,
                "name": "厦门大学"
            }
        }
        ```
        响应：HTTP 204  
4. RegistUI  
    ![RegistUI](images/RegistUI.png)
    * .NET手机号密码注册:`POST /register`  
        请求数据：  
        ```javascript
        {
          "phone":"18811114514",
          "password":"qwer2345!"
        }
        ```
        响应数据：HTTP 200  
        ``` javascript
        {
            "id": 3486,
            "type": "unbinded",
            "name": "",
            "jwt":     "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjaWQiOiJPQTAwMDEiLCJpYXQiOjE0ODI2NTcyODQyMjF9.TeJpy936w610Vrrm+c3+RXouCA9k1AX0Bk8qURkYkdo="
        }
        ```
        > RegistUI 应为 RegisterUI
5. ChooseCharacter  
    ![ChooseCharacter](images/ChooseCharacter.png)  
    * 选择身份`PUT /me`  
        请求数据：  
        ```javascript
        {
          "type":"student"
        }
        ```
        响应：HTTP 204

### 小程序 Teacher

1. Teacher_MainUI：  
    ![Teacher_MainUI](images/Teacher_MainUI.png)
    * 获取：  
        上：`GET /me`  
        请求数据：无  
        响应数据：包含教师账号信息的JSON，如  
        ``` javascript
        {
            "id": 3486,
            "type": "teacher",
            "name": "张三",
            "number": "24321432534",
            "phone": "18159215806",
            "email": "23320152202333@stu.xmu.edu.cn",
            "gender": "male",
            "school": {
                "id": 32,
                "name": "厦门大学"
            },
            "title": "教授",
            "avatar": "/avatar/3486.png"
        }
        ```
        下：`GET /course`  
        请求数据：无  
        响应数据：包括与该教师关联的所有课程名称，如  
        ``` javascript
        [{"id": 1, "name": "J2EE"},
        {"id": 2, "name": "OOAD"},
        {"id": 3, "name": "操作系统"},
        {"id": 4, "name": "数据仓库"}]
        ```
2. CheckTeacherInfoUI：  
    ![CheckTeacherInfoUI](images/CheckTeacherInfoUI.png)
    * 获取：`GET /me`  
        请求数据：无  
        响应数据：包含教师基本信息的JSON，如  
        ``` javascript
        {
            "id": 3486,
            "type": "teacher",
            "name": "张三",
            "number": "24321432534",
            "phone": "18159215806",
            "email": "23320152202333@stu.xmu.edu.cn",
            "gender": "male",
            "school": {
                "id": 32,
                "name": "厦门大学"
            },
            "title": "教授",
            "avatar": "/avatar/3486.png"
        }
        ```
    * 点击“点击头像修改”：  
        `POST /upload/avatar`  
        请求数据：头像图片文件  
        响应数据：
        ``` javascript
        {
            "url": "/avatar/3486.png"
        }
        ```
        `PUT /me`  
        请求数据：
        ``` javascript
        {"avatar": "/avatar/3486.png"}
        ```
        响应数据：HTTP 204  
    * 点击“解绑”按钮(J2EE)：`PUT /me`  
        请求数据：
        ``` javascript
        {"unionID": ""}
        ```
        响应数据：HTTP 204  
    * 点击“解绑账号”按钮(.NET)：`PUT /me`  
        请求数据：
        ``` javascript
        {"phone": ""}
        ```
        响应数据：HTTP 204  
3. ClassManage：  
    ![ClassManage](images/ClassManage.png)
    * 获取课程信息 `GET /course/{courseId}`  
        请求数据:无  
        响应数据：  
        ``` javascript
        {
            "id": 23,
            "name": "OOAD",
            "description": "面向对象分析与设计"
        }
        ```
    * 获取讨论课信息  `GET /course/{courseId}/seminar`  
        请求数据:无  
        响应数据：
        ``` javascript
        [
            {
                "id": 29,
                "name": "界面原型设计",
                "description": "界面原型设计",
                "groupingMethod": "fixed",
                "startTime": "2017-09-25",
                "endTime": "2017-10-09"
            },
            {
                "id": 32,
                "name": "概要设计",
                "description": "模型层与数据库设计",
                "groupingMethod": "fixed",
                "startTime": "2017-10-10",
                "endTime": "2017-10-24"
            }
        ]
        ```
    * 获取班级:`GET /course/{courseId}/class`  
        请求数据:无  
        响应数据:
        ``` javascript  
        [
            {
                "id": 45,
                "name": "周三1-2节"
            },
            {
                "id": 48,
                "name": "周三3-4节"
            }
        ]
        ```
4. FixedRollStartCallUI：  
    ![FixedRollStartCallUI](images/FixedRollStartCallUI.png)
    * 班级详情：`GET /class/{classId}`  
        请求数据：无
        响应数据：
        ``` javascript
        {
            "id": 23,
            "name": "周三1-2节",
            "numStudent": 120,
            "time": "周三 一二节",
            "calling": true,
            "roster": "/roster/周三12班.xlsx",
            "proportions": {
                "3": 20,
                "4": 60,
                "5": 20,
                "report": 50,
                "presentation": 50
            }
        }
        ```
        > 下面签到部分不再重复班级详情
    * 开始签到：`PUT /class/{classId}`  
        请求数据:  
        ``` javascript
        {"calling": true}
        ```
        响应：HTTP 204
5. FixedRollCallUI：  
    ![FixedRollCallUI](images/FixedRollCallUI.png)
    * 结束签到：`PUT /class/{classId}`  
        请求数据:  
        ``` javascript
        {"calling": true}
        ```
        响应：HTTP 204
6. FixedEndRollCallUI：  
    无
7. RollCallListUI  
    ![RollCallListUI](images/RollCallListUI.png)
    * 获取签到中状态： `/class/{classId}/attendance?showPresent=true`  
    请求数据: 无  
    响应数据：  
    ``` javascript
    {
        "numPresent": 4,
        "present": [
            {
            "id": 2357,
            "name": "张三"
            },
            {
            "id": 8232,
            "name": "李四"
            }
        ]
    }
    ```  
8. FixedRollCallEndUI1  
    ![FixedRollCallEndUI1](images/FixedRollCallEndUI1.png)
    * 获取签到结束状态：`/class/{classId}/attendance?showPresent=true&showLate=true&showAbsent=true`  
    请求数据: 无  
    响应数据：  
    ``` javascript
    {
        "numPresent": 4,
        "present": [
            {
            "id": 2357,
            "name": "张三"
            },
            {
            "id": 8232,
            "name": "李四"
            }
        ],
        "late": [
            {
            "id": 3412,
            "name": "王五"
            },
            {
            "id": 5234,
            "name": "王七九"
            }
        ],
        "absent": [
            {
            "id": 34,
            "name": "张六"
            }
        ]
    }
    ```  
9. FixedGroupInfoUI  
    ![FixedGroupInfoUI](images/FixedGroupInfoUI.png)
    * 查看分组:`GET /seminar/{seminar}/group`  
        请求数据: 无  
        响应数据: 小组ID列表  
        ``` javascript
        [
            {
                "id": 28
            },
            {
                "id": 29
            }
        ]
        ```
        对于展开的小组：  
        `GET /group/{groupId}?embedTopics=true`  
        请求数据:无  
        响应数据:  
        ``` javascript
        {
            "id": 28,
            "leader": {
                "id": 8888,
                "name": "张三"
            },
            "members": [
                {
                "id": 5324,
                "name": "李四"
                },
                {
                "id": 5678,
                "name": "王五"
                }
            ],
            "topics": [
                {
                "id": 257,
                "name": "领域模型与模块"
                }
            ],
            "report": ""
        }
        ```
10. GroupInfoUI:  
    ![GroupInfoUI](images/GroupInfoUI.png)
    * 查看分组同上
    * 查看迟到签到学生  
        `GET /class/{classId}/attandance?showLate=true`  
         请求数据：无  
         响应数据:包含迟到的学生  
        ``` javascript
        {
            "numPresent": 4,
            "present": [
                {
                "id": 2357,
                "name": "张三"
                },
                {
                "id": 8232,
                "name": "李四"
                }
            ],
            "late": [
                {
                "id": 3412,
                "name": "王五"
                },
                {
                "id": 5234,
                "name": "王七九"
                }
            ],
            "absent": [
                {
                "id": 34,
                "name": "张六"
                }
            ]
        }
        ```
    * 点击+号：`PUT /group/{groupId}`  
        请求数据:  
        ``` javascript
        {
            "leader": {
                "id": 5678
            },
            "members": [
                {
                "id": 8888
                },
                {
                "id": 5324
                }
            ]
        }
        ```
         响应数据:HTTP 204  
11. GroupInfoUI2：  
    同上  
12. RandomRollStartCallUI  
    见4  
13. RandomRollCallUI  
    见5  
14. RandomEndRollCallUI  
    见6  
15. RollCallListUI  
    见7
16. ChooseSchool1/ChooseSchool2  
    无
17. ChooseSchool3/ChooseSchool4  
    ![ChooseSchool4](images/ChooseSchool4.png)
    * 获取：`GET /school?city=厦门`  
        请求数据：无  
        响应数据：符合查询条件的学校列表
        ``` javascript
        [
            {
                "id": 32,
                "name": "厦门大学",
                "province": "福建",
                "city": "厦门"
            },
            {
                "id": 37,
                "name": "厦门理工学院",
                "province": "福建",
                "city": "厦门"
            }
        ]
        ```  
    * 选中“厦门大学”：`PUT /me`  
        请求数据：
        ``` javascript
        {
            "school": {
                "id": 32,
                "name": "厦门大学",
                "province": "福建",
                "city": "厦门"
            }
        }
        ```
        响应数据：HTTP 204  
18. ChooseSchool5-6/ChooseSchoolNoSchoolForTeacher1-4  
    ![ChooseSchoolNoSchoolForTeacher2](images/ChooseSchoolNoSchoolForTeacher2.png)  
    * 查询获取：`GET /school?nameBeginWith=厦门`  
        请求数据：无  
        响应数据：符合查询条件的学校列表
        ``` javascript
        [
            {
                "id": 32,
                "name": "厦门大学",
                "province": "福建",
                "city": "厦门"
            },
            {
                "id": 37,
                "name": "厦门理工学院",
                "province": "福建",
                "city": "厦门"
            },
            {
                "id": 38,
                "name": "厦门城市职业学院",
                "province": "福建",
                "city": "厦门"
            }
        ]
        ```  
19. CreateSchoolUI  
    ![CreateSchoolUI](images/CreateSchoolUI.png)  
    * 点击“创建”按钮：`POST /school`  
        请求数据：  
        ```javascript
        {
            "name": "厦门市人民公园",
            "province": "福建",
            "city": "厦门"
        }
        ```
        响应数据：HTTP 201  
        ```javascript
        {"id":38}     //返回学校的ID
        ```  
20. Teacher_MainUI2：  
    ![Teacher_MainUI2](images/Teacher_MainUI2.png)
    * 获取：`GET /course`  
        请求数据：无  
        响应数据：包括与该教师关联的所有课程名称，如  
        ``` javascript
        [{"id": 1, "name": "J2EE"},
        {"id": 2, "name": "OOAD"},
        {"id": 3, "name": "操作系统"},
        {"id": 4, "name": "数据仓库"}]
        ```

### 小程序 Student

1. CheckStudentInfoUI：  
    ![CheckStudentInfoUI](images/CheckStudentInfoUI.png)
    * 获取：`GET /me`  
        请求数据：无  
        响应数据：包含学生基本信息  
        ``` javascript
        {
            "id": 3486,
            "type": "student",
            "name": "张三",
            "number": "23320152202333",
            "phone": "18911114514",
            "email": "23320152202333@stu.xmu.edu.cn",
            "gender": "male",
            "school": {
                "id": 32,
                "name": "厦门大学"
            },
            "title": "",
            "avatar": "/avatar/3486.png"
        }
        ```
    * 点击“点击头像修改”：  
        `POST /upload/avatar`  
        请求数据：头像图片文件  
        响应数据：
        ``` javascript
        {
            "url": "/avatar/3486.png"
        }
        ```
        `PUT /me`  
        请求数据：
        ``` javascript
        {"avatar": "/avatar/3486.png"}
        ```
        响应数据：HTTP 204  
    * 点击“解绑”按钮(J2EE)：`PUT /me`  
        请求数据：
        ``` javascript
        {"unionID": ""}
        ```
        响应数据：HTTP 204  
    * 点击“解绑账号”按钮(.NET)：`PUT /me`  
        请求数据：
        ``` javascript
        {"phone": ""}
        ```
        响应数据：HTTP 204
2. CourseUI  
    ![CourseUI](images/CourseUI.png)  
    `GET /course/{courseId}/seminar`  
    请求数据：无  
    响应数据：包含该学生的该课程的讨论课  
    ``` javascript
    [
        {
            "id": 29,
            "name": "界面原型设计",
            "description": "界面原型设计",
            "groupingMethod": "fixed",
            "startTime": "2017-09-25",
            "endTime": "2017-10-09"
        },
        {
            "id": 32,
            "name": "概要设计",
            "description": "模型层与数据库设计",
            "groupingMethod": "fixed",
            "startTime": "2017-10-10",
            "endTime": "2017-10-24"
        }
    ]
    ```  
    对于每个讨论课，请求 `GET /seminar/{seminarId}/group?include={studentId}` 获取小组 `{groupId}`  
    `GET /group/{groupId}/grade`  
    请求数据：无  
    响应数据：  
    ``` javascript
    {
        "presentationGrade": [
            {
                "topicId": 257,
                "grade": 4
            },
            {
                "topicId": 258,
                "grade": 5
            }
        ],
        "reportGrade": 3,
        "grade": 4
    }
    ```  
3. CourseInfoUI  
    ![CourseInfoUI](images/CourseInfoUI.png)  
    `GET /course/{courseId}`  
    请求数据：无  
    响应数据：课程信息
    ``` javascript
    {
    "id": 23,
    "name": "OOAD",
    "description": "面向对象分析与设计",
    "teacherName": "邱明",
    "teacherEmail": "mingqiu@xmu.edu.cn"
    }
    ```  
4. 已完成分组（FixedGroupLeaderUI、FixedGroupMemberUI、FixedGroupNoLeaderUI）  
    ![FixedGroupNoLeaderUI2](images/FixedGroupNoLeaderUI2.png)  
    * 获得分组信息：
        `GET /seminar/{seminarId}/group?include={studentId}`  
        请求数据：无  
        响应数据：该学生所在的分组的ID  
        ``` javascript
        [{
            "id": 28
        }]
        ```  
        `GET /group/{groupId}?embedTopics=true`
        请求数据：无  
        响应数据：小组详情  
        ``` javascript
        {
            "id": 28,
            "leader": {
                "id": 8888,
                "name": "张三"
            },
            "members": [
                {
                "id": 5324,
                "name": "李四"
                },
                {
                "id": 5678,
                "name": "王五"
                }
            ],
            "topics": [
                {
                "id": 257,
                "name": "领域模型与模块"
                }
            ],
            "report": ""
        }
        ```  
    * 队长辞职/成为队长：`PUT /group/{groupID}`  
        请求数据：包含修改的信息（队长id）  
        ``` javascript
        {
            "leader": {
                "id": 5678
            },
            "members": [
                {
                    "id": 8888
                },
                {
                    "id": 5324
                }
            ]
        }
        ```  
        响应数据：HTTP 204  
5. 选题（FixedGroupChooseTopicUI1、FixedGroupChooseTopicUI2）  
    ![FixedGroupChooseTopicUI1](images/FixedGroupChooseTopicUI1.png)  
    * 获得所有话题 `GET /seminar/{seminarId}/topic`  
        请求数据：无  
        响应数据：包含所有话题的信息的JSON  
        ``` javascript
        [
            {
                "id": 257,
                "name": "领域模型与模块",
                "description": "Domain model与模块划分",
                "groupLimit": 5,
                "groupLeft": 2
            }
        ]
        ```  
    ![FixedGroupChooseTopicUI2](images/FixedGroupChooseTopicUI2.png)  
    * 选择话题 `POST /group/{groupId}/topic`
        请求数据：话题ID  
        ``` javascript
        {
            "id": 23
        }
        ```
        响应数据：  
        ``` javascript
        {
            "url": "/group/28/topic/23"
        }
        ```
6. 打分（GradePresentationUI、GradePresentationEndUI）  
    ![GradePresentationUI](images/GradePresentationUI.png)  
    `GET /seminar/{seminarId}/group?gradeable={true}`  
    请求数据：无  
    响应数据：学生可打分的组的列表  
    ``` javascript
    [
        {
            "id": 27,
            "name": A1
        },
        {
            "id": 29,
            "name": B2
        }
    ]
    ```
    打分 `PUT /group/{groupId}/grade/{studentId}`  
    请求数据：包含分数的JSON  
    ``` javascript
    {
        "presentationGrade": [
        {
            "topicId": 257,
            "grade": 4
        },
        {
            "topicId": 258,
            "grade": 5
        }
        ]
    }
    ```  
    响应数据：HTTP 204  
7. RandomGroupUI：  
    ![RandomGroupUI](images/RandomGroupUI.png)  
    `GET /seminar/{seminarId}/group?include={studentId}`  
    请求数据：无  
    响应数据：学生所在的组的ID  
    ``` javascript
    [{
        "id": 28
    }]
    ```
    `GET /group/{groupId}?embedTopics=true`  
    请求数据：无  
    响应数据：小组详情  
    ``` javascript
    {
        "id": 28,
        "leader": {
            "id": 8888,
            "name": "张三"
        },
        "members": [
            {
                "id": 5324,
                "name": "李四"
            },
            {
                "id": 5678,
                "name": "王五"
            }
        ],
        "topics": [
            {
                "id": 257,
                "name": "领域模型与模块"
            }
        ],
        "report": ""
    }
    ```
    `PUT /group/{groupId}`  
    请求数据：固定格式，即更改自己的角色为队长  
    ```javascript
    {
      "id": 29,
      "leader": {
        "id": 1,
        "name": "张二",
        "number": "24320162093849"
      },
      "members": [
        {
          "id": 230,
          "name": "李四",
          "number": "24320152202978"
        },
        {
          "id": 2908,
          "name": "李二狗",
          "number": "24320152202998"
        }
      ],
      "topics": [
            {
                "id": 10,
                "name": "领域模型"
            }
        ]
    }
    ```
    响应数据：HTTP 204  
8. RandomGroupChoosetopic：  
    同 4  
9. RollCallUI：  
    ![RollCallUI](images/RollCallUI.png)  
    `GET /seminar/{seminarId}/detail`  
    [按ID获取讨论课详情](https://app.swaggerhub.com/apis/liqueurlibrazy/classmanagementsystem/1.0.0-beta.3#/seminar/getSeminarDetailById)  
    `PUT /class/{classId}/attendance/{studentId}`  
    请求数据：GPS位置
    ``` javascript
    {
        "longitude": 118.1132721,
        "latitude": 24.4307197,
        "elevation": 18.42
    }
    ```
    响应数据：HTTP 204  
