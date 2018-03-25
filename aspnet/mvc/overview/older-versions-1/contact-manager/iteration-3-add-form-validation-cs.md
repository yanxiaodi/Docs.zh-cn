---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: "迭代 #3 – 添加窗体验证 (C#) |Microsoft 文档"
author: microsoft
description: "在第三个迭代中，我们将添加基本窗体验证。 我们可以防止人员提交窗体，但不完成需要的表单域。 我们还验证 emai..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 120c35755784ba5a08a9592fdc58f17879848631
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="iteration-3--add-form-validation-c"></a>迭代 #3 – 添加窗体验证 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下载代码](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> 在第三个迭代中，我们将添加基本窗体验证。 我们可以防止人员提交窗体，但不完成需要的表单域。 我们还验证电子邮件地址和电话号码。


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

此第二个迭代中的联系人管理器应用程序，我们将添加基本窗体验证。 我们可以防止人员而无需为所需的窗体字段中输入值提交联系人。 我们还验证电话号码和电子邮件地址 （请参见图 1）。


[![新项目对话框](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**图 01**： 带有验证的窗体 ([单击以查看实际尺寸的图像](iteration-3-add-form-validation-cs/_static/image2.png))


在此迭代中，我们直接向控制器操作添加验证逻辑。 一般情况下，这不是将验证添加到 ASP.NET MVC 应用程序的建议的方法。 更好的方法是将在单独的应用程序的验证逻辑[服务层](http://martinfowler.com/eaaCatalog/serviceLayer.html)。 在下一步的迭代中，我们将重构联系人管理器应用程序，使应用程序更易于维护。

在此迭代中，若要为简便起见，我们编写验证代码的所有手动。 而不是自己编写验证代码，我们无法充分利用验证框架。 例如，可以使用 Microsoft 企业库验证应用程序块 (VAB) 为你的 ASP.NET MVC 应用程序中实现验证逻辑。 若要了解有关验证应用程序块的详细信息，请参阅：

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>将验证添加到创建视图

让我们来开始通过将验证逻辑添加到创建视图。 幸运的是，由于我们生成具有 Visual Studio 的创建视图，创建视图已包含的所有必要的用户界面逻辑，以显示验证消息。 列表 1 中包含创建视图。

**列表 1-\Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

请注意对正上方的 HTML 窗体的 Html.ValidationSummary() 帮助器方法的调用。 如果有验证错误消息，此方法对项目符号列表中显示验证消息。

请注意，此外，每个窗体字段旁边显示对 Html.ValidationMessage() 的调用。 ValidationMessage() 帮助器显示单独的验证错误消息。 在清单 1 的情况下验证错误时，将显示一个星号。

最后，Html.TextBox() helper 自动呈现级联样式表类中的帮助程序显示的属性与关联的验证错误时。 Html.TextBox() 帮助器呈现一个名为类**输入验证错误**。

在创建新的 ASP.NET MVC 应用程序时，名为 Site.css 的样式表的内容文件夹中自动创建。 此样式表包含验证错误消息的外观与相关的 CSS 类的以下定义：

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

字段验证错误类用于设置样式的 Html.ValidationMessage() 帮助程序呈现的输出。 输入验证错误类用于设置样式呈现 Html.TextBox() 帮助程序文本框中 （输入）。 验证摘要错误类用于设置样式呈现 Html.ValidationSummary() 帮助程序未经排序的列表。

> [!NOTE] 
> 
> 你可以修改自定义的验证错误消息的外观此部分中所述的样式表类。


## <a name="adding-validation-logic-to-the-create-action"></a>添加验证逻辑，以创建操作

现在，创建视图永远不会显示验证错误消息，因为我们不可以编写逻辑来生成任何消息。 若要显示验证错误消息，你需要向 ModelState 添加错误消息。

> [!NOTE] 
> 
> UpdateModel() 方法将错误消息添加到 ModelState 自动错误的表单域的值分配到属性时。 例如，如果你尝试将字符串"apple"分配到的出生日期属性。 接受 DateTime 值，然后 UpdateModel() 方法将错误添加到 ModelState。


列出 2 中修改后的 create （） 方法包含一个新部分，将新的联系人插入到数据库之前验证联系人类的属性。

**列出 2-Controllers\ContactController.cs （创建与验证）**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

验证部分强制执行四个不同的验证规则：

- FirstName 属性必须大于零的长度 （和它不能只由空格）
- LastName 属性必须大于零的长度 （和它不能只由空格）
- 如果电话属性的值 （长度大于 0） 则 Phone 属性必须与正则表达式匹配。
- 如果电子邮件属性的值 （长度大于 0） 则电子邮件属性必须与正则表达式匹配。

如果没有验证规则违规，则将一条错误消息添加到 ModelState AddModelError() 方法的帮助。 当你将一条消息添加到 ModelState 时，需要提供属性的名称和验证错误消息的文本。 Html.ValidationSummary() 和 Html.ValidationMessage() 帮助器方法的情况下，此错误消息显示在视图中。

执行验证规则后，将检查 ModelState IsValid 属性。 在任意验证错误消息添加到 ModelState 后，IsValid 属性返回 false。 如果验证失败，则会将创建表单重新显示的错误消息。

> [!NOTE] 
> 
> 我收到了用于验证在正则表达式存储库中的电话号码和电子邮件地址的正则表达式[ *http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>将验证逻辑添加到的编辑操作

Edit （） 操作会更新联系人。 Edit （） 操作必须执行的 create （） 操作的验证完全相同。 而不是复制相同的验证代码时，我们应重构联系人控制器以便 create （） 和 edit （） 操作调用相同的验证方法。

修改的联系人控制器类包含在清单 3。 此类已在 create （） 和 edit （） 操作中调用的新 ValidateContact() 方法。

**Listing 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>摘要

在此迭代中，我们将添加基本窗体验证到我们的联系人管理器应用程序。 我们验证逻辑阻止用户提交新的联系人或编辑现有联系人，无需提供 FirstName 和 LastName 属性的值。 此外，用户必须提供有效的电话号码和电子邮件地址。

在此迭代中，我们添加验证逻辑到我们的联系人管理器应用程序中可能的最简单方法。 但是，到我们控制器逻辑混合我们验证逻辑将问题为创建我们在长期内。 我们的应用程序将会更加难以维护，并随着时间的推移修改。

在下一步的迭代中，我们将重构我们的验证逻辑和数据库访问逻辑外我们控制器。 我们将充分利用多个软件设计原则，以便能够创建一个更松散耦合，且更易于维护，应用程序。

>[!div class="step-by-step"]
[上一页](iteration-2-make-the-application-look-nice-cs.md)
[下一页](iteration-4-make-the-application-loosely-coupled-cs.md)
