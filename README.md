# C# / .NET Core Identity with MySQL
Create new project:
```bash
mkidr some-project && cd "$_"
dotnet new mvc --auth Individual
dotnet add package Pomelo.EntityFrameworkCore.MySql
dotnet restore
```

Modify the connection string in **`appsettings.json`**:
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "server=localhost;port=3306;database=DATABASE;user=USERNAME;password=PASSWORD;CharSet=utf8;SslMode=none;"
  },
  
  // ...
}
```

Reset old migragtions:
```bash
dotnet ef database update 0
dotnet ef migrations remove
```

And perform migration:
```bash
dotnet ef migrations add InitialCreate -o ./Data/Migrations/
dotnet ef database update
```

Then change from `UseSqlite()` to `UseMySql()` in `Startup.cs`:
```c#
public void ConfigureServices(IServiceCollection services)
{
    // ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseMySql(Configuration.GetConnectionString("DefaultConnection"))
        );
    services.AddDefaultIdentity<IdentityUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>();

    // ...
}
```

### Resources
1. [C# / .NET Core Identity with MySQL](https://retifrav.github.io/blog/2018/03/20/csharp-dotnet-core-identity-mysql/#net-core-21)
2. [Pomelo.EntityFrameworkCore.MySql Github Forum](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/issues/679)
