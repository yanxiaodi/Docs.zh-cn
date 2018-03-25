---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: "使用 Visual Studio 的 ASP.NET Web 部署： 命令行部署 |Microsoft 文档"
author: tdykstra
description: "本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，使用的..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8446b3fc05e3ef4a5a30c753c989252fd7f1a56f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>使用 Visual Studio 的 ASP.NET Web 部署： 命令行部署
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，通过使用 Visual Studio 2012 或 Visual Studio 2010。 有关序列的信息，请参阅[序列中的第一个教程](introduction.md)。


## <a name="overview"></a>概述

本教程演示如何调用 Visual Studio web 发布管道从命令行。 这是适用于方案中，你希望[自动执行部署过程](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)而不要手动 Visual Studio 中，通常通过使用[源代码版本控制系统中](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)。

## <a name="make-a-change-to-deploy"></a>若要部署更改

当前关于页面显示的模板代码。

![有关使用模板代码页](command-line-deployment/_static/image1.png)

你将将其替换为显示学生登记的摘要的代码。

打开*About.aspx*页上，删除所有内部标记`MainContent``Content`元素，并插入其位置中的以下标记：

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

运行该项目并选择**有关**页。

![有关页面](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>通过使用命令行，将部署到测试

你不会部署另一个数据库更改，因此禁用 dbDacFx 数据库部署 aspnet ContosoUniversity 数据库。 打开**发布 Web**向导，以及在这三个发布配置文件中，清除**更新数据库**上的复选框**设置**选项卡。

在 Windows 8 起始页中，搜索**vs2012 的开发人员命令提示**。

右键单击的图标**vs2012 的开发人员命令提示**单击**以管理员身份运行**。

输入以下命令在命令提示符下，将解决方案文件的路径替换为你的解决方案文件的路径：

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild 生成解决方案，并将其部署到测试环境。

![命令行输出](command-line-deployment/_static/image3.png)

打开浏览器并转到`http://localhost/ContosoUniversity`，然后单击**有关**页后，可以验证部署是否成功。

如果你尚未创建任何学生在测试中，你将看到在下的一个空页**学生正文统计信息**标题。 转到**学生**页上，单击**添加学生**，和添加一些学生，然后返回到**有关**页后，可以查看学生统计信息。

![有关测试环境中的页面](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>密钥的命令行选项

你输入的命令传递到 MSBuild 的解决方案文件路径和两个属性：

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>部署与部署单个项目解决方案

指定的解决方案文件导致要生成解决方案中的所有项目。 如果解决方案中有多个 web 项目，则 MSBuild 适用以下行为：

- 在命令行指定的属性传递给每个项目。 因此，每个 web 项目都必须具有你指定的名称的发布配置文件。 如果指定`/p:PublishProfile=Test`，每个 web 项目都必须具有名为的发布配置文件*测试*。
- 即使不生成另一个时，可能会成功发布一个项目。 有关详细信息，请参阅 stackoverflow 线程[MSBuild 失败，两个包](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages)。

如果指定单个项目而不是一种解决方案，你必须添加一个参数，指定 Visual Studio 版本。 如果你正在使用 Visual Studio 2012 命令行应类似于下面的示例：

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Visual Studio 2010 版本的版本号为 10.0。 有关详细信息，请参阅[Visual Studio 项目兼容性和 VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi 博客上。

### <a name="specifying-the-publish-profile"></a>指定的发布配置文件

按名称或完整路径，可以指定发布配置文件*.pubxml*文件，如下面的示例中所示：

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Web 发布命令行发布支持的方法

三个发布方法支持用于命令行发布：

- `MSDeploy`-通过使用 Web 部署发布。
- `Package`通过创建 Web 部署包发布。 必须独立于创建它的 MSBuild 命令安装包。
- `FileSystem`通过将文件复制到指定的文件夹发布。

### <a name="specifying-the-build-configuration-and-platform"></a>指定生成配置和平台

在 Visual Studio 或命令行上，必须设置生成配置和平台。 发布配置文件还包括命名的属性`LastUsedBuildConfiguration`和`LastUsedPlatform`，但无法设置这些属性，以确定如何生成该项目。 有关详细信息，请参阅[MSBuild： 如何设置配置属性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)Sayed Hashimi 博客上。

## <a name="deploy-to-staging"></a>部署到过渡环境

若要部署到 Azure，必须将密码添加到命令行中。 如果您在 Visual Studio 中的发布配置文件中保存了密码，它已存储在加密形式你*。 pubxml.user*文件。 当你执行操作的命令行部署，因此你必须在密码命令行参数中传递，MSBuild 不访问该文件。

1. 复制中所需的密码*.publishsettings*前面为过渡网站下载的文件。 密码是值`userPWD`用于 Web 部署的属性`publishProfile`元素。

    ![Web 部署密码](command-line-deployment/_static/image5.png)
2. 在 Windows 8 起始页中，搜索**vs2012 的开发人员命令提示**，然后单击图标以打开命令提示符。 （你不需要它以管理员身份打开这一次因为你不部署到 IIS 在本地计算机上。）
3. 输入以下命令在命令提示符下，解决方案文件的路径替换为你的解决方案文件和你的密码与密码的路径：

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    请注意，此命令行包含了额外的参数： `/p:AllowUntrustedCertificate=true`。 本教程正在编写的如`AllowUntrustedCertificate`属性必须设置从命令行发布到 Azure 时。 当发布此 bug 修复后时，你不需要该参数。
4. 打开浏览器并转至你暂存站点的 URL，然后单击**有关**页后，可以验证部署是否成功。

    如你此前看到的针对测试环境，你可能需要创建一些学生上查看统计信息**有关**页。

## <a name="deploy-to-production"></a>部署到生产环境

部署到生产环境的过程是临时的过程相似。

1. 复制中所需的密码*.publishsettings*前面为生产 web 站点下载的文件。
2. 打开**vs2012 的开发人员命令提示**。
3. 输入以下命令在命令提示符下，解决方案文件的路径替换为你的解决方案文件和你的密码与密码的路径：

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    用于实际生产站点时，如果还没有数据库更改，你会通常将复制*应用\_offline.htm*文件之前，作为部署站点并将其删除成功部署后。
4. 打开浏览器并转至你暂存站点的 URL，然后单击**有关**页后，可以验证部署是否成功。

## <a name="summary"></a>摘要

现在，你已通过使用命令行来部署应用程序更新。

![有关测试环境中的页面](command-line-deployment/_static/image6.png)

在下一步的教程中，你将看到举例说明如何扩展 web 发布管道。 该示例将演示如何部署应用程序不包括在项目文件。

>[!div class="step-by-step"]
[上一页](deploying-a-database-update.md)
[下一页](deploying-extra-files.md)
