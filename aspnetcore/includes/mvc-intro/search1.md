# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>将搜索添加到 ASP.NET Core MVC 应用

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本部分中，将向 `Index` 操作方法添加搜索功能，以实现按“类型”或“名称”搜索电影。

使用以下代码更新 `Index` 方法：
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

`Index` 操作方法的第一行创建了 [LINQ](https://docs.microsoft.com/dotnet/standard/using-linq) 查询用于选择电影：

```csharp
var movies = from m in _context.Movie
             select m;
```

此时仅对查询进行了定义，它还不会针对数据库运行。

如果 `searchString` 参数包含一个字符串，电影查询则会被修改为根据搜索字符串的值进行筛选：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

上面的 `s => s.Title.Contains()` 代码是 [Lambda 表达式](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。 Lambda 在基于方法的 [LINQ](https://docs.microsoft.com/dotnet/standard/using-linq) 查询中用作标准查询运算符方法的参数，如 [Where](https://docs.microsoft.com//dotnet/api/system.linq.enumerable.where) 方法或 `Contains`（上述的代码中所使用的）。 在对 LINQ 查询进行定义或通过调用方法（如 `Where`、`Contains` 或 `OrderBy`）进行修改后，此查询不会被执行。 相反，会延迟执行查询。  这意味着表达式的计算会延迟，直到真正循环访问其实现的值或者调用 `ToListAsync` 方法为止。 有关延迟执行查询的详细信息，请参阅[Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution)（查询执行）。

注意：[Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法在数据库上运行，而不是在上面显示的 c# 代码中运行。 查询是否区分大小写取决于数据库和排序规则。 在 SQL Server 上，[Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 映射到 [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql)，这是不区分大小写的。 在 SQLlite 中，由于使用了默认排序规则，因此需要区分大小写。

导航到 `/Movies/Index`。 将查询字符串（如 `?searchString=Ghost`）追加到 URL。 筛选的电影将显示出来。

![索引视图](../../tutorials/first-mvc-app/search/_static/ghost.png)

如果将 `Index` 方法的签名更改为具有名称为 `id` 的参数，则 `id` 参数将匹配 Startup.cs 中设置的默认路由的可选 `{id}` 占位符。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
