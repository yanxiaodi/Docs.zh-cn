---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: "在 ASP.NET MVC 应用程序 (2 个 10) 中实现与实体框架的基本 CRUD 功能 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d031cd760fb578d29626933eed39fe987ef796d7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>在 ASP.NET MVC 应用程序 (2 个 10) 中实现与实体框架的基本 CRUD 功能
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 你可以从头开始教程系列或[下载这一章的初学者项目](building-the-ef5-mvc4-chapter-downloads.md)和从这里开始。
> 
> > [!NOTE] 
> > 
> > 如果你遇到无法解决的问题[下载已完成的章](building-the-ef5-mvc4-chapter-downloads.md)并尝试重现你的问题。 你通常可以通过比较你的代码已完成的代码会发现问题的解决方案。 一些常见的错误和如何解决它们，请参阅[错误和解决方法。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在以前的教程，你创建的 MVC 应用程序存储和显示数据使用实体框架和 SQL Server LocalDB。 在本教程中，你将查看和自定义 CRUD （创建、 读取、 更新、 删除） 的 MVC 基架自动为你创建在控制器和视图中的代码。

> [!NOTE]
> 它是常见的做法，以便创建你的控制器和数据访问层之间的抽象层实现存储库模式。 若要简化这些教程，则不会直到本系列后面的教程实现存储库。


在本教程中，你将创建以下网页：

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>创建详细信息页

学生的基架的代码`Index`页中省略`Enrollments`属性，因为该属性包含集合。 在`Details`将集合的内容显示在 HTML 表的页。

 在*Controllers\StudentController.cs*的操作方法`Details`查看使用`Find`方法来检索单个`Student`实体。 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 密钥的值传递给该方法为`id`参数和来自路线中的数据**详细信息**索引网页上的超链接。 

1. 打开*Views\Student\Details.cshtml*。 使用显示每个字段`DisplayFor`帮助器，如下面的示例中所示： 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. 后`EnrollmentDate`字段和立即在关闭前`fieldset`标记中，添加代码以显示列表的注册，如下面的示例中所示：

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    此代码循环访问中的实体`Enrollments`导航属性。 每个`Enrollment`实体属性中，它显示课程标题和评分。 正在从检索课程标题`Course`中存储的实体`Course`导航属性`Enrollments`实体。 所有这些数据是从数据库中检索自动需要时。 （在换而言之，你正在使用延迟加载此处。 未指定*预先加载*为`Courses`导航属性，使你尝试访问该属性第一次，查询将发送到数据库以检索数据。 你可以阅读更多有关延迟加载和中的预先加载[读取相关数据](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)更高版本中这一系列教程。)
3. 通过选择运行页面**学生**选项卡上，单击**详细信息**Alexander Carson 的链接。 为所选学生查看课程和年级的列表：

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>更新创建页

1. 在*Controllers\StudentController.cs*，替换`HttpPost``Create`操作方法替换为以下代码以添加`try-catch`块和[绑定属性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)到基架方法： 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    此代码将添加`Student`实体创建的 ASP.NET MVC 模型联编程序到`Students`实体设置，然后将所做的更改保存到数据库。 (*模型联编程序*指使它可以更轻松地处理提交的窗体的数据; 模型联编程序将发布窗体到 CLR 类型值，并将其传递给参数中的操作方法的 ASP.NET MVC 功能。 模型联编程序的实例化 in this case，`Student`为你使用属性的实体值从`Form`集合。)

    `ValidateAntiForgeryToken`属性有助于防止[跨站点请求伪造](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)攻击。

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. 通过选择运行页面**学生**选项卡上，单击**新建**。

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    默认情况下，某些数据验证工作原理。 输入名称和一个无效的日期，然后单击**创建**可查看错误消息。

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    以下突出显示的代码演示模型验证检查。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    将日期更改为有效的值，如 9/1/2005年，单击**创建**若要查看显示在新学生**索引**页。

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>更新编辑发布页

在*Controllers\StudentController.cs*、 `HttpGet` `Edit`方法 (而不是`HttpPost`属性) 使用`Find`方法来检索所选`Student`作为你看到的实体，在`Details`方法。 你不需要更改此方法。

但是，替换`HttpPost``Edit`操作方法替换为以下代码以添加`try-catch`块和[绑定属性](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

此代码将类似于你在中看到`HttpPost``Create`方法。 但是，而不是添加到实体集模型联编程序所创建的实体，此代码，该值指示它已被更改的实体上设置的标志。 当[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)调用方法时，[已修改](https://msdn.microsoft.com/library/system.data.entitystate.aspx)标志会导致实体框架创建 SQL 语句，以更新数据库行。 将更新的数据库行所有列，包括用户没有更改，并且会忽略并发冲突。 （你将了解如何在本系列后面的教程中处理并发）。

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>实体状态的附加和 SaveChanges 方法

是否在内存中的实体是在数据库中，其对应行与同步，此信息确定在调用时，会发生什么情况，将跟踪的数据库上下文`SaveChanges`方法。 例如，当传递到新实体[添加](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx)方法，该实体的状态设置为方法`Added`。 然后调用[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)方法，数据库上下文发出 SQL`INSERT`命令。

实体可能之一[以下状态](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`。 实体在数据库中尚不存在。 `SaveChanges`方法必须发出`INSERT`语句。
- `Unchanged`。 无需使用通过此实体完成`SaveChanges`方法。 当从数据库读取实体时，该实体开始时具有此状态。
- `Modified`。 某些或所有实体的属性值已都更改。 `SaveChanges`方法必须发出`UPDATE`语句。
- `Deleted`。 实体已标记为删除。 `SaveChanges`方法必须发出`DELETE`语句。
- `Detached`。 实体不跟踪的数据库上下文。

在桌面应用中，通常会自动设置状态更改。 在桌面类型的应用程序，你可以阅读实体并对它的一些属性值进行更改。 这将导致其实体状态自动更改为`Modified`。 然后调用`SaveChanges`，实体框架生成 SQL`UPDATE`更新仅你更改的实际属性的语句。

Web 应用断开连接的特性不允许对此连续序列。 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)读取实体已释放之后呈现一个页面。 当`HttpPost``Edit`操作方法调用、 新的请求进行并且您具有的新实例[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)，因此你必须手动将实体状态设置为`Modified.`然后当调用`SaveChanges`，实体框架更新数据库行中，所有的列，因为上下文具有无法知道您更改了哪些属性。

如果你想 SQL`Update`语句才能更新字段的用户实际更改，以便它们可用时，可以以某种方式 （如隐藏字段） 保存的原始值`HttpPost``Edit`调用方法。 然后你可以创建`Student`实体使用原始值，调用`Attach`实体，该原始版本的方法的新值更新实体的值，然后调用`SaveChanges.`详细信息，请参阅[实体状态和 SaveChanges](https://msdn.microsoft.com/data/jj592676)和[本地数据](https://msdn.microsoft.com/data/jj592872)MSDN 数据开发人员中心中。

中的代码*Views\Student\Edit.cshtml*类似于你在中看到*Create.cshtml*，并需进行任何更改。

通过选择运行页面**学生**选项卡，然后单击**编辑**超链接。

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

更改某些数据，再单击**保存**。 你看到索引页面中的更改的数据。

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>更新删除页

在*Controllers\StudentController.cs*的模板代码`HttpGet``Delete`方法使用`Find`方法来检索所选`Student`实体，当你在中看到`Details`和`Edit`方法。 但是，若要实现自定义错误消息时对调用`SaveChanges`失败，需要将某些功能添加到此方法，其相应的视图。

当你看到的针对更新，并创建操作，删除操作需要两个操作方法。 调用以响应 GET 请求的方法显示为用户提供了机会批准或取消删除操作的视图。 如果用户批准它，则创建 POST 请求。 在这种情况， `HttpPost` `Delete`调用方法，该方法然后实际执行删除操作。

你将添加`try-catch`阻止`HttpPost``Delete`方法以处理更新数据库时可能出现的任何错误。 如果发生错误， `HttpPost` `Delete`方法调用`HttpGet``Delete`方法，将其传递参数，该值指示发生错误。 `HttpGet Delete`方法然后重新显示确认页以及错误消息，向用户提供机会取消或重试。

1. 替换`HttpGet``Delete`替换为以下代码，它管理错误报告的操作方法： 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    此代码接受[可选](https://msdn.microsoft.com/library/dd264739.aspx)布尔参数可指示是否它在无法保存更改之后调用。 此参数是`false`时`HttpGet``Delete`不上一次失败的情况下调用方法。 当调用`HttpPost``Delete`方法中对数据库更新错误响应，该参数是`true`和一条错误消息传递给视图。
- 替换`HttpPost``Delete`操作方法 (名为`DeleteConfirmed`) 替换为以下代码，该执行实际的删除操作并捕获任何数据库更新错误。

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

    此代码检索所选的实体，然后调用[删除](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx)方法将实体的状态设置为`Deleted`。 当`SaveChanges`调用时，SQL`DELETE`生成命令。 您还已更改的操作方法名称`DeleteConfirmed`到`Delete`。 名为的基架的代码`HttpPost``Delete`方法`DeleteConfirmed`以便`HttpPost`方法一个唯一的签名。 （CLR 需要具有不同的方法参数的重载的方法。）签名是唯一的现在可以坚持使用 MVC 约定，使用相同的名称用于`HttpPost`和`HttpGet`删除方法。

    如果提高大容量应用程序中的性能是优先级，则无法避免不必要的 SQL 查询，以检索行，通过将调用的代码行`Find`和`Remove`方法替换为以下代码黄色中所示突出显示：

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

    此代码实例化`Student`实体使用仅的主键值然后将实体状态设置为`Deleted`。 这就是所有实体框架需要以便删除该实体。

    如前所述， `HttpGet` `Delete`方法不会删除数据。 执行 delete 操作以响应 GET 请求 （或对于此问题，执行任何编辑操作，创建操作或更改数据的任何其他操作） 会产生安全风险。 有关详细信息，请参阅[ASP.NET MVC 提示 #46-不要使用删除的链接，因为他们创建的安全漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)Stephen Walther 博客上。
- 在*Views\Student\Delete.cshtml*，添加一条错误消息之间`h2`标题和`h3`标题下，如下面的示例中所示：

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

    通过选择运行页面**学生**选项卡上，单击**删除**超链接：

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
- 单击**删除**。 索引页面显示没有已删除的学生。 (你将看到举例说明的错误处理代码中的操作中[处理并发](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)更高版本中这一系列教程。)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>确保数据库连接不保持打开状态

若要确保能正确关闭数据库连接，并且它们具有向上已释放的资源，你应该会看到与其上下文实例已释放。 就是为什么基架的代码提供[释放](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx)方法末尾`StudentController`类*StudentController.cs*，下面的示例中所示：

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

基`Controller`类已经实现`IDisposable`接口，所以此代码只需将添加到替代`Dispose(bool)`方法以显式释放上下文实例。

## <a name="summary"></a>摘要

你现在具有一组完整的执行有关的简单 CRUD 操作的页`Student`实体。 MVC 帮助器用于生成数据字段的 UI 元素。 MVC 帮助器有关的详细信息，请参阅[呈现窗体使用 HTML 帮助器](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx)（该页是 MVC 3 且不仍适用于 MVC 4）。

在下一教程将通过添加排序和分页扩展索引页的功能。

在找不到其他实体框架资源的链接[ASP.NET 数据访问内容映射](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[下一页](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
