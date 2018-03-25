使用以下内容替换 Razor 视图文件 Views/HelloWorld/Index.cshtml 的内容：

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

导航到 `http://localhost:xxxx/HelloWorld`。 `HelloWorldController` 中的 `Index` 方法作用不大；它运行 `return View();` 语句，指定此方法应使用视图模板文件来呈现对浏览器的响应。 因为没有显式指定视图模板文件的名称，所以 MVC 默认使用 /Views/HelloWorld 文件夹中的 Index.cshtml 视图文件。 下面图片显示了视图中硬编码的 字符串“Hello from our View Template!”

![浏览器窗口](../../tutorials/first-mvc-app/adding-view/_static/hell_template.png)

如果浏览器窗口较小（例如在移动设备上），则可能需要在右上角切换（点击）[Bootstrap 导航按钮](http://getbootstrap.com/components/#navbar)以查看“首页”、“关于”和“联系人”链接。

![突出显示 Bootstrap 导航按钮的浏览器窗口](../../tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>更改视图和布局页面

点击菜单链接（“MvcMovie”、“首页”、“关于”）。 每页显示相同的菜单布局。 菜单布局是在 Views/Shared/_Layout.cshtml 文件中实现的。 打开 Views/Shared/_Layout.cshtml 文件。

[布局](xref:mvc/views/layout)模板使你能够在一个位置指定网站的 HTML 容器布局，然后将它应用到网站中的多个页面。 查找 `@RenderBody()` 行。 `RenderBody` 是显示创建的所有特定于视图的页面的占位符，已包装在布局页面中。 例如，如果选择“关于”链接，Views/Home/About.cshtml 视图将在 `RenderBody` 方法中呈现。

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>更改布局文件中的标题和菜单链接

在标题元素中，将 `MvcMovie` 更改为 `Movie App`。 将布局模板中的定位文本从 `MvcMovie` 更改为 `Mvc Movie`，并将控制器从 `Home` 更改为 `Movies`，如下所示：

注意：ASP.NET Core 2.0 版本略有不同。 它不包含 `@inject ApplicationInsights` 和 `@Html.Raw(JavaScriptSnippet.FullScript)`。

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

>[!WARNING]
> 我们尚未实现 `Movies` 控制器，所以如果单击此链接，将收到 404（未找到）错误。

保存更改并点击“关于”链接。 请注意浏览器选项卡上的标题现在显示的是**“关于 - 电影应用”**，而不是**“关于 - Mvc 电影”**: 

![关于标签](../../tutorials/first-mvc-app/adding-view/_static/about2.png)

点击“联系人”链接，请注意，标题和定位文本还会显示“电影应用”。 我们能够在布局模板中进行一次更改，让网站上的所有页面都反映新的链接文本和新标题。

检查 Views/_ViewStart.cshtml 文件：


```HTML
@{
    Layout = "_Layout";
}
```

Views/_ViewStart.cshtml 文件将 Views/Shared/_Layout.cshtml 文件引入到每个视图中。 可以使用 `Layout` 属性设置不同的布局视图，或将它设置为 `null`，这样将不会使用任何布局文件。

更改 `Index` 视图的标题。

打开 Views/HelloWorld/Index.cshtml。 有两处需要更改的地方：

   * 浏览器标题中显示的文字。
   * 辅助标题（`<h2>` 元素）。

稍稍对它们进行一些更改，这样可以看出哪一段代码更改了应用的哪部分。


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

上述代码中的 `ViewData["Title"] = "Movie List";` 将 `ViewData` 字典的 `Title` 属性设置为“Movie List”。 `Title` 属性在布局页面中的 `<title>` HTML 元素中使用：


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

保存更改并导航到 `http://localhost:xxxx/HelloWorld`。 请注意，浏览器标题、主标题和辅助标题已更改。 （如果没有在浏览器中看到更改，则可能正在查看缓存的内容。 在浏览器中按 Ctrl + F5 强制加载来自服务器的响应。）浏览器标题是使用我们在 Index.cshtml 视图模板中设置的 `ViewData["Title"]` 以及在布局文件中添加的额外“ - Movie App”创建的。

还要注意，Index.cshtml 视图模板中的内容是如何与 Views/Shared/_Layout.cshtml 视图模板合并的，并且注意单个 HTML 响应被发送到了浏览器。 凭借布局模板可以很容易地对应用程序中所有页面进行更改。 若要了解更多信息，请参阅[布局](../../mvc/views/layout.md)。

![电影列表视图](../../tutorials/first-mvc-app/adding-view/_static/hell3.png)

但我们这一点点“数据”（在此示例中为“Hello from our View Template!” 消息）是硬编码的。 MVC 应用程序有一个“V”（视图），而你已有一个“C”（控制器），但还没有“M”（模型）。

## <a name="passing-data-from-the-controller-to-the-view"></a>将数据从控制器传递给视图

控制器操作会被调用以响应传入的 URL 请求。 控制器类是编写处理传入浏览器请求的代码的地方。 控制器从数据源检索数据，并决定将哪些类型的响应发送回浏览器。 可以从控制器使用视图模板来生成并格式化对浏览器的 HTML 响应。

控制器负责提供所需的数据，使视图模板能够呈现响应。 最佳做法：视图模板不应该直接执行业务逻辑或与数据库进行交互。 相反，视图模板应仅使用由控制器提供给它的数据。 保持此“关注点分离”有助于保持代码干净、可测试性和可维护性。

目前，`HelloWorldController` 类中的 `Welcome` 方法采用 `name` 和 `ID` 参数，然后将值直接输出到浏览器。 应将控制器更改为使用视图模板，而不是使控制器将此响应呈现为字符串。 视图模板会生成动态响应，这意味着必须将适当的数据位从控制器传递给视图以生成响应。 为此，可以让控制器将视图模板所需的动态数据（参数）放置在视图模板稍后可以访问的 `ViewData` 字典中。

返回到 HelloWorldController.cs 文件，并更改 `Welcome` 方法以将 `Message` 和 `NumTimes` 值添加到 `ViewData` 字典。 `ViewData` 字典是一个动态对象，这意味着你可以将任何所需的内容放在其中；只有将内容放在其中后 `ViewData` 对象才具有定义的属性。 [MVC 模型绑定系统](xref:mvc/models/model-binding)自动将命名参数（`name` 和 `numTimes`）从地址栏中的查询字符串映射到方法中的参数。 完整的 HelloWorldController.cs 文件如下所示：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` 字典对象包含将传递给视图的数据。 

创建一个名为 Views/HelloWorld/Welcome.cshtml 的 Welcome 视图模板。

在 Welcome.cshtml 视图模板中创建一个循环，显示“Hello” `NumTimes`。 使用以下内容替换 Views/HelloWorld/Welcome.cshtml 的内容：

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

保存更改并浏览到以下 URL：

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

数据取自 URL，并传递给使用 [MVC 模型绑定器](xref:mvc/models/model-binding)的控制器。 控制器将数据打包到 `ViewData` 字典中，并将该对象传递给视图。 然后，视图将数据作为 HTML 呈现给浏览器。

![“关于”视图，显示了 Welcome 标签以及四个“Hello Rick”短语](../../tutorials/first-mvc-app/adding-view/_static/rick2.png)

在上面的示例中，我们使用 `ViewData` 字典将数据从控制器传递给视图。 稍后在本教程中，我们将使用视图模型将数据从控制器传递给视图。 传递数据的视图模型方法通常比 `ViewData` 字典方法更为优先。 有关详细信息，请参阅 [ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc)（MVC 中 ViewModel、ViewData、ViewBag、TempData 和 Session 之间的比较）。

当然，这是模型的一种“M”类型，而不是数据库类。 让我们用学到的内容来创建一个电影数据库。