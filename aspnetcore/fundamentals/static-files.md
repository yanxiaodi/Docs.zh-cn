---
title: "在 ASP.NET Core 中处理静态文件"
author: rick-anderson
description: "了解如何在 ASP.NET Core Web 应用中提供和保护静态文件，以及如何配置静态文件托管中间件行为。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 7b156830ab59db3c08fbff6b2c4180d8765a113b
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>在 ASP.NET Core 中处理静态文件

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Scott Addie](https://twitter.com/Scott_Addie)

静态文件（如 HTML、CSS、图像和 JavaScript）是 ASP.NET Core 应用直接提供给客户端的资产。 需要进行一些配置才能提供这些文件。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="serve-static-files"></a>提供静态文件

静态文件存储在项目的 Web 根目录中。 默认目录是 \<content_root>/wwwroot，但可通过 [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) 方法更改目录。 有关详细信息，请参阅[内容根目录](xref:fundamentals/index#content-root)和 [Web 根目录](xref:fundamentals/index#web-root)。

应用的 Web 主机必须识别内容根目录。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

采用 `WebHost.CreateDefaultBuilder` 方法可将内容根目录设置为当前目录：

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

调用 `Program.Main` 中的 [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) 将内容根目录设为当前目录：

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

可通过 Web 根目录的相关路径访问静态文件。 例如，Web 应用程序项目模板包含 wwwroot 文件夹中的多个文件夹：

* **wwwroot**
  * **css**
  * **images**
  * **js**

用于访问 images 子文件夹中的文件的 URI 格式为 http://\<server_address>/images/\<image_file_name>。 例如，http://localhost:9189/images/banner3.svg。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果以 .NET Framework 为目标，请将 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) 包添加到项目。 如果以 .NET Core 为目标，请将 [Microsoft.AspNetCore.All 元包](xref:fundamentals/metapackage)加入此包。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

将 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) 包添加到项目。

---

配置提供静态文件的[中间件](xref:fundamentals/middleware/index)。

### <a name="serve-files-inside-of-web-root"></a>提供 Web 根目录内的文件

调用 `Startup.Configure` 中的 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 方法：

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

无参数 `UseStaticFiles` 方法重载将 Web 根目录中的文件标记为可用。 以下标记引用 wwwroot/images/banner1.svg：

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>提供 Web 根目录外的文件

考虑一个目录层次结构，其中要提供的静态文件位于 Web 根目录之外：

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * banner1.svg

按如下方式配置静态文件中间件后，请求可访问 banner1.svg 文件：

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

在前面的代码中，MyStaticFiles 目录层次结构通过 StaticFiles URI 段公开。 请求 http://\<server_address>/StaticFiles/images/banner1.svg 提供 banner1.svg 文件。

以下标记引用 MyStaticFiles/images/banner1.svg：

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>设置 HTTP 响应标头

[StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) 对象可用于设置 HTTP 响应标头。 除配置从 Web 根目录提供静态文件外，以下代码还设置 `Cache-Control` 标头：

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) 方法存在于 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 包中。

可公开缓存这些文件 10 分钟（600 秒）：

![已添加显示 Cache-Control 标头的响应头](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>静态文件授权

静态文件中间件不提供授权检查。 可公开访问由静态文件中间件提供的任何文件，包括 wwwroot 下的文件。 根据授权提供文件：

* 将文件存储在 wwwroot 和静态文件中间件可访问的任何目录之外并
* 通过有授权的操作方法提供文件。 返回 [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) 对象：

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>启用目录浏览

通过目录浏览，Web 应用的用户可查看目录列表和指定目录中的文件。 出于安全考虑，目录浏览默认处于禁用状态（请参阅[注意事项](#considerations)）。 调用 `Startup.Configure` 中的 [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) 方法来启用目录浏览：

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

调用 `Startup.ConfigureServices` 中的[AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 方法来添加所需服务：

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

上述代码允许使用 URL http://\<server_address>/MyImages 浏览 wwwroot/images 文件夹的目录，并链接到每个文件和文件夹：

![目录浏览](static-files/_static/dir-browse.png)

有关启用浏览时的安全风险，请参阅[注意事项](#considerations)。

请注意以下示例中的两个 `UseStaticFiles` 调用。 第一个调用提供 wwwroot 文件夹中的静态文件。 第二个调用使用 URL http://\<server_address>/MyImages 浏览 wwwroot/images 文件夹的目录：

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>提供默认文档

设置默认主页为访问者访问网站时提供了逻辑起点。 若要在用户不完全限定 URI 的情况下提供默认页面，请调用 `Startup.Configure` 中的 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 方法：

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> 要提供默认文件，必须在 `UseStaticFiles` 前调用 `UseDefaultFiles`。 `UseDefaultFiles` 实际上用于重写 URL，不提供文件。 通过 `UseStaticFiles` 启用静态文件中间件来提供文件。

使用 `UseDefaultFiles` 请求文件夹搜索：

* default.htm
* default.html
* index.htm
* index.html

将请求视为完全限定 URI，提供在列表中找到的第一个文件。 浏览器 URL 继续反映请求的 URI。

以下代码将默认文件名更改为 mydefault.html：

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 结合了 `UseStaticFiles`、`UseDefaultFiles` 和 `UseDirectoryBrowser` 的功能。

以下代码提供静态文件和默认文件。 未启用目录浏览。

```csharp
app.UseFileServer();
```

以下代码通过启用目录浏览基于无参数重载进行构建：

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

考虑以下目录层次结构：

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * banner1.svg
  * default.html

以下代码启用静态文件、默认文件和及 `MyStaticFiles` 的目录浏览：

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`EnableDirectoryBrowsing` 属性值为 `true` 时必须调用 `AddDirectoryBrowser`：

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

使用文件层次结构和前面的代码，URL 解析如下：

| URI            |                             响应  |
| ------- | ------|
| http://\<server_address>/StaticFiles/images/banner1.svg    |      MyStaticFiles/images/banner1.svg |
| http://\<server_address>/StaticFiles             |     MyStaticFiles/default.html |

如果 MyStaticFiles 目录中不存在默认命名文件，则 http://\<server_address>/StaticFiles 返回包含可单击链接的目录列表：

![静态文件列表](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles` 和 `UseDirectoryBrowser` 使用不带尾部反斜杠的 URL http://\<server_address>/StaticFiles 触发客户端并重定向到 http://\<server_address>/StaticFiles/。 注意添加尾部反斜杠。 无尾部反斜杠，文档中的相对 URL 被视为无效。

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) 类包含 `Mappings` 属性，用作文件扩展名到 MIME 内容类型的映射。 在以下示例中，多个文件扩展名注册到了已知的 MIME 类型。 替换了 .rtf 扩展名，删除了 .mp4。

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

请参阅 [MIME 内容类型](http://www.iana.org/assignments/media-types/media-types.xhtml)。

## <a name="non-standard-content-types"></a>非标准内容类型

静态文件中间件可理解近 400 种已知文件内容类型。 如果用户请求未知文件类型的文件，则静态文件中间件会返回 HTTP 404（未找到）响应。 如果启用了目录浏览，则会显示该文件的链接。 URI 返回 HTTP 404 错误。

以下代码提供未知类型，并以图像形式呈现未知文件：

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

使用前面的代码，请求的文件含未知内容类型时，以图像形式返回请求。

> [!WARNING]
> 启用 [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) 存在安全风险。 它默认处于禁用状态，不建议使用。 [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) 提供了更安全的替代方法来提供含非标准扩展名的文件。

### <a name="considerations"></a>注意事项

> [!WARNING]
> `UseDirectoryBrowser` 和 `UseStaticFiles` 可能会泄漏机密。 强烈建议在生产中禁用目录浏览。 请仔细查看 `UseStaticFiles` 或 `UseDirectoryBrowser` 启用了哪些目录。 整个目录及其子目录均可公开访问。 将适合公开的文件存储在专用目录中，如 \<content_root>/wwwroot。 将这些文件与 MVC 视图、Razor 页面（仅限 2.x）和配置文件等分开

* 使用 `UseDirectoryBrowser` 和 `UseStaticFiles` 公开的内容的 URL 受大小写和基础文件系统字符限制的影响。 例如，Windows 不区分大小写 &mdash; Mac 和 Linux 却要区分。

* 托管于 IIS 中的 ASP.NET Core 应用使用 [ASP.NET Core 模块 (ANCM)](xref:fundamentals/servers/aspnet-core-module) 将所有请求转发到应用，包括静态文件请求。 未使用 IIS 静态文件处理程序。 在 ANCM 处理请求前，处理程序没有机会处理请求。

* 在 IIS Manager 中完成以下步骤，删除服务器或网站级别的 IIS 静态文件处理程序：
    1. 转到“模块”功能。
    1. 在列表中选择 StaticFileModule。
    1. 单击“操作”侧栏中的“删除”。

> [!WARNING]
> 如果启用了 IIS 静态文件处理程序且 ANCM 配置不正确，则会提供静态文件。 例如，如果未部署 web.config 文件，则会发生这种情况。

* 将代码文件（包括 .cs 和 .cshtml ）放在应用项目的 Web 根目录之外。 这样就在应用的客户端内容和基于服务器的代码间创建了逻辑分隔。 可以防止服务器端代码泄漏。

## <a name="additional-resources"></a>其他资源

* [中间件](xref:fundamentals/middleware/index)
* [ASP.NET Core 简介](xref:index)
