---
uid: webhooks/receiving/receivers
title: "ASP.NET Webhook 接收方 |Microsoft 文档"
author: rick-anderson
description: "ASP.NET Webhook 接收方"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 8c42db4056dd7a6ef77c7bcbc0eca3b5bf7c87e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receivers"></a>ASP.NET Webhook 接收方

接收 Webhook 取决于发件人是谁。 有时，有注册 WebHook 验证实际上正在侦听的订阅服务器上的其他步骤。 某些 Webhook 提供 HTTP POST 请求仅包含对然后则需要单独检索的事件信息的引用推送到拉取模型。 通常的安全模型很多各不相同。

Microsoft ASP.NET Webhook 的用途是时间的，使其更简单且更一致，以将你的 API 而无需花费了大量了解如何处理 Webhook 任何特定变体。

WebHook 接收方负责接受和验证来自特定发件人的 Webhook。 WebHook 接收方可以支持任何数量的 Webhook，每个都有自己的配置。 例如，GitHub WebHook 接收方可以接受从任意数量的 GitHub 存储库的 Webhook。

## <a name="webhook-receiver-uris"></a>WebHook 接收方 Uri

通过安装 Microsoft ASP.NET Webhook，你可以接受从无限数量的服务的 WebHook 请求的常规 WebHook 控制器。 当请求到达时，它会选取的已安装用于处理特定的 WebHook 发件人的相应接收方。

此控制器的 URI 是向服务注册的 WebHook URI 和形式：

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

出于安全原因，许多 WebHook 接收方需要的 URI 是*https* URI 并在某些情况下，它还必须包含一个其他查询参数用于强制此要求仅预定的接收方可以将 Webhook 发送到上面的 URI.

 *<receiver>* 组件是接收方的名称，例如*github*或*slack*。

*{Id}*是用于标识特定的 WebHook 接收方配置的可选标识符。 这可以用于 N 的 Webhook 注册特定的接收方。 例如，以下三个 Uri 可以用于为三个独立的 Webhook 注册：

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>安装 WebHook 接收方

若要接收使用 Microsoft ASP.NET Webhook 的 Webhook，你首先安装为 WebHook 提供程序或你想要接收从 Webhook 的提供程序 Nuget 包。 Nuget 包名为[Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)其中的最后部分表示支持的服务。 例如

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub)提供用于从 GitHub 接收 Webhook 的支持和[Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom)用于接收 Webhook 生成的 ASP 提供支持。NET Webhook。

可以在初始状态下找到对 Dropbox、 GitHub、 MailChimp、 PayPal、 Pusher、 Salesforce、 Slack、 条带、 Trello，和 WordPress 的支持，但可以支持任意数量的其他提供程序。

## <a name="configuring-a-webhook-receiver"></a>配置 WebHook 接收方

通过配置的 WebHook 接收方[IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface 特定实现该接口的来注册和使用任何依赖关系注入模型。 默认实现使用应用程序设置，它可设置在 Web.config 文件中，或者，如果使用 Azure Web 应用程序，可通过设置[Azure 门户](https://portal.azure.com/)。

![Azure 应用程序设置](_static/AzureAppSettings.png)

应用程序设置密钥的格式如下所示：

```
MS_WebHookReceiverSecret_<receiver>
```

值是匹配的值的以逗号分隔列表*{id}*值的 Webhook 已注册，例如：

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>初始化 WebHook 接收方

WebHook 接收方初始化通过注册它们，通常在*WebApiConfig*静态类，例如：

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
