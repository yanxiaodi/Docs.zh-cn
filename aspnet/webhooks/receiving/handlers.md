---
uid: webhooks/receiving/handlers
title: "ASP.NET Webhook 处理程序 |Microsoft 文档"
author: rick-anderson
description: "如何处理 ASP.NET Webhook 请求。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 4cf5770a731ef77842eb29b0a66ee0aac5d85d85
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-webhooks-handlers"></a>ASP.NET Webhook 处理程序

一旦 Webhook 请求经过验证 WebHook 接收方，已准备好由用户代码处理。 这就是*处理程序*进入。 处理程序派生自[IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)接口，但通常使用[WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)而不是直接从接口派生的类。

WebHook 请求可由一个或多个处理程序处理。 处理程序调用顺序基于其各自*顺序*转从最低到高顺序其中简单的整数 （建议必须介于 1 和 100） 的属性：

![WebHook 处理程序订单属性关系图](_static/Handlers.png)

可以选择性地设置一个处理程序*响应*WebHookHandlerContext 这将导致在处理停止和响应发回的 WebHook 的 HTTP 响应上的属性。 在上面的示例中，因为它具有更高的阶比 B 和 B 设置响应，将不会调用处理程序 C。

设置响应是通常仅适用于 Webhook 响应可以在其中执行回原始的 API 的信息。 例如，这是带有 Slack Webhook 其中响应发回到 WebHook 来自何处通道的用例。 如果它们仅想要从该特定的接收方接收 Webhook，处理程序可以设置的接收方属性。 如果它们不能设置为所有这些调用的接收方。

响应的一个其他常见用途是使用*410 不再存在*响应以指示 WebHook 不再处于活动状态，并且应提交任何进一步的请求。

默认情况下将由所有 WebHook 接收方调用处理程序。 但是，如果*接收方*属性设置为处理程序的名称，则该处理程序将仅从该接收方收到 WebHook 请求。

## <a name="processing-a-webhook"></a>处理 WebHook

当调用处理程序时，它获取[WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs)包含有关 WebHook 请求的信息。 有从可用的数据，通常将 HTTP 请求正文，*数据*属性。

数据类型通常是 JSON 或 HTML 窗体数据，但可以根据需要强制转换为更具体的类型。 例如，由 ASP.NET Webhook 生成自定义 Webhook 可以转换为类型[CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) ，如下所示：

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a>排队的处理

如果在几秒内不生成响应，大多数 WebHook 发件人将重新发送 WebHook。 这意味着您的处理程序必须完成的处理顺序不以使它能够再次调用该时间范围内。

如果处理花费的时间长，或单独更好地处理则[WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)可以用于例如提交到队列，WebHook 请求[Azure 存储队列](https://msdn.microsoft.com/library/azure/dd179353.aspx)。

概述[WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)此处提供实现：

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
