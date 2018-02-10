# dotNet Core WebApi 学习Demo



## dotNet Core WebApi 跨域
使用cors组件实现跨域

- 引入 cors组件
```shell
dotnet add package Microsoft.AspNetCore.Cors --version 2.0.1
```

- 添加 cors服务 到 `ConfigureServices()`方法
```cs
services.AddCors(options => options.AddPolicy("CorsSample",p => p.WithOrigins("http://localhost:5000").AllowAnyMethod().AllowAnyHeader()));
```

- 设定header original 到 `Configure()`方法
```cs
//配置Cors
app.UseCors("CorsSample");

```

- 修改controller的 get 方法
```cs
namespace webApiDemo1.Controllers
{
    [Route("api/[controller]")]
    public class ValuesController : Controller
    {
        // GET api/values
        [HttpGet]
        [EnableCors("CorsSample")]
        public IEnumerable<string> Get()
        {
            return new string[] { DateTime.Now.ToString() };
        }

    }
}
```