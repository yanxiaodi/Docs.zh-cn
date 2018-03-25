---
uid: webhooks/source
title: "ASP.NET Webhook 源代码和 NuGet 包 |Microsoft 文档"
author: rick-anderson
description: "指向 ASP.NET Webhook 源代码和 NuGet 包"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2018
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET Webhook 源代码和 NuGet 包

Microsoft ASP.NET Webhook 是模块的 Microsoft ASP.NET 系列的一部分，作为承载[GitHub 上的开放源项目](https://github.com/aspnet/WebHooks)。 这意味着我们接受的贡献，但请查看[贡献准则](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md)之前提交拉取请求。

你正在阅读此联机文档现在还作为承载[GitHub 上的开放源代码](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide)和还接受的贡献。

## <a name="nuget-packages"></a>NuGet 包

[NuGet 包](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks)分为三个部分：

* [常见](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common)： 发送者和接收者之间共享的常见包。

* [发件人](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom)： 一组支持发送给他人自己 Webhook 的包。 用于发送 Webhook 的功能所述更详细地[发送 Webhook](sending/index.md)。

* [接收方](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)： 一组支持其他设备接收 Webhook 的包。 用于接收 Webhook 的功能中的更多详细信息中所述[接收 Webhook](receiving/index.md)。
