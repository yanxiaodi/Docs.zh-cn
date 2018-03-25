---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: "使用验证码以使用 ASP.NET Web Razor 防止机器人） 的站点 |Microsoft 文档"
author: microsoft
description: "此文章介绍了如何使用 ReCaptcha （一种安全措施） 来防止自动的程序 （机器人） 执行任务中的 ASP.NET Web Pages (Razor) 我们..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>使用验证码以使用 ASP.NET Web Razor 防止机器人） 站点
====================
通过[Microsoft](https://github.com/microsoft)

> 此文章介绍了如何使用 ReCaptcha （一种安全措施） 来防止自动的程序 （机器人） 在 ASP.NET Web 页 (Razor) 网站中执行任务。
> 
> **你将学习：** 
> 
> - 如何将 CAPTCHA 测试添加到你的站点。
> 
> 这些是文章中引入的 ASP.NET 功能：
> 
> - `ReCaptcha`帮助器。
> 
> > [!NOTE]
> > 这篇文章中的信息适用于 ASP.NET Web Pages 1.0 和 Web Pages 2。


## <a name="about-captchas"></a>有关 CAPTCHAs

在你的站点，或即使只是使用户可以注册任何时间输入名称和 URL （如博客注释时），你可能会出现大量的假名称。 这些通常会尝试将 Url 保留在用户可在找到每个网站的自动程序 （机器人） 保留。 （常见动机是 post 的产品销售的 Url。）

你可以帮助确保用户是真正的用户而不是计算机程序，通过使用*CAPTCHA*来验证用户在注册或否则输入其名称和站点时。 CAPTCHA 代表完全自动公共执行测试来告诉计算机和理解分离。 验证码是*质询-响应*便于人员完成但硬的自动程序执行的测试要求用户执行某些操作。 最常用的 CAPTCHA 类型是一个位置，你可以看到一些失真的号和需要键入它们。 （扭曲应该以使其难以机器人才能破译字母。）

## <a name="adding-a-recaptcha-test"></a>添加 ReCaptcha 测试

在 ASP.NET 页中，你可以使用`ReCaptcha`用于呈现基于 ReCaptcha 服务的 CAPTCHA 测试帮助 ([http://recaptcha.net](http://recaptcha.net))。 `ReCaptcha`帮助程序显示用户必须之前验证页，请输入正确的两个失真单词的图像。 用户响应 ReCaptcha.Net 服务通过验证。

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. 注册你的网站在 ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net))。 你已完成注册，你会获得一个公钥和私钥。
2. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未。
3. 如果你还没有 *\_AppStart.cshtml*文件，在网站的根文件夹中创建名为的文件 *\_AppStart.cshtml*。
4. 添加以下`Recaptcha`中的帮助器设置 *\_AppStart.cshtml*文件： 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. 设置`PublicKey`和`PrivateKey`属性使用你自己的公共和私有密钥。
6. 保存 *\_AppStart.cshtml*文件并将其关闭。
7. 在网站的根文件夹中，创建名为的新页*Recaptcha.cshtml*。
8. 将现有内容替换为以下： 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. 运行*Recaptcha.cshtml*在浏览器中的页。 如果`PrivateKey`值是否有效，该页显示 ReCaptcha 控件和一个按钮。 如果全局在尚未设置密钥 *\_AppStart.html*，页面将显示错误。 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. 测试输入的单词。 如果您传入 ReCaptcha 测试，你将看到一条消息针对此效果。 否则，你看到一条错误消息和 ReCaptcha 控件重新显示。

> [!NOTE]
> 如果你的计算机处于使用代理服务器的域，你可能需要配置`defaultproxy`元素*Web.config*文件。 下面的示例演示*Web.config*文件`defaultproxy`元素配置为启用 ReCaptcha 服务正常工作。
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源


- [中的 ASP.NET Web 页网站的自定义站点范围行为](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha 站点](https://www.google.com/recaptcha)
