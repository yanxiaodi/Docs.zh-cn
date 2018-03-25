---
title: "使用 ASP.NET Core 中的 Bower"
author: rick-anderson
description: "管理 Bower 的客户端包。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: 67695843846cfaf1619db11a7bffcc65802e0f69
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>管理 ASP.NET Core 中的 Bower 的客户端包

通过[Rick Anderson](https://twitter.com/RickAndMSFT)，[了米](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)，和[Scott Addie](https://scottaddie.com) 

> [!IMPORTANT]
> 而维持 Bower，其 maintainer 建议使用另一种解决方案。 与 Webpack yarn 是一个常用的替代项为其[迁移说明](https://bower.io/blog/2017/how-to-migrate-away-from-bower/)可用。

[Bower](https://bower.io/)调用自身"web 程序包管理器"。 内部.NET 生态系统，它将填入 void 留下的 NuGet 的无法传送静态内容的文件。 对于 ASP.NET Core 项目，这些静态文件，则所固有的客户端库，如[jQuery](http://jquery.com/)和[Bootstrap](http://getbootstrap.com/)。 对于.NET 库，你仍然使用[NuGet](https://www.nuget.org/)程序包管理器。

设置客户端的 ASP.NET Core 项目模板创建的新项目生成过程。 [jQuery](http://jquery.com/)和[Bootstrap](http://getbootstrap.com/)安装，并支持 Bower。

在列出客户端包*bower.json*文件。 ASP.NET 核心项目模板配置*bower.json* jQuery、 jQuery 验证与 Bootstrap。

在本教程中，我们将添加对支持[字体出色](http://fontawesome.io)。 可以使用安装 bower 包**管理 Bower 包**UI 或手动在*bower.json*文件。

### <a name="installation-via-manage-bower-packages-ui"></a>通过管理 Bower 包 UI 的安装

* 创建新的 ASP.NET 核心 Web 应用程序与**ASP.NET 核心 Web 应用程序 (.NET Core)**模板。 选择**Web 应用程序**和**无身份验证**。

* 右键单击解决方案资源管理器中的项目并选择**管理 Bower 包**(或者从主菜单中，**项目** > **管理 Bower 包**).

* 在**Bower:\<项目名称\>**窗口中，单击"浏览"选项卡，并输入，然后筛选包列表`font-awesome`的搜索框中：

 ![管理 bower 包](bower/_static/manage-bower-packages.png)

* 确认"保存更改为*bower.json*"复选框已选中。 从下拉列表中选择一个版本，然后单击**安装**按钮。 **输出**窗口显示的安装详细信息。

### <a name="manual-installation-in-bowerjson"></a>手动安装在 bower.json

打开*bower.json*文件并添加到的依赖项的"字体出色"。 IntelliSense 会显示可用的包。 某个包被选中，将显示可用的版本。 下面的映像不较旧，并不会匹配你看到的内容。

![Bower 包资源管理器的智能感知](bower/_static/add-package.png)

![bower 版本 IntelliSense](bower/_static/version-intelliSense.png)

Bower 使用[语义版本控制](http://semver.org/)来组织依赖关系。 语义版本控制，也称为 SemVer，标识包的编号方案\<主要 >。\<次要 >。\<修补程序 >。 IntelliSense 显示仅几个常用的选项，从而简化了语义版本控制。 IntelliSense 列表 (在上面的示例 4.6.3) 中的顶级项被视为包的最新稳定版本。 脱字号 (^) 符号匹配的最新的主版本和波形符 （~） 与最新次要版本匹配。

保存*bower.json*文件。 Visual Studio 监视*bower.json*文件的更改。 保存后， *bower 安装*执行命令。 请参见输出窗口**Bower/npm**视图执行的确切命令。

打开*.bowerrc*文件下*bower.json*。 `directory`属性设置为*wwwroot/lib*指示的位置 Bower 将安装包资产。

```json
{
 "directory": "wwwroot/lib"
}
```

在解决方案资源管理器搜索框中可用于查找并显示字体出色的包。

打开*views/shared\_Layout.cshtml*文件并将字体出色的 CSS 文件添加到环境[标记帮助器](xref:mvc/views/tag-helpers/intro)为`Development`。 从解决方案资源管理器，将拖*字体 awesome.css*内`<environment names="Development">`元素。

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

在生产应用程序会添加*字体 awesome.min.css*到环境标记帮助器`Staging,Production`。

内容替换*Views\Home\About.cshtml* Razor 文件替换为以下标记：

[!code-html[](bower/sample/About.cshtml)]

运行应用程序并导航到关于视图，以验证字体出色包正常运行。

## <a name="exploring-the-client-side-build-process"></a>浏览客户端生成过程

大多数 ASP.NET Core 项目模板已配置为使用 Bower。 此下一个演练中创建空的 ASP.NET Core 项目启动，并手动添加每个部分，以便您可以如何在项目中使用 Bower 获取获得感觉。 你可以查看到的项目结构和运行时，输出会和每个配置更改会发生什么情况。

将客户端生成过程用于 Bower 的常规步骤如下：

* 定义项目中使用的包。 <!-- once defined, you don't need to download them, VS does -->
* 从 web 页面的引用包。

### <a name="define-packages"></a>定义包

一旦列表中的包*bower.json*文件，Visual Studio 将下载它们。 下面的示例使用 Bower 加载 jQuery 和引导定向到*wwwroot*文件夹。

* 创建新的 ASP.NET 核心 Web 应用程序与**ASP.NET 核心 Web 应用程序 (.NET Core)**模板。 选择**空**项目模板，然后单击**确定**。

* 在解决方案资源管理器，右键单击项目 >**添加新项**和选择**Bower 配置文件**。 注意： A *.bowerrc*还添加文件。

* 打开*bower.json*，并添加 jquery 和引导到`dependencies`部分。 生成*bower.json*文件将如下所示下面的示例。 版本将会发生更改，并且可能不匹配下图所示。

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* 保存*bower.json*文件。

 验证该项目包括*bootstrap*和*jQuery*中的目录*wwwroot/lib*。 Bower 使用*.bowerrc*文件以安装中的资产*wwwroot/lib*。

 注意:"管理 Bower 包"UI 提供手动文件编辑的替代方法。

### <a name="enable-static-files"></a>启用静态文件

* 添加`Microsoft.AspNetCore.StaticFiles`到项目的 NuGet 包。
* 启用静态文件提供与[静态文件中间件](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)。 添加对的调用[UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)到`Configure`方法`Startup`。

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>引用包

在本部分中，你将创建 HTML 页以验证它可以访问已部署的包。

* 添加一个名为的新 HTML 页*Index.html*到*wwwroot*文件夹。 注意： 你必须将添加到的 HTML 文件*wwwroot*文件夹。 默认情况下，静态内容无法提供外部*wwwroot*。 请参阅[使用静态文件](xref:fundamentals/static-files)有关详细信息。

 内容替换*Index.html*替换为以下标记：

[!code-html[](bower/sample/Index.html)]

* 运行应用程序并导航到`http://localhost:<port>/Index.html`。 或者，使用*Index.html*打开，按`Ctrl+Shift+W`。 验证应用 jumbotron 样式，jQuery 代码响应时单击该按钮，以及启动按钮更改状态。

 ![应用的 jumbotron 样式](bower/_static/jumbotron.png)
