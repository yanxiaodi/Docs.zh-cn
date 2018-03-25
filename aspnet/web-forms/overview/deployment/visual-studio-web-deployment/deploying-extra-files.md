---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: "使用 Visual Studio 的 ASP.NET Web 部署： 部署额外文件 |Microsoft 文档"
author: tdykstra
description: "本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，使用的..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 46e18ba81c3db8bb04c5cb997bcc1607e4e38bae
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署额外文件
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，通过使用 Visual Studio 2012 或 Visual Studio 2010。 有关序列的信息，请参阅[序列中的第一个教程](introduction.md)。


## <a name="overview"></a>概述

本教程演示如何扩展 Visual Studio web 发布管道在部署过程中执行其他任务。 任务是复制未向目标 web 站点的项目文件夹中的额外文件。

对于本教程中，你将复制一个额外的文件： *robots.txt*。 你想要将此文件部署到过渡环境，但不适用于生产。 在[到生产环境部署](deploying-to-production.md)教程，则此文件添加到项目，配置生产发布配置文件中排除它。 在本教程中，你将看到一种替代方法来处理此情况下，一个对于你想要部署，但不想在项目中包含的任何文件都很有用。

## <a name="move-the-robotstxt-file"></a>移动 robots.txt 文件

若要准备处理的不同方法*robots.txt*、 本教程的本部分中你将文件移动到未包含在项目中，文件夹和您删除*robots.txt*从暂存环境。 它是删除该文件从过渡，以便你可以验证，你将文件部署到该环境的新方法能正常工作所需。

1. 在**解决方案资源管理器**，右键单击*robots.txt*文件并单击**从项目中排除**。
2. 使用 Windows 文件资源管理器，在解决方案文件夹中创建一个新文件夹并将其命名*ExtraFiles*。
3. 移动*robots.txt*文件从*ContosoUniversity*到项目文件夹*ExtraFiles*文件夹。

    ![ExtraFiles 文件夹](deploying-extra-files/_static/image1.png)
4. 使用 FTP 工具，删除*robots.txt*从过渡网站上的文件。

    作为替代方法，则可以选择**删除目标位置的其他文件**下**文件发布选项**上**设置**选项卡上的过渡发布配置文件，并重新发布到过渡环境。

## <a name="update-the-publish-profile-file"></a>更新发布配置文件

只需*robots.txt*暂存，因此暂存你需要更新，以便将其部署的唯一发布配置文件中。

1. 在 Visual Studio 中，打开*Staging.pubxml*。
2. 在结束之前，在文件末尾`</Project>`标记中，添加以下标记：

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    此代码创建一个新*目标*，将收集要部署的其他文件。 目标组成一个或多个 MSBuild 将执行的任务基于你指定的条件。

    `Include`属性指定在其中查找文件的文件夹为*ExtraFiles*、 位于项目文件夹与相同的级别。 MSBuild 将从该文件夹，然后从 （双精度的星号指定递归子文件夹） 的任何子文件夹以递归方式收集所有文件。 此代码与你可能使多个文件和文件处于子文件夹内*ExtraFiles*将部署文件夹，以及所有。

    `DestinationRelativePath`元素指定的文件夹和文件应复制到相同的文件和文件夹结构中的目标 web 站点的根文件夹中找到*ExtraFiles*文件夹。 如果你想要复制*ExtraFiles*文件夹本身，`DestinationRelativePath`值将为*ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*。
3. 在结束之前，在文件末尾`</Project>`标记中，添加以下标记，指定何时执行新的目标。

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    此代码会导致新`CustomCollectFiles`每当执行将文件复制到目标文件夹的目标时将执行的目标。 没有与创建部署包发布的单独目标和新目标注入到这两个目标，以防你决定使用部署包，而不发布部署。

    *.Pubxml*文件现在看起来像下面的示例：

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. 保存并关闭*Staging.pubxml*文件。

## <a name="publish-to-staging"></a>发布到过渡环境

使用一键式发布或命令行，通过使用临时配置文件中发布应用程序。

如果你使用一键式发布，你可以验证在**预览**窗口， *robots.txt*将被复制。 否则，使用 FTP 工具来验证*robots.txt*文件是在部署后的网站的根文件夹中。

## <a name="summary"></a>摘要

这将完成这一系列的 ASP.NET web 应用程序部署到第三方托管提供商的教程。 有关任何这些教程中所涵盖的主题的详细信息，请参阅[ASP.NET 部署内容映射](https://go.microsoft.com/fwlink/p/?LinkId=282413)。

## <a name="more-information"></a>详细信息

如果你知道如何使用 MSBuild 文件，你可以通过在编写代码来自动处理许多其他部署任务*.pubxml*文件 （针对特定配置文件的任务） 或项目*。 wpp.targets*文件 （的任务适用于所有配置文件）。 有关详细信息*.pubxml*和*。 wpp.targets*文件，请参阅[如何： 编辑发布配置文件 (.pubxml) 文件中的部署设置与。 wpp.targets 在 Visual Studio Web 的文件项目](https://msdn.microsoft.com/library/ff398069)。 有关 MSBuild 的代码的基本介绍，请参阅**剖析项目文件**中[企业部署系列： 了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 若要了解如何使用 MSBuild 文件为你自己的方案执行任务，请参阅本书：[内 Microsoft 生成引擎： 使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi 和 William Bartholomew。

## <a name="acknowledgements"></a>致谢

我要特别感谢以下人员执行了巨大的内容的本系列教程的贡献：

- [Alberto Poblacion、 MVP &amp; MCT、 西班牙](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson，数据平台开发 MVP，美国
- 恶劣 Mittal，Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava Microsoft
- [Raffaele Rialdi，意大利](http://www.iamraf.net/)
- [Rick Anderson Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi、 Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott 搜寻、 Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović、 塞尔维亚共和国](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi、 Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

>[!div class="step-by-step"]
[上一页](command-line-deployment.md)
[下一页](troubleshooting.md)
