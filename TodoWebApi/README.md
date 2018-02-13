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
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.3" />
  </ItemGroup>
```

## 添加模型类
添加名为`Models`的文件夹。 虽然可将模型类置于项目的任意位置，但按照惯例会使用`模型`文件夹。

添加带有以下代码的 TodoItem 类：
```cs
namespace TodoApi.Models
{
    public class TodoItem
    {
        public long Id { get; set; }
        public string Name { get; set; }
        public bool IsComplete { get; set; }
    }
}
```
创建 `TodoItem` 时，数据库将生成 `Id`。

## 创建数据库上下文

数据库上下文是为给定数据模型协调 `Entity Framework` 功能的主类。 将通过从 `Microsoft.EntityFrameworkCore.DbContext` 类派生的方式创建此类。

在`Models`文件夹中添加 TodoContext 类：

```cs
using Microsoft.EntityFrameworkCore;

namespace TodoApi.Models
{
    public class TodoContext : DbContext
    {
        public TodoContext(DbContextOptions<TodoContext> options)
            : base(options)
        {
        }

        public DbSet<TodoItem> TodoItems { get; set; }

    }
}
```

## 注册数据库上下文
在该步骤中，向依赖关系注入容器注册数据库上下文。 向依赖关系注入 (DI) 容器注册的服务（例如数据库上下文）可供控制器使用。

使用依赖关系注入的内置支持将数据库上下文注册到服务容器。 
```cs
//添加EF Core 和 Models
using Microsoft.EntityFrameworkCore;
using TodoApi.Models;

//在 ConfigureServices()方法中加入 DbContext 的注册 指定将内存数据库注入到服务容器中。
services.AddDbContext<TodoContext>(opt => opt.UseInMemoryDatabase("TodoList"));
```

## 添加控制器
在`Controllers`文件夹中，创建名为 `TodoController` 的类。 添加以下代码：
```cs
using System.Collections.Generic;
using Microsoft.AspNetCore.Mvc;
using TodoApi.Models;
using System.Linq;

namespace TodoApi.Controllers
{
    [Route("api/[controller]")]
    public class TodoController : Controller
    {
        private readonly TodoContext _context;

        //构造函数将一个项（如果不存在）添加到内存数据库。
        public TodoController(TodoContext context)
        {
            _context = context;

            if (_context.TodoItems.Count() == 0)
            {
                _context.TodoItems.Add(new TodoItem { Name = "Item1" });
                _context.SaveChanges();
            }
        }       
    }
}
```

## 获取待办事项
若要获取待办事项，请将下面的方法添加到 `TodoController` 类中。

```cs
[HttpGet]
public IEnumerable<TodoItem> GetAll()
{
    return _context.TodoItems.ToList();
}

[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
{
    var item = _context.TodoItems.FirstOrDefault(t => t.Id == id);
    if (item == null)
    {
        return NotFound();
    }
    return new ObjectResult(item);
}
```

## 路由和 URL 路径
`[HttpGet]` 特性指定 `HTTP GET` 方法。 每个方法的 URL 路径构造如下所示：

 在控制器的 Route 属性中采用模板字符串：

```cs
namespace TodoApi.Controllers
{
    [Route("api/[controller]")]
    public class TodoController : Controller
    {
        private readonly TodoContext _context;
```

 - 将 [controller] 替换为控制器的名称，即在控制器类名称中去掉“Controller”后缀。 对于此示例，控制器类名称为“Todo”控制器，根名称为“todo”。 ASP.NET Core 路由不区分大小写。
 - 如果 [HttpGet] 特性具有路由模板（如 [HttpGet("/products")]），则将它追加到路径。 此示例不使用模板。

 **在 GetById 方法中：**
```cs
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
{
    var item = _context.TodoItems.FirstOrDefault(t => t.Id == id);
    if (item == null)
    {
        return NotFound();
    }
    return new ObjectResult(item);
}
```
`{id}` 是 todo 项 的 ID 的占位符变量。 调用 GetById 时，它会将 URL 中`{id}`的值分配给方法的 id 参数。

Name = "GetTodo" 创建具名路由。

