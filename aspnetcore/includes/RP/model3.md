<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>添加基架工具并执行初始迁移

从命令行运行以下 .NET Core CLI 命令：

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`add package` 命令安装运行基架引擎所需的工具。

`ef migrations add InitialCreate` 命令生成用于创建初始数据库架构的代码。 此架构以（Models/MovieContext.cs 文件中的）`DbContext` 中指定的模型为基础。 `Initial` 参数用于为迁移命名。 可以使用任意名称，但是按照惯例应选择描述迁移的名称。 有关详细信息，请参阅[迁移简介](xref:data/ef-mvc/migrations#introduction-to-migrations)。

`ef database update` 命令在用于创建数据库的 Migrations/\<time-stamp>_InitialCreate.cs 文件中运行 `Up` 方法。