---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "第 8 部分： 最终页、 异常处理和结论 |Microsoft 文档"
author: JoeStagner
description: "本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 8 部分将添加一个联系人页面，页面上和异常有关..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: fce1a20f9d1093b6c60542d8a786ddf54fdc922c
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>第 8 部分： 最终页、 异常处理和结论
====================
通过[Joe stagner 将](https://github.com/JoeStagner)

> Tailspin Spyworks 演示创建在.NET 平台的功能强大、 可扩展应用程序是如何非常简单。 它演示如何使用 ASP.NET 4 中出色的新功能构建在线商店，包括购物、 结帐和管理关闭。
> 
> 本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 8 部分添加联系人页，有关页上和异常处理。 这是序列结束。


## <a id="_Toc260221680"></a>联系人页 （从 ASP.NET 发送电子邮件）

创建一个名为 ContactUs.aspx 的新页

使用设计器，创建以下形式采用特殊的注释，以包括 ToolkitScriptManager 和从 AjaxdControlToolkit 编辑器控件。 .

![](tailspin-spyworks-part-8/_static/image1.jpg)

双击"提交"按钮以在代码隐藏文件中生成一个 click 事件处理程序和实现方法以作为电子邮件发送的联系信息。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

此代码要求你的 web.config 文件包含指定要用于发送邮件的 SMTP 服务器的配置节中的条目。

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>有关页面

创建一个名为 AboutUs.aspx 页并添加任何您喜欢的内容。

## <a id="_Toc260221682"></a>全局异常处理程序

最后，在整个应用程序中，我们具有引发异常并且没有出现未预见的情况下，冷还在我们的 web 应用程序中的原因的未经处理的异常。

我们永远不希望显示给网站访问者未经处理的异常。

![](tailspin-spyworks-part-8/_static/image2.jpg)

除了正在可怕的用户体验未经处理的异常也可以是安全问题。

若要解决此问题，我们将实现全局异常处理程序。

若要执行此操作，打开 Global.asax 文件，并请注意以下预生成的事件处理程序。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

添加代码以实现应用程序\_，如下所示的错误处理程序。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

然后添加一个名为到解决方案的 Error.aspx 页，并添加此标记代码段。

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

现在，在页\_从请求对象加载事件处理程序提取错误消息。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>结论

我们已了解，ASP.NET WebForms 便于对来创建复杂的网站和数据库访问，成员身份，AJAX 等。 非常快速。

希望本教程已授予所需若要开始构建应用程序的你自己 ASP.NET WebForms 工具 ！

>[!div class="step-by-step"]
[上一篇](tailspin-spyworks-part-7.md)
