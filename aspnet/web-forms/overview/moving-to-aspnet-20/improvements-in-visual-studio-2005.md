---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: "Visual Studio 2005 中的改进 |Microsoft 文档"
author: microsoft
description: "Visual Studio 2005 提供 Web 应用程序开发人员的改进和增强功能，Web 项目的长列表。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: aafc59980e807677d6023110d324365ce92bb5fc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2018
---
<a name="improvements-in-visual-studio-2005"></a>Visual Studio 2005 中的改进
====================
by [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 提供 Web 应用程序开发人员的改进和增强功能，Web 项目的长列表。


Visual Studio 2005 提供 Web 应用程序开发人员的改进和增强功能，Web 项目的长列表。 具有强大的功能与 Visual Studio.NET 2002年和 2003年，许多投诉中包括的 Web 项目已处理的方式。 Visual Studio 2005 将大量新功能添加为了解决这些问题。 有关这些喜欢 Visual Studio.NET 2003 中处理的 Web 应用程序的编译方式，请参见[Web 应用程序项目](https://go.microsoft.com/fwlink/?LinkId=57870)。

在此模块中，也包含 Web 项目创建、 管理和开发中的改进。 在更高版本的模块，涵盖了很好地生成 Web 项目并将它们部署中的改进。

## <a name="frontpage-server-extensions"></a>FrontPage 服务器扩展

Visual Studio.NET 2002年和 2003年所需在框上才能创建或构建 Web 项目的 FrontPage 服务器扩展。 开发人员未之间进行选择两种不同的访问模式 （FrontPage 服务器扩展或文件访问模式），这两种使用 FrontPage 服务器扩展来执行任务，例如在 IIS 等设置的应用程序根目录。

Visual Studio 2005 中移除而言，对于本地项目依赖于 FrontPage 服务器扩展。 Visual Studio 2005 现在访问 IIS 元数据库直接而不是使用 FrontPage 服务器扩展。 Visual Studio 2005 还将添加对 FTP 这允许远程项目访问而无需 FrontPage 服务器扩展的支持。

对于那些开发人员想要在其项目中使用 FrontPage 服务器扩展，该选项是仍然可用。 但是，基于强的 ASP.NET 开发人员社区反馈，它不是一项要求。

> [!NOTE]
> FrontPage 服务器扩展插件是仍然需要远程项目创建、 打开，等等。


## <a name="aspnet-development-server"></a>ASP.NET Development Server

Visual Studio 2005 附带了新的 Web 服务器称为 ASP.NET 开发服务器。 （此 Web 服务器以前称为 Cassini。）

有很多好处的 ASP.NET 开发服务器。

- 现可能非管理员用于开发和调试针对 Web 服务器。
- ASP.NET Development Server 动态映射虚拟目录到的任何位置允许灵活项目位置的文件系统中。
- 现在，已在使用 IIS 在 Windows XP Professional 上的用户将能够创建新的 Web 应用程序将不会影响其默认网站在 IIS 中的文件或文件夹结构。

充分利用 ASP.NET 开发服务器不需要任何特殊的配置。 调试或浏览文件系统承载的 Web 项目后，Visual Studio 2005 在随机端口，以请求提供服务上将自动启动 ASP.NET Development Server 的实例。

在 ASP.NET 开发服务器上更高版本在此模块将在详细信息。

## <a name="improved-file-management"></a>增强对文件管理

在 Visual Studio 2002 和 2003年中，项目文件 (对于 VB.NET.vbproj) 和 C# 中为.csproj 存储在 Web 应用程序中的所有文件的信息。 解决方案资源管理器显示取决于项目文件中的文件信息。 因此，解决方案资源管理器通常将在其中使用外部编辑器的情况下显示不准确的信息。 Visual Studio 2002 和 2003年将通常覆盖文件更改，或者不会显示文件的最新版本。

Visual Studio 2005 项目文件进行消失。 相反，它直接从磁盘，从而导致准确显示你的项目中的文件读取的文件和文件夹信息。 在 Visual Studio 2002 和 2003年引用文件夹不表示 Web 应用程序中的实际文件夹，因为 Visual Studio 2005 还会从解决方案资源管理器中删除引用文件夹。 若要访问 Visual Studio 2005 中的项目的引用，你应使用项目属性页。

## <a name="creating-web-projects"></a>创建 Web 项目

Web 开发人员在 Visual Studio 2005 中有许多可用于项目创建的新选项。 网站文件系统中现在在任何位置创建，然后可以调试或浏览使用新的 ASP.NET 开发服务器。 开发人员还可以创建新的网站使用 FTP。

若要查看在 Visual Studio 2005 中创建 Web 项目的视频演练，请单击此处。


![](improvements-in-visual-studio-2005/_static/image1.png)


[打开全屏幕视频](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>文件系统项目

正如你看到视频演练中，你可以选择在本地计算机上或远程位置通过文件共享上文件系统上创建网站。 在文件系统创建的网站的浏览和调试使用 ASP.NET 开发服务器。

> [!NOTE]
> ASP.NET 开发服务器可能会导致某些混淆的客户。 如果 IISs 目录结构 (即 c: / inetpub/wwwroot) 中的文件系统上创建 Web 项目，则将仍可通过 ASP.NET 开发服务器在从 Visual Studio 2005 内启动时浏览网站。 因此，任何 IIS 配置 （即身份验证方法） 不适用。


大量还会删除默认 web 项目中的系统开销通过仅包括 Default.aspx 页、 default.cs 文件和应用程序/_Data 文件夹。 需要添加 web.config 和特殊文件夹 (即应用程序/_code)。 你的 web 项目仅包括的文件和你需要的文件夹。

### <a name="http-projects"></a>HTTP 项目

HTTP 项目可以是本地 IIS 网站上或远程网站上创建的项目。 默认项目位置`http://localhost`。 如果单击浏览按钮时，有两个 HTTP 选项： 本地 IIS 和远程站点。 在这两个选项中的主要区别是 web 站点的信息在其中显示在选择位置对话框中和如何将文件复制到 Web 服务器的方法。

本地 IIS 选项从本地计算机上的元数据库读取站点信息并将文件复制使用文件系统。 远程站点选项使用 FrontPage 服务器扩展和站点信息和文件将复制使用 HTTP，FrontPage 服务器扩展 RPC 调用。

> [!NOTE]
> Vs###/_tmp.htm 文件和 get/_aspx/_ver.aspx 不再用于确定版本信息。


默认 HTTP 选项是本地 IIS。 此选项读取 IIS 元数据库，若要确定哪些站点可用，并要在其中创建内容的位置。 可以通过在树视图中选择该选择不同的文件夹或虚拟目录。 你可以还创建一个新的虚拟目录，将标记为应用程序的文件夹，以及从该对话框中删除现有的虚拟目录。


![选择位置对话框](improvements-in-visual-studio-2005/_static/image1.gif)

**图 1**: 选择位置对话框


与不同在早期版本的 Visual Studio 中，如果你检查**使用安全套接字层**复选框和 SSL 证书与您在浏览的 URL 不匹配，你将看到一个安全警报的对话框，询问你是否你将要处理。 使用 Visual Studio.NET 2003 中，如果证书不是一个匹配，创建项目将失败。


![安全警报的有关 SSL 证书](improvements-in-visual-studio-2005/_static/image2.gif)

**图 2**： 有关 SSL 证书的安全警报


### <a name="note-on-host-headers"></a>有关主机头的说明

如果要在一个绑定到特定 IP 的站点创建 Web 应用程序，你将需要确保配置主机标头。 否则，Visual Studio 将创建以下站点`http://localhost`，但站点处于浏览或从 IDE 中调试时，IP 地址将无法正确解析。

如果选择远程站点选项，该对话框会更改，以便您可以输入新的 Web 站点的目标 URL。 此 URL 必须是已启用 FrontPage 服务器扩展的服务器上。 如果你想要使用你本地 Web 服务器上使用 FrontPage 服务器扩展，你可以使用远程站点选项并指定本地 URL。


![在远程服务器上创建网站](improvements-in-visual-studio-2005/_static/image1.jpg)

**图 3**： 在远程服务器上创建网站


如果不匹配的 SSL 证书，请在通过 SSL，远程站点上创建应用程序，确认对话框时，略有不同于使用本地 IIS 选项时显示的对话框。


![远程站点安全警报](improvements-in-visual-studio-2005/_static/image3.gif)

**图 4**： 远程站点安全警报


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 中引入了用于创建通过 FTP 的网站的选项。 当你使用此选项时，IDE 的用户临时文件夹中本地创建文件，然后使用 FTP 将文件移到的 FTP 位置。

> [!NOTE]
> 临时文件夹位置是 c: / Documents and Settings /&lt;用户&gt;/本地设置/Temp/VWDWebCache/&lt;服务器&gt;/_&lt;应用程序名称&gt;


使用 FTP 选项时，你将显示选择位置对话框。 在此对话框，如下所示输入所需的 FTP 连接信息。


![选择 ftp 位置对话框](improvements-in-visual-studio-2005/_static/image2.jpg)

**图 5**: 选择 ftp 位置对话框


## <a name="lab-setup-ftp-site-and-create-a-project"></a>实验室： 设置 FTP 站点，并创建一个项目

以下步骤配置的 FTP 站点，使用户具有只有他们可以通过 FTP 上载到的位置。

### <a name="install-the-ftp-service"></a>安装 FTP 服务

1. 打开添加或删除程序，选择添加/删除 Windows 组件
2. 选择 Internet 信息服务 （Windows 2003 上的应用程序服务器），然后单击**详细信息**。
3. 检查**文件传输协议 (FTP) 服务**单击**确定**。
4. 单击**下一步**安装 FTP 服务。

### <a name="create-a-new-folder-for-content"></a>为内容创建一个新文件夹

1. 在 Windows 资源管理器，创建一个名为的新文件夹**User1** c: / inetpub/wwwroot 内。

#### <a name="configure-folders-and-permissions-on-folders"></a>在文件夹上配置文件夹和权限。

1. 从管理工具打开 Internet 信息服务管理单元中。 现在，你将有计算机名称节点下的 FTP 站点文件夹。
2. 展开**FTP 站点**。
3. 右键单击**默认 FTP 站点**，选择**新建**，然后**虚拟目录**，然后单击**下一步**。
4. 输入**User1**虚拟目录名称然后单击**下一步**。
5. 输入**c: / inetpub/wwwroot/User1**的路径和单击**下一步**。
6. 单击**下一步**然后**完成**以完成向导。
7. 右键单击**User1**默认 FTP 站点，然后选择下的虚拟目录**属性**。
8. 检查**编写**复选框，然后单击**确定**关闭对话框。
9. 右键单击**默认 FTP 站点**和选择**属性**。
10. 上**安全帐户**选项卡上，取消选中**允许匿名连接**。
11. 单击**是**询问您是否要继续对话框中。
12. 单击**确定**关闭对话框。
13. 展开**Default Web Site**下**网站**节点。
14. 右键单击**User1**目录，然后选择**属性**
15. 在**应用程序设置**部分中，单击**创建**将标记为应用程序的文件夹。
16. 单击**确定**关闭对话框。
17. 关闭 Internet Information Services 管理单元。

### <a name="create-web-project"></a>创建 web 项目

1. 打开 Visual Studio 2005。
2. 从**文件**菜单上，选择**新网站**。
3. 在**位置**下拉列表中，选择**FTP**。
4. 单击“浏览” 。
5. 输入**localhost**中**服务器**文本框。
6. 输入**User1**在目录文本框中。
7. 单击**打开**。 FTP 位置将输入到新建网站对话框。
8. 单击 **“确定”**。
9. 取消选中**匿名登录**在 FTP 登录对话框中，输入你的凭据，然后单击**确定**。
10. 项目的 URL 是什么？ （该项目的 URL 将显示在解决方案资源管理器。）
11. 从**生成**菜单上，选择**生成网站**或**生成解决方案**。
12. 右键单击解决方案资源管理器中的 Default.aspx，然后选择**用浏览器查看**。
13. 在要求提供网站 URL 对话框中，输入`http://localhost/user1`的 URL，然后单击**确定**。

> [!NOTE]
> 如果你收到错误，表示无法加载类型 /_Default，请确保您的网站和不早期版本上运行 ASP.NET 2.0。 你可以从在 Internet 信息服务中的 ASP.NET 选项卡来执行此操作。


## <a name="opening-web-projects"></a>打开 Web 项目

打开 Web 项目是类似于创建项目。 下列各节调出区域以跟踪出有关 IDE 中工作时。 它还介绍了与使用 HTTP 和 FTP 的 Web 项目的工作。

若要打开 Web 项目，请从文件菜单中选择打开网站。 系统将提示您提供以前涵盖相同选择位置对话框，并可以供你使用相同的四个选项： 文件系统、 本地 IIS、 FTP 和远程站点。

<a id="_Toc116100245"></a>

## <a name="file-system"></a>文件系统

如上所述此模块中，Visual Studio 将无法再使用项目文件。 因此，如果你选择从文件系统中打开的网站，实际上可以选择你想，任何文件夹，即使你选择的文件夹不作为最初在 Visual Studio 中的 Web 项目创建。 例如，你可以选择打开作为网站的我的文档文件夹和 Visual Studio 将不假思索地将其打开并显示你的文件，如下所示。


![我作为网站打开的文档](improvements-in-visual-studio-2005/_static/image3.jpg)

**图 6**:*我的文档*作为网站打开


因为 Visual Studio 将仅创建其他文件和文件夹在必要时，没有其他文件或文件夹将添加到你打开的位置。 此体系结构的副作用是，它会阻止你嵌套网站文件系统上。 例如，考虑以下目录结构。

在 c: / MyWebSite 的 web 项目

在 c: / MyWebSite/嵌套的另一个 web 项目

打开位于 c: / MyWebSite 的网站时，嵌套文件夹将显示为该应用程序的一个子文件夹中。

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

在打开时通过 HTTP 的网站，将读取设置，通过 IIS 元数据库 (本地 IIS) 或使用 FrontPage 服务器扩展 （远程站点。）如果有嵌套的 web 应用程序，这些是还显示与将它们标识为应用程序的图标。 如果你熟悉使用 FrontPage 中 web 应用程序，Visual Studio 2005 中的行为非常相似。

即使 Visual Studio 将显示在 IDE 中当前打开的应用程序下嵌套的应用程序的图标，它将不允许你展开它们以查看其内容。 但是，可以双击它们以打开它们。 执行操作时，你将显示一个对话框，提示您打开 web 应用程序 （并替换当前打开的解决方案），也添加到当前解决方案的 Web 应用程序。


![双击一个嵌套的应用程序图标展示了此对话框](improvements-in-visual-studio-2005/_static/image4.jpg)

**图 7**： 双击嵌套的应用程序图标展示了此对话框


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>FTP 站点

当你打开通过 FTP 站点时，文件所有本地复制到临时文件夹。 本地存储位置的完整路径显示在项目属性窗格中，并使用以下格式进行创建。

C: / Documents and Settings /&lt;用户&gt;/本地设置/Temp/VWDWebCache/&lt;服务器&gt;/_&lt;应用程序名称&gt;

使用 FTP 时，Visual Studio 将需要指定你的项目的基 URL，以便你可以浏览它，如下所示。 如果未指定基 URL，Visual Studio 会要求您为其第一次尝试浏览网站中的页。


![为 FTP 站点指定的基 URL](improvements-in-visual-studio-2005/_static/image5.jpg)

**图 8**： 为 FTP 站点指定的基 URL


## <a name="improvements-in-compilation"></a>在编译中的改进

使用 Visual Studio 2005 中的 Web 应用程序是显著比以前版本更快。 这是因为在没有小部分编译体系结构中的更改。

在 Visual Studio 2002 和 2003年中，Web 应用程序进行了编译到驻留在 /bin 文件夹中的一个主要的程序集。 在 Visual Studio 2005 中，已添加的应用程序/_Code 文件夹。 类和其他非 UI 代码添加到应用程序/_Code 文件夹中。 Visual Studio 生成项目，应用程序/_Code 文件夹中的所有文件都编译为单个 App/_Code.dll 文件中。 此更改的结果是，随后生成要短得比在早期版本中。

> [!NOTE]
> MSBuild 命令行实用工具还可用来生成 ASP.NET Web 应用程序。 该工具将在模块 9 中介绍。


另一编译增强功能是在生成菜单上的新生成页选项。 此功能允许开发人员，以便可以更快地编译更改重新生成仅在当前网页 （连同，过程中，和依赖项）。 由于 C# 不提供的更新 IntelliSense 等目的的后台编译，它们将起到极大从此功能受益因为这可以使 intellisense 只需重新生成单个页面快速更新。

一个项目的生成属性，可以配置执行的启动页前发生的生成的类型。 开发人员可以选择仅生成当前页，以便 Visual Studio 可以开始在代码更改后更快地调试应用程序。


![生成页开始操作](improvements-in-visual-studio-2005/_static/image6.jpg)

**图 9**： 生成页开始操作


到 Visual Studio 和 ASP.NET 体系结构的另一个很好的增强功能的编辑区域中，并继续。 在 Visual Studio 2005 中，开发人员可以开始调试项目，而无需分离调试器对项目进行代码更改。 事实上，你可以按原义开始调试项目、 添加新的类，将代码添加到该类、 将代码添加到你创建此类的新实例的页和执行类，所有操作都无分离调试器的方法。 执行新的代码都按其原义易如反掌刷新浏览器 ！

单击此处可查看视频演练编辑和继续在 Visual Studio 2005 中的功能。


![](improvements-in-visual-studio-2005/_static/image2.png)


[打开全屏幕视频](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


可靠的编辑并继续在 ASP.NET 2.0 中的功能和 Visual Studio 2005 是由于 ASP.NET 应用程序的体系结构更改。 在 ASP.NET 中 1.x，在 Visual Studio 2002/2003 中创建的应用程序编译到 /bin 文件夹中存储的主程序集中。 所有类，页，等的应用程序已编译成一个 DLL。 然后在运行时，ASP.NET 将编译的所有控件、 标记和在的页内的 ASP.NET 代码，并将这些 Dll 复制到 ASP.NET 临时文件夹。

使用 ASP.NET 2.0 中，这两个编译模型概述上面 （一个用于 Visual Studio），另一个用于 ASP.NET 在运行时的 Visual Studio 2005 中已合并到一个通用的编译模型。 这意味着，所有编译问题现在都捕获在开发阶段而不是在运行时。 它还允许为设计器和功能，如用户控件和母版页的 IntelliSense 支持。

若要查看用户控件的设计器支持的视频演练，请单击此处。


![](improvements-in-visual-studio-2005/_static/image3.png)


[打开全屏幕视频](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> 从页面中，删除用户控件时@Register指令保留在标记中，并且应手动删除以避免分析器错误，如果从网站中删除用户控件。


Visual Studio 编译模型中的另一个改进是发布网站功能。 因为发布功能对预编译网站，开发人员可以享受的无需编译任何按需添加的性能。 它还预编译的应用程序/_Code 文件夹中的所有源代码为 DLL，以便在没有源代码必须进行部署。


![发布网站对话框](improvements-in-visual-studio-2005/_static/image7.jpg)

**图 10**： 发布 Web 站点对话框


> [!NOTE]
> Aspnet/_compile.exe 实用工具还可以使用预编译的 ASP.NET Web 应用。 该工具将在模块 9 中介绍。


当你发布网站，预编译的文件存储在临时 ASP.NET 文件的文件夹如下所示。 文件都具有*为.compiled*文件扩展名是特定的 Dll 定义的依赖项的 XML 文件。 任何 web 窗体或用户控件被编译为开头的随机 Dll*应用 /_Web /_*。

如果你离开*允许可更新此预编译的网站*选中复选框，内部 Webforms 和用户控件的标记将不会预编译为 DLL，允许你在部署后进行更改。 如果你想要锁定标记，以便不允许对部署的内容的更改，请取消选中此框。

*使用固定命名和单个页程序集*复选框，您可以禁用批处理编译，以便每个页面会编译成程序集具有固定名称。 将此框保留为未选中允许你利用批处理编译。

*启用强命名在预编译的程序集*复选框可将强名称你预编译的程序集。

> [!NOTE]
> 在 ASP.NET 中 1.x，强名称的程序集必须安装到全局程序集缓存 (GAC) 中。 在 ASP.NET 2.0 中，则不需要具有强名称程序集安装到 GAC。


![ASP.NET 应用程序预编译的文件](improvements-in-visual-studio-2005/_static/image8.jpg)

**图 11**: ASP.NET 应用程序预编译的文件


> [!NOTE]
> 在上面的应用中，没有 web.config 文件。 如果没有过，它将调用了*PrecompiledApp.config*站点过程之后发布 Web。


## <a name="improvements-in-deployment"></a>部署中的改进

作为 Visual Studio 2002 和 2003，Visual Studio 2005 提供的复制项目功能。 但是，该功能在 Visual Studio 2005 中已得到增强，并为现在称为复制网站。

复制网站对话框中被划分为左的框架和右侧的框架。 左的框架中称为源 Web 站点，右栏中称为远程网站。 可能混淆一些开发人员的一点是，在右栏中显示的站点不一定是远程站点。 这可能是在本地文件系统上或在 IIS 的本地实例上的站点。 此外，显示在左框架中的站点不一定源 Web 站点因为对话框允许你从远程网站上发布*到*源 Web 站点。

如果要将项目复制到远程网站，该站点必须具有在其上安装的 FrontPage 服务器扩展。 如果没有，你将需要使用 FTP 进行连接。 另一方面，如果将项目复制到本地 IIS 实例，则不需要 FrontPage 服务器扩展。

> [!NOTE]
> 如果你尝试在本地 IIS 实例上创建新网站，且安装 FrontPage 2002 Server Extensions，你将获取错误消息，告诉你，创建网站不支持在 SharePoint 服务器上。 在这种情况下，你可以选择安装 FrontPage 2000 服务器扩展或删除 FrontPage 服务器扩展。


有关复制网站功能的视频演练，请单击此处。


![](improvements-in-visual-studio-2005/_static/image4.png)


[打开全屏幕视频](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>在调试中的改进

有四个重要改进调试 Visual Studio 2005 中。

- 以非管理员身份在进行本地调试有可能在初始状态。
- Compilation 元素的调试属性现 false 默认情况下。
- 远程调试设置和配置是比之前更。
- 现在可以通过 FTP 位置打开的网站进行调试。

## <a name="debugging-as-a-non-administrator"></a>以非管理员身份调试

添加 ASP.NET 开发服务器允许非管理员可以轻松地调试现成的 ASP.NET 应用程序。 调试本地文件系统上运行的 ASP.NET 应用程序时, Visual Studio 将启动登录的用户的上下文中的 ASP.NET 开发服务器。 然后，该用户可以调试该应用程序无需任何其他配置。

## <a name="debug-is-false-by-default"></a>调试为 False 默认情况下

在 ASP.NET 中 1.x*调试*属性中*编译*web.config 文件元素设置为*true*默认情况下。 始终具有已以下建议开发人员将此属性设置为*false*之前应用程序部署到生产环境，但是，由于大多数开发人员完全不了解将调试属性设置为保留的后果为 true，它们只需从左其作为-是。

最严重的问题与具有调试属性设置为 true 是，它会禁用 ASP.NETs 批处理编译模型。 因此，每个页面会编译成单独的 DLL。 如果 Web 应用程序包含数千个页面 （不前所未有的通过任何方式），它表示该应用程序将创建若干个千位小 Dll。 虽然这些 Dll 很小，它们不是加载到内存中的任何特定位置。 因此，它们在系统内存中导致碎片，并且可参与 OutOfMemoryException 匹配项。

在 ASP.NET 2.0 中，默认情况下调试属性设置为 false。 如你已经看到，当开发人员调试 ASP.NET 应用程序在 Visual Studio 2005 中，系统会提示他们添加启用了调试的 web.config 文件。 这样做会导致在 ASP.NET 中存在的相同缺点 1.x，但现在开发人员清楚地发出警告，该属性应重置为 false 然后再移动到生产应用程序。

## <a name="remote-debugging-setup-and-configuration"></a>远程调试设置和配置

在 Visual Studio 2002/2003 中，远程调试依赖于计算机调试管理器 (mdm.exe) 和 vs7jit.exe 过程。 正因如此，解决远程调试的问题通常是客户黑色框和它通常不是更好的产品支持部门联系。

Visual Studio 2005 中移除而言，依赖于在 mdm.exe 和 vs7jit.exe 过程。 相反，它现在使用远程调试监视器服务 (msvsmon.exe)。

远程调试 Visual Studio 2005 中的要求是非常简单。 你需要在调试之前在远程服务器上运行 msvsmon.exe。 你可以从 Visual Studio CD 安装远程调试监视器或只需运行 msvsmon.exe 从共享不会在所有 Web 服务器上安装任何内容。

当你运行 msvsmon.exe 时，则很可能将抱怨有关端口被阻止进行远程调试。 幸运的是，你可以轻松地取消阻止端口，从警告对话框中的权限如下所示。


![Windows 防火墙处于阻止远程调试的通知](improvements-in-visual-studio-2005/_static/image9.jpg)

**图 12**： 通知 Windows 防火墙，则阻止远程调试


你已取消阻止后调试所需的端口，你将看到远程调试监视器，如下所示。 此接口，从你可以监视连接并更改轻松地调试权限。


![远程调试监视器](improvements-in-visual-studio-2005/_static/image10.jpg)

**图 13**： 远程调试监视器


还有可能远程调试 Web 应用程序通过 FTP 打开。 步骤是方式与之前介绍的相同。 但是，你将需要指定用于浏览 FTP 项目此模块中前面所述的基 URL。

## <a name="lab-2"></a>Lab 2

## <a name="remote-debugging-with-visual-studio-2005"></a>使用 Visual Studio 2005 的远程调试

此实验室将引导你完成使用 Visual Studio 2005 的远程调试。

有关本实验的视频演练，请单击此处。


![](improvements-in-visual-studio-2005/_static/image5.png)


[打开全屏幕视频](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


此实验室要求你拥有两台计算机，一个正在运行的 Visual Studio 2005 和其他正在运行 IIS 5 或更高版本。

1. 打开 Visual Studio 2005 并在远程服务器上创建新网站。

> [!NOTE]
> 远程 IIS 实例上或通过 FTP，可以创建网站。


1. 在远程 Web 服务器上，使用 UNC 路径的开发计算机上找到 msvsmon.exe 并执行它。  
 Msvsmon.exe 的默认位置是 //server/c$/Program 文件/Microsoft Visual Studio 8/Common7/IDE/远程调试器/x86。
2. 如果系统提示您取消阻止端口进行远程调试，这样做。
3. 从开发计算机上，打开 Default.aspx 代码隐藏和页/（_l） 方法中设置断点。
4. 从开发计算机开始调试。

你应按预期方式命中该断点。

## <a name="aspnet-development-server"></a>ASP.NET Development Server

如我们前面已经讨论过，Visual Studio 2005 附带称为 ASP.NET 开发服务器的 Web 服务器。 （ASP.NET 开发服务器有时称为 Cassini。）此 Web 服务器是一种方便的方式浏览和调试 Web 应用程序在文件系统上运行。

ASP.NET 开发服务器是受限制的 Web 服务器。 它不允许远程连接，因此它不允许任何请求启动 Web 服务器的用户以外的任何用户。 它还没有为 ASP 页面提供服务的功能。 仅 ASP.NET 资源和 HTML 资源 （包括图像、 CSS 文件等） 进行处理。

可以通过运行位于 c:/Windows/Microsoft.NET/Framework/v2.0./ WebDev.WebServer.exe 文件通过命令行启动 ASP.NET Development Server */*  /  */*/*. 以下对话框显示可用的参数。


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**图 14**


> [!NOTE]
> ASP.NET Development Server 不支持通过命令行显式启动时。
