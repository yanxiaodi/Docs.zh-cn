# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>将视图添加到 ASP.NET Core MVC 应用

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本部分中，将修改 `HelloWorldController` 类，进而使用 Razor 视图模板文件来顺利封装为客户端生成 HTML 响应的过程。

使用 Razor 创建视图模板文件。 基于 Razor 的模板具有“.cshtml”文件扩展名。 它们提供了一种巧妙的方法来使用 C# 创建 HTML 输出。

当前，`Index` 方法返回带有在控制器类中硬编码的消息的字符串。 在 `HelloWorldController` 类中，将 `Index` 方法替换为以下代码：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

上述代码返回 `View` 对象。 它使用视图模板对浏览器生成 HTML 响应。 类似上述 `Index` 方法的控制器方法（也称为操作方法）通常返回 [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult)（或派生自 `ActionResult` 的类），而非类似字符串的类型。
