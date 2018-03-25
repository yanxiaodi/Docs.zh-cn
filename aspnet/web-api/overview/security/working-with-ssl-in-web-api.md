---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: "使用 Web API 中的 SSL |Microsoft 文档"
author: MikeWasson
description: "演示如何使用 SSL 与 ASP.NET Web API，包括使用 SSL 客户端证书。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 127b336cb628e55bd59481ecb1c4df83960dc25b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="working-with-ssl-in-web-api"></a>使用 Web API 中的 SSL
====================
通过[Mike Wasson](https://github.com/MikeWasson)

通过一般 HTTP 几种常见的身份验证方案不安全。 具体而言，基本身份验证和窗体身份验证发送未加密的凭据。 若要是安全的这些身份验证方案*必须*使用 SSL。 此外，可以使用 SSL 客户端证书进行身份验证客户端。

## <a name="enabling-ssl-on-the-server"></a>启用服务器上的 SSL

若要配置 IIS 7 或更高版本的 SSL 设置：

- 创建或获取证书。 用于测试，你可以创建自签名的证书。
- 将添加 HTTPS 绑定。

有关详细信息，请参阅[如何设置 SSL 在 IIS 7 上](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)。

对于本地测试，您可以启用在 IIS Express 从 Visual Studio 中的 SSL。 在属性窗口中，设置**启用 SSL**到**True**。 记下的值**SSL URL**; 使用此 URL 用于测试 HTTPS 连接。

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>强制实施 SSL Web API 控制器中

如果你有 HTTPS 和 HTTP 绑定，客户端仍可以使用 HTTP 访问站点。 你可能会使某些资源，可通过 HTTP，而其他资源需要 SSL。 在这种情况下，使用操作筛选器要求将 SSL 用于受保护的资源。 下面的代码演示检查 SSL 的 Web API 身份验证筛选器：

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

将此筛选器添加到要求使用 SSL 的任何 Web API 操作：

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>SSL 客户端证书

SSL 通过使用公钥基础结构证书提供身份验证。 服务器必须提供客户端到服务器进行身份验证的证书。 较不常用于客户端提供一个证书到服务器，但这是用于身份验证客户端的一个选项。 若要使用 SSL 客户端证书，你需要分配到你的用户的签名的证书的方法。 对于许多应用程序类型，这不会获得良好的用户体验，但在某些环境中 （例如，企业），它可能是一种可行。

| 优点 | 缺点 |
| --- | --- |
| -证书凭据都比用户名/密码更强。 -SSL 提供了完整的安全通道，使用身份验证、 消息完整性和消息加密。 | -你必须获取并管理 PKI 证书。 -客户端平台必须支持 SSL 客户端证书。 |

若要配置 IIS 以接受客户端证书，打开 IIS 管理器，然后执行以下步骤：

1. 单击树视图中的站点节点。
2. 双击**SSL 设置**在中间窗格中的功能。
3. 下**客户端证书**，选择以下选项之一： 

    - **接受**: IIS 将接受来自客户端的证书，但不要求。
    - **需要**： 需要客户端证书。 （若要启用此选项，你还必须选择"要求 SSL"）

你也可以在 ApplicationHost.config 文件中设置这些选项：

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

**SslNegotiateCert** IIS 将接受来自客户端的证书，但不需要进行一次 （等效于"接受"选项在 IIS 管理器） 的标记表示。 如果需要证书，请设置**SslRequireCert**标志。 用于测试，你还可以设置这些选项在 IIS Express 中，在本地 applicationhost。位于"Documents\IISExpress\config"中的配置文件。

### <a name="creating-a-client-certificate-for-testing"></a>创建客户端证书以便进行测试

出于测试目的，你可以使用[MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx)创建客户端证书。 首先，创建测试根颁发机构：

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert 将提示你输入私钥的密码。

接下来，将证书添加到测试服务器的"受信任根证书颁发机构"存储，，如下所示：

1. 打开 MMC。
2. 下**文件**，选择**添加/删除管理单元中**。
3. 选择**计算机帐户**。
4. 选择**本地计算机**并完成向导。
5. 在导航窗格中，展开"受信任根证书颁发机构"节点。
6. 上**操作**菜单上，指向**所有任务**，然后单击**导入**以启动证书导入向导。
7. 浏览到证书文件，TempCA.cer。
8. 单击**打开**，然后单击**下一步**并完成向导。 （你将会提示您重新输入密码。）

现在创建的第一个证书由签名的客户端证书：

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>在 Web API 中使用客户端证书

在服务器端中，可以获取客户端证书，通过调用[GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx)请求消息。 该方法返回没有客户端证书的情况下为 null。 否则，它将返回**X509Certificate2**实例。 使用此对象获取的证书，如签发者和使用者中的信息。 然后你可以使用此信息用于身份验证和/或授权。

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
