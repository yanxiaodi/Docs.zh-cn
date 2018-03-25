<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

之前的 `Index` 方法：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

更新后带 `id` 参数的 `Index` 方法：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

现可将搜索标题作为路由数据（ URL 段）而非查询字符串值进行传递。

![索引视图，显示添加到 URL 的“ghost”一词以及返回的两部电影（Ghostbusters 和 Ghostbusters 2）的电影列表](../../tutorials/first-mvc-app/search/_static/g2.png)

但是，不能指望用户在每次要搜索电影时都修改 URL。 因此需要添加 UI 来帮助他们筛选电影。 若已更改 `Index` 方法的签名，以测试如何传递绑定路由的 `ID` 参数，请改回原样，使其采用名为 `searchString` 的参数：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

打开“Views/Movies/Index.cshtml”文件，并添加以下突出显示的 `<form>` 标记：

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

此 HTML `<form>` 标记使用[表单标记帮助程序](../../mvc/views/working-with-forms.md)，因此提交表单时，筛选器字符串会发布到电影控制器的 `Index` 操作。 保存更改，然后测试筛选器。

![显示标题筛选器文本框中键入了 ghost 一词的索引视图](../../tutorials/first-mvc-app/search/_static/filter.png)

如你所料，不存在 `Index` 方法的 `[HttpPost]` 重载。 无需重载，因为该方法不更改应用的状态，仅筛选数据。

可添加以下 `[HttpPost] Index` 方法。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` 参数用于创建 `Index` 方法的重载。 本教程稍后将对此进行探讨。

如果添加此方法，则操作调用程序将与 `[HttpPost] Index` 方法匹配，且将运行 `[HttpPost] Index` 方法，如下图所示。

![显示“来自 HttpPost 索引: 筛选 ghost”应用程序响应的浏览器窗口](../../tutorials/first-mvc-app/search/_static/fo.png)

但是，即使添加 `Index` 方法的 `[HttpPost]` 版本，其实现方式也受到限制。 假设你想要将特定搜索加入书签，或向朋友发送一个链接，让他们单击链接即可查看筛选出的相同电影列表。 请注意，HTTP POST 请求的 URL 与 GET 请求的 URL 相同 (localhost:xxxxx/Movies/Index)，其中不包含搜索信息。 搜索字符串信息作为[表单域值](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data)发送给服务器。 可使用浏览器开发人员工具或出色的 [Fiddler 工具](http://www.telerik.com/fiddler)对其进行验证。 下图展示了 Chrome 浏览器开发人员工具：

![Microsoft Edge 中开发人员工具的“网络”选项卡，显示了 ghost 的 searchString 值的请求正文](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

在请求正文中，可看到搜索参数和 [XSRF](../../security/anti-request-forgery.md) 标记。 请注意，正如之前教程所述，[表单标记帮助程序](../../mvc/views/working-with-forms.md) 会生成一个 [XSRF](../../security/anti-request-forgery.md) 防伪标记。 不会修改数据，因此无需验证控制器方法中的标记。

搜索参数位于请求正文而非 URL 中，因此无法捕获该搜索信息进行书签设定或与他人共享。 将通过指定请求为 `HTTP GET` 进行修复。
