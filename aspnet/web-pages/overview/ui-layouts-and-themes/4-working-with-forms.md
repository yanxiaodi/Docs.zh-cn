---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: "使用 ASP.NET Web 页 (Razor) 站点中的 HTML 窗体 |Microsoft 文档"
author: tfitzmac
description: "窗体是用户输入控件，如文本框、 复选框、 单选按钮和下拉列表的放置位置的 HTML 文档的节。 使用窗体疑问词..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>使用 ASP.NET Web 页 (Razor) 站点中的 HTML 窗体
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文介绍如何处理 HTML 窗体 （使用文本框和按钮） 当你使用在 ASP.NET Web 页 (Razor) 网站中。
> 
> **你将学习：** 
> 
> - 如何创建的 HTML 窗体。
> - 如何从窗体中读取用户输入。
> - 如何验证用户输入。
> - 如何还原窗体值提交页面后。
> 
> 这些是 ASP.NET 的文章中介绍的编程概念：
> 
> - `Request` 对象。
> - 输入的验证。
> - HTML 编码。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


## <a name="creating-a-simple-html-form"></a>创建简单的 HTML 窗体

1. 创建新网站。
2. 在根文件夹中，创建一个名为的 web 页*Form.cshtml*并输入以下标记：

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. 启动你的浏览器中的页。 (在 WebMatrix 中，在**文件**工作区中，右键单击该文件，然后选择**在浏览器中启动**。)一种简单形式具有三个输入字段和**提交**显示按钮。

    ![带有三个文本框的窗体的屏幕截图。](4-working-with-forms/_static/image1.jpg)

    此时，如果你单击**提交**按钮时，会执行任何操作。 若要使表单有用，必须添加将在服务器运行某些代码。

## <a name="reading-user-input-from-the-form"></a>从窗体中读取用户输入

若要处理窗体，你可以添加代码用于读取提交的字段值并进行某项操作它们。 此过程演示了如何读取的字段，并显示在页面上的用户输入。 （在生产应用程序中，你通常执行更值得关注操作需要用户输入。 你将执行该操作中使用数据库有关的文章。）

1. 在顶部*Form.cshtml*文件中，输入以下代码：

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    当用户第一次请求页时，将显示仅空窗体。 （它将是你） 用户在填写窗体，然后单击**提交**。 这将 (post) 提交到服务器的用户输入。 默认情况下，请求将转到同一页 (即*Form.cshtml*)。

    当提交页面此时，窗体的正上方显示你输入的值：

    ![显示页面上显示您输入的值的屏幕截图。](4-working-with-forms/_static/image2.jpg)

    查看页的代码。 首次使用`IsPost`方法来确定是否正在发页面 &#8212; 也就是说，用户单击按钮是否**提交**按钮。 如果这是 post， `IsPost` ，则返回 true。 这是标准方法中的 ASP.NET Web Pages 以确定是否你正在使用的初始请求 （GET 请求） 或回发 （POST 请求）。 (有关 GET 和 POST 的详细信息，请参阅"HTTP GET 和 POST 和 IsPost 属性的"边栏中[ASP.NET 网页编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost)。)

    接下来，获取从用户填充的值`Request.Form`对象，并且你将它们放在变量为更高版本。 `Request.Form`对象包含所有已提交的页面中，每个键标识的值。 键是等效于`name`你想要读取的表单域的属性。 例如，若要读取`companyname`字段 （文本框中），你使用`Request.Form["companyname"]`。

    窗体值存储在`Request.Form`对象作为字符串。 因此，你必须使用与数字、 日期或某些其他类型的一个值，必须将其从字符串转换为该类型。 在示例中，`AsInt`方法`Request.Form`用于将员工字段 （其中包含员工计数） 的值转换为整数。
2. 启动你的浏览器中的页、 填充窗体字段中，并单击**提交**。 页面显示您输入的值。

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>HTML 编码的外观和安全
> 
> HTML 有特殊字符，如用于`<`， `>`，和`&`。 如果这些特殊字符出现不认为它们不其中，他们可以破坏的外观和功能的网页。 例如，浏览器解释`<`愿意为 HTML 元素，开始字符 （除非它后面跟一个空格）`<b>`或`<input ...>`。 如果浏览器无法识别的元素，它只需放弃开头的字符串`<`直至其达到的内容，它将再次识别。 显然，这可能导致某些太怪异呈现在页中。
> 
> HTML 编码用浏览器将解释为正确的符号的代码替换以下保留的字符。 例如，`<`字符替换`&lt;`和`>`字符替换`&gt;`。 浏览器呈现的字符，你想要查看这些替换字符串。
> 
> 它是使用 HTML 编码显示字符串的任何时间 （输入） 你从用户获取一个好主意。 如果没有，用户可尝试获取你 web 页后，可以运行恶意脚本或做其他事情这影响了你站点的安全或不想。 （这一点特别重要，如果你需要较长用户输入，将其存储某个地方，，然后显示其更高版本 &#8212; 例如，博客注释，作为用户查看，或类似的。）
> 
> 为了帮助防止这些问题，ASP.NET 网页自动进行 HTML 编码的任何文本内容，您在代码中输出。 例如，当你显示变量或使用代码类似于的表达式的内容时，才`@MyVar`，ASP.NET 网页自动对输出进行编码。


## <a name="validating-user-input"></a>验证用户输入

用户犯的错误。 要求他们填写字段，并在忘记，或要求其输入的雇员数和它们而是键入的名称。 若要确保，窗体具有已正确填写处理它之前，你可以验证用户的输入。

此过程演示如何验证所有三个窗体字段，以确保用户未将其留空。 你还检查员工计数值是一个数字。 如果不存在错误，则将显示错误消息，告诉用户哪些值未通过验证。

1. 在*Form.cshtml*文件中，将第一个代码块替换为以下代码： 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    若要验证用户输入，你可以使用`Validation`帮助器。 通过调用注册必填的字段`Validation.RequireField`。 通过调用注册其他类型的验证`Validation.Add`，并指定要验证的字段和要执行的验证类型。

    页运行时，ASP.NET 将执行为你的所有验证。 你可以通过调用检查结果`Validation.IsValid`，它返回的所有内容传递如果为 true 和 false 如果验证失败的任何字段。 通常，你可以调用`Validation.IsValid`输入对用户执行任何处理之前。
2. 更新`<body>`元素通过添加的三个调用`Html.ValidationMessage`方法，如下：

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    若要显示验证错误消息，你可以调用 Html。`ValidationMessage` 并将其传递所需的消息的字段的名称。
3. 运行页面。 将字段留空并单击**提交**。 你看到错误消息。

    ![显示显示的错误消息，是否用户输入未通过验证的屏幕截图。](4-working-with-forms/_static/image3.jpg)
4. 将字符串 (例如，"ABC") 添加到**员工计数**字段，然后单击**提交**试。 你看到错误，该值指示此时间字符串未采用正确的格式，即一个整数。

    ![显示用户是否输入字符串为员工字段显示的错误消息的屏幕截图。](4-working-with-forms/_static/image4.jpg)

ASP.NET Web 页提供了更多用于验证用户输入，包括能够自动执行验证使用客户端脚本，以便用户在浏览器中获取的即时反馈选项。 请参阅[其他资源](#Additional_Resources)更高版本的详细信息的部分。

## <a name="restoring-form-values-after-postbacks"></a>在回发后还原窗体值

在上一节中测试页面，你可能已注意到，如果有验证错误，输入 （而不仅仅是无效的数据） 的所有内容已消失，并且你必须重新输入的所有字段的值。 这说明了重要的一点： 提交页面，其进行处理，并随后再次呈现页时都会从头重新创建此页面。 如你所看到的这意味着时提交页中的所有值都都会丢失。

你可以解决此问题，但是。 您有权访问已提交的值 (在`Request.Form`对象，因此你可以返回到窗体的字段中填充这些值，当呈现页面。

1. 在*Form.cshtml*文件中，将`value`属性`<input>`元素使用`value`属性。: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    `value`属性`<input>`元素已设置为动态读取字段值外的`Request.Form`对象。 第一次请求页时中的值`Request.Form`对象均是空。 这没什么关系，因为这样的形式是空白。
2. 启动你的浏览器中，填写窗体字段或将其留空，然后单击**提交**。 将显示页面，其中显示已提交的值。

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源

- [从 Web 用户获取输入另外 1001 方法](https://msdn.microsoft.com/library/ms971057.aspx)
- [使用窗体和处理用户输入](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [在 ASP.NET 网站中验证用户输入](https://go.microsoft.com/fwlink/?LinkId=253002)
- [在 HTML 窗体中使用自动完成](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
