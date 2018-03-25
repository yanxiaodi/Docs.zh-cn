---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: "迭代 #2 – 使应用程序，看上去很好的 (C#) |Microsoft 文档"
author: microsoft
description: "在此迭代中，我们通过修改默认 ASP.NET MVC 视图母版页和级联样式表改进应用程序的外观。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: 10379f5321773155aaff4c384d8e0716d7e0e874
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="iteration-2--make-the-application-look-nice-c"></a>迭代 #2 – 使应用程序，看上去很好的 (C#)
====================
通过[Microsoft](https://github.com/microsoft)

[下载代码](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> 在此迭代中，我们通过修改默认 ASP.NET MVC 视图母版页和级联样式表改进应用程序的外观。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>生成联系人管理 ASP.NET MVC 应用程序 (C#)
  

在这一系列的教程，我们生成整个联系人管理应用程序从头到尾完成。 联系人管理器应用程序，可存储的名称，电话号码和电子邮件地址的联系人信息有关的人员列表。

我们在多次迭代中生成应用程序。 每次迭代时，我们逐渐提高应用程序。 此多个迭代方法旨在使您能够了解每个更改的原因。

- 迭代 #1-创建应用程序。 在第一次迭代中，我们创建联系人管理器中的最简单方法可能。 我们将添加对基本数据库操作的支持： 创建、 读取、 更新和删除 (CRUD)。

- 迭代 #2-使应用程序，看上去很好。 在此迭代中，我们通过修改默认 ASP.NET MVC 视图母版页和级联样式表改进应用程序的外观。

- 迭代 #3-添加窗体验证。 在第三个迭代中，我们将添加基本窗体验证。 我们可以防止人员提交窗体，但不完成需要的表单域。 我们还验证电子邮件地址和电话号码。

- 迭代 #4-请松散耦合的应用程序。 在此第三个迭代中，我们利用多个软件设计模式以使其更轻松地监视和修改联系人管理器应用程序。 例如，我们将重构应用程序以使用存储库模式和依赖关系注入模式。

- 迭代 #5-创建单元测试。 在第五个迭代中，我们使我们的应用程序更轻松地监视和修改通过添加单元测试。 我们模拟我们数据模型类，并生成单元测试控制器和验证逻辑。

- 迭代 #6-使用测试驱动开发。 在此第六个迭代中，我们将添加新功能到我们的应用程序通过首先编写单元测试和针对单元测试编写代码。 在此迭代中，我们添加联系人的组。

- 迭代 #7-添加 Ajax 功能。 在第七个迭代中，我们通过添加对 Ajax 的支持提高响应能力和我们的应用程序的性能。

## <a name="this-iteration"></a>此迭代

此迭代的目的是提高联系人管理器应用程序的外观。 当前，联系人管理器使用默认 ASP.NET MVC 视图母版页和级联样式表 （请参见图 1）。 这些不要看起来不正常，但我不想要看起来正像每个其他 ASP.NET MVC 网站的联系人管理器。 我想要将这些文件替换为自定义文件。


[![新项目对话框](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**图 01**: ASP.NET MVC 应用程序的默认外观 ([单击以查看实际尺寸的图像](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


在此迭代中，我讨论两种方法来改进我们的应用程序的可视设计。 首先，我向你演示如何充分利用 ASP.NET MVC 设计库以下载可用的 ASP.NET MVC 设计模板。 ASP.NET MVC 设计库，可创建专业人员的 web 应用程序不执行任何操作。

我决定不使用模板从 ASP.NET MVC 设计库联系人管理器应用程序。 相反，我必须由专业人员设计公司的自定义设计。 在本教程的第二个阶段，我将说明我专业人员设计公司，以创建最终的 ASP.NET MVC 设计的工作方式。

## <a name="the-aspnet-mvc-design-gallery"></a>ASP.NET MVC 设计库

ASP.NET MVC 设计库是由 Microsoft 提供的可用资源。 ASP.NET MVC 库位于以下地址：

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC 设计库承载专门为使用 ASP.NET MVC 项目中创建的免费网站设计的集合。 设计上载由社区的成员。 库的访问者可以对其最喜欢的设计进行投票 （请参见图 2）。


[![新项目对话框](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**图 02**: ASP.NET MVC 设计库 ([单击以查看实际尺寸的图像](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


当我撰写本教程中，库中最常用的设计是由 David Hauser 命名年 10 月的设计。 你可以通过完成以下步骤为这种设计用于 ASP.NET MVC 项目：

1. 单击**下载**按钮 October.zip 文件下载到你的计算机。
2. 右键单击下载的 October.zip 文件，然后单击**解除阻止**按钮 （请参见图 3）。
3. 将文件提取到名为年 10 月的文件夹。
4. 从包含在年 10 月文件夹 DesignTemplate 文件夹中选择的所有文件，右键单击文件，然后选择菜单选项**复制**。
5. 右键单击 Visual Studio 解决方案资源管理器窗口中的 ContactManager 项目节点并选择菜单选项**粘贴**（请参见图 4）。
6. 选择 Visual Studio 菜单选项**编辑、 查找和替换，快速替换**和替换*[MyProjectName]*与*ContactManager* （请参见图 5）。


[![新项目对话框](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**图 03**： 取消阻止文件从 web 下载 ([单击以查看实际尺寸的图像](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![新项目对话框](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**图 04**： 覆盖在解决方案资源管理器中的文件 ([单击以查看实际尺寸的图像](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![新项目对话框](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**图 05**: [ProjectName] 替换为 ContactManager ([单击以查看实际尺寸的图像](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


完成这些步骤后，web 应用程序将使用新的设计。 在图 6 中页演示年 10 月设计的联系人管理器应用程序的外观。


[![新项目对话框](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**图 06**: ContactManager 年 10 月模板 ([单击以查看实际尺寸的图像](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>创建自定义 ASP.NET MVC 设计

ASP.NET MVC 设计库具有不同的设计样式很好选择。 库为你提供了一种自定义的 ASP.NET MVC 应用程序的外观的轻松方法。 而且，当然，库中包含的完全免费而言优势巨大。

但是，你可能需要创建你的网站的完全唯一设计。 在这种情况下，最好使用网站设计公司。 我决定采用此方法为联系人管理器应用程序的设计。

我压缩向上联系人管理器从迭代 #1，并发送到设计公司的项目。 它们不归 Visual Studio （耻辱在其上 ！），但这不是 t 会出现问题。 它们是可以从免费下载 Microsoft Visual Web Developer [https://www.asp.net](https://www.asp.net)网站，然后打开 Visual Web Developer 中的联系人管理器应用程序。 在几天，它们必须生成图 7 中的设计。


[![新项目对话框](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**图 07**: ASP.NET MVC 联系人管理器设计 ([单击以查看实际尺寸的图像](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


新设计由两个主要文件组成： 新的级联样式表文件和新视图母版页文件。 视图的母版页包含的布局和共享中的 ASP.NET MVC 应用程序视图的内容。 例如，视图的母版页包括标头、 导航选项卡和页脚显示在图 7。 我使用从设计公司中，新的 Site.Master 文件覆盖现有 Site.Master 视图母版页 Views\Shared 文件夹中

设计公司还创建一个新的级联样式表和一组的映像。 我的内容文件夹中放置这些新文件，并覆盖现有的 Site.css 文件。 应将所有静态内容放在内容文件夹。

请注意，新设计的联系人管理器中包含图像编辑和删除联系人。 联系人的 HTML 表中每个联系人旁边将显示编辑和删除映像。

最初，这些链接在呈现的 HTML。ActionLink() 帮助器如下：

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Html.ActionLink() 方法不支持 （方法 HTML 编码出于安全原因此链接文本） 的映像。 因此，我可以对 Html.ActionLink() 的调用替换对 Url.Action() 如下的调用：

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Html.ActionLink() 方法呈现整个 HTML 超链接。 Url.Action() 方法中，另一方面，呈现只而无需 URL &lt;&gt;标记。

此外，请注意，新的设计包括选定的和未选定的选项卡。 例如，在图 8:**创建新联系人**选择选项卡和**我的联系人**不选择选项卡。


[![新项目对话框](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**图 08**： 选中和取消选中选项卡 ([单击以查看实际尺寸的图像](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


若要支持呈现选定和未选定的选项卡，我创建了名为 MenuItemHelper 的自定义 HTML 帮助。 此帮助器方法呈现或者&lt;li&gt;标记或&lt;li 类 ="所选"&gt;具体取决于当前控制器和操作是否与传递给帮助器的控制器和操作名称对应的标记。 MenuItemHelper 的代码包含在清单 1。

**列表 1-Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

MenuItemHelper 在内部使用 TagBuilder 类来生成&lt;li&gt; HTML 标记。 TagBuilder 类是当需要生成新的 HTML 标记时可以使用一个非常有用的实用程序类。 它包括用于添加属性、 添加 CSS 类、 生成 Id，和修改标记的方法内部 HTML。

## <a name="summary"></a>摘要

在此迭代中，我们改进我们的 ASP.NET MVC 应用程序的可视设计。 首先，已引入到 ASP.NET MVC 设计库。 您学习了如何从你可以在 ASP.NET MVC 应用程序中使用 ASP.NET MVC 设计库下载免费设计模板。

接下来，我们讨论了通过修改默认级联样式表文件和母版视图中页面文件，你就可以创建自定义设计。 为了支持新的设计，我们必须对我们的联系人管理器应用程序进行少量更改。 例如，我们将添加名为显示选定和未选定的选项卡 MenuItemHelper 的新 HTML 帮助。

在下一步的迭代中，我们解决的验证非常重要的主题。 以便用户无法未首先提供所需的值，如人员 s 情况下创建一个新的联系人和姓氏，我们将验证代码添加到我们的应用程序。

>[!div class="step-by-step"]
[上一页](iteration-1-create-the-application-cs.md)
[下一页](iteration-3-add-form-validation-cs.md)
