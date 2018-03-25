上面突出显示的代码显示了要添加到[依赖关系注入](xref:fundamentals/dependency-injection)容器的电影数据库上下文（在 Startup.cs 文件中）。 `services.AddDbContext<MvcMovieContext>(options =>` 指定要使用的数据库和连接字符串。 `=>` 是 [lambda 运算符](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator)。

打开 Controllers/MoviesController.cs 文件并检查构造函数：

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

构造函数使用[依赖关系注入](xref:fundamentals/dependency-injection)将数据库上下文 (`MvcMovieContext `) 注入到控制器中。 数据库上下文将在控制器中的每个 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法中使用。

<a name="strongly-typed-models-keyword-label"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>强类型模型和 @model 关键词

在本教程之前的内容中，已经介绍了控制器如何使用 `ViewData` 字典将数据或对象传递给视图。 `ViewData` 字典是一个动态对象，提供了将信息传递给视图的方便的后期绑定方法。

MVC 还提供将强类型模型对象传递给视图的功能。 凭借此强类型方法可更好地对代码进行编译时检查。 基架机制在创建方法和视图时，通过 `MoviesController` 类和视图使用了此方法（即传递强类型模型）。

检查 Controllers/MoviesController.cs 文件中生成的 `Details` 方法：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

`id` 参数通常作为路由数据传递。 例如 `http://localhost:5000/movies/details/1` 的设置如下：

* 控制器被设置为 `movies` 控制器（第一个 URL 段）。
* 操作被设置为 `details`（第二个 URL 段）。
* ID 被设置为 1（最后一个 URL 段）。

还可以使用查询字符串传入 `id`，如下所示：

`http://localhost:1234/movies/details?id=1`

在未提供 ID 值的情况下，`id` 参数可定义为[可以为 null 的类型](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`)。

[Lambda 表达式](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)会被传入 `SingleOrDefaultAsync` 以选择与路由数据或查询字符串值相匹配的电影实体。

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

如果找到了电影，`Movie` 模型的实例则会被传递到 `Details` 视图：

```csharp
return View(movie);
   ```

检查 Views/Movies/Details.cshtml 文件的内容：

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

通过将 `@model` 语句包括在视图文件的顶端，可以指定视图期望的对象类型。 创建电影控制器时，Visual Studio 会自动在 Details.cshtml 文件的顶端包括以下 `@model` 语句：

```HTML
@model MvcMovie.Models.Movie
   ```

此 `@model` 指令使你能够使用强类型的 `Model` 对象访问控制器传递给视图的电影。 例如，在 Details.cshtml 视图中，代码通过强类型的 `Model` 对象将每个电影字段传递给 `DisplayNameFor` 和 `DisplayFor`HTML 帮助程序。 `Create` 和 `Edit` 方法以及视图也传递一个 `Movie` 模型对象。

检查电影控制器中的 Index.cshtml 视图和 `Index` 方法。 请注意代码在调用 `View` 方法时是如何创建 `List` 对象的。 代码将此 `Movies` 列表从 `Index` 操作方法传递给视图：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

创建电影控制器时，基架会自动在 Index.cshtml 文件的顶端包含以下 `@model` 语句：

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` 指令使你能够使用强类型的 `Model` 对象访问控制器传递给视图的电影列表。 例如，在 Index.cshtml 视图中，代码使用 `foreach` 语句通过强类型 `Model` 对象对电影进行循环遍历：

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

因为 `Model` 对象为强类型（作为 `IEnumerable<Movie>` 对象），因此循环中的每个项都被类型化为 `Movie`。 除其他优点之外，这意味着可对代码进行编译时检查：
