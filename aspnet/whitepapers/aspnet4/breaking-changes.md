---
uid: whitepapers/aspnet4/breaking-changes
title: "ASP.NET 4 的重大更改 |Microsoft 文档"
author: rick-anderson
description: "本文档介绍更改所做的.NET Framework 版本 4 可能会影响使用创建的应用程序的发行版..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: d68723b52ae1ee80142fb1aca3b0b10de34332d1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 的重大更改
====================
> 本文档介绍更改所做的.NET Framework 版本 4 可能会影响使用早期版本中，其中包括 ASP.NET 4 Beta 1 和 Beta 2 版本创建的应用程序的版本。
> 
> [下载此白皮书](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>内容

[Web.config 文件中的 ControlRenderingCompatibilityVersion 设置](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode 更改](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode 和执行 UrlEncode 现在编码单引号](#0.1__Toc256770143 "_Toc256770143")  
[ASP.NET 页面 (.aspx) 分析器是 Stricter](#0.1__Toc256770144 "_Toc256770144")  
[浏览器定义文件更新](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll 从根 Web 配置文件中删除](#0.1__Toc256770146 "_Toc256770146")  
[ASP.NET 请求验证](#0.1__Toc256770147 "_Toc256770147")  
[哈希算法的默认值为 Now HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[与新的 ASP.NET 4 根配置相关的配置错误](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET 4 个子应用程序启动失败的时下 ASP.NET 2.0 或 ASP.NET 3.5 应用程序](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 Web 站点无法在其中安装 SharePoint 的计算机上开始](#0.1__Toc256770151 "_Toc256770151")  
[HttpRequest.FilePath 属性不再包括 PathInfo 值](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 应用程序可能会产生 HttpException 错误引用 eurl.axd](#0.1__Toc256770153 "_Toc256770153")  
[事件处理程序可能不会不引发在 IIS 7 或 IIS 7.5 中的默认文档中集成模式下](#0.1__Toc256770154 "_Toc256770154")  
[ASP.NET 代码访问安全性 (CAS) 实现的更改](#0.1__Toc256770155 "_Toc256770155")  
[在移动 MembershipUser 中和其他类型 System.Web.Security Namespace](#0.1__Toc256770156 "_Toc256770156")  
[输出缓存的更改以改变\*HTTP 标头](#0.1__Toc256770157 "_Toc256770157")  
[Passport System.Web.Security 类型是已过时](#0.1__Toc256770158 "_Toc256770158")  
[未能呈现 ASP.NET 4 中的图像 MenuItem.PopOutImageUrl 属性](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl 和 Menu.DynamicPopOutImageUrl 无法呈现图像时路径包含反斜杠](#0.1__Toc256770160 "_Toc256770160")  
[Disclaimer](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Web.config 文件中的 ControlRenderingCompatibilityVersion 设置

.NET Framework 版本 4 中的已更改 ASP.NET 控件以便允许您指定更确切地说它们的呈现方式标记。 在以前版本的.NET Framework 中，某些控件发出具有无法禁用的标记。 默认情况下，无法再生成 ASP.NET 4 这种类型的标记。

如果你使用 Visual Studio 2010 从 ASP.NET 2.0 或 ASP.NET 3.5 升级你的应用程序，该工具会自动添加到设置`Web.config`保留旧的呈现的文件。 但是，如果通过在 IIS 中将应用程序池更改为面向 .NET Framework 4 来升级应用程序，ASP.NET 默认使用新的呈现模式。 若要禁用新的呈现模式下，添加中的以下设置`Web.config`文件：

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

新的行为将主要呈现更改如下所示：

- **映像**和**ImageButton**控件不再呈现`border="0"`属性。
- **BaseValidator**从它派生的类和验证控件不再呈现红色文本默认情况下。
- **HtmlForm**控件不呈现**名称**属性。
- **表**控件不再呈现`border="0"`属性。
- 不用于用户输入的控件 (例如，**标签**控件) 不再呈现`disabled="disabled"`属性如果其**已启用**属性设置为**false**（或如果容器控件从其继承此设置）。

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode 更改

**ClientIDMode** ASP.NET 4 中的设置允许您指定如何生成 ASP.NET **id**对于 HTML 元素的属性。 在以前版本的 ASP.NET，默认行为是等效于**AutoID**设置**ClientIDMode**。 但是，默认设置是现在**可预测**。

如果你使用 Visual Studio 2010 从 ASP.NET 2.0 或 ASP.NET 3.5 升级你的应用程序，该工具会自动添加到设置`Web.config`保留早期版本的.NET Framework 的行为的文件。 但是，如果通过在 IIS 中将应用程序池更改为面向 .NET Framework 4 来升级应用程序，ASP.NET 默认使用新的模式。 若要禁用新的客户端 ID 模式下，添加中的以下设置`Web.config`文件：

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode 和执行 UrlEncode 现在编码单引号

在 ASP.NET 4 中， **HtmlEncode**和**执行 UrlEncode**方法**HttpUtility**和**HttpServerUtility**类已更新为编码的单引号字符 （'），如下所示：

- **HtmlEncode**方法将编码的单引号实例为。
- **执行 UrlEncode**方法将实例的单引号编码为 %27。

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>ASP.NET 页面 (.aspx) 分析器是 Stricter

ASP.NET 页的页分析器 (`.aspx`文件) 和用户控件 (`.ascx`文件) ASP.NET 4 中更严格，则将拒绝的无效标记的更多实例。 例如，以下两个代码段将成功分析在早期版本的 ASP.NET，但现在将引发 ASP.NET 4 中的分析器错误。

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

请注意在末尾无效分号**HiddenField**标记。

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

请注意闭合**样式**时遇到的属性**CssClass**属性。

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>更新的浏览器定义文件

浏览器定义文件已更新，以包括有关新的和已更新的浏览器和设备的信息。 旧式浏览器和设备（如 Netscape Navigator）已被删除，并且已添加更新的浏览器和设备（如 Google Chrome 和 Apple iPhone）。

如果应用程序包含的自定义浏览器定义继承自一个已删除的浏览器定义，将会出现错误。 例如，如果`App_Browsers`文件夹包含继承自 IE2 浏览器定义的浏览器定义，则你将收到以下配置错误消息：

- 找不到具有 ID IE2 的浏览器或网关元素。

> [!NOTE]
> **HttpBrowserCapabilities**对象 (这公开的页面的**Request.Browser**属性) 由浏览器定义文件驱动。 因此，通过访问此对象在 ASP.NET 4 中的属性返回的信息可能不同于 ASP.NET 的早期版本中返回的信息。


您可以通过复制以下文件夹中的浏览器定义文件还原到旧的浏览器定义文件：

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

将文件复制到相应`\CONFIG\Browsers`为 ASP.NET 4 的文件夹。 复制文件后，运行 Aspnet\_regbrowsers.exe 命令行工具。

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll 从根 Web 配置文件中删除

在以前版本的 ASP.NET，System.Web.Mobile.dll 程序集的引用已包含在根`Web.config`文件中**程序集**部分下。 为了改进性能，已删除对此程序集的引用。

System.Web.Mobile.dll 程序集包含在 ASP.NET 4 中，但它已弃用。 如果你想要使用 System.Web.Mobile.dll 程序集的类型，则将对此程序集的引用添加到任一`Web.config`文件或某个应用程序`Web.config`文件。 例如，如果你想要使用的任何 （已弃用） 的 ASP.NET 移动控件，则必须添加到 System.Web.Mobile.dll 程序集的引用`Web.config`文件。

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>ASP.NET 请求验证

在 ASP.NET 中的请求验证功能可提供特定级别的针对跨站点脚本 (XSS) 攻击的默认保护。 在以前版本的 ASP.NET，默认情况下已启用请求验证。 但是，它仅应用于 ASP.NET 页 (`.aspx`文件和其类文件) 和仅当这些页执行。

在 ASP.NET 4 中，默认情况下，请求启用验证对于所有请求，因为它已启用之前**BeginRequest**阶段的 HTTP 请求。 因此，请求验证适用于所有 ASP.NET 资源，而不仅仅是.aspx 页请求的请求。 这包括 Web 服务调用等自定义 HTTP 处理程序的请求。 请求验证还处于活动状态的自定义 HTTP 模块将读取的 HTTP 请求内容时。

因此，以前未触发错误的请求可能现在会发生请求验证错误。 若要恢复为 ASP.NET 2.0 请求验证功能的行为，请添加中的以下设置`Web.config`文件：

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

但是，我们建议你分析任何请求验证错误，以确定是否有可能访问现有的处理程序、 模块或其他自定义代码可能是 XSS 的不安全 HTTP 输入攻击向量。

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>哈希算法的默认值为 Now HMACSHA256

ASP.NET 使用加密算法和哈希算法来帮助保护数据（如窗体身份验证 Cookie 和视图状态）。 默认情况下，ASP.NET 4 现在使用 HMACSHA256 算法进行哈希操作，cookie 并视图状态。 ASP.NET 的早期版本使用较旧的 HMACSHA1 算法。

如果你运行混合的 ASP.NET 2.0/ASP.NET 4，可能会影响你的应用程序，如窗体的身份验证 cookie 的数据必须可以 across.NET Framework 版本的环境。 若要配置 ASP.NET 4 Web 应用程序使用较旧的 HMACSHA1 算法，添加中的以下设置`Web.config`文件：

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>与新的 ASP.NET 4 根配置相关的配置错误

根配置文件 (`machine.config`文件和根`Web.config`文件) 的.NET Framework 4 （以及因此 ASP.NET 4） 已经更新，以包括大部分样本配置信息，在 ASP.NET 3.5 中找到应用程序`Web.config`文件。 由于托管的 IIS 7 和 IIS 7.5 配置系统的复杂性，运行 ASP.NET 3.5 应用程序和 IIS 7 和 IIS 7.5 ASP.NET 4 下可能导致 ASP.NET 或 IIS 配置错误。

我们建议如果可行，通过在 Visual Studio 2010 中，使用的项目升级工具升级到 ASP.NET 4 ASP.NET 3.5 应用程序。 Visual Studio 2010 自动修改 ASP.NET 3.5 应用程序的`Web.config`文件，以包含 ASP.NET 4 中的相应设置。

但是，它是受支持的方案运行使用.NET Framework 4，无需重新编译的 ASP.NET 3.5 应用程序。 在这种情况下，你可能需要手动修改应用程序的`Web.config`文件之前运行应用程序和 IIS 7 或 IIS 7.5 下.NET Framework 4。

接下来的两部分介绍你可能需要的软件的不同组合进行的更改。

**Windows Vista SP1 或 Windows Server 2008 SP1、 修补程序 KB958854 也 SP2 都不必安装位置。** 在此配置中，IIS 7 配置系统错误地将应用程序的托管的配置合并通过比较应用程序级`Web.config`文件为 ASP.NET 2.0`machine.config`文件。 由于此问题，应用程序级`Web.config`来自.NET Framework 3.5 或更高版本的文件都必须具有**system.web.extensions**为了不导致 IIS 7 验证失败的配置部分定义 （元素）。

但是，手动修改应用程序级`Web.config`不精确匹配原始样本配置部分定义中引入的 Visual Studio 2008 的文件条目将会导致 ASP.NET 配置错误。 （生成的 Visual Studio 2008 的默认配置条目正常工作。）常见的问题是手动修改`Web.config`文件已经离开了**allowDefinition**和**requirePermission**的各种配置节中找到的配置属性定义。 这将导致应用程序级别中的缩写的配置部分不匹配`Web.config`文件和 ASP.NET 4 中的完整定义`machine.config`文件。 因此，在运行时，ASP.NET 4 配置系统将引发配置错误。

**Windows Vista SP2、 Windows Server 2008 SP2、 Windows 7、 Windows Server 2008 R2，并还 Windows Vista SP1 和 Windows Server 2008 SP1 安装了修补程序 KB958854。**

在这种情况下，IIS 7 和 IIS 7.5 本机配置系统返回配置错误因为它对执行文本比较**类型**为受管理的配置节处理程序定义的属性。 因为所有`Web.config`都由 Visual Studio 2008 和 Visual Studio 2008 SP1 的文件具有"3.5"中的类型字符串**system.web.extensions** （和相关） 配置节处理程序，因为和 ASP.NET4`machine.config`文件具有"4.0"**类型**属性相同配置节处理程序，始终在 Visual Studio 2008 或 Visual Studio 2008 SP1 中生成的应用程序通过在 IIS 7 中的配置验证和IIS 7.5。

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>解决这些问题

第一个方案的解决方法是更新应用程序级`Web.config`文件将从样本配置文本包含`Web.config`由 Visual Studio 2008 自动生成的文件。

备用解决方法的第一个方案是在你的计算机上安装 Service Pack 2 for Vista 或 Windows Server 2008 或安装修补程序 KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) 若要修复的错误IIS 配置系统的配置合并行为。 但是，在执行上述任一操作后，你的应用程序将可能会遇到配置错误，因为对于第二个方案描述的问题。

第二个方案的解决方法是删除或注释掉所有**system.web.extensions**配置部分定义和配置节组定义的应用程序级`Web.config`文件。 这些定义通常是在应用程序级顶部`Web.config`文件，并可以由标识**configSections**元素及其子级。

对于这两个方案，建议你可以手动删除**system.codedom**部分，虽然这不是必需。

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET 4 个子应用程序启动失败的时下 ASP.NET 2.0 或 ASP.NET 3.5 应用程序

由于配置或编译错误，配置为运行 ASP.NET 早期版本的应用程序子级的 ASP.NET 4 应用程序可能无法启动。 下面的示例演示受影响的应用程序的目录结构。

`/parentwebapp`（配置为使用 ASP.NET 2.0 或 ASP.NET 3.5）  
`/childwebapp`（配置为使用 ASP.NET 4）

中的应用程序`childwebapp`文件夹将不能在 IIS 7 或 IIS 7.5 上启动，并将报表配置错误。 错误文本将包括类似于以下的消息：

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

在 IIS 6 中的应用程序上`childwebapp`文件夹也将无法启动，但它将报告不同错误。 例如，错误文本可能状态如下：

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

因为，会出现这些情况中的父应用程序中的配置信息`parentwebapp`文件夹是确定由子站点的最终的合并的配置设置的配置信息的层次结构的一部分在应用程序`childwebapp`文件夹。 具体取决于 ASP.NET 4 Web 应用程序是否在 IIS 7 （或 IIS 7.5） 或 IIS 6 上运行，IIS 配置系统或 ASP.NET 4 中编译系统将返回错误。

若要解决此问题，并获取子 ASP.NET 4 应用程序可以必须遵循的步骤取决于 ASP.NET 4 应用程序是否在 IIS 7 （或 IIS 7.5） 上运行了 IIS 6 或。

### <a name="step-1-iis-7-or-iis-75-only"></a>步骤 1 （IIS 7 或仅 IIS 7.5）

此步骤是必需只会在运行 IIS 7 的操作系统或 IIS 7.5，包括 Windows Vista、 Windows Server 2008、 Windows 7 和 Windows Server 2008 R2。

移动**configSections**中的定义`Web.config`父应用程序 （运行 ASP.NET 2.0 或 ASP.NET 3.5 应用程序） 的文件到根`Web.config`的.net Framework 2.0 的文件。 IIS 7 和 IIS 7.5 native 配置系统会扫描**configSections**当它合并配置文件的层次结构的元素。 移动**configSections**从父 Web 应用程序的定义`Web.config`到根文件`Web.config`文件有效地隐藏子 ASP.NET 4 进行的配置合并过程中的元素应用程序。

在 32 位操作系统上或 32 位应用程序池，根`Web.config`为 ASP.NET 2.0 和 ASP.NET 3.5 文件通常位于以下文件夹：

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

在 64 位操作系统上或 64 位应用程序池，根`Web.config`为 ASP.NET 2.0 和 ASP.NET 3.5 文件通常位于以下文件夹：

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

如果在 64 位计算机上运行 32 位和 64 位 Web 应用程序，则必须移动**configSections**元素上的插入根`Web.config`32 位和 64 位系统文件。

当将**configSections**根目录中的元素`Web.config`文件中，粘贴部分后立即**配置**元素。 下面的示例演示根哪些的上半部分`Web.config`文件应如下所示完成后移动的元素。

> [!NOTE]
> 在下面的示例中，已换行以提高可读性。


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>步骤 2 （所有版本的 IIS）

此步骤是必需的 ASP.NET 4 子 Web 应用程序是否正在运行 IIS 6 或 IIS 7 （或 IIS 7.5） 上。

在`Web.config`文件的父 Web 应用程序运行的 ASP.NET 2 或 ASP.NET 3.5 中，添加**位置**（对于 IIS 和 ASP.NET 配置系统） 显式指定的标记，仅配置条目将应用于父 Web 应用程序。 下面的示例演示的语法**位置**要添加元素：

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

下面的示例演示如何**位置**标记用于包装开头的所有配置节**appSettings**部分，以结束**system.webServer**部分。

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

完成步骤 1 和 2 后，子 ASP.NET 4 Web 应用程序将启动且未发生错误。

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 Web 站点无法在其中安装 SharePoint 的计算机上开始

运行 SharePoint 的 web 服务器具有`Web.config`部署在 SharePoint 网站的根目录的文件 (例如，`c:\inetpub\wwwroot\web.config`对 Default Web Site)。 在此`Web.config`文件中，SharePoint 设置自定义的部分信任级别名为 WSS\_最小。

如果你尝试这种类型的 SharePoint 网站的子级运行部署一个 ASP.NET 4 Web 站点，你将看到以下错误：

`Could not find permission set named 'ASP.NET'.`

因为 ASP.NET 4 代码访问安全性 (CAS) 基础结构查找名为 ASP.NET 的权限集，将出现此错误。 但是，使用部分信任配置文件引用的 WSS\_最小不包含具有该名称的任何权限集。

当前没有 SharePoint 可用与 ASP.NET 兼容的版本。 因此，你不应尝试站点作为子站点 SharePoint Web 下运行 ASP.NET 4 网站。

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>HttpRequest.FilePath 属性不再包括 PathInfo 值

包含的 ASP.NET 的早期版本**PathInfo**从各种文件路径相关属性，包括返回值中的值**HttpRequest.FilePath**， **HttpRequest.AppRelativeCurrentExecutionFilePath**，和**HttpRequest.CurrentExecutionFilePath**。 ASP.NET 4 不再包括**PathInfo**从这些属性的返回值中的值。 相反， **PathInfo**中提供了信息**HttpRequest.PathInfo**。 例如，假定以下 URL 片段：

`/testapp/Action.mvc/SomeAction`

在早期版本的 ASP.NET， **HttpRequest**属性具有以下值：

**HttpRequest.FilePath**:`/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: （空）

在 ASP.NET 4 中， **HttpRequest**属性改为具有以下值：

**HttpRequest.FilePath**:`/testapp/Action.mvc`

**HttpRequest.PathInfo**:`SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 应用程序可能会产生引用 eurl.axd 的 HttpException 错误

ASP.NET 4 启用 IIS 6 上之后，在 （在 Windows Server 2003 或 Windows Server 2003 R2） 的 IIS 6 运行的 ASP.NET 2.0 应用程序可能会产生错误如下所示：

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

当 ASP.NET 检测到网站配置为使用 ASP.NET 4，ASP.NET 4 本机组件会将无扩展名的 URL 交给 ASP.NET 的托管部分进行进一步处理会发生此错误。 但是，如果是 ASP.NET 4 Web 站点下的虚拟目录配置为使用 ASP.NET 2.0，处理中修改 URL，其中包含字符串"eurl.axd"此方式导致的无扩展名的 URL。 然后，此修改后的 URL 将发送到 ASP.NET 2.0 应用程序。 ASP.NET 2.0 无法识别"eurl.axd"格式。 因此，ASP.NET 2.0 将尝试查找名为的文件`eurl.axd`并执行它。 因为此类文件不存在，请求将失败， **HttpException**异常。

你可以解决此问题，使用以下选项之一。

### <a name="option-1"></a>选项 1

如果 ASP.NET 4 不是运行网站所必需的重新映射的站点，以改为使用 ASP.NET 2.0。

### <a name="option-2"></a>选项 2

如果 ASP.NET 4 需要运行网站，移动到不同的网站映射到 ASP.NET 2.0 任何子 ASP.NET 2.0 虚拟目录。

### <a name="option-3"></a>选项 3

如果不可行，若要重新映射到 ASP.NET 2.0 的 Web 站点，或要更改虚拟目录的位置，显式禁用 ASP.NET 4 中的处理无扩展名的 URL。 使用以下过程：

1. 在 Windows 注册表中，打开以下节点：

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. 创建一个新**DWORD**名为值**EnableExtensionlessUrls**。
2. 设置**EnableExtensionlessUrls**为 0。 这会禁用无扩展名的 URL 行为。
3. 保存注册表值并关闭注册表编辑器。
4. 运行**iisreset**命令行工具，这会导致 IIS 读取新的注册表值。

> [!NOTE]
> 设置**EnableExtensionlessUrls**为 1 可启用无扩展名的 URL 行为。 这是默认设置，如果未不指定任何值。


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>事件处理程序可能不会不引发在 IIS 7 或 IIS 7.5 中的默认文档中集成模式

ASP.NET 4 中包含的更改的修改如何**操作**特性的 html**窗体**元素呈现时无扩展名的 URL 解析为默认文档。 无扩展名 URL 解析为默认文档的示例将[http://contoso.com/](http://contoso.com/)，这会导致到的请求中[http://contoso.com/Default.aspx](http://contoso.com/Default.aspx)。

ASP.NET 4 现在的 HTML 呈现**窗体**元素的**操作**属性值为一个空字符串，当请求时，都具有映射到它的默认文档的无扩展名的 URL。 例如，在早期版本 ASP.NET 中，对的请求的[http://contoso.com](http://contoso.com)将导致对请求`Default.aspx`。 在该文档中，打开**窗体**标记将呈现如以下示例所示：

`<form action="Default.aspx" />`

在 ASP.NET 4 中，对的请求[http://contoso.com](http://contoso.com)也会导致对请求`Default.aspx`。 但是，ASP.NET 现在呈现 HTML 打开**窗体**标记，如以下示例所示：

`<form action="" />`

如何在这种差异**操作**呈现特性，则可能导致细微的更改窗体发布请求服务由 IIS 和 ASP.NET 的处理方式。 当**操作**属性为空字符串，IIS **DefaultDocumentModule**对象将创建子请求到`Default.aspx`。 大多数情况下，此子请求是透明的应用程序代码和`Default.aspx`页会正常运行。

但托管代码和 IIS 7 或 IIS 7.5 集成模式之间可能的交互会导致托管 .aspx 页在子请求期间停止正常工作。 如果发生以下情况时，对的子请求`Default.aspx`文档将导致错误或意外行为：

1. 一个.aspx 页发送到浏览器中使用**窗体**元素的**操作**属性设置为""。
2. 窗体回发到 ASP.NET。
3. 托管的 HTTP 模块将读取的实体正文的某些部分。 例如，模块将读取**Request.Form**或**Request.Params**。 这会使 POST 请求的实体正文读入托管内存中。 因此，实体正文不再对在 IIS 7 或 IIS 7.5 集成模式中运行的任何本机代码模块可用。
4. IIS **DefaultDocumentModule**对象最终运行，并创建到的子请求`Default.aspx`文档。 但由于一段托管代码已读取实体正文，因此没有可发送给子请求的实体正文。
5. HTTP 管道子请求的处理程序的运行时`.aspx`对子阶段运行的文件。
6. 由于没有任何实体主体，有任何窗体变量和视图状态，并因此任何信息将用.aspx 页处理程序，以确定哪些事件 （如果有） 应引发。 因此，不会运行针对受影响 .aspx 页的任何回发事件处理程序。

可以通过以下方式来解决此问题：

- 标识在默认文档的请求期间访问请求的实体主体的 HTTP 模块，并确定是否可以将它配置为仅对托管请求运行。 在 IIS 7 和 IIS 7.5 的集成模式下，可以标记 HTTP 模块仅对托管请求通过将以下属性添加到模块的运行**system.webServer/modules**条目：

- `precondition="managedHandler"`

- 在模块中的搜索请求 IIS 7 和 IIS 7.5 确定为不符合此设置禁用管理请求。 有关默认文档的请求，第一个请求是一个无扩展名的 URL。 因此，IIS 不运行任何标记的托管的模块与托管处理程序的前提条件初始请求处理过程。 因此，托管的模块将不会意外地读取的实体正文和实体正文因此仍然可用，并且传递到子请求和默认文档。

- 如果有问题的 HTTP 模块必须运行的所有请求 (静态文件，无扩展名的 url 解析为的**DefaultDocumentModule**对象，以便托管的请求，等等)，显式修改受影响的.aspx 页面按照设置**操作**属性页面的**System.Web.UI.HtmlControls.HtmlForm**控件添加到非空字符串。 例如，如果默认文档是`Default.aspx`，修改页的代码以显式设置**HtmlForm**控件的**操作**"Default.aspx"的属性。

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>ASP.NET 代码访问安全性 (CAS) 实现的更改

ASP.NET 2.0 中，并通过扩展在 3.5 中，已添加的 ASP.NET 功能使用的.NET Framework 1.1 和 2.0 代码访问安全性 (CAS) 模型。 但是，实质上已对 ASP.NET 4 中的 CAS 实现进行了检查。 因此，部分信任的 ASP.NET 应用程序依赖于在全局程序集缓存 (GAC) 中运行的受信任代码可能失败，各种安全异常。 部分信任应用程序依赖于广泛修改计算机 CAS 策略也可能失败，且安全异常。

你可以还原部分信任 ASP.NET 4 应用程序的行为的 ASP.NET 1.1 和 2.0 使用新**legacyCasModel**属性中**信任**配置元素，如下面的示例中所示:

`<trust level= "Medium" legacyCasModel="true" />`

当你还原到旧 CA 模型时，已启用以下的旧 CA 行为：

- 则遵循机 CAS 策略。
- 允许单个应用程序域中的多个不同的权限集。
- 显式权限断言不需要 GAC 中，只有 ASP.NET 或其他.NET Framework 代码是在堆栈上时调用的程序集。

一种情况将无法还原在.NET Framework 4： 非 Web 部分信任应用程序可以不再调用 System.Web.dll 和 System.Web.Extensions.dll 中的某些 Api。 在以前版本的.NET Framework 中，它是适用于非 Web 部分信任应用程序将被显式授予**AspNetHostingPermission**权限。 然后，这些应用程序可以使用**System.Web.HttpUtility**中的类型**System.Web.ClientServices。\***与成员资格、 角色和配置文件相关的命名空间和类型。 在.NET Framework 4 中不再支持从非 Web 部分信任应用程序中调用这些类型。

> [!NOTE]
> **HtmlEncode**和**HtmlDecode**功能**System.Web.HttpUtility**类已移到新的.NET Framework 4 **System.Net.WebUtility**类。 如果这是使用的唯一 ASP.NET 功能，应用程序的代码修改为使用新**WebUtility**类。


下面是对 ASP.NET 4 中的默认 CA 实现的更改的简要概述：

- ASP.NET 应用程序域现同类的应用程序域。 应用程序域中可用仅部分信任和完全信任的授予集。
- ASP.NET 部分信任的授予集是独立于任何企业级、 计算机级别或用户级别的 CAS 策略。
- ASP.NET 3.5 和 3.5 SP1 中附带的程序集已转换为使用.NET Framework 4 透明度模型。
- 使用 ASP.NET **AspNetHostingPermission**属性已大幅减少。 已从公共 ASP.NET Api 中删除此属性的大多数实例。
- 更新了动态编译的程序集由 ASP.NET 生成提供程序，以显式标记为透明的程序集。
- 现在，所有 ASP.NET 程序集已都标记仅在 Web 宿主环境中接受为 APTCA 特性的方式。 部分受信任的非 Web 宿主环境如 ClickOnce 不能调入 ASP.NET 程序集。

有关新的 ASP.NET 4 代码访问安全模型的详细信息，请参阅[ASP.NET 应用程序中使用代码访问安全性](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx)MSDN 网站上。

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>在移动 MembershipUser 和 System.Web.Security Namespace 中的其他类型

在 ASP.NET 成员资格中使用某些类型已从已移`System.Web.dll`对新 System.Web.ApplicationServices.dll 程序集。 移动这些类型是为了解析客户端中的类型与扩展的 .NET Framework SKU 中的类型之间的体系结构层依赖关系。

网站项目没有因移动这些类型，而问题原因 System.Web.ApplicationServices.dll ASP.NET 编译系统已添加到引用的程序集的列表中，默认情况下使用。 如果升级通过在 Visual Studio 2010 中打开使用早期版本的 ASP.NET 到 ASP.NET 4 创建的网站项目，项目将编译错误。

同样，如果升级由早期版本的 ASP.NET 到 ASP.NET 4 中在 Visual Studio 2010 中打开一个 Web 应用程序项目时，升级过程将对 System.Web.ApplicationServices.dll 的引用添加到项目。 因此，升级的 Web 应用程序项目还将编译错误。

使用 ASP.NET 的早期版本创建的已编译 （二进制） 文件也会正常运行在 ASP.NET 4 中，即使成员资格类型移到另一个程序集。 类型转发信息已添加到 ASP.NET 4 版的`System.Web.dll`，自动将这些类型的运行时引用路由到的类型的新位置。

但是，类库使用特定的成员资格类型以及从 ASP.NET 的早期版本已升级，将无法编译时在 ASP.NET 4 项目中使用。 例如，一个类库项目可能会失败，若要编译并报告错误如下所示：

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

你可以通过将你的类库项目中的引用添加到 System.Web.ApplicationServices.dll 要解决此问题。

下面的列表显示*System.Web.Security*类型从移动的`System.Web.dll`System.Web.ApplicationServices.dll 到：

- *System.Web.Security.MembershipCreateStatus*
- *System.Web.Security.Membership.CreateUserException*
- *System.Web.Security.MembershipPasswordException*
- *System.Web.Security.MembershipPasswordFormat*
- *System.Web.Security.MembershipProvider*
- *System.Web.Security.MembershipProviderCollection*
- *System.Web.Security.MembershipUser*
- *System.Web.Security.MembershipUserCollection*
- *System.Web.Security.MembershipValidatePasswordEventHandler*
- *System.Web.Security.ValidatePasswordEventArgs*
- *System.Web.Security.RoleProvider*
- <a id="0.1_a"></a>*System.Web.Configuration.MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>输出缓存的更改以改变\*HTTP 标头

在 ASP.NET 1.0 bug 导致缓存指定的页`Location="ServerAndClient"`作为输出 – 缓存设置，以便发出`Vary:*`在响应中的 HTTP 标头。 这还能起到告知客户端浏览器绝不要本地对页进行缓存的作用。

在 ASP.NET 1.1 **System.Web.HttpCachePolicy.SetOmitVaryStar**方法已添加，其可以调用以禁止显示`Vary:*`标头。 此方法已选择原因是更改发出的 HTTP 标头被认为是可能影响重大更改时。 但是，开发人员都会感到困惑由在 ASP.NET 中，行为和 bug 报告建议开发人员没有意识到现有**SetOmitVaryStar**行为。

在 ASP.NET 4 中，决策已做出的修复根本问题。 `Vary:*` HTTP 标头，将不再发出从指定下面的指令的响应：

`<%@OutputCache Location="ServerAndClient" %>`

因此， **SetOmitVaryStar**不再需要以禁止显示`Vary:*`标头。

指定的应用程序中`Location="ServerAndClient"`中**@ OutputCache**指令在页面上，现在，你将看到的名称所暗示的行为**位置**属性的值-即，将页可在你调用而无需浏览器缓存**SetOmitVaryStar**方法。

如果你的应用程序中的页必须发出`Vary:*`，调用**AppendHeader**方法，如下面的示例所示：

`HttpResponse.AppendHeader("Vary","*");`

或者，你可以更改的 caching 的输出值**位置**属性设为"Server"。

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Passport System.Web.Security 类型是已过时

ASP.NET 2.0 中内置的 Passport 支持已过时，不支持几年由于 Passport (现在 LiveID) 中的更改。 因此，五种类型与中的 Passport **System.Web.Security**现在标记为**ObsoleteAttribute**属性。

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>MenuItem.PopOutImageUrl 属性未能呈现 ASP.NET 4 中的图像

在 ASP.NET 3.5 中， *MenuItem.PopOutImageUrl*属性，可以指定显示在菜单项以指示该菜单项具有动态子菜单的图像的 URL。 下面的示例演示如何在 ASP.NET 3.5 中的标记中指定此属性。

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

因 ASP.NET 4 中设计更改，而无输出呈现为*PopOutImageUrl*如果将属性设置为*MenuItem*类。 作为替代，你必须指定直接在图像 URL*菜单*控制使用*StaticPopOutImageUrl*属性或*DynamicPopOutImageUrl*属性。 当您使用静态菜单时*Menu.StaticPopOutImageUrl*属性指定显示，以便指示静态菜单项包含子菜单，如下面的示例中所示的图像的 URL:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

如果你正在使用动态菜单中，则使用*Menu.DynamicPopOutImageUrl*属性指定，该值指示动态菜单项包含子菜单的图像的 URL。 下面的示例类似于前一个，但演示如何设置*DynamicPopOutImageUrl*动态菜单的属性。

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

如果*Menu.DynamicPopOutImageUrl*未设置属性和*Menu.DynamicEnableDefaultPopOutImage*属性设置为*false*，不显示任何图像。 同样，如果*StaticPopOutImageUrl*未设置属性和*StaticEnableDefaultPopOutImage*属性设置为*false*，不显示任何图像。

当你设置这些属性的路径时，则会将正斜杠 （/） 代替反斜杠 (\)。 有关详细信息，请参阅[Menu.StaticPopOutImageUrl 和 Menu.DynamicPopOutImageUrl 故障到呈现图像时路径包含反斜杠](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu。") 在其他位置在本文档中。

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl 和 Menu.DynamicPopOutImageUrl 无法呈现图像时路径包含反斜杠

在 ASP.NET 4 中，使用指定的映像*Menu.StaticPopOutImageUrl*和*Menu.DynamicPopOutImageUrl*属性将不会呈现如果路径包含 backlashes (\)。 这是从 ASP.NET 的早期版本的更改。

下面的示例对*菜单*控件标记显示*StaticPopOutImageUrl*使用包含一个反斜杠的路径设置的属性。 在 ASP.NET 4 中，将不会呈现在属性中指定的映像。

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

若要更正此问题，请更改中指定的路径值*StaticPopOutImageUrl*和*DynamicPopOutImageUrl*要使用正斜杠 （/） 的属性。 下面的示例演示此更改：

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

请注意，已从早期版本的 ASP.NET 到 ASP.NET 4 迁移的应用程序可能也会受到影响，因为*MenuItem.PopOutImageUrl*属性已更改。 有关详细信息，请参阅[MenuItem.PopOutImageUrl 属性未能呈现 ASP.NET 4 中的映像](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert")本文档中的其他位置。

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>免责声明

这是一份初稿，并可能在本文所述软件最终商业发布之前进行大幅更改。

本文所含信息代表 Microsoft Corporation 对截至发布之日所讨论问题持有的当前观点。 由于 Microsoft 必须对不断变化的市场情况作出响应，所以不应将本文解释为是 Microsoft 做出的承诺，Microsoft 并不保证所提供的任何信息在公布之日后的准确性。

本白皮书仅用于提供信息。 MICROSOFT 对本文档中的信息不做任何明示、暗示或法定的担保。

遵守所有适用的著作权法是用户的责任。 未经 Microsoft Corporation 明确的书面许可，不得出于任何目的或以任何形式或任何手段（电子、机械、影印、记录或其他方法）复制本文档的任何部分，或者将其存储或引入检索系统，或者将其进行传播。受版权法保护的权利不受此限制。

对于本文档中的主题，Microsoft 可能具有专利、专利申请、商标、版权或其他知识产权。 除非 Microsoft 的任何书面许可协议明确提出，否则，本文档的提供并不表示 Microsoft 已将这些专利、商标、版权或其他知识产权的任何许可权限授予您。

除非另行说明，示例公司、 组织、 产品、 域名、 电子邮件地址、 徽标、 人物、 地点和事件此处所述虚构，是的无意与任何真实的公司、 组织、 产品、 域名、 电子邮件地址、 徽标、 人员、 位置或事件旨在或妄加推断。

© 2010 Microsoft Corporation. 保留所有权利。

Microsoft 和 Windows 是 Microsoft Corporation 在美国和/或其他国家/地区的注册商标或商标。

此处提到的真实公司和产品的名称可能是其各自所有者的商标。
