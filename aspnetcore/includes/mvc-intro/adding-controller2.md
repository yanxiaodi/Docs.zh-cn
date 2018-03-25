将“Controllers/HelloWorldController.cs”的内容替换为以下内容：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

控制器中的每个 `public` 方法均可作为 HTTP 终结点调用。 上述示例中，两种方法均返回一个字符串。  请注意每个方法前面的注释。

HTTP 终结点是 Web 应用程序中可定向的 URL（例如 `http://localhost:1234/HelloWorld`），其中结合了所用的协议 `HTTP`、TCP 端口等 Web 服务器的网络位置 `localhost:1234`，以及目标 URI `HelloWorld`。

第一条注释指出这是一个 [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) 方法，它通过向基 URL 追加“/HelloWorld/”进行调用。 第二条注释指定一个 [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) 方法，它通过向 URL 追加“/HelloWorld/Welcome/”进行调用。 本教程稍后将使用基架引擎生成 `HTTP POST` 方法。

在非调试模式下运行应用，并将“HelloWorld”追加到地址栏中的路径。 `Index` 方法返回一个字符串。

![显示“这是我的默认操作”应用程序响应的浏览器窗口](../../tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC 根据入站 URL 调用控制器类（及其中的操作方法）。 MVC 所用的默认 [URL 路由逻辑](../../mvc/controllers/routing.md)使用如下格式来确定调用的代码：

`/[Controller]/[ActionName]/[Parameters]`

在 Startup.cs 文件的 `Configure` 方法中设置路由的格式。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

如果运行应用且不提供任何 URL 段，它将默认为上面突出显示的模板行中指定的“Home”控制器和“Index”方法。

第一个 URL 段决定要运行的控制器类。 因此 `localhost:xxxx/HelloWorld` 映射到 `HelloWorldController` 类。 该 URL 段的第二部分决定类上的操作方法。 因此 `localhost:xxxx/HelloWorld/Index` 将触发 `HelloWorldController` 类的 `Index` 运行。 请注意，只需浏览到 `localhost:xxxx/HelloWorld`，而 `Index` 方法默认调用。 原因是 `Index` 是默认方法，如果未显式指定方法名称，则将在控制器上调用它。 URL 段的第三部分 (`id`) 针对的是路由数据。 稍后可在本教程中看到路由数据。

浏览到 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome` 方法运行并返回字符串“这是 Welcome 操作方法...”。 对于此 URL，采用 `HelloWorld` 控制器和 `Welcome` 操作方法。 目前尚未使用 URL 的 `[Parameters]` 部分。

![显示“这是 Welcome 操作方法”应用程序响应的浏览器窗口](../../tutorials/first-mvc-app/adding-controller/_static/welcome.png)

修改代码，将一些参数信息从 URL 传递到控制器。 例如 `/HelloWorld/Welcome?name=Rick&numtimes=4`。 更改 `Welcome` 方法以包括以下代码中显示的两个参数： 

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

前面的代码：

* 使用 C# 可选参数功能指示，未为 `numTimes` 参数传递值时该参数默认为 1。
* 使用 `HtmlEncoder.Default.Encode` 防止恶意输入（即 JavaScript）损害应用。 
* 使用[内插字符串](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/keywords/interpolated-strings)。

运行应用并浏览到：

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

（将 xxxx 替换为端口号。）可在 URL 中对 `name` 和 `numtimes` 使用其他值。 MVC [模型绑定](../../mvc/models/model-binding.md)系统可将命名参数从地址栏中的查询字符串自动映射到方法中的参数。 有关详细信息，请参阅[模型绑定](../../mvc/models/model-binding.md)。

![显示“你好 Rick，NumTimes 为 4”应用程序响应的浏览器窗口](../../tutorials/first-mvc-app/adding-controller/_static/rick4.png)

在上图中，未使用 URL 段 (`Parameters`)，且 `name` 和 `numTimes` 参数作为[查询字符串](https://wikipedia.org/wiki/Query_string)进行传递。 上述 URL 中的 `?`（问号）为分隔符，后接查询字符串。 `&` 字符用于分隔查询字符串。

将 `Welcome` 方法替换为以下代码：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

运行应用并输入以下 URL：`http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![显示“你好 Rick，ID 为 3”应用程序响应的浏览器窗口](../../tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

此时，第三个 URL 段与路由参数 `id` 相匹配。 `Welcome` 方法包含 `MapRoute` 方法中匹配 URL 模板的参数 `id`。 后面的 `?`（`id?` 中）表示 `id` 参数可选。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

上述示例中，控制器始终执行 MVC 的“VC”部分，即视图和控制器工作。 控制器将直接返回 HTML。 通常不希望控制器直接返回 HTML，因为编码和维护非常繁琐。 通常，需使用单独的 Razor 视图模板文件来帮助生成 HTML 响应。 可在下一教程中执行该操作。
