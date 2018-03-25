---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: "在 ASP.NET MVC 应用程序中实现与实体框架的基本 CRUD 功能 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e3dbea51199722bfe50f201c4ddcc90aa081927d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>在 ASP.NET MVC 应用程序中实现与实体框架的基本 CRUD 功能
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在以前的教程，你创建的 MVC 应用程序存储和显示数据使用实体框架和 SQL Server LocalDB。 在本教程中，你将查看和自定义 CRUD （创建、 读取、 更新、 删除） 的 MVC 基架自动为你创建在控制器和视图中的代码。

> [!NOTE]
> 它是常见的做法，以便创建你的控制器和数据访问层之间的抽象层实现存储库模式。 若要使这些教程简单和关注教学如何使用实体框架本身，它们不使用存储库。 有关如何实现存储库的信息，请参阅[ASP.NET 数据访问内容映射](../../../../whitepapers/aspnet-data-access-content-map.md)。


在本教程中，你将创建以下网页：

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>创建详细信息页

学生的基架的代码`Index`页中省略`Enrollments`属性，因为该属性包含集合。 在`Details`将集合的内容显示在 HTML 表的页。

 在*Controllers\StudentController.cs*的操作方法`Details`查看使用[查找](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx)方法来检索单个`Student`实体。 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

密钥的值传递给该方法为`id`参数和来自*将数据路由*中**详细信息**索引网页上的超链接。

### <a name="tip-route-data"></a>提示：**路由数据**

路由数据是在路由表中指定的 URL 段中找到模型联编程序的数据。 例如，默认路由指定`controller`， `action`，和`id`段：

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

在下面的 URL，将默认路由映射`Instructor`作为`controller`，`Index`作为`action`并将它作为 1 `id`; 这些是路由数据值。

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"？ courseID = 2021年"是一个查询字符串值。 模型联编程序还将起作用，如果你通过`id`作为查询字符串值：

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

通过创建 Url `ActionLink` Razor 视图中的语句。 在下面的代码中，`id`参数和默认路由匹配，因此`id`将添加到路由数据。

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

在下面的代码中，`courseID`不匹配默认路由中的参数，因此，它将添加作为查询字符串。

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. 打开*Views\Student\Details.cshtml*。 使用显示每个字段`DisplayFor`帮助器，如下面的示例中所示：

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. 后`EnrollmentDate`字段和立即在关闭前`</dl>`标记中，添加突出显示的代码，以显示列表的注册，如下面的示例中所示：

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    如果后粘贴代码，代码缩进有误，，按 CTRL-K-D 更正它。

    此代码循环访问中的实体`Enrollments`导航属性。 每个`Enrollment`实体属性中，它显示课程标题和评分。 正在从检索课程标题`Course`中存储的实体`Course`导航属性`Enrollments`实体。 所有这些数据是从数据库中检索自动需要时。 （在换而言之，你正在使用延迟加载此处。 未指定*预先加载*为`Courses`导航属性，因此注册未检索到在同一查询中获取学生。 相反，第一次尝试访问`Enrollments`导航属性，一个新的查询将发送到数据库以检索数据。 你可以阅读更多有关延迟加载和中的预先加载[读取相关数据](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)更高版本中这一系列教程。)
3. 通过选择运行页面**学生**选项卡上，单击**详细信息**Alexander Carson 的链接。 （如果 Details.cshtml 文件处于打开状态时按 CTRL + F5，则将会返回 HTTP 400 错误因为 Visual Studio 尝试运行的详细信息页，但它不从一个链接，以指定要显示学生已达到。 In that case，只需从 URL 中删除"学生/详细信息"和重试，或关闭浏览器、 右键单击项目，然后单击**视图**，然后单击**用浏览器查看**。)

    为所选学生查看课程和年级的列表：

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>更新创建页

1. 在*Controllers\StudentController.cs*，替换`HttpPost``Create`操作方法替换为以下代码以添加`try-catch`阻止和删除`ID`从[绑定属性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)为基架的方法：

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    此代码将添加`Student`实体创建的 ASP.NET MVC 模型联编程序到`Students`实体设置，然后将所做的更改保存到数据库。 (*模型联编程序*指使它可以更轻松地处理提交的窗体的数据; 模型联编程序将发布窗体到 CLR 类型值，并将其传递给参数中的操作方法的 ASP.NET MVC 功能。 模型联编程序的实例化 in this case，`Student`为你使用属性的实体值从`Form`集合。)

    你删除`ID`从绑定特性，因为`ID`是 SQL Server 将插入行时自动设置的主键值。 来自用户的输入不会设置`ID`值。

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>安全警告-`ValidateAntiForgeryToken`属性有助于防止[跨站点请求伪造](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻击。 它需要相应`Html.AntiForgeryToken()`语句在视图中，您可以看到更高版本。

    `Bind`属性是一种方法，以防止*过度发布*中创建的方案。 例如，假设`Student`实体包含`Secret`你不希望此网页可设置的属性。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    即使你没有`Secret`在网页上，黑客字段无法使用一种工具如[fiddler](http://fiddler2.com/home)，或编写一些 JavaScript，发布`Secret`形成的值。 而无需[绑定](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)限制模型联编程序使用在创建时的字段的特性`Student`实例*，*模型联编程序会选取，`Secret`形成的值以及使用它创建`Student`实体实例。 然后任何值为指定黑客`Secret`窗体字段将在你的数据库中更新。 下图显示 fiddler 工具添加`Secret`（具有值"OverPost"） 到已发布的窗体值的字段。

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    随后将可"OverPost"成功添加到的值`Secret`属性插入行，尽管你永远不会预期网页能够设置该属性。

    它是使用的安全最佳实践`Include`参数`Bind`属性设为*白名单*字段。 还有可能要使用`Exclude`参数*黑名单*你想要排除的字段。 原因`Include`更安全是，当将新属性添加到实体，新的字段不会自动受`Exclude`列表。

    你可以阻止在编辑方案中的 overposting 是通过先从数据库读取实体，然后再调用`TryUpdateModel`，并传递显式允许的属性列表中。 这是这些教程中使用的方法。

    防止首选许多开发人员的 overposting 一种备用方法是使用视图模型，而不是实体类与模型绑定。 包括你想要在视图模型中更新的属性。 完成 MVC 模型联编程序后，将视图模型属性复制到实体实例，根据需要使用一种工具如[AutoMapper](http://automapper.org/)。 使用数据库。要将其状态设置为未更改，并将 Property("PropertyName") 的实体实例的项。为 true 的每个实体属性的视图模型中包含的 IsModified。 此方法适用于同时编辑和创建方案。

    除`Bind`属性，`try-catch`块是唯一的基架的代码所做的更改。 如果异常派生自[DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx)是捕捉到正在保存所做的更改时，将显示一般错误消息。 [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx)以便建议用户以重试，由外部的应用程序，而不是编程错误，有时会导致异常。 尽管在此示例中未实现，但生产质量应用程序会将异常记录。 有关详细信息，请参阅**要深入探索日志**主题中[监视和遥测 （构建真实世界云应用程序与 Azure）](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)。

    中的代码*Views\Student\Create.cshtml*类似于你在中看到*Details.cshtml*，只不过`EditorFor`和`ValidationMessageFor`帮助器用于每个字段而不是`DisplayFor`. 下面是相关的代码：

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml*还包括`@Html.AntiForgeryToken()`，其工作方式与`ValidateAntiForgeryToken`属性中的控制器，以帮助防止[跨站点请求伪造](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻击。

    在需要进行任何更改*Create.cshtml*。
2. 通过选择运行页面**学生**选项卡上，单击**新建**。
3. 输入名称和一个无效的日期，然后单击**创建**可查看错误消息。

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    这是默认情况下; 获取的服务器端验证在更高版本的教程中，你将看到如何添加还将生成代码以便进行客户端验证的属性。 以下突出显示的代码演示模型验证检查在**创建**方法。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. 将日期更改为有效的值并单击**创建**若要查看显示在新学生**索引**页。

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>更新编辑 HttpPost 方法

在*Controllers\StudentController.cs*、 `HttpGet` `Edit`方法 (而不是`HttpPost`属性) 使用`Find`方法来检索所选`Student`作为你看到的实体，在`Details`方法。 你不需要更改此方法。

但是，替换`HttpPost``Edit`操作方法替换为以下代码：

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

这些更改实现安全的最佳做法，以防止[过多发布](#overpost)，生成基架`Bind`属性，并添加到的实体集使用已修改标志模型联编程序所创建的实体。 因为不再建议代码`Bind`属性将清除任何预先存在的数据中未列出的字段中`Include`参数。 将来，MVC 控制器 scaffolder 将进行更新，以便它不会生成`Bind`编辑方法的属性。

新的代码读取的现有实体和调用[TryUpdateModel](https://msdn.microsoft.com/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx)更新中的已发布的窗体数据中的用户输入的字段。 实体框架自动更改跟踪设置[已修改](https://msdn.microsoft.com/library/system.data.entitystate.aspx)实体上的标志。 当[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)调用方法时，`Modified`标志会导致实体框架创建 SQL 语句，以更新数据库行。 [并发冲突](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)将被忽略，并更新数据库行中的所有列，包括那些用户未更改。 (后面的教程演示如何处理并发冲突，如果你只想要更新数据库中的各个字段，你可以设置为未更改的实体，设置单个字段以修改时间。)

作为最佳做法是以防止 overposting，你想要通过编辑页可更新的字段被列入白名单中的`TryUpdateModel`参数。 当前没有额外字段，你要保护的但列出你想要绑定的模型联编程序的字段可确保，如果在将来将字段添加到数据模型，它们是否自动受到保护之前你显式将其添加此处。

由于这些更改，HttpPost 编辑方法的方法签名等同于 HttpGet 编辑方法;因此，你已重命名方法 EditPost。

> [!TIP] 
> 
> **实体状态的附加和 SaveChanges 方法**
> 
> 是否在内存中的实体是在数据库中，其对应行与同步，此信息确定在调用时，会发生什么情况，将跟踪的数据库上下文`SaveChanges`方法。 例如，当传递到新实体[添加](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx)方法，该实体的状态设置为方法`Added`。 然后调用[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)方法，数据库上下文发出 SQL`INSERT`命令。
> 
> 实体可能之一[以下状态](https://msdn.microsoft.com/library/system.data.entitystate.aspx):
> 
> - `Added`。 实体在数据库中尚不存在。 `SaveChanges`方法必须发出`INSERT`语句。
> - `Unchanged`。 无需使用通过此实体完成`SaveChanges`方法。 当从数据库读取实体时，该实体开始时具有此状态。
> - `Modified`。 某些或所有实体的属性值已都更改。 `SaveChanges`方法必须发出`UPDATE`语句。
> - `Deleted`。 实体已标记为删除。 `SaveChanges`方法必须发出`DELETE`语句。
> - `Detached`。 实体不跟踪的数据库上下文。
> 
> 在桌面应用中，通常会自动设置状态更改。 在桌面类型的应用程序，你可以阅读实体并对它的一些属性值进行更改。 这将导致其实体状态自动更改为`Modified`。 然后调用`SaveChanges`，实体框架生成 SQL`UPDATE`更新仅你更改的实际属性的语句。
> 
> Web 应用断开连接的特性不允许对此连续序列。 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)读取实体已释放之后呈现一个页面。 当`HttpPost``Edit`操作方法调用、 新的请求进行并且您具有的新实例[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)，因此你必须手动将实体状态设置为`Modified.`然后当调用`SaveChanges`，实体框架更新数据库行中，所有的列，因为上下文具有无法知道您更改了哪些属性。
> 
> 如果你想 SQL`Update`语句才能更新字段的用户实际更改，以便它们可用时，可以以某种方式 （如隐藏字段） 保存的原始值`HttpPost``Edit`调用方法。 然后你可以创建`Student`实体使用原始值，调用`Attach`实体，该原始版本的方法的新值更新实体的值，然后调用`SaveChanges.`详细信息，请参阅[实体状态和 SaveChanges](https://msdn.microsoft.com/data/jj592676)和[本地数据](https://msdn.microsoft.com/data/jj592872)MSDN 数据开发人员中心中。


中的 HTML 和 Razor 代码*Views\Student\Edit.cshtml*类似于你在中看到*Create.cshtml*，并需进行任何更改。

通过选择运行页面**学生**选项卡，然后单击**编辑**超链接。

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

更改某些数据，再单击**保存**。 你看到索引页面中的更改的数据。

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>更新删除页

在*Controllers\StudentController.cs*的模板代码`HttpGet``Delete`方法使用`Find`方法来检索所选`Student`实体，当你在中看到`Details`和`Edit`方法。 但是，若要实现自定义错误消息时对调用`SaveChanges`失败，需要将某些功能添加到此方法，其相应的视图。

当你看到的针对更新，并创建操作，删除操作需要两个操作方法。 调用以响应 GET 请求的方法显示为用户提供了机会批准或取消删除操作的视图。 如果用户批准它，则创建 POST 请求。 在这种情况， `HttpPost` `Delete`调用方法，该方法然后实际执行删除操作。

你将添加`try-catch`阻止`HttpPost``Delete`方法以处理更新数据库时可能出现的任何错误。 如果发生错误， `HttpPost` `Delete`方法调用`HttpGet``Delete`方法，将其传递参数，该值指示发生错误。 `HttpGet Delete`方法然后重新显示确认页以及错误消息，向用户提供机会取消或重试。

1. 替换`HttpGet``Delete`替换为以下代码，它管理错误报告的操作方法： 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    此代码接受[可选参数](https://msdn.microsoft.com/library/dd264739.aspx)，该值指示是否在出现故障，若要保存更改后调用该方法。 此参数是`false`时`HttpGet``Delete`不上一次失败的情况下调用方法。 当调用`HttpPost``Delete`方法中对数据库更新错误响应，该参数是`true`和一条错误消息传递给视图。
- 替换`HttpPost``Delete`操作方法 (名为`DeleteConfirmed`) 替换为以下代码，该执行实际的删除操作并捕获任何数据库更新错误。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    此代码检索所选的实体，然后调用[删除](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx)方法将实体的状态设置为`Deleted`。 当`SaveChanges`调用时，SQL`DELETE`生成命令。 您还已更改的操作方法名称`DeleteConfirmed`到`Delete`。 名为的基架的代码`HttpPost``Delete`方法`DeleteConfirmed`以便`HttpPost`方法一个唯一的签名。 （CLR 需要具有不同的方法参数的重载的方法。）签名是唯一的现在可以坚持使用 MVC 约定，使用相同的名称用于`HttpPost`和`HttpGet`删除方法。

    如果提高大容量应用程序中的性能是优先级，则无法避免不必要的 SQL 查询，以检索行，通过将调用的代码行`Find`和`Remove`方法替换为以下代码：

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    此代码实例化`Student`实体使用仅的主键值然后将实体状态设置为`Deleted`。 这就是所有实体框架需要以便删除该实体。

    如前所述， `HttpGet` `Delete`方法不会删除数据。 执行 delete 操作以响应 GET 请求 （或对于此问题，执行任何编辑操作，创建操作或更改数据的任何其他操作） 会产生安全风险。 有关详细信息，请参阅[ASP.NET MVC 提示 #46-不要使用删除的链接，因为他们创建的安全漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)Stephen Walther 博客上。
- 在*Views\Student\Delete.cshtml*，添加一条错误消息之间`h2`标题和`h3`标题下，如下面的示例中所示：

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

    通过选择运行页面**学生**选项卡上，单击**删除**超链接：

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
- 单击**删除**。 索引页面显示没有已删除的学生。 (你将看到举例说明的错误处理代码中的操作中[并发教程](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)。)

## <a name="closing-database-connections"></a>关闭数据库连接

若要关闭的数据库连接并释放它们尽快持有的资源，请释放上下文实例完成后使用它。 就是为什么基架的代码提供[释放](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx)方法末尾`StudentController`类*StudentController.cs*，下面的示例中所示：

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

基`Controller`类已经实现`IDisposable`接口，所以此代码只需将添加到替代`Dispose(bool)`方法以显式释放上下文实例。

<a id="transactions"></a>
## <a name="handling-transactions"></a>处理事务

默认情况下实体框架隐式实现事务。 在方案中，对多个行或表进行更改，然后调用`SaveChanges`，实体框架自动可确保所有所做的更改成功或所有失败。 如果先完成一些更改，然后，将发生错误，则这些更改可能是自动回滚。 有关方案，你需要更多的控制-例如，如果你想要包括在实体框架外部事务-中执行的操作，请参阅[使用事务](https://msdn.microsoft.com/data/dn456843)MSDN 上。

## <a name="summary"></a>摘要

你现在具有一组完整的执行有关的简单 CRUD 操作的页`Student`实体。 MVC 帮助器用于生成数据字段的 UI 元素。 MVC 帮助器有关的详细信息，请参阅[呈现窗体使用 HTML 帮助器](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx)（该页是 MVC 3 且不仍适用于 MVC 5）。

在下一教程将通过添加排序和分页扩展索引页的功能。

请在如何喜欢本教程的方式，我们可以提高上，留下反馈。 你还可以请求新主题[教我编写代码](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

在找不到其他实体框架资源的链接[ASP.NET 数据访问的推荐资源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[下一页](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
