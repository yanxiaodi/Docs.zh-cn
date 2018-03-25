---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: "成员资格和管理 |Microsoft 文档"
author: Erikre
description: "本系列教程将教您生成有关我们使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 的 ASP.NET Web 窗体应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: a10dbfe1ca49baee1604aac8dd9a1f93ccfcb7f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="membership-and-administration"></a>成员资格和管理
====================
通过[艾力克 Reitan](https://github.com/Erikre)

[下载 Wingtip Toys 示例项目 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下载电子书 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教程将教您构建使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web 窗体应用程序的基础知识。 Visual Studio 2013[包含 C# 源代码项目](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)是可以附带本系列教程。


本教程演示了如何更新 Wingtip Toys 示例应用程序来添加自定义角色并使用 ASP.NET 标识。 它还演示如何实现自定义角色的用户可以从中添加和删除从网站的产品的管理页。

[ASP.NET 标识](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)是用于生成 ASP.NET web 应用程序的成员身份系统，可以在 ASP.NET 4.5。 在 Visual Studio 2013 Web 窗体项目模板，以及的模板使用 ASP.NET 标识[ASP.NET MVC](../../../../mvc/index.md)， [ASP.NET Web API](../../../../web-api/index.md)，和[ASP.NET 单页面应用程序](../../../../single-page-application/index.md). 你还可以安装使用 NuGet，当你开始与空 Web 应用程序的 ASP.NET 标识系统。 但是，在本教程系列你使用**Web 窗体**projecttemplate，包括 ASP.NET 标识系统。 ASP.NET 标识容易地将特定于用户的配置文件的数据集成与应用程序数据。 此外，ASP.NET 标识，可选择你的应用程序中的用户配置文件的持久性模型。 你可以将数据存储在 SQL Server 数据库或其他数据存储，包括*NoSQL*数据存储 Windows Azure 存储表等。

本教程以前一教程 Wingtip Toys 教程系列中标题为"签出和付款与 PayPal"上。

## <a name="what-youll-learn"></a>你将学习：

- 如何使用代码将自定义角色和用户添加到应用程序。
- 如何限制对管理文件夹和页的访问。
- 如何提供自定义角色所属的用户的导航。
- 如何使用模型绑定填充[DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx)产品类别的控件。
- 如何将文件上载到 web 应用程序使用[FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx)控件。
- 如何验证控件用于实现输入的验证。
- 如何添加和删除从应用程序的产品。

## <a name="these-features-are-included-in-the-tutorial"></a>本教程中包括这些功能：

- ASP.NET 标识
- 配置和授权
- 模型绑定
- 非介入式验证

ASP.NET Web 窗体提供了成员资格功能。 通过使用默认模板，你会获得应用程序运行时，可以立即使用的内置成员资格功能。 本教程演示如何使用 ASP.NET 标识添加自定义角色，并将用户分配给该角色。 您将学习如何限制对管理文件夹的访问。 你将添加到允许具有自定义角色的用户添加和删除产品，以及预览产品后已添加的管理文件夹页。

## <a name="adding-a-custom-role"></a>添加自定义角色

使用 ASP.NET 标识，可以添加自定义角色，并将用户分配给该角色使用代码。

1. 在**解决方案资源管理器**，右键单击*逻辑*文件夹并创建一个新类。
2. 将新类*RoleActions.cs*。
3. 修改代码，以使其显示，如下所示：  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. 在**解决方案资源管理器**，打开*Global.asax.cs*文件。
5. 修改*Global.asax.cs*文件添加代码以黄色突出显示，以使其显示，如下所示：  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. 请注意，`AddUserAndRole`以红色下划线标记。 双击 AddUserAndRole 代码。  
 将带有下划线的字母"A"突出显示方法的开头。
7. 将鼠标悬停在字母"A"，然后单击用户界面，可用于生成方法存根出于`AddUserAndRole`方法。 

    ![成员资格和 Advministration-生成方法存根 （stub）](membership-and-administration/_static/image1.png)
8. 单击标题为的选项：  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. 打开*RoleActions.cs*文件从*逻辑*文件夹。  
 `AddUserAndRole`方法也已添加到类文件。
10. 修改*RoleActions.cs*通过删除文件`NotImplementedeException`和添加以黄色，突出显示的代码，以使其显示，如下所示：  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

上面的代码首先建立用于成员资格数据库的数据库上下文。 成员资格数据库也存储为*.mdf*文件中*应用\_数据*文件夹。 你将能够查看此数据库后的第一个用户已登录到此 web 应用程序。 

> [!NOTE] 
> 
> 如果你想要存储以及产品的数据的成员身份数据，则可以考虑使用相同**DbContext**用于将产品数据存储在上面的代码。


 *内部*关键字是类型 （例如类） 和 （如方法或属性） 的类型成员的访问修饰符。 内部类型或成员是只能在同一程序集中包含的文件中访问*(.dll*文件)。 当你生成应用程序，程序集文件*(.dll*) 创建包含运行你的应用程序时执行的代码。 

A`RoleStore`对象，它提供角色管理，创建基于数据库上下文。

> [!NOTE] 
> 
> 请注意，当`RoleStore`创建对象它使用泛型`IdentityRole`类型。 这意味着，`RoleStore`则仅允许包含`IdentityRole`对象。 此外通过使用泛型，在内存中的资源的更好地处理。


接下来，`RoleManager`对象，基于创建`RoleStore`刚创建的对象。 `RoleManager`对象公开角色相关 API 可以用于自动将更改保存至`RoleStore`。 `RoleManager`则仅允许包含`IdentityRole`对象，因为该代码使用`<IdentityRole>`泛型类型。

你调用`RoleExists`方法来确定是否存在成员资格数据库中的"canEdit"角色。 如果不是这样，在创建角色。

创建`UserManager`对象看起来更复杂比`RoleManager`控制，但是它是几乎相同。 它只是上一个行，而不是多个编码的。 在这里，您要传递的参数作为包含在括号中的新对象实例化。

接下来通过创建一个新创建的"canEditUser"用户`ApplicationUser`对象。 然后，如果你已成功创建用户，你将用户添加到新的角色。

> [!NOTE] 
> 
> 错误处理将在稍后在本教程系列"ASP.NET 错误处理"本教程期间更新。


下次应用程序启动时，将作为名为应用程序的"canEdit"角色添加名为"canEditUser"的用户。 稍后在本教程中，你将登录为"canEditUser"用户以显示将在本教程过程中添加的其他功能。 有关 API 有关 ASP.NET Identity 的详细信息，请参阅[Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx)。 有关初始化的 ASP.NET 标识系统的其他详细信息，请参阅[AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs)。

### <a name="restricting-access-to-the-administration-page"></a>将访问限制为管理页

Wingtip Toys 示例应用程序允许匿名用户和登录的用户都能查看和购买产品。 但是，有自定义的"canEdit"角色的登录的用户可以访问受限制的页面以便添加和删除产品。

#### <a name="add-an-administration-folder-and-page"></a>添加管理文件夹和页

接下来，将创建名为的文件夹*管理员*"canEditUser"用户属于 Wingtip Toys 的自定义角色示例应用程序。

1. 右键单击项目名称 (**Wingtip Toys**) 中**解决方案资源管理器**和选择**添加** - &gt; **新文件夹**.
2. 将新文件夹命名*管理员*。
3. 右键单击*管理员*文件夹，然后选择**添加** - &gt; **新项**。   
 随即出现“添加新项”对话框。
4. 选择**Visual C#** - &gt; **Web**左侧的模板组。 从中间列表中选择**包含母版页的 Web 窗体**，将其命名为*AdminPage.aspx***，** ，然后选择**添加**。
5. 选择*Site.Master*文件作为主页上，，然后选择**确定**。

#### <a name="add-a-webconfig-file"></a>添加 Web.config 文件

通过添加*Web.config*文件为*管理员*文件夹中，你可以限制对访问文件夹中包含的页。

1. 右键单击*管理员*文件夹，然后选择**添加** - &gt; **新项**。  
 随即出现“添加新项”对话框。
2. 从 Visual C# web 模板列表中，选择**Web 配置文件**从中间列表中，接受默认名称*Web.config***，** ，然后选择**添加**。
3. 替换的现有 XML 内容中*Web.config*替换为以下文件：  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

保存*Web.config*文件。 *Web.config*文件指定属于"canEdit"角色的应用程序的唯一用户可以访问此页中包含*管理员*文件夹。

### <a name="including-custom-role-navigation"></a>包括自定义角色导航

若要启用自定义的"canEdit"角色的用户导航到应用程序的管理部分，你必须添加一个链接到*Site.Master*页。 只有属于"canEdit"角色的用户将能够看到**管理员**链接并访问管理部分。

1. 在解决方案资源管理器，找到并打开*Site.Master*页。
2. 若要创建的"canEdit"角色的用户的链接，将添加在黄色再到以下未经排序的列表中突出显示的标记`<ul>`元素以便，列表显示为如下所示：  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. 打开*Site.Master.cs*文件。 请**管理员**链接仅对通过添加代码以到黄色突出显示"canEditUser"用户可见`Page_Load`处理程序。 `Page_Load`处理程序将显示，如下所示：   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

加载页面，代码将检查是否已登录的用户具有的"canEdit"角色。 如果用户属于"canEdit"角色，其中包含到链接的 span 元素*AdminPage.aspx*页 （并因此范围内的链接） 均可见。

### <a name="enabling-product-administration"></a>启用产品管理

到目前为止，你已创建"canEdit"角色，并添加"canEditUser"用户、 管理文件夹和管理页。 你已设置为管理文件夹和页的访问权限，并已添加到应用程序的"canEdit"角色的用户的导航链接。 接下来，你将添加到标记*AdminPage.aspx*页上和代码设为*AdminPage.aspx.cs*将启用"canEdit"角色的用户添加和删除产品的代码隐藏文件。

1. 在**解决方案资源管理器**，打开*AdminPage.aspx*文件从*管理员*文件夹。
2. 现有的标记替换为以下代码：  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. 接下来，打开*AdminPage.aspx.cs*代码隐藏文件，请右键单击*AdminPage.aspx*并单击**查看代码**。
4. 中的现有代码替换*AdminPage.aspx.cs*代码隐藏文件替换为以下代码：  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

在代码中为输入*AdminPage.aspx.cs*代码隐藏文件中，一个名为类`AddProducts`执行将产品添加到数据库的实际工作。 此类尚不存在，因此现在你将创建它。

1. 在**解决方案资源管理器**，右键单击*逻辑*文件夹，然后选择**添加** - &gt; **新项**。   
 随即出现“添加新项”对话框。
2. 选择**Visual C#**  - &gt; **代码**左侧的模板组。 然后，选择**类**从中间列表并将其命名*AddProducts.cs*。   
 将显示新的类文件。
3. 将现有代码替换为以下代码：  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx*页允许用户属于"canEdit"角色添加和删除产品。 添加新产品后，有关该产品的详细信息验证，然后输入到数据库。 新的产品是立即可供所有用户的 web 应用程序。

#### <a name="unobtrusive-validation"></a>非介入式验证

用户在提供的产品详细信息*AdminPage.aspx*页使用验证控件验证 (`RequiredFieldValidator`和`RegularExpressionValidator`)。 这些控件自动使用非介入式验证。 非介入式验证允许使用 JavaScript 客户端验证逻辑，这意味着页不需要到要验证的服务器的行程的验证控件。 默认情况下，非介入式验证包括在*Web.config*文件基于以下配置设置：

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>正则表达式

上的产品价格*AdminPage.aspx*使用验证页**RegularExpressionValidator**控件。 此控件验证是否关联的输入控件 （"AddProductPrice"文本框中） 的值与指定正则表达式模式匹配。 正则表达式是一个模式匹配表示法，可用于快速查找和匹配特定的字符模式。 **RegularExpressionValidator**控件包含一个名为属性`ValidationExpression`包含用来验证价格输入，如下所示的正则表达式：

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>FileUpload 控件

除了输入和验证控件中，你添加**FileUpload**控制转移到*AdminPage.aspx*页。 此控件提供功能以将文件上载。 在这种情况下，你仅允许要上载的图像文件。 中的代码隐藏文件 (*AdminPage.aspx.cs*)，当`AddProductButton`单击后，代码将检查`HasFile`属性**FileUpload**控件。 如果控件有一个文件，并且如果允许 （基于文件扩展名） 的文件类型，将图像保存到*映像*文件夹和*映像/拇指*应用程序文件夹。

#### <a name="model-binding"></a>模型绑定

本系列教程中前面你使用模型绑定来填充**ListView**控件， **FormsView**控件， **GridView**控件，和一个**DetailView**控件。 在本教程中，你使用模型绑定来填充**DropDownList**具有的产品类别列表控件。

添加到的标记*AdminPage.aspx*文件包含**DropDownList**控件称为`DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

使用模型绑定来填充此**DropDownList**通过设置`ItemType`属性和`SelectMethod`属性。 `ItemType`属性指定使用`WingtipToys.Models.Category`键入填充控件时。 通过创建定义这种类型的开头的本系列教程`Category`类 （如下所示）。 `Category`类位于*模型*内的文件夹*Category.cs*文件。

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod`属性**DropDownList**控件指定使用`GetCategories`方法 （如下所示），它是包含在代码隐藏文件 (*AdminPage.aspx.cs*)。

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

此方法指定`IQueryable`接口用于针对查询的计算结果`Category`类型。 返回的值用于填充**DropDownList**中页的标记 (*AdminPage.aspx*)。

通过将设置指定列表中的每个项的显示文本`DataTextField`属性。 `DataTextField`属性使用`CategoryName`的`Category`（上面所示） 以显示每个类别中的类**DropDownList**控件。 中选择一项时传递的实际值**DropDownList**控件基于`DataValueField`属性。 `DataValueField`属性设置为`CategoryID`中定义`Category`类 （如上所示）。

### <a name="how-the-application-will-work"></a>应用程序将如何工作

当属于"canEdit"角色的用户首次导航到页面`DropDownAddCategory` **DropDownList**控件中填充上文所述。 `DropDownRemoveProduct` **DropDownList**控件还使用相同的方法的产品中填充。 属于"canEdit"角色的用户选定的类别类型，并添加产品详细信息 (**名称**，**说明**，**价格**，和**映像文件**). 当用户属于"canEdit"角色单击**添加产品**按钮，`AddProductButton_Click`触发事件处理程序。 `AddProductButton_Click`事件处理程序位于的代码隐藏文件 (*AdminPage.aspx.cs*) 检查要确保它匹配允许的文件类型的图像文件*(.gif*， *.png*， *.jpeg*，或*.jpg*)。 然后，该图像文件保存到 Wingtip Toys 示例应用程序的文件夹。 接下来，将新的产品添加到数据库。 若要完成添加一个新的产品的新实例`AddProducts`类被创建并命名为产品。 `AddProducts`类具有一个名为方法`AddProduct`，和产品对象调用此方法以将产品添加到数据库。

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

查询字符串值如果代码成功向数据库添加新的产品，都会重新加载页面`ProductAction=add`。

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

时将重新加载页面，则会将查询字符串包含在 URL 中。 通过重新加载该页面，属于"canEdit"角色的用户可以立即查看中的更新**DropDownList**上的控件*AdminPage.aspx*页。 此外，通过包括查询字符串的 url，页可以显示一条成功消息给用户属于"canEdit"角色。

当*AdminPage.aspx*页上的重新加载需要，`Page_Load`事件时调用。

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load`事件处理程序检查查询字符串值，并确定是否显示一条成功消息。

## <a name="running-the-application"></a>运行应用程序

你可以在购物车中运行应用程序现在以查看如何添加、 删除和更新项目。 购物车总计将反映在购物车中的所有项的总成本。

1. 在解决方案资源管理器，按**F5**运行 Wingtip Toys 示例应用程序。  
 浏览器将打开并显示*Default.aspx*页。
2. 单击**登录**页顶部的链接。 

    ![成员资格和管理的登录链接](membership-and-administration/_static/image2.png)

 *Login.aspx*显示页。
3. 使用以下用户名和密码：  
 用户名：canEditUser@wingtiptoys.com  
 密码： Pa $$ word1 

    ![成员资格和管理的登录页](membership-and-administration/_static/image3.png)
4. 单击**登录**附近页面底部的按钮。
5. 在下一步的页的顶部，选择**管理员**链接以导航到*AdminPage.aspx*页。 

    ![成员资格和管理-管理员链接](membership-and-administration/_static/image4.png)
6. 若要测试输入的验证，请单击**添加产品**而无需添加任何产品详细信息的按钮。 

    ![成员资格和管理-管理员页](membership-and-administration/_static/image5.png)

 请注意，会显示必填的字段消息。
7. 添加新产品的详细信息，然后单击**添加产品**按钮。 

    ![成员资格和管理-添加产品](membership-and-administration/_static/image6.png)
8. 选择**产品**添加从顶部导航菜单中查看新的产品。 

    ![成员资格和管理-显示新产品](membership-and-administration/_static/image7.png)
9. 单击**管理员**链接以返回到管理页。
10. 在**删除产品**部分的页上，选择你在中添加新产品**DropDownListBox**。
11. 单击**删除产品**按钮可以从应用程序中删除新的产品。 

    ![成员资格和管理-删除产品](membership-and-administration/_static/image8.png)
12. 选择**产品**从顶部导航菜单以确认已删除产品。
13. 单击**注销**存在管理模式。   
 请注意，顶部导航窗格中将不再显示**管理员**菜单项。

## <a name="summary"></a>摘要

在本教程中，你将添加自定义角色和用户属于的自定义角色，对管理文件夹和页上，访问受到限制并提供属于自定义角色的用户的导航。 使用模型绑定来填充**DropDownList**与数据的控件。 你实现**FileUpload**控件和验证控件。 此外，你已学习如何添加和从数据库中删除产品。 在下一步的教程中，你将了解如何实现 ASP.NET 路由。

## <a name="additional-resources"></a>其他资源

[Web.config 的授权元素](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET 标识](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[将包含成员资格、 OAuth 和 SQL 数据库的安全的 ASP.NET Web 窗体应用程序部署到 Azure 网站](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure 的免费试用版](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
[上一页](checkout-and-payment-with-paypal.md)
[下一页](url-routing.md)
