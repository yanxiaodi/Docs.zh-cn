---
uid: webhooks/diagnostics/logging
title: "日志记录的 ASP.NET Webhook |Microsoft 文档"
author: rick-anderson
description: "如何在 ASP.NET Webhook 日志记录。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET Webhook 日志记录

Microsoft ASP.NET Webhook 作为报告事件和问题的一种方式使用日志记录。 默认情况下使用编写日志[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)它们可以托管使用[跟踪侦听器](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx)像任何其他日志流。

在部署 Web 应用程序作为 Azure Web 应用时，会自动选取的日志，以及可以与任何其他一起管理[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)日志记录。 有关详细信息，请参阅[启用 Azure App Service 中的 web 应用的诊断日志记录](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

此外，可以直接从获取日志中所述的 Visual Studio 内[对 Azure App Service 中使用 Visual Studio 中的 web 应用进行故障排除](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)。

## <a name="redirecting-logs"></a>重定向日志

而不是写入的日志传输到[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)，可以提供一个备用的日志记录实现，如记录到日志管理器直接[Log4Net](http://logging.apache.org/log4net/)和[NLog](http://nlog-project.org/). 只需提供的实现[ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs)和将其注册到所选的一个依赖关系注入引擎，并且将获取 Microsoft ASP.NET Webhook 选取。 请参阅[ASP.NET Web API 2 中的依赖关系注入](https://www.asp.net/web-api/overview/advanced/dependency-injection)有关详细信息。
