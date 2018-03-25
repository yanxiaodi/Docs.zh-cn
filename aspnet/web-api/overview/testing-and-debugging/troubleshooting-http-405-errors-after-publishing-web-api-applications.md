---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: "故障排除 HTTP 405 错误后发布 Web API 2 应用程序 |Microsoft 文档"
author: rmcmurray
description: "本教程介绍如何在发布到生产 web 服务器的 Web API 应用程序后解决 HTTP 405 错误。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: b87ae7420e1295030e90c30e97b1e331413ce263
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2017
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>故障排除 HTTP 405 错误后发布 Web API 2 应用程序
====================
通过[Robert McMurray](https://github.com/rmcmurray)

> 本教程介绍如何在发布到生产 web 服务器的 Web API 应用程序后解决 HTTP 405 错误。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Internet 信息服务 (IIS)](https://www.iis.net/) （7 或更高版本）
> - [Web API](../../index.md) （版本 1 或 2）


Web API 应用程序通常使用多个常见的 HTTP 谓词： GET、 POST、 PUT、 DELETE，有时修补程序。 就是说，开发人员可能会碰到情况下这些谓词由另一个工作正常，Visual Studio 中或开发服务器上的 Web API 控制器将返回其中的情况下会导致其生产服务器上的 IIS 模块HTTP 405 错误时将其部署到生产服务器。 幸运的是此问题可以轻松地得到解决，但解析保证，在发生此问题的原因的说明。

## <a name="what-causes-http-405-errors"></a>什么原因 HTTP 405 错误

学习如何问题 HTTP 405 错误的第一步是了解 HTTP 405 错误实际意味着什么。 HTTP 是主控制文档[RFC 2616](http://www.ietf.org/rfc/rfc2616.txt)，后者定义了 HTTP 405 状态代码作为***不允许的方法***，并进一步说明情况下的此状态代码其中&quot;方法指定在请求 URI 标识的资源不允许请求的行。&quot;换而言之，HTTP 客户端已请求的特定 URL 的情况下不允许的 HTTP 谓词。

作为简要评审，以下是几种最常用的 HTTP 方法在 RFC 2616、 RFC 4918 和 RFC 5789 中定义：

| HTTP 方法 | 描述 |
| --- | --- |
| **获取** | 此方法用于从和检索数据的 URI，它可能最常用的 HTTP 方法。 |
| **HEAD** | 此方法具有非常类似于 GET 方法中，只不过它实际不于请求 URI 中检索数据-它只需检索 HTTP 状态。 |
| **发布** | 此方法通常用于将新数据发送到 URI;POST 通常用于提交窗体数据。 |
| **PUT** | 此方法通常用于将原始数据发送到 URI;PUT 通常用于提交到 Web API 应用程序的 JSON 或 XML 数据。 |
| **删除** | 此方法用于从 URI 中删除数据。 |
| **选项** | 此方法通常用于检索所支持的 uri 的 HTTP 方法的列表。 |
| **复制移动** | 这两种方法用于 WebDAV，和它们的目的是一目了然。 |
| **MKCOL** | 使用 WebDAV，使用此方法，它用于在指定 URI 处创建集合 （例如一个目录）。 |
| **PROPFIND PROPPATCH** | 这两种方法用于 WebDAV，并不用于查询或设置一个 URI 的属性。 |
| **锁定取消锁定** | 这两种方法用于 WebDAV，并且它们用于锁定/取消锁定创作时，请求 URI 标识的资源。 |
| **修补程序** | 此方法用于修改现有的 HTTP 资源。 |

如果这些 HTTP 方法之一配置为使用在服务器上，服务器将响应的 HTTP 状态和其他适用于请求的数据。 (例如，GET 方法可能会收到 HTTP 200***确定***响应和 PUT 方法可能会收到 HTTP 201***创建***响应。)

如果没有为服务器上使用配置的 HTTP 方法，服务器将响应附带 HTTP 501***未实现***错误。

但是，如果为在服务器上，使用配置了 HTTP 方法，但已禁用为给定的 URI，则服务器将响应附带 HTTP 405***不允许的方法***错误。

## <a name="example-http-405-error"></a>示例 HTTP 405 错误

以下示例 HTTP 请求和响应说明了其中 HTTP 客户端试图将值放到在 web 服务器上，Web API 应用程序和服务器将返回 HTTP 错误状态 PUT 方法不是允许的情况下：


HTTP 请求：


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


HTTP 响应：


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


在此示例中，HTTP 客户端向在 web 服务器上，Web API 应用程序的 URL 发送了有效的 JSON 请求，但服务器返回 HTTP 405 错误消息，从而指示 PUT 方法不允许的 url。 与此相反，如果请求 URI 与 Web API 应用程序的路由不匹配，则服务器将返回 HTTP 404***找不到***错误。

## <a name="resolving-http-405-errors"></a>解决 HTTP 405 错误

原因有多种原因可能不允许特定的 HTTP 谓词，但没有前导导致此错误在 IIS 中的一个主要方案： 多个处理程序由定义相同的谓词/方法，并且一个处理程序正在阻止从预期的处理程序处理请求。 通过说明，IIS 处理从第一个上次根据 applicationHost.config 和 web.config 文件，其中的路径、 谓词、 资源等的第一个匹配组合将用于处理该请求中的顺序处理程序条目的处理程序。

下面的示例是使用 PUT 方法将数据提交到 Web API 应用程序时，已返回 HTTP 405 错误的 IIS 服务器的 applicationHost.config 文件的摘录。 在此摘录中，定义了多个 HTTP 处理程序，和每个处理程序具有一组不同的有关在配置的 HTTP 方法-在列表中的最后一项是静态内容处理程序，这是其他处理程序过程中获得了 chanc 后将使用的默认处理e，以检查请求：

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

在上面的示例中，WebDAV 处理程序和扩展名的 URL 处理程序 （这用于 Web API） 的 ASP.NET 清楚地定义用于不同的 HTTP 方法的列表。 尽管此配置不一定会导致错误，请注意，对于所有的 HTTP 方法，配置 ISAPI DLL 处理程序。 但是，配置设置，例如 HTTP 405 错误故障排除时，考虑此需要。

在上面的示例中，ISAPI DLL 处理程序不是问题的原因。事实上，在 IIS 服务器的 applicationHost.config 文件中未定义问题-问题由在 Visual Studio 中创建 Web API 应用程序时在 web.config 文件所做的条目。 从应用程序的 web.config 文件的以下摘录部分显示问题的位置：

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

在此摘录中，ASP.NET 扩展名的 URL 处理程序重新定义，以包括其他将与 Web API 应用程序使用的 HTTP 方法。 但是，由于 WebDAV 处理程序为定义了一组类似的 HTTP 方法，将发生冲突。 在此特定情况下，定义和加载的 IIS，即使为包括 Web API 应用程序的网站禁用 WebDAV 属性 WebDAV 处理程序。 在处理期间的 HTTP PUT 请求，IIS 会调用 WebDAV 模块由于它定义为 PUT 谓词。 当调用 WebDAV 模块时，它将检查其配置并看到表示它将被禁用，因此它将返回 HTTP 405***不允许的方法***错误有关的任何请求都类似于 WebDAV 请求。 若要解决此问题，应从在其中定义 Web API 应用程序的网站的 HTTP 模块的列表中删除 WebDAV。 下面的示例演示了，可能会看到如下：

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

应用程序从开发环境发布到生产环境中，并出现这种情况是因为处理程序中的模块的列表是你的开发和生产环境之间的差异之后，经常会遇到这种情况。 例如，如果你使用 Visual Studio 2012 或 2013年开发一个 Web API 应用程序，IIS Express 8 是用于测试的默认 web 服务器。 此开发 web 服务器是某个服务器产品中, 附带的完整 IIS 功能按比例缩小版本，并且此开发 web 服务器包含几个开发方案已添加的更改。 例如，WebDAV 模块通常安装在运行 IIS 的完整版本的生产 web 服务器上尽管它可能不会在实际应用中。 开发版本的 IIS，(IIS Express)，安装 WebDAV 模块，但 WebDAV 模块的项有意注释掉，因此 WebDAV 模块在 IIS Express 上永远不会进行加载，除非您专门更改你 IIS Express 的配置若要添加到 IIS Express 安装 WebDAV 功能的设置。 因此，web 应用程序可能在开发计算机上正常工作，但在发布到生产 web 服务器的 Web API 应用程序时，你可能会遇到 HTTP 405 错误。

## <a name="summary"></a>摘要

HTTP 405 错误，会导致 HTTP 方法不允许 web 服务器请求的 URL。 当特定的处理程序已定义的指定的动词，并且该处理程序重写您希望处理该请求的处理程序，则会通常会出现这种情况。

如果你遇到的情况下，其中你收到 HTTP 501 错误消息，这意味着在服务器上未实现特定的功能，这通常意味着没有在您的 IIS 设置中定义相匹配的 HTTP 请求，没有处理程序的可能指示内容未正确安装在系统上，或者内容已修改您的 IIS 设置，因此没有没有处理程序定义该支持特定的 HTTP 方法。 若要解决该问题，你将需要重新安装任何应用程序尝试使用它可以对其具有没有相应的模块或处理程序定义的 HTTP 方法。
