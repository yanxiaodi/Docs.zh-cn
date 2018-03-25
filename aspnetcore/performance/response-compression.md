---
title: "有关 ASP.NET 核心响应压缩中间件"
author: guardrex
description: "了解如何响应压缩以及如何在 ASP.NET Core 应用中使用响应压缩中间件。"
manager: wpickett
ms.author: riande
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: d05256af4e62834b8d43689786a7b8bb3a5e58fb
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="response-compression-middleware-for-aspnet-core"></a>有关 ASP.NET 核心响应压缩中间件

作者：[Luke Latham](https://github.com/guardrex)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

网络带宽是有限的资源。 通常减小响应的大小将通常显著增加应用的响应能力。 减小负载大小的一种方法是压缩应用的响应。

## <a name="when-to-use-response-compression-middleware"></a>何时使用响应压缩中间件
使用在 IIS、 Apache 或 Nginx 基于服务器的响应压缩技术。 中间件的性能可能不会与匹配服务器模块。 [HTTP.sys 服务器](xref:fundamentals/servers/httpsys)和[Kestrel](xref:fundamentals/servers/kestrel)当前不提供内置的压缩支持。

当你时，请使用响应压缩中间件：

* 无法使用以下基于服务器的压缩技术：
  * [IIS 动态压缩模块](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Apache mod_deflate 模块](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Nginx 压缩和解压缩](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* 直接在上托管：
  * [HTTP.sys 服务器](xref:fundamentals/servers/httpsys)(以前称为[WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>响应压缩
通常情况下，本身不压缩任何响应可从响应压缩中受益。 通常本身不压缩的响应包括： CSS、 JavaScript、 HTML、 XML 和 JSON。 本机压缩的资产，如 PNG 文件，你不应将其压缩。 如果你尝试进一步压缩本机压缩的响应，任何小的其他缩短的大小和传输的时间将可能屏蔽按处理压缩所花费的时间。 不压缩小于大约 150-1000年字节 （具体取决于该文件的内容和压缩的效率） 的文件。 压缩的小文件的开销可能会产生大于未压缩的文件的压缩的文件。

客户端时客户端可以处理压缩的内容，必须通过发送通知其功能的服务器`Accept-Encoding`与请求的标头。 当服务器发送压缩的内容时，它必须包括中的信息`Content-Encoding`如何编码压缩的响应标头。 支持的中间件的内容编码指定下表所示。

| `Accept-Encoding` 标头值 | 支持的中间件 | 描述                                                 |
| :-----------------------------: | :------------------: | ----------------------------------------------------------- |
| `br`                            | 否                   | Brotli 压缩的数据格式                               |
| `compress`                      | 否                   | UNIX"压缩"的数据格式                                 |
| `deflate`                       | 否                   | "deflate"内的"zlib"数据格式的压缩的数据     |
| `exi`                           | 否                   | W3C 高效 XML 交换                               |
| `gzip`                          | 是 （默认值）        | gzip 文件格式                                            |
| `identity`                      | 是                  | "没有编码"标识符： 响应必须进行编码。 |
| `pack200-gzip`                  | 否                   | Java 存档的网络传输格式                   |
| `*`                             | 是                  | 编码不显式请求的任何可用内容     |

有关详细信息，请参阅[IANA 官方内容编码列表](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。

该中间件，可添加额外的压缩提供程序自定义`Accept-Encoding`标头值。 有关详细信息，请参阅[自定义提供程序](#custom-providers)下面。

该中间件是能够对的质量值作出反应 (qvalue， `q`) 权重时客户端发送为优先处理压缩方案。 有关详细信息，请参阅[RFC 7231: Accept-encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4)。

压缩算法受到压缩速度和压缩的效率之间的权衡。 *有效性*在此上下文中引用的输出的大小压缩后。 最小大小通过最*最佳*压缩。

参与请求的标头，发送、 缓存和接收压缩的内容是下表中所述。

| Header             | 角色 |
| ------------------ | ---- |
| `Accept-Encoding`  | 从客户端发送到服务器以指示编码方案可接受的客户端的内容。 |
| `Content-Encoding` | 从服务器发送到客户端以指示负载中的内容的编码。 |
| `Content-Length`   | 在进行压缩时会，`Content-Length`标头被删除，因为正文内容更改时压缩响应。 |
| `Content-MD5`      | 在进行压缩时会，`Content-MD5`移除标头，因为已更改的正文内容，且哈希将不再有效。 |
| `Content-Type`     | 指定内容的 MIME 的类型。 每个响应应指定其`Content-Type`。 该中间件检查此值以确定是否应压缩响应。 该中间件指定的一组[默认 MIME 类型](#mime-types)其可以进行编码，但您可以替换或添加 MIME 类型。 |
| `Vary`             | 由值为服务器发送时`Accept-Encoding`到客户端和代理，`Vary`标头指示的客户端或代理，它应缓存 （有所不同） 响应基于值`Accept-Encoding`的请求标头。 返回具有内容的结果`Vary: Accept-Encoding`标头是同时压缩和未压缩的响应将单独进行缓存。 |

你可以浏览的功能与的响应压缩中间件[示例应用程序](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples)。 此示例阐释了：
* 使用 gzip 和自定义压缩提供程序的应用程序响应的压缩。
* 如何将 MIME 类型添加到压缩的 MIME 类型的默认列表。

## <a name="package"></a>Package
若要在你的项目中包含该中间件，添加到引用[ `Microsoft.AspNetCore.ResponseCompression` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)包或使用[ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)包。 此功能适用于面向 ASP.NET Core 1.1 或更高版本的应用。

## <a name="configuration"></a>配置
下面的代码演示如何启用响应压缩中间件与默认 gzip 压缩和默认 MIME 类型。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](response-compression/samples/2.x/StartupBasic.cs?name=snippet1&highlight=4,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](response-compression/samples/1.x/StartupBasic.cs?name=snippet1&highlight=3,8)]

---

> [!NOTE]
> 使用之类的工具[Fiddler](http://www.telerik.com/fiddler)， [Firebug](http://getfirebug.com/)，或[Postman](https://www.getpostman.com/)设置`Accept-Encoding`请求标头并研究响应标头、 大小和正文。

提交到示例应用程序而无需请求`Accept-Encoding`标头并观察响应是否未压缩。 `Content-Encoding`和`Vary`标头不在响应上存在。

![Fiddler 窗口中显示的无 Accept-encoding 标头的请求的结果。 不压缩响应。](response-compression/_static/request-uncompressed.png)

向示例应用程序与提交一个申请`Accept-Encoding: gzip`标头并观察压缩的响应。 `Content-Encoding`和`Vary`标头会显示在响应。

![Fiddler 窗口中显示的 Accept-encoding 标头的请求的结果和 gzip 的值。 变化和的 Content-encoding 标头添加到的响应中。 压缩响应。](response-compression/_static/request-compressed.png)

## <a name="providers"></a>提供程序
### <a name="gzipcompressionprovider"></a>GzipCompressionProvider
使用`GzipCompressionProvider`以压缩使用 gzip 响应。 如果未指定，这是默认压缩提供程序。 你可以设置压缩级别与`GzipCompressionProviderOptions`。 

Gzip 压缩提供程序默认为最快的压缩级别 (`CompressionLevel.Fastest`)，这可能不会产生最有效的压缩。 如果需要最有效的压缩，则你可以配置的中间件理想的压缩。

| 压缩级别                | 描述                                                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `CompressionLevel.Fastest`       | 即使未以最佳方式压缩生成的输出，应尽可能快地完成压缩。 |
| `CompressionLevel.NoCompression` | 应执行不进行压缩。                                                                           |
| `CompressionLevel.Optimal`       | 响应应以最佳方式压缩，即使压缩操作将使用更多时间才能完成。                |


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=3,8-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,10-13)]

---

## <a name="mime-types"></a>MIME 类型
该中间件指定一组默认的 MIME 类型为压缩：
* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

您可以替换或追加的响应压缩中间件选项的 MIME 类型。 请注意该通配符 MIME 类型，如`text/*`不受支持。 示例应用程序添加的 MIME 类型`image/svg+xml`和压缩，并提供横幅图像的 ASP.NET Core (*banner.svg*)。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7)]

---

### <a name="custom-providers"></a>自定义提供程序
你可以创建自定义压缩实现与`ICompressionProvider`。 `EncodingName`表示的内容编码此`ICompressionProvider`生成。 该中间件使用此信息来选择基于列表中指定的提供程序`Accept-Encoding`的请求标头。

使用示例应用程序，客户端提交的请求`Accept-Encoding: mycustomcompression`标头。 该中间件使用自定义压缩的实现，并返回响应，其中`Content-Encoding: mycustomcompression`标头。 客户端必须能够解压缩顺序用于工作的自定义压缩实现的自定义编码。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=4)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

向示例应用程序与提交一个申请`Accept-Encoding: mycustomcompression`标头并观察响应标头。 `Vary`和`Content-Encoding`标头会显示在响应。 此示例未压缩响应正文 （未显示）。 没有在压缩的实现`CustomCompressionProvider`类中的示例。 但是，此示例演示您可以用来实现此类的压缩算法。

![Fiddler 窗口中显示的 Accept-encoding 标头的请求的结果和 mycustomcompression 的值。 变化和的 Content-encoding 标头添加到的响应中。](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a>使用安全的协议压缩
可以通过安全连接压缩的响应来控制`EnableForHttps`选项，它默认处于禁用状态。 使用动态生成的页压缩可以导致安全问题如[犯罪](https://wikipedia.org/wiki/CRIME_(security_exploit))和[违反](https://wikipedia.org/wiki/BREACH_(security_exploit))攻击。

## <a name="adding-the-vary-header"></a>添加 Vary 标头
当压缩响应基于`Accept-Encoding`标头，有可能的多个压缩的版本响应和未压缩的版本。 若要指示客户端和代理服务器缓存，多个版本存在，并且应存储`Vary`标头添加与`Accept-Encoding`值。 在 ASP.NET Core 1.x 添加`Vary`手动完成到响应的标头。 在 ASP.NET Core 2.x，中间件将添加`Vary`压缩响应时自动标头。

**ASP.NET 核心 1.x 仅**

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>后面 Nginx 反向代理时的中间件问题
当请求代理 nginx，`Accept-Encoding`删除标头。 这可以防止该中间件压缩响应。 有关详细信息，请参阅[NGINX： 压缩和解压缩](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。 此问题将跟踪[找出传递压缩 Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123)。

## <a name="working-with-iis-dynamic-compression"></a>使用 IIS 动态压缩
如果你有一个 active IIS 动态压缩模块你想要禁用的应用程序的服务器级别配置，可以进行此操作而添加到你*web.config*文件。 有关详细信息，请参阅[禁用 IIS 模块](xref:host-and-deploy/iis/modules#disabling-iis-modules)。

## <a name="troubleshooting"></a>疑难解答
使用之类的工具[Fiddler](http://www.telerik.com/fiddler)， [Firebug](http://getfirebug.com/)，或[Postman](https://www.getpostman.com/)，这允许你设置`Accept-Encoding`请求标头并研究响应标头、 大小和正文。 响应压缩中间件压缩满足以下条件的响应：
* `Accept-Encoding`标头的值位于`gzip`， `*`，或与匹配的自定义压缩提供程序已建立的自定义编码。 值不能`identity`或具有质量值 (qvalue， `q`) 设置为 0 （零）。
* MIME 类型 (`Content-Type`) 必须设置，并且必须匹配上配置的 MIME 类型`ResponseCompressionOptions`。
* 请求不得包括`Content-Range`标头。
* 请求必须使用不安全的协议 (http)，除非在响应压缩中间件选项中配置安全协议 (https)。 *请注意危险[上述](#compression-with-secure-protocol)启用安全的内容压缩时。*

## <a name="additional-resources"></a>其他资源
* [应用程序启动](xref:fundamentals/startup)
* [中间件](xref:fundamentals/middleware/index)
* [Mozilla 开发人员网络： 接受的编码](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 部分 3.1.2.1： 内容 Codings](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 部分 4.2.3: Gzip 编码](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [GZIP 文件格式规范版本 4.3](http://www.ietf.org/rfc/rfc1952.txt)
