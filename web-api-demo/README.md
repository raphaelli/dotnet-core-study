# dotNet Core WebApi 学习Demo

## 实现一个todo demo
本实例参考[MSDN文档](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-web-api)

本教程将创建以下 API：

|     API             |        描述    |   请求正文  |    响应正文    |
|     :----:          |      :----:    |   :----:   |    :----:     |  
| GET /api/todo       | 获取所有待办事项 |   无       | 待办事项的数组 |
|GET /api/todo/{id}   | 按 ID 获取项    |      无    |   待办事项     |
| POST /api/todo      |    添加新项     |  待办事项   |    待办事项    |
| PUT /api/todo/{id}  |   更新现有项    | 待办事项    |       无       |
|DELETE /api/todo/{id}|    删除项       |   无       |       无       |

结构设计图示

![todo](./img/mvc-view.png)

## 实现一个诗词demo

## dotNet Core WebApi 跨域
查看Wiki[dotNet Core WebApi 跨域](https://github.com/raphaelli/dotnet-core-study/wiki/dotNet-Core-WebApi-Cors-跨域) 