# Todo WebApi

## 实现一个todo demo
本实例参考[MSDN文档](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-web-api)


## Todo Demo 将创建以下 API：

|     API             |        描述    |   请求正文  |    响应正文    |
|     :----:          |      :----:    |   :----:   |    :----:     |  
| GET /api/todo       | 获取所有待办事项 |   无       | 待办事项的数组 |
|GET /api/todo/{id}   | 按 ID 获取项    |      无    |   待办事项     |
| POST /api/todo      |    添加新项     |  待办事项   |    待办事项    |
| PUT /api/todo/{id}  |   更新现有项    | 待办事项    |       无       |
|DELETE /api/todo/{id}|    删除项       |   无       |       无       |

结构设计图示

![todo](./img/mvc-view.png)

## 添加项目
```sh
mkdir TodoApi
cd TodoApi
dotnet new webapi
```