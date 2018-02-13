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

尝试运行 `dotnet run ` 然后再本地访问测试

`http://localhost:5000/api/values` 

返回 `["value1","value2"]`

## 添加对 Entity Framework Core 的支持
本实例使用 EF Core 调用本地 Sql 进行测试

在 `Todo WebApi.csproj` 文件中添加如下内容

```cs
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <Folder Include="wwwroot\" />
  </ItemGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.3" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.1" />
  </ItemGroup>

</Project>
```