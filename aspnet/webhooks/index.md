---
uid: webhooks/index
title: "ASP.NET Webhook 概述 |Microsoft 文档"
author: rick-anderson
description: "ASP.NET Webhook 简介。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-overview"></a>ASP.NET Webhook 概述

Webhook 是一种轻型的 HTTP 模式，为连接在一起的 Web Api 和 SaaS 服务提供简单的发布/订阅模型。 时事件发生在服务中，通知形式的 HTTP POST 请求发送到已注册的订阅服务器。 POST 请求包含有关事件这样就可以接收方可以采取相应行动的信息。

由于其简单起见，Webhook 已公开的大量服务包括[Dropbox](http://dropbox.com/)， [GitHub](http://www.github.com/)， [Bitbucket](https://bitbucket.org/)， [MailChimp](http://www.mailchimp.com/)， [PayPal](http://www.paypal.com/)， [Slack](http://www.slack.com)，[条带](http://www.stripe.com)， [Trello](http://www.trello.com/)，以及更多。 例如，WebHook 可以指示文件已在进行了更改[Dropbox](http://dropbox.com/)，或代码更改已提交在 GitHub 中，或在已启动付款[PayPal](http://www.paypal.com/)，或已在中创建了卡[Trello](http://www.trello.com/)。 可能的匹配项是无限的 ！

Microsoft ASP.NET Webhook 容易地同时发送和接收 Webhook ASP.NET 应用程序的一部分：

* 在接收端，它用于接收和处理从任意数量的 WebHook 提供程序的 Webhook 提供一个通用模型。 就支持现成[Dropbox](http://dropbox.com/)， [GitHub](http://www.github.com/)， [Bitbucket](https://bitbucket.org/)， [MailChimp](http://www.mailchimp.com/)， [PayPal](http://www.paypal.com/)， [pusher](http://www.pusher.com)， [Salesforce](http://www.salesforce.com)， [Slack](http://www.slack.com)，[条带](http://www.stripe.com)， [Trello](http://www.trello.com/)，[WordPress](http://www.wordpress.com)和[Zendesk](https://www.zendesk.com/)但很容易添加支持的详细信息。

* 在发送端为管理和存储用于将事件通知发送到正确的一组订阅服务器的订阅也提供支持。 这允许你定义自己的事件订阅服务器只能订阅和在操作发生时通知他们集。

两个部分可以根据你的方案拆分或一起使用。 如果你只需从其他服务接收 Webhook，则你可以使用只需的接收方部分;如果你只想要公开其他人的 Webhook 来使用，你可以做到这。

代码以 ASP.NET Web API 2 和 ASP.NET MVC 5 为目标，可用作[GitHub 上的 OSS](https://github.com/aspnet/WebHooks)。

## <a name="webhooks-overview"></a>Webhook 概述

Webhook 是一种模式，这意味着，可变及其使用方式从服务到服务，但基本要旨是相同。 可以将 Webhook 视为简单的发布/订阅模型其中用户可以在其他位置发生的事件订阅。 事件通知作为 HTTP POST 请求包含有关事件本身的信息会传播。

通常 HTTP POST 请求包含 JSON 对象或由 WebHook 发件人包括有关导致 WebHook 事件的信息来触发的 HTML 窗体数据。 例如，从 WebHook POST 请求正文的示例[GitHub](http://www.github.com/)如下所示由于特定的存储库中打开一个新问题：

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

若要确保 WebHook 确实是预期的发件人发，POST 请求被以某种方式保护，然后验证接收方。 例如， [GitHub Webhook](https://developer.github.com/webhooks/)包括*X 中心签名*与它使你不必担心由接收方实现检查请求正文的哈希的 HTTP 标头。

WebHook 流通常将类似如下内容：

* WebHook 发件人公开客户端可以订阅的事件。 事件描述可观察更改到系统，例如新的数据项已插入，完成后进程，或其他内容。

* WebHook 接收方订阅注册 WebHook 包含四件事：

     1. 针对事件通知应其中发布在 HTTP POST 请求; 形式的 URI

     2. 一组描述为其应激发 WebHook; 的特定事件的筛选器

     3. 用于对 HTTP POST 请求中; 签名密钥

     4. 这是要包含在 HTTP POST 请求中的其他数据。 例如，这可以是其他 HTTP 标头字段或在 HTTP POST 请求正文中包含的属性。

* 一旦事件发生，找到匹配的 WebHook 注册并提交 HTTP POST 请求。 通常情况下，HTTP POST 请求的生成是如果出于某种原因未响应收件人或 HTTP POST 请求将导致错误响应重试几次。

## <a name="webhooks-processing-pipeline"></a>Webhook 处理管道

传入的 Webhook 的 Microsoft ASP.NET Webhook 处理管道类似如下所示：

![ASP.NET Webhook 处理管道](_static/WebHookReceivers.png)

这两个关键概念此处*接收方*和*处理程序*:

* *接收方*负责处理来自给定发件人的 WebHook 的特定风格和强制实施安全性检查，以确保确实是预期的发件人发 WebHook 请求。

* *处理程序*通常是用户代码运行处理特定的 WebHook。

在以下节点中更多详细信息描述了这些概念。
