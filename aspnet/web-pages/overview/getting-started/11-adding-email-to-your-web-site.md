---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: "发送电子邮件从 ASP.NET Web 页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "本章介绍如何从网站发送自动的电子邮件。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: c5878c3bc468daef050dcebee99f64441066409a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>从 ASP.NET Web 页 (Razor) 站点发送电子邮件
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何使用 ASP.NET Web 页 (Razor) 时，从网站发送电子邮件。
> 
> 你将学习：
> 
> - 如何从您的网站发送电子邮件。
> - 如何将文件附加到电子邮件。
> 
> 这是文章中引入的 ASP.NET 功能：
> 
> - `WebMail`帮助器。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>从你的网站发送电子邮件消息

有各种原因为什么你可能需要从你的网站发送电子邮件。 可能将确认消息发送给用户，或你可能向自己发送通知 （例如情况下，新的用户已注册。）`WebMail`帮助程序轻松用于发送电子邮件。

若要使用`WebMail`帮助器，你必须有权访问 SMTP 服务器。 (代表 SMTP*简单邮件传输协议*。)SMTP 服务器是一个电子邮件服务器，仅将消息转发给接收方的服务器和 #8212;它是电子邮件的出站端。 如果为你的网站使用托管提供商，它们可能你使用设置电子邮件，它们可以告诉你你 SMTP 服务器名称是什么。 如果你正在公司网络内部，管理员或 IT 部门可通常提供有关你可以使用的 SMTP 服务器的信息。 如果你在在家工作，即使可能能够使用你普通的电子邮件提供商联系，可以告诉他们 SMTP 服务器的名称进行测试。 你通常需要：

- SMTP 服务器的名称。
- 端口号。 这几乎始终为 25。 但是，你的 ISP 可能要求你为使用端口 587。 如果将安全套接字层 (SSL) 用于电子邮件，你可能需要不同的端口。 咨询你的电子邮件提供商。
- 凭据 （用户名、 密码）。

在此过程中，你将创建两个页。 第一页具有一个窗体，可让用户输入的描述，就像它们已填写技术支持窗体。 第一页提交到第二页其信息。 在第二个页中，代码中提取用户的信息，并发送电子邮件。 它还显示确认已接收的问题报告的消息。

![[image]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> 为了简化本示例，此代码初始化`WebMail`帮助器直接在其中使用的页。 但是，对于实际网站，很好地了解如下的初始化代码放在全局文件中，以便你初始化`WebMail`中你的网站的所有文件的帮助器。 有关详细信息，请参阅[自定义站点范围的行为的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers)。


1. 创建新网站。
2. 添加一个名为的新页*EmailRequest.cshtml*并添加以下标记： 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    请注意， `action` form 元素的属性已设置为*ProcessRequest.cshtml*。 这意味着窗体，将提交到该页面而不是返回当前页。
3. 添加一个名为的新页*ProcessRequest.cshtml*到网站并添加下面的代码和标记：   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    在代码中，你将获取已提交到该页的窗体字段的值。 然后，你调用`WebMail`帮助器的`Send`方法创建并发送电子邮件。 在这种情况下，要使用的值组成的串联从窗体提交的值的文本。

    此页的代码位于`try/catch`块。 如果出于任何原因将发送一封电子邮件不起作用 （例如，设置不正确） 中的代码`catch`块运行并设置`errorMessage`变量已发生的错误。 (有关详细信息`try/catch`块或`<text>`标记中，请参阅[ASP.NET 网页编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors)。)

    在正文中页上，如果`errorMessage`变量为空 （默认值），则用户将看到一条消息，发送电子邮件消息。 如果`errorMessage`变量设置为 true，用户所见一条消息已发送消息的问题。

    请注意，在显示一条错误消息的页的部分，没有其他的测试： `if(debuggingFlag)`。 这是一个变量，可以设置为 true，如果你无法发送电子邮件。 当`debuggingFlag`为 true，并且如果发送电子邮件问题，一条错误消息将显示表明 ASP.NET 尝试发送电子邮件时已报告任何内容。 公平警告，但： 它不能发送电子邮件时，ASP.NET 将报告错误消息可以是泛型。 例如，如果 ASP.NET 无法联系 SMTP 服务器 （例如，因为你的服务器名称中所做错误），错误为`Failure sending mail`。

    > [!NOTE] 
    > 
    > **重要**当从异常对象中获取一条错误消息 (`ex`在代码中)，请执行*不*定期将该消息通过传递给用户。 异常对象通常包含的信息的用户看不到，这甚至可能出现安全漏洞。 这就是为什么此代码包含具有变量`debuggingFlag`，使用作为开关以显示错误消息和为什么默认情况下的变量设置为 false。 应设置为 true （，因此显示错误消息） 该变量*仅*如果遇到问题发送电子邮件，并且需要进行调试。 已修复任何问题之后, 设置`debuggingFlag`回 false。

    修改以下电子邮件的代码中的相关的设置：

    - 设置`your-SMTP-host`到可以访问 SMTP 服务器的名称。
    - 设置`your-user-name-here`到您的 SMTP 服务器帐户的用户名。
    - 设置`your-account-password`到您的 SMTP 服务器帐户的密码。
    - 设置`your-email-address-here`为您自己的电子邮件地址。 这是从发送消息的电子邮件地址。 (某些电子邮件提供商不允许你指定一个不同`From`地址，并将使用你的用户名称作为`From`地址。)

    > [!TIP] 
    > 
    > <a id="configuring_email_settings"></a>
    > ### <a name="configuring-email-settings"></a>配置电子邮件设置
    > 
    > 它可以是一个难题，有时以确保具有正确的 SMTP 服务器端口号和等的设置。 下面是一些提示：
    > 
    > - SMTP 服务器名称通常是类似`smtp.provider.com`或`smtp.provider.net`。 但是，如果你将你的站点发布到托管提供商，SMTP 服务器名称在该点可能是`localhost`。 这是因为已发布和提供程序的服务器上运行你的站点后，电子邮件服务器可能还会从你的应用程序的角度本地。 这一更改服务器名称可能意味着你必须更改发布过程的一部分的 SMTP 服务器名称。
    > - 端口号通常为 25。 但是，某些访问接口要求你为使用端口 587 或某些其他端口。
    > - 请确保使用正确的凭据。 如果你已将站点发布到宿主提供程序，使用提供程序专门指示适用于电子邮件的凭据。 这些可能不同于用来发布的凭据。
    > - 有时则根本不需要凭据。 如果您要发送电子邮件使用您的个人 ISP，电子邮件提供商可能已经知道你的凭据。 在发布后，你可能需要使用比测试本地计算机上的不同凭据。
    > - 如果你的电子邮件提供商使用加密，则必须设置`WebMail.EnableSsl`到`true`。
4. 运行*EmailRequest.cshtml*在浏览器中的页。 (请确保页中选择**文件**工作区之前运行它。)
5. 输入您的姓名和问题的说明中，，然后单击**提交**按钮。 要重定向到*ProcessRequest.cshtml*页，其中确认你的消息并向你发送电子邮件。 

    ![[image]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>使用电子邮件的文件发送

你还可以发送附加到电子邮件的文件。 在此过程中，你可以创建文本文件和两个 HTML 页。 你将使用电子邮件附件的文本文件。

1. 在网站中，添加新的文本文件并将其命名*MyFile.txt*。
2. 复制以下文本并将其粘贴在文件中： 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. 创建一个名为页*SendFile.cshtml*并添加以下标记： 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. 创建一个名为页*ProcessFile.cshtml*并添加以下标记： 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. 修改以下电子邮件的示例中的代码中的相关的设置：

    - 设置`your-SMTP-host`到可以访问 SMTP 服务器的名称。
    - 设置`your-user-name-here`到您的 SMTP 服务器帐户的用户名。
    - 设置`your-email-address-here`为您自己的电子邮件地址。 这是从发送消息的电子邮件地址。
    - 设置`your-account-password`到您的 SMTP 服务器帐户的密码。
    - 设置`target-email-address-here`为您自己的电子邮件地址。 （如之前，你通常会发送一封电子邮件给其他人，但对于测试，你可以将其发送给自己。）
6. 运行*SendFile.cshtml*在浏览器中的页。
7. 输入你的名称、 的主题行中，和要附加的文本文件的名称 (*MyFile.txt*)。
8. 单击 `Submit` 按钮。 如之前，要重定向到*ProcessFile.cshtml*页上，其中确认你的消息和其向你发送电子邮件与附加的文件。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源


- [ASP.NET 网页 (Razor) 疑难解答指南](https://go.microsoft.com/fwlink/?LinkId=253001)
- [简单邮件传输协议](https://msdn.microsoft.com/library/aa480435.aspx)
- [为 ASP.NET 网页自定义站点范围的行为](https://go.microsoft.com/fwlink/?LinkId=202906)
