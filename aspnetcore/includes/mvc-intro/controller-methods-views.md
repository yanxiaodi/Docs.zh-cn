
我们将在下一教程中介绍 [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6)。 [Display](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) 特性指定要显示的字段名称的内容（本例中应为“Release Date”，而不是“ReleaseDate”）。 [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) 属性指定数据的类型（日期），使字段中存储的时间信息不会显示。

浏览到 `Movies` 控制器，并将鼠标指针悬停在“编辑”链接上以查看目标 URL。

![鼠标悬停在“编辑”链接上的浏览器窗口，显示了 http://localhost:1234/Movies/Edit/5 的链接 URL](../../tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

“编辑”、“详细信息”和“删除”链接是在 Views/Movies/Index.cshtml 文件中由 Core MVC 定位标记帮助程序生成的。

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

[标记帮助程序](xref:mvc/views/tag-helpers/intro)使服务器端代码可以在 Razor 文件中参与创建和呈现 HTML 元素。 在上面的代码中，`AnchorTagHelper` 从控制器操作方法和路由 ID 动态生成 HTML `href` 特性值。在最喜欢的浏览器中使用“查看源”，或使用开发人员工具来检查生成的标记。 生成的 HTML 的一部分如下所示：

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

重新调用在 Startup.cs 文件中设置的[路由](xref:mvc/controllers/routing)的格式：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

ASP.NET Core 将 `http://localhost:1234/Movies/Edit/4` 转换为对 `Movies` 控制器的 `Edit` 操作方法的请求，参数 `Id` 为 4。 （控制器方法也称为操作方法。）

[标记帮助程序](xref:mvc/views/tag-helpers/intro)是 ASP.NET Core 中最受欢迎的新功能之一。 有关详细信息，请参阅[其他资源](#additional-resources)。

打开 `Movies` 控制器并检查两个 `Edit` 操作方法。 以下代码显示了 `HTTP GET Edit` 方法，此方法将提取电影并填充由 Edit.cshtml Razor 文件生成的编辑表单。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

以下代码显示 `HTTP POST Edit` 方法，它会处理已发布的电影值：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

`[Bind]` 特性是防止[过度发布](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost)的一种方法。 只应在 `[Bind]` 特性中包含想要更改的属性。 有关详细信息，请参阅 [Protect your controller from over-posting](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)（防止控制器过度发布）。 [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) 提供了一种替代方法以防止过度发布。

请注意第二个 `Edit` 操作方法的前面是 `[HttpPost]` 特性。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

`HttpPost` 特性指定只能为 `POST` 请求调用此 `Edit` 方法。 可将 `[HttpGet]` 属性应用于第一个编辑方法，但不是必需，因为 `[HttpGet]` 是默认设置。

`ValidateAntiForgeryToken` 特性用于[防止请求伪造](xref:security/anti-request-forgery)，并与编辑视图文件 (Views/Movies/Edit.cshtml) 中生成的防伪标记相配对。 编辑视图文件使用[表单标记帮助程序](xref:mvc/views/working-with-forms)生成防伪标记。

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

[表单标记帮助程序](xref:mvc/views/working-with-forms)会生成隐藏的防伪标记，此标记必须与电影控制器的 `Edit` 方法中 `[ValidateAntiForgeryToken]` 生成的防伪标记相匹配。 有关详细信息，请参阅[反请求伪造](xref:security/anti-request-forgery)。

`HttpGet Edit` 方法采用电影 `ID` 参数，使用Entity Framework `SingleOrDefaultAsync` 方法查找电影，并将所选电影返回到“编辑”视图。 如果无法找到电影，则返回 `NotFound` (HTTP 404)。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

当基架系统创建“编辑”视图时，它会检查 `Movie` 类并创建代码为类的每个属性呈现 `<label>` 和 `<input>` 元素。 以下示例显示由 Visual Studio 基架系统生成的“编辑”视图：

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/EditCopy.cshtml?highlight=1)]

请注意视图模板在文件顶端有一个 `@model MvcMovie.Models.Movie` 语句。 `@model MvcMovie.Models.Movie` 指定视图期望的视图模板的模型为 `Movie` 类型。

基架的代码使用几个标记帮助程序方法来简化 HTML 标记。 [标签标记帮助程序](xref:mvc/views/working-with-forms)显示字段的名称（“Title”、“ReleaseDate”、“Genre”或“Price”）。 [输入标记帮助程序](xref:mvc/views/working-with-forms)呈现 HTML `<input>` 元素。 [验证标记帮助程序](xref:mvc/views/working-with-forms)显示与该属性相关联的任何验证消息。

运行应用程序并导航到 `/Movies` URL。 点击“编辑”链接。 在浏览器中查看页面的源。 为 `<form>` 元素生成的 HTML 如下所示。

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

`<input>` 元素位于 `HTML <form>` 元素中，后者的 `action` 特性设置为发布到 `/Movies/Edit/id` URL。 当单击 `Save` 按钮时，表单数据将发布到服务器。 关闭 `</form>` 元素之前的最后一行显示[表单标记帮助程序](xref:mvc/views/working-with-forms)生成的隐藏的 [XSRF](xref:security/anti-request-forgery) 标记。

## <a name="processing-the-post-request"></a>处理 POST 请求

以下列表显示了 `Edit` 操作方法的 `[HttpPost]` 版本。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

`[ValidateAntiForgeryToken]` 特性验证[表单标记帮助程序](xref:mvc/views/working-with-forms)中的防伪标记生成器生成的隐藏的 [XSRF](xref:security/anti-request-forgery) 标记

[模型绑定](xref:mvc/models/model-binding)系统采用发布的表单值，并创建一个作为 `movie` 参数传递的 `Movie` 对象。 `ModelState.IsValid` 方法验证表单中提交的数据是否可以用于修改（编辑或更新）`Movie` 对象。 如果数据有效，将保存此数据。 通过调用数据库上下文的 `SaveChangesAsync` 方法，将更新（编辑）的电影数据保存到数据库。 保存数据后，代码将用户重定向到 `MoviesController` 类的 `Index` 操作方法，此方法显示电影集合，包括刚才所做的更改。

在表单发布到服务器之前，客户端验证会检查字段上的任何验证规则。 如果有任何验证错误，则将显示错误消息，并且不会发布表单。 如果禁用 JavaScript，则不会进行客户端验证，但服务器将检测无效的发布值，并且表单值将与错误消息一起重新显示。 稍后在本教程中，我们将更详细地研究[模型验证](xref:mvc/models/validation)。 Views/Movies/Edit.cshtml 视图模板中的[验证标记帮助程序](xref:mvc/views/working-with-forms)负责显示相应的错误消息。

![“编辑”视图：不正确的“价格”值 abc 的异常，说明“价格”字段必须是一个数字。 不正确的“发布日期”值 xyz 的异常，请输入有效的日期。](../../tutorials/first-mvc-app/controller-methods-views/_static/val.png)

电影控制器中的所有 `HttpGet` 方法都遵循类似的模式。 它们获取电影对象（对于 `Index`获取的是对象列表）并将对象（模型）传递给视图。 `Create` 方法将空的电影对象传递给 `Create` 视图。 在方法的 `[HttpPost]` 重载中，创建、编辑、删除或以其他方式修改数据的所有方法都执行此操作。 以 `HTTP GET` 方式修改数据是一种安全隐患。 以 `HTTP GET` 方法修改数据也违反了 HTTP 最佳做法和架构 [REST](http://rest.elkstein.org/) 模式，后者指定 GET 请求不应更改应用程序的状态。 换句话说，执行 GET 操作应是没有任何隐患的安全操作，也不会修改持久数据。

## <a name="additional-resources"></a>其他资源

* [全球化和本地化](xref:fundamentals/localization)
* [标记帮助程序简介](xref:mvc/views/tag-helpers/intro)
* [创作标记帮助程序](xref:mvc/views/tag-helpers/authoring)
* [防请求伪造](xref:security/anti-request-forgery)
* 防止控制器[过度发布](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
* [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [表单标记帮助程序](xref:mvc/views/working-with-forms)
* [输入标记帮助程序](xref:mvc/views/working-with-forms)
* [标签标记帮助程序](xref:mvc/views/working-with-forms)
* [选择标记帮助程序](xref:mvc/views/working-with-forms)
* [验证标记帮助程序](xref:mvc/views/working-with-forms)
