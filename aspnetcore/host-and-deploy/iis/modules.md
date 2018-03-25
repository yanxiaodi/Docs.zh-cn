---
title: "使用 ASP.NET Core 的 IIS 模块"
author: guardrex
description: "发现 ASP.NET Core 应用和如何管理 IIS 模块的活动和非活动 IIS 的模块。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: a6610e33abdc3eafb5908728b3299e95e6e7183f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="using-iis-modules-with-aspnet-core"></a>使用 ASP.NET Core 的 IIS 模块

作者：[Luke Latham](https://github.com/guardrex)

反向代理配置中，由 IIS 承载 ASP.NET Core 应用。 一些本机 IIS 模块和所有 IIS 管理模块不是可用于处理请求的 ASP.NET Core 应用。 在许多情况下，ASP.NET Core 提供 IIS 本机和托管模块的功能的替代方法。

## <a name="native-modules"></a>本机模块

| 模块 | .NET 核心活动 | ASP.NET Core Option |
| ------ | :--------------: | ------------------- |
| **匿名身份验证**<br>`AnonymousAuthenticationModule` | 是 | |
| **基本身份验证**<br>`BasicAuthenticationModule` | 是 | |
| **客户端证书映射身份验证**<br>`CertificateMappingAuthenticationModule` | 是 | |
| **CGI**<br>`CgiModule` | 否 | |
| **配置验证**<br>`ConfigurationValidationModule` | 是 | |
| **HTTP 错误**<br>`CustomErrorModule` | 否 | [状态代码页中间件](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **自定义日志记录**<br>`CustomLoggingModule` | 是 | |
| **默认文档**<br>`DefaultDocumentModule` | 否 | [默认文件中间件](xref:fundamentals/static-files#serve-a-default-document) |
| **摘要式身份验证**<br>`DigestAuthenticationModule` | 是 | |
| **目录浏览**<br>`DirectoryListingModule` | 否 | [目录浏览中间件](xref:fundamentals/static-files#enable-directory-browsing) |
| **动态压缩**<br>`DynamicCompressionModule` | 是 | [响应压缩中间件](xref:performance/response-compression) |
| **跟踪**<br>`FailedRequestsTracingModule` | 是 | [ASP.NET 核心日志记录](xref:fundamentals/logging/index#the-tracesource-provider) |
| **文件缓存**<br>`FileCacheModule` | 否 | [响应缓存中间件](xref:performance/caching/middleware) |
| **HTTP 缓存功能**<br>`HttpCacheModule` | 否 | [响应缓存中间件](xref:performance/caching/middleware) |
| **HTTP 日志记录**<br>`HttpLoggingModule` | 是 | [ASP.NET 核心日志记录](xref:fundamentals/logging/index)<br>实现： [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)， [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)， [NLog](https://github.com/NLog/NLog.Extensions.Logging)， [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **HTTP 重定向**<br>`HttpRedirectionModule` | 是 | [URL 重写中间件](xref:fundamentals/url-rewriting) |
| **IIS 客户端证书映射身份验证**<br>`IISCertificateMappingAuthenticationModule` | 是 | |
| **IP 和域限制**<br>`IpRestrictionModule` | 是 | |
| **ISAPI 筛选器**<br>`IsapiFilterModule` | 是 | [中间件](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | 是 | [中间件](xref:fundamentals/middleware/index) |
| **协议支持**<br>`ProtocolSupportModule` | 是 | |
| **请求筛选**<br>`RequestFilteringModule` | 是 | [URL 重写中间件 `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **请求监视器**<br>`RequestMonitorModule` | 是 | |
| **URL 重写**<br>`RewriteModule` | Yes&#8224; | [URL 重写中间件](xref:fundamentals/url-rewriting) |
| **服务器端包括**<br>`ServerSideIncludeModule` | 否 | |
| **静态压缩**<br>`StaticCompressionModule` | 否 | [响应压缩中间件](xref:performance/response-compression) |
| **静态内容**<br>`StaticFileModule` | 否 | [静态文件中间件](xref:fundamentals/static-files) |
| **令牌缓存**<br>`TokenCacheModule` | 是 | |
| **URI 缓存**<br>`UriCacheModule` | 是 | |
| **URL 授权**<br>`UrlAuthorizationModule` | 是 | [ASP.NET 核心标识](xref:security/authentication/identity) |
| **Windows 身份验证**<br>`WindowsAuthenticationModule` | 是 | |

&#8224;URL 重写模块的`isFile`和`isDirectory`匹配类型不使用 ASP.NET Core 应用中的更改由于[目录结构](xref:host-and-deploy/directory-structure)。

## <a name="managed-modules"></a>托管的模块

| 模块                  | .NET 核心活动 | ASP.NET Core Option |
| ----------------------- | :--------------: | ------------------- |
| AnonymousIdentification | 否               | |
| DefaultAuthentication   | 否               | |
| FileAuthorization       | 否               | |
| FormsAuthentication     | 否               | [Cookie 身份验证中间件](xref:security/authentication/cookie) |
| OutputCache             | 否               | [响应缓存中间件](xref:performance/caching/middleware) |
| 配置文件                 | 否               | |
| RoleManager             | 否               | |
| ScriptModule-4.0        | 否               | |
| 会话                 | 否               | [会话中间件](xref:fundamentals/app-state) |
| UrlAuthorization        | 否               | |
| UrlMappingsModule       | 否               | [URL 重写中间件](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | 否               | [ASP.NET 核心标识](xref:security/authentication/identity) |
| WindowsAuthentication   | 否               | |

## <a name="iis-manager-application-changes"></a>IIS 管理器应用程序更改

当使用 IIS 管理器配置设置， *web.config*更改应用程序文件。 如果部署的应用程序并包括*web.config*，做使用 IIS 管理器的任何更改将被覆盖的部署*web.config*文件。 如果在更改服务器的*web.config*文件中，复制已更新*web.config*于立即出现在本地项目的服务器上的文件。

## <a name="disabling-iis-modules"></a>禁用 IIS 模块

如果必须为一个应用程序的补充，到应用程序的禁用的服务器级别配置的 IIS 模块*web.config*文件可以禁用该模块。 将模块保留原位和停用它 （如果可用） 使用配置设置，或者从应用程序中删除模块。

### <a name="module-deactivation"></a>模块停用

多个模块提供配置设置，从而使这些要禁用但不从应用程序移除模块。 这是最简单且最快速方式停用模块。 例如，可以使用禁用 IIS URL 重写模块 **\<httpRedirect >**中的元素*web.config*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

禁用使用配置设置的模块的详细信息，请按照中的链接*子元素*部分[IIS \<system.webServer >](/iis/configuration/system.webServer/)。

### <a name="module-removal"></a>模块删除

如果选择加入以移除模块中的设置与*web.config*、 解锁模块和解锁**\<模块 >**部分*web.config*第一个：

1. 解锁的服务器级别的模块。 选择在 IIS 管理器中的 IIS 服务器**连接**侧栏。 打开**模块**中**IIS**区域。 在列表中选择该模块。 在**操作**侧栏右侧，选择**解锁**。 解锁任意多个模块，因为你打算移除*web.config*更高版本。

1. 部署应用程序而无需**\<模块 >**主题中*web.config*。如果应用程序部署与*web.config*包含**\<模块 >**部分不具有解锁部分首先在 IIS 管理器，配置管理器的情况下将引发异常无法解除锁定节。 因此，部署应用程序而无需**\<模块 >**部分。

1. 解锁**\<模块 >**部分*web.config*。在**连接**栏中，选择在网站**站点**。 在**管理**区域中，打开**配置编辑器**。 使用导航控件选择`system.webServer/modules`部分。 在**操作**侧栏右侧，选择**解锁**部分。

1. 此时， **\<模块 >**部分可以添加到*web.config*文件**\<删除 >**要移除从模块元素应用程序。 多个**\<删除 >**可添加元素以移除多个模块。 如果*web.config*服务器上进行更改，立即对相同的更改进行项目的*web.config*文件在本地。 删除这种方式的模块不会影响服务器上的其他应用的模块使用。

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

对于与默认模块安装 IIS 安装，使用以下**\<模块 >**部分，以移除默认模块。

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

IIS 模块也可能会删除与*Appcmd.exe*。 提供`MODULE_NAME`和`APPLICATION_NAME`命令中：

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

例如，删除`DynamicCompressionModule`从默认网站：

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>最小模块配置

若要运行 ASP.NET Core 应用所需的唯一模块是匿名身份验证模块和 ASP.NET 核心模块。

![显示最少模块配置到模块打开 IIS 管理器](modules/_static/modules.png)

## <a name="additional-resources"></a>其他资源

* [使用 IIS 在 Windows 上进行托管](xref:host-and-deploy/iis/index)
* [简介 IIS 体系结构： 在 IIS 中的模块](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [IIS 模块概述](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [自定义 IIS 7.0 角色和模块](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
