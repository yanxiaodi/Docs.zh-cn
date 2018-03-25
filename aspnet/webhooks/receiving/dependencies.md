---
uid: webhooks/receiving/dependencies
title: "ASP.NET Webhook 接收方依赖项 |Microsoft 文档"
author: rick-anderson
description: "接收方依赖项，在 ASP.NET Webhook 的依赖关系注入。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>ASP.NET Webhook 接收方依赖项

Microsoft ASP.NET Webhook 设计为具有记住的依赖关系注入。 可以使用备用实现使用依赖关系注入引擎替换系统中的大多数依赖项。

请参阅[DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs)的接收方依赖项的列表。 如果已不注册的任何依赖项，则使用默认实现。 请参阅[ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs)有关的默认实现的列表。
