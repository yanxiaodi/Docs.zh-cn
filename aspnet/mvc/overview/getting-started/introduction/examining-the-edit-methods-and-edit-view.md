---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: "检查编辑方法和编辑视图 |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: d7e1ba503b8aa815cebf431d2f5ffc9436b3575b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>检查编辑方法和编辑视图
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

在此部分中，将检查生成`Edit`操作方法和电影控制器的视图。 但首先将使短的转换，以便更好的外观的发行日期。 打开*Models\Movie.cs*文件并添加突出显示的行，如下所示：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

你还可以进行日期区域性特定如下：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

我们将在下一教程中介绍 [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。 [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) 特性指定要显示的字段名称的内容（本例中应为“Release Date”，而不是“ReleaseDate”）。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性指定的数据类型，在这种情况下它是一个日期，因此不显示在字段中存储的时间信息。 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性用于呈现日期格式不正确的 Chrome 浏览器中的 bug。

运行应用程序，并浏览到`Movies`控制器。 将鼠标指针停留在**编辑**链接以查看链接到的 URL。

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**编辑**链接通过生成`Html.ActionLink`中的方法*Views\Movies\Index.cshtml*视图：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html`对象是使用上的属性公开一个帮助程序[System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx)基类。 `ActionLink`的帮助器方法，可以轻松地动态生成 HTML 的超链接，链接到控制器上的操作方法。 第一个参数`ActionLink`方法是要呈现的链接文本 (例如， `<a>Edit Me</a>`)。 第二个参数是要调用的操作方法的名称 (在这种情况下，`Edit`操作)。 最后一个参数是[匿名对象](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)，用来生成路由数据 （在本例中为 4 的 ID）。

前面的图像中所示的生成的链接是`http://localhost:1234/Movies/Edit/4`。 默认路由 (在中建立*应用\_Start\RouteConfig.cs*) 采用的 URL 模式`{controller}/{action}/{id}`。 因此，ASP.NET 将转换`http://localhost:1234/Movies/Edit/4`放置到请求`Edit`操作方法`Movies`与参数的控制器`ID`等于 4。 检查下面的代码从*应用\_Start\RouteConfig.cs*文件。 [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md)方法用于将 HTTP 请求路由到正确的控制器和操作方法并提供可选的 ID 参数。 [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md)方法也可由[HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx)如`ActionLink`生成给定控制器、 操作方法和任何路由数据的 Url。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

你还可以传递使用查询字符串的操作方法参数。 例如，URL`http://localhost:1234/Movies/Edit?ID=3`还传递了参数`ID`3： 的`Edit`操作方法`Movies`控制器。

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

打开`Movies`控制器。 这两个`Edit`操作方法如下所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

请注意第二个 `Edit` 操作方法的前面是 `HttpPost` 特性。 此属性指定的重载`Edit`可以仅针对的 POST 请求调用方法。 你可以应用`HttpGet`属性设置为第一个编辑方法，但这它们是不必要，因为它是默认值。 (我们在指隐式分配的操作方法`HttpGet`属性为`HttpGet`方法。)[绑定](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)属性是可防止黑客过度发布与你的模型的数据的另一个重要的安全机制。 仅应在你想要更改的绑定属性中包含属性。 你可以阅读有关 overposting 和中的绑定属性我[过多发布安全说明](https://go.microsoft.com/fwlink/?LinkId=317598)。 在本教程中使用简单模型中，我们将绑定模型中的所有数据。 [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx)属性可用于阻止的请求伪造，并使用成对`@Html.AntiForgeryToken()`编辑视图文件中 (*Views\Movies\Edit.cshtml*)，部分如下所示：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` 将生成在都必须匹配隐藏的表单防伪令牌`Edit`方法`Movies`控制器。 你可以阅读更多有关跨站点请求伪造 （也称为 XSRF 或 CSRF） 在我的教程[mvc XSRF/CSRF 预防](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)。

`HttpGet` `Edit`方法采用电影 ID 参数，以查找使用实体框架电影`Find`方法，并将所选的电影返回到编辑视图。 如果找不到一部电影， [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx)返回。 当基架系统创建“编辑”视图时，它会检查 `Movie` 类并创建代码为类的每个属性呈现 `<label>` 和 `<input>` 元素。 下面的示例显示由 visual studio 基架系统生成的编辑视图：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

请注意如何查看模板`@model MvcMovie.Models.Movie`语句文件的顶部 — 这指定视图需要查看模板类型的模型`Movie`。

基架的代码使用了若干个*帮助器方法*来简化的 HTML 标记。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx)帮助器显示的字段的名称 (&quot;标题&quot;， &quot;ReleaseDate&quot;，&quot;流派&quot;，或&quot;价格&quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx)帮助器上呈现 HTML`<input>`元素。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx)帮助器显示与该属性相关联的任何验证消息。

运行应用程序并导航到*/Movies* URL。 点击“编辑”链接。 在浏览器中查看页面的源。 下面显示了窗体元素的 HTML。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>`元素是在 HTML`<form>`元素其`action`特性设置为发布到*/电影/编辑*URL。 窗体数据将发送到服务器时**保存**单击按钮。 第二行显示隐藏[XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)生成令牌`@Html.AntiForgeryToken()`调用。

## <a name="processing-the-post-request"></a>处理 POST 请求

以下列表显示了 `Edit` 操作方法的 `HttpPost` 版本。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx)属性验证[XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)生成令牌`@Html.AntiForgeryToken()`调用在视图中。

[ASP.NET MVC 模型联编程序](https://msdn.microsoft.com/library/dd410405.aspx)采用已发布的窗体值，并创建`Movie`作为传递对象`movie`参数。 `ModelState.IsValid` 方法验证表单中提交的数据是否可以用于修改（编辑或更新）`Movie` 对象。 如果数据有效，影片数据保存到`Movies`集合`db(MovieDBContext`实例)。 新的影片数据保存到数据库上，通过调用`SaveChanges`方法`MovieDBContext`。 保存数据后，代码将用户重定向到 `MoviesController` 类的 `Index` 操作方法，此方法显示电影集合，包括刚才所做的更改。

只要客户端验证确定字段的值均无效，将显示一条错误消息。 如果禁用 JavaScript 时，你无需客户端验证，但服务器将检测已发布的值均无效，和窗体值将重新显示错误消息。 在教程后面部分中，我们将查看更多详细信息中的验证。

`Html.ValidationMessageFor`中的帮助器*Edit.cshtml*视图模板负责显示相应的错误消息。

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

所有`HttpGet`方法遵循类似的模式。 它们获取电影对象 (或列表中的对象，这种情况的`Index`)，并将模型传递到该视图。 `Create`方法将空的电影对象传递给创建视图。 在方法的 `HttpPost` 重载中，创建、编辑、删除或以其他方式修改数据的所有方法都执行此操作。 博客文章中所述修改 HTTP GET 方法中的数据是安全风险， [ASP.NET MVC 提示 #46 – 不要使用删除的链接，因为他们创建的安全漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。 HTTP 最佳实践和体系结构在 GET 方法中修改数据也违反了[REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)模式，它指定 GET 请求不应更改你的应用程序的状态。 换句话说，执行 GET 操作应是没有任何隐患的安全操作，也不会修改持久数据。

如果你使用的英语 （美国） 的计算机，则可以跳过此部分并转到下一步的教程。 你可以下载此教程的 Globalize 版本[此处](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475)。 国际化绝佳由两部分教程，请参阅[Nadeem 的 ASP.NET MVC 5 国际化](http://afana.me/post/aspnet-mvc-internationalization.aspx)。


> [!NOTE]
> 若要为使用逗号的非英语区域设置支持 jQuery 验证 (&quot;，&quot;) 对于小数点，和非美国英语的日期格式中，您必须包含*globalize.js*和您的特定*cultures/globalize.cultures.js*文件 (从[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) 和 JavaScript 使用`Globalize.parseFloat`。 你可以从 NuGet 获取 jQuery 非英语验证。 （请勿安装 Globalize 如果你使用的英语区域设置。）


1. 从**工具**菜单上，单击**NuGetLibrary 程序包管理器**，然后单击**管理解决方案的 NuGet 包**。  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. 在左窗格中，选择 **浏览*。 * * * （参阅下图）。
3. 在输入框中，输入 * Globalize * *。  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) 选择`jQuery.Validation.Globalize`，选择`MvcMovie`单击**安装**。 *Scripts\jquery.globalize\globalize.js*文件将添加到你的项目。 *Scripts\jquery.globalize\cultures\*文件夹将包含多个区域性 JavaScript 文件。 请注意，它可能需要 5 分钟，安装此包。

 下面的代码演示对 Views\Movies\Edit.cshtml 文件进行修改： 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

若要避免重复此代码中的每个编辑视图，你可以将其移到布局文件中。 若要优化脚本下载，请参阅我的教程[捆绑和缩减](../../performance/bundling-and-minification.md)。

有关详细信息请参阅[ASP.NET MVC 3 国际化](http://afana.me/post/aspnet-mvc-internationalization.aspx)和[ASP.NET MVC 3 国际化-第 2 部分 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx)。

作为临时解决问题，如果你不能收到验证在你的区域设置中工作，你可以强制计算机以使用美国英语，也可以在浏览器中禁用 JavaScript。 若要强制计算机以使用美国英语，你可以将全球化元素添加到项目根*web.config*文件。 下面的代码演示与设置为英语 （美国） 区域性的全球化元素。

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> 在下一步的教程中，我们将实现搜索功能。

>[!div class="step-by-step"]
[上一页](accessing-your-models-data-from-a-controller.md)
[下一页](adding-search.md)
