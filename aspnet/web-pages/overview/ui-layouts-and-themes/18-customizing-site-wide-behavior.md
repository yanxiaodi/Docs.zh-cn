---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: "自定义的 ASP.NET 网页 (Razor) 站点的站点范围行为 |Microsoft 文档"
author: tfitzmac
description: "本章介绍如何对整个网站或中的整个文件夹，而不是一页中进行设置。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: b1caa26a23517bd976addfefac89375ae965eb91
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>中的 ASP.NET Web 页 (Razor) 网站的自定义站点范围行为
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文介绍如何在 ASP.NET Web 页 (Razor) 网站中生成的页面的站点端设置。
> 
> 你将学习：
> 
> - 如何运行代码，可让你设置的站点中的所有页值 （全局值或帮助器设置）。
> - 如何运行的代码，可让你设置的文件夹中的所有页的值。
> - 如何运行代码之前和之后页面加载。
> - 如何将错误发送到中央错误页。
> - 如何将身份验证添加到文件夹中的所有页面。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 2
> - WebMatrix 3
> - ASP.NET Web 帮助程序库 （NuGet 包）
>   
> 
> 本教程还适用于 ASP.NET Web Pages 3，并且 Visual Studio 2013 （或 Visual Studio Express 2013 for Web），除非你能使用 ASP.NET Web 帮助程序库。


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>为 ASP.NET 网页添加网站启动代码

对于很多编写 ASP.NET 网页中的代码中，单个页面可以包含具有所需的该页面的所有代码。 例如，如果页发送电子邮件，则可以为该操作的所有代码放都入的一页中。 这可能包括代码以初始化用于发送电子邮件的设置 (即，SMTP 服务器) 和用于发送电子邮件消息。

但是，在某些情况下，你可能想要在站点上的任何页面运行之前运行某些代码。 这可用于设置可以使用在站点中的任意位置的值 (称为*全局值*。)例如，一些帮助器要求你提供如电子邮件设置或帐户密钥的值。 它可方便起见，可以将这些设置保留在全局值。

你可以执行此操作通过创建一个名为页 *\_AppStart.cshtml*站点的根目录中。 如果存在此页，它将运行在首次请求站点中的任何页面。 因此，它是运行代码以设置全局值的良好开端。 (因为 *\_AppStart.cshtml*具有下划线的前缀，即使用户直接请求 ASP.NET 不会将页面发送到浏览器。)

下图显示了 *\_AppStart.cshtml*页上工作。 当请求传入的页面，并且如果这是任何第一个请求页在站点中，ASP.NET 首先检查是否 *\_AppStart.cshtml*存在页。 如果是这样中的任何代码 *\_AppStart.cshtml*页上运行，并运行请求的页。

![[image]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>为你的网站设置全局值

1. 在 WebMatrix 网站的根文件夹中，创建名为的文件 *\_AppStart.cshtml*。 该文件必须在站点的根目录中。
2. 将现有内容替换为以下： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    此代码将值存储在`AppState`字典，会自动提供给站点中的所有页面。 请注意，  *\_AppStart.cshtml*文件在其中没有任何标记。 页将运行代码，然后重定向到最初请求的页面。

    > [!NOTE]
    > 将代码放入时要特别小心 *\_AppStart.cshtml*文件。 如果在代码中出现任何错误 *\_AppStart.cshtml*文件，该网站将不会启动。
3. 在根文件夹中，创建一个名为的新页*AppName.cshtml*。
4. 将替换为以下默认标记和代码： 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    此代码中提取的值从`AppState`中设置的对象 *\_AppStart.cshtml*页。
5. 运行*AppName.cshtml*在浏览器中的页。 (请确保页中选择**文件**工作区之前运行它。)该页面显示的全局值。 

    ![[image]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>帮助器的设置值

为很好的用途 *\_AppStart.cshtml*文件是为帮助程序，在你的站点使用时，必须初始化设置值。 典型的示例包括的电子邮件设置`WebMail`帮助器和的私有和公共密钥`ReCaptcha`帮助器。 在类似这样的情况下，你可以设置一次中的值 *\_AppStart.cshtml*然后它们正在已经为设置所有页在你的网站。

此过程演示如何设置`WebMail`设置全局。 (有关使用`WebMail`帮助器，请参阅[添加到 ASP.NET Web Pages 站点的电子邮件](../getting-started/11-adding-email-to-your-web-site.md)。)

1. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未添加它。
2. 如果你还没有 *\_AppStart.cshtml*文件，在网站的根文件夹中创建名为的文件 *\_AppStart.cshtml*。
3. 添加以下`WebMail`设置应用于 *\_AppStart.cshtml*文件： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    修改以下电子邮件的代码中的相关的设置：

    - 设置`your-SMTP-host`到可以访问 SMTP 服务器的名称。
    - 设置`your-user-name-here`到您的 SMTP 服务器帐户的用户名。
    - 设置`your-account-password`到您的 SMTP 服务器帐户的密码。
    - 设置`your-email-address-here`为您自己的电子邮件地址。 这是从发送消息的电子邮件地址。 (某些电子邮件提供商不允许你指定一个不同`From`地址，并将使用你的用户名称作为`From`地址。)

    有关 SMTP 设置的详细信息，请参阅[配置电子邮件设置](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings)文章中[从 ASP.NET Web 页 (Razor) 站点发送电子邮件](https://go.microsoft.com/fwlink/?LinkID=202899)和[问题发送电子邮件](https://go.microsoft.com/fwlink/?LinkId=253001#email)中[ASP.NET 网页 (Razor) 故障排除指南 》](https://go.microsoft.com/fwlink/?LinkId=253001)。
- 保存 *\_AppStart.cshtml*文件并将其关闭。
- 在网站的根文件夹中，创建名为的新页*TestEmail.cshtml*。
- 将现有内容替换为以下： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
- 运行*TestEmail.cshtml*在浏览器中的页。
- 填写字段来向自己发送一封电子邮件，然后单击**发送**。
- 检查你的电子邮件，以确保你已收到消息。

此示例的重要部分是，通常不更改的设置-喜欢的 SMTP 服务器和你的电子邮件凭据的名称-在中设置 *\_AppStart.cshtml*文件。 这样，无需它们再次设置每个页面中，发送电子邮件。 （尽管如果出于某种原因，你需要更改这些设置，你可以设置它们单独在页中。）在页中，只能设置通常每次发生变化，如收件人和电子邮件消息的正文的值。

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>运行代码之前和之后文件夹中的文件

就像可以使用 *\_AppStart.cshtml*站点中的页运行之前，请编写代码，你可以编写代码运行之前 （和之后） 运行的特定文件夹中的任意页。 这可设置的同一布局页面为一个文件夹中的所有页或检查在文件夹中运行一个页面之前登录用户之类的内容。

对于页尤其文件夹，你可以创建代码在名为的文件中 *\_PageStart.cshtml*。 下图显示了 *\_PageStart.cshtml*页上工作。 当请求的页面，ASP.NET 首先检查 *\_AppStart.cshtml*页上和运行的。 然后 ASP.NET 将检查是否存在 *\_PageStart.cshtml*页，然后如果是这样，当运行。 然后，它将运行请求的页。

内部 *\_PageStart.cshtml*页上，您可以在你想请求的页后，可以通过包括运行的处理过程中指定位置`RunPage`方法。 这样就可以在请求的页运行之前运行代码，然后再次在它后面。 如果不包含`RunPage`中的所有代码 *\_PageStart.cshtml*运行，然后请求的页自动运行。

![[image]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET 允许你创建的层次结构 *\_PageStart.cshtml*文件。 您可以放入 *\_PageStart.cshtml*文件根目录中的站点和任何子文件夹中。 请求页时，  *\_PageStart.cshtml*文件在最顶层级别 （距离最近的站点根目录中） 运行后, 跟 *\_PageStart.cshtml*下一步中的文件子文件夹，诸如此类之前请求到达包含请求的页的文件夹的子文件夹结构。 后所有适用 *\_PageStart.cshtml*文件已运行，请求的页将运行。

例如，你可能具有的以下组合 *\_PageStart.cshtml*文件和*Default.cshtml*文件：

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

当你运行*/myfolder/default.cshtml*，你将看到以下：

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>正在运行的初始化代码的文件夹中的所有页面

为很好的用途 *\_PageStart.cshtml*文件是初始化的单个文件夹中的所有文件相同的布局页。

1. 在根文件夹中，创建一个名为的新文件夹*InitPages*。
2. 在*InitPages*文件夹中你的网站，创建名为的文件 *\_PageStart.cshtml*和默认标记和代码替换为以下代码： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. 在网站的根目录，创建名为的文件夹*共享*。
4. 在*共享*文件夹中，创建名为的文件 *\_Layout1.cshtml*和默认标记和代码替换为以下代码： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. 在*InitPages*文件夹中，创建名为的文件*Content1.cshtml*和现有内容替换为以下代码： 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. 在*InitPages*文件夹中，创建名为的另一个文件*Content2.cshtml*和默认标记替换为以下代码： 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. 运行*Content1.cshtml*在浏览器。 

    ![[image]](18-customizing-site-wide-behavior/_static/image4.jpg)

    当*Content1.cshtml*页上运行，  *\_PageStart.cshtml*文件中设置`Layout`，并设置`PageData["MyBackground"]`为一种颜色。 在*Content1.cshtml*，应用的布局和颜色。
8. 显示*Content2.cshtml*在浏览器。 

    布局是相同的因为两个页面使用的相同的布局页和颜色在中初始化 *\_PageStart.cshtml*。

## <a name="using-pagestartcshtml-to-handle-errors"></a>使用\_PageStart.cshtml 来处理错误

另一个良好用于 *\_PageStart.cshtml*文件是创建处理的编程错误 （异常） 可能发生的任何一种方式*.cshtml*文件夹中的页。 此示例演示了一种方法执行此操作。

1. 在根文件夹中，创建名为的文件夹*InitCatch*。
2. 在*InitCatch*文件夹中你的网站，创建名为的文件 *\_PageStart.cshtml*和现有标记和代码替换为以下代码： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    在此代码中，则尝试通过调用显式运行请求的页`RunPage`中的方法`try`块。 如果在请求中出现的任何编程错误页上内的代码`catch`块将运行。 在这种情况下，代码将重定向到一个页面 (*Error.cshtml*)，并将传递作为 URL 的一部分中遇到错误的文件的名称。 （你将创建页很快。）
3. 在*InitCatch*文件夹中你的网站，创建名为的文件*Exception.cshtml*和现有标记和代码替换为以下代码： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    此示例的目的，你要在此页中执行的操作有意创建错误通过尝试打开不存在的数据库文件。
4. 在根文件夹中，创建名为的文件*Error.cshtml*和现有标记和代码替换为以下代码： 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    在此页中，表达式`@Request["source"]`获取值超出 URL 并将其显示。
5. 在工具栏中，单击**保存**。
6. 运行*Exception.cshtml*在浏览器。 

    ![[image]](18-customizing-site-wide-behavior/_static/image5.jpg)

    因为在发生错误*Exception.cshtml*、  *\_PageStart.cshtml*页面重定向到*Error.cshtml*文件，其中显示的消息。

    有关异常的详细信息，请参阅[ASP.NET 网页编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkID=251587)。

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>使用\_PageStart.cshtml 限制文件夹访问

你还可以使用 *\_PageStart.cshtml*文件来限制访问的文件夹中的所有文件。

1. 在 WebMatrix 中，创建新的网站使用**站点从模板**选项。
2. 从可用模板中，选择**入门站点**。
3. 在根文件夹中，创建名为的文件夹*AuthenticatedContent*。
4. 在*AuthenticatedContent*文件夹中，创建名为的文件 *\_PageStart.cshtml*和现有标记和代码替换为以下代码： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    代码将启动通过阻止从正在缓存的文件夹中的所有文件。 （这是必需的各种方案，例如不希望一个用户的缓存的页面，可用于下一个用户的公用计算机。）接下来的代码将确定是否用户已登录到该站点之前它们可以查看文件夹中的任何页面。 如果用户未登录，代码将重定向到登录页。 登录页可以将用户返回到如果包含一个名为的查询字符串值最初请求的页面`ReturnUrl`。
5. 创建新的页中*AuthenticatedContent*文件夹名为*Page.cshtml*。
6. 默认标记替换为以下代码：  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. 运行*Page.cshtml*在浏览器。 代码将你重定向到登录页。 你必须注册之前登录。 已注册和登录后，你可以导航到的页并查看其内容。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源

[编程使用 Razor 语法的 ASP.NET Web Pages 简介](https://go.microsoft.com/fwlink/?LinkID=251587)
