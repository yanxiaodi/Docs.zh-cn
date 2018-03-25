---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: "使用 Visual Studio 的 ASP.NET Web 部署： 部署某一代码更新 |Microsoft 文档"
author: tdykstra
description: "本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，使用的..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: f6861c702c1ccb19e5a4eee484a622079e205f86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署某一代码更新
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，通过使用 Visual Studio 2012 或 Visual Studio 2010。 有关序列的信息，请参阅[序列中的第一个教程](introduction.md)。


## <a name="overview"></a>概述

在初始部署后维护和开发您的网站的工作仍然存在，并且在长时间之前你将想要将更新部署。 本教程将指导您完成将更新部署到你的应用程序代码的过程。 实现和部署在本教程中的更新不涉及数据库更改;你将看到有关部署下一教程中的数据库更改的差异。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](troubleshooting.md)。

## <a name="make-a-code-change"></a>进行代码更改

为你的应用程序到更新的简单示例，你将添加到**教师**页上通过所选教师讲授的课程的列表。

如果你运行**教师**页上，你会注意到没有**选择**链接在网格中，但它们不执行任何操作生成以外行背景变灰。

![选择教师页](deploying-a-code-update/_static/image1.png)

现在你将添加时运行的代码**选择**链接单击，并显示由所选教师讲授的课程的列表。

1. 在*Instructors.aspx*，添加以下标记后立即**ErrorMessageLabel** `Label`控件：

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. 运行页面，然后选择一个教师。 你看到通过该教师讲授的课程的列表。

    ![讲授的课程教师页](deploying-a-code-update/_static/image2.png)
3. 关闭浏览器。

## <a name="deploy-the-code-update-to-the-test-environment"></a>将代码更新部署到测试环境

你可以使用发布配置文件将部署到测试、 过渡和生产之前，你需要更改数据库发布选项。 不再需要运行用于成员资格数据库的 grant 和数据部署脚本。

1. 打开**发布 Web**向导通过右键单击 ContosoUniversity 项目并单击**发布**。
2. 单击**测试**在配置文件中**配置文件**下拉列表。
3. 单击**设置**选项卡。
4. 下**DefaultConnection**中**数据库**部分中，清除**更新数据库**复选框。
5. 单击**配置文件**选项卡上，并依次**过渡**在配置文件中**配置文件**下拉列表。
6. 当系统询问你是否要保存到所做的更改**测试**配置文件，单击**是**。
7. 临时配置文件中进行相同的更改。
8. 重复此过程以生产配置文件中进行相同的更改。
9. 关闭**发布 Web**向导。

部署到测试环境，现在运行一次单击即可重新发布。 若要使此过程更快，可以使用**Web 单键发布**工具栏。

1. 在**视图**菜单上，选择**工具栏**，然后选择**Web 单键发布**。

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. 在**解决方案资源管理器**，选择 ContosoUniversity 项目。
3. **Web 单键发布**工具栏上，选择**测试**发布配置文件，然后单击**发布 Web** （带有箭头指向左侧和右侧的图标）。

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio 部署更新的应用程序，并浏览器将自动打开到主页。
5. 运行教师页并选择一个教师以验证更新成功部署。

你通常会还执行回归测试 （即，测试站点后，若要确保新的更改不中断任何现有功能的其余部分）。 但对于本教程将跳过该步骤并继续将更新部署到过渡和生产。

当重新部署时，Web 部署自动确定哪些文件已更改，以及仅副本更改为服务器的文件。 默认情况下，Web 部署使用上次更改日期上文件来确定哪些功能已更改。 在不更改文件内容时，某些源代码管理系统将更改文件甚至的日期。 在这种情况下，你可能想要配置 Web Deploy 以使用文件校验和以确定哪些文件已更改。 有关详细信息，请参阅[为什么执行所有我的文件获取重新部署未更改其虽然？](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET 部署常见问题中。

## <a name="take-the-application-offline-during-deployment"></a>使应用程序脱机在部署过程

要部署的更改现在是对单个页面的简单更改。 但有时你部署较大的更改，或部署代码和数据库的更改，以及如果用户请求页，在部署完成之前，该站点可能会错误地那样。 若要防止用户访问站点，正在进行部署时，可以使用*应用\_offline.htm*文件。 当将一个名为文件*应用\_offline.htm*在你的应用程序根文件夹中，IIS 会自动显示该文件而不是运行你的应用程序。 因此，若要在部署过程中阻止访问，你将*应用\_offline.htm*在根文件夹中，运行在部署过程，然后删除*应用\_offline.htm*后成功部署。

你可以配置 Web 部署以将默认自动*应用\_offline.htm*启动部署时服务器上的文件并将其删除完成时。 若要执行所有您需要做即可将下面的 XML 元素添加到你的发布配置文件 (.pubxml) 文件：

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

对于本教程中，你将看到如何创建和使用自定义*应用\_offline.htm*文件。

使用*应用\_offline.htm*中的暂存站点不是必需的因为你不具有访问暂存站点的用户。 但作为最佳做法，使用临时测试的所有内容你打算在生产中部署的方式。

### <a name="create-appofflinehtm"></a>创建应用\_offline.htm

1. 在**解决方案资源管理器**，右键单击该解决方案，然后单击**添加**，然后单击**新项**。
2. 创建**HTML 页**名为*应用\_offline.htm* (中删除最后一个"l" *.html*扩展 Visual Studio 将创建默认情况下)。
3. 将模板标记替换为以下标记：

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. 保存并关闭文件。

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>复制应用\_offline.htm 到网站的根文件夹

你可以使用任何 FTP 工具将文件复制到网站。 [FileZilla](http://filezilla-project.org/)是 FTP 工具和屏幕截图中所示。

若要使用 FTP 工具，需要以下三项： FTP URL、 用户名和密码。

URL 将显示在网站的仪表板页上，在 Azure 管理门户中，并且可以在中找到的用户名和密码 FTP *.publishsettings*前面下载的文件。 以下步骤演示如何获取这些值。

1. 在 Azure 管理门户中，单击**网站**选项卡，然后单击过渡网站。
2. 上**仪表板**页上，向下滚动到的 FTP 主机中的名称的查找**速览**部分。

    ![FTP 主机名](deploying-a-code-update/_static/image5.png)
3. 打开过渡*.publishsettings*在记事本或其他文本编辑器中的文件。
4. 查找`publishProfile`FTP 配置文件的元素。
5. 复制`userName`和`userPWD`值。

    ![FTP 凭据](deploying-a-code-update/_static/image6.png)
6. 打开你的 FTP 工具并登录到 FTP URL。
7. 复制*应用\_offline.htm*解决方案文件夹中找到到*/站点/wwwroot*文件夹中的暂存站点。

    ![复制 app_offline](deploying-a-code-update/_static/image7.png)
8. 浏览到暂存站点的 URL。 你看到*应用\_offline.htm*页现在显示而不是你的主页。

    ![在浏览器窗口的 app_offline.htm](deploying-a-code-update/_static/image8.png)

现在你就可以部署到过渡环境。

## <a name="deploy-the-code-update-to-staging-and-production"></a>将代码更新部署到过渡和生产

1. 在**Web 单键发布**工具栏上，选择**过渡**发布配置文件，然后单击**发布 Web**。

    Visual Studio 将部署更新的应用程序，并打开浏览器向站点的主页。 *应用\_offline.htm*显示文件。 你可以对其进行测试以验证是否成功的部署之前，必须删除*应用\_offline.htm*文件。
2. 返回到你的 FTP 工具，并删除**应用\_offline.htm**从暂存站点。
3. 在浏览器中，在过渡站点中，打开教师页，然后选择一个教师以验证更新成功部署。
4. 就像过渡，请按照生产同样的过程。

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>查看更改并部署特定的文件

Visual Studio 2012 还为你提供了部署单独的文件的功能。 选中的文件中，你可以查看本地版本和已部署的版本之间的差异、 将文件部署到目标环境中，或目标环境中的文件复制到本地项目。 在本教程的本部分中，你将看到如何使用这些功能。

### <a name="make-a-change-to-deploy"></a>若要部署更改

1. 打开*Content/Site.css*，并找到的块`body`标记。
2. 值更改`background-color`从`#fff`到`darkblue`。

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>在发布预览窗口中查看更改

当你使用**发布 Web**向导以发布该项目，你可以查看更改是否要通过双击中的文件发布**预览**窗口。

1. 右键单击 ContosoUniversity 项目并单击**发布**。
2. 从**配置文件**下拉列表中，选择**测试**发布配置文件。
3. 单击**预览**，然后单击**开始预览**。
4. 在**预览**窗格中，双击**Site.css**。

    ![双击 Site.css](deploying-a-code-update/_static/image9.png)

    **预览更改**对话框显示预览将部署的更改。

    ![预览更改为 Site.css](deploying-a-code-update/_static/image10.png)

    如果双击*Web.config*文件，**预览更改**对话框显示配置转换的生成的效果和发布配置文件转换。 此时你尚未这样做将导致的任何内容*Web.config*要发生变化，因此你会不看到任何更改的服务器上的文件。 但是，**预览更改**窗口错误地将显示两个更改。 看起来，将删除两个 XML 元素。 这些元素添加发布过程中，当你选择**执行 Code First 迁移应用程序启动**Code First 的上下文类。 在发布过程添加这些元素，以便看起来，尽管它们不会删除它们要删除之前进行比较。 未来版本中，将更正此错误。
5. 单击 **“关闭”**。
6. 单击“发布” 。
7. 当浏览器打开到测试站点主页后时，按 CTRL + F5 才能看到 CSS 更改的效果导致硬刷新。

    ![CSS 更改效果](deploying-a-code-update/_static/image11.png)
8. 关闭浏览器。

### <a name="publish-specific-files-from-solution-explorer"></a>将特定文件从解决方案资源管理器发布

假设你不喜欢背景是蓝色，并且想要还原到原始的颜色。 在本部分中，你将还原原始设置通过发布特定文件直接从**解决方案资源管理器**。

1. 打开*Content/Site.css*和还原`background-color`将设置为`#fff`。
2. 在**解决方案资源管理器**，右键单击*Content/Site.css*文件。

    上下文菜单显示三个发布选项。

    ![发布解决方案资源管理器选项](deploying-a-code-update/_static/image12.png)
3. 单击**预览更改为 Site.css**。

    窗口将打开以显示在目标环境中的本地文件和它的版本之间的差异。

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. 在**解决方案资源管理器**，右键单击**Site.css**再次单击**发布 Site.css**。

    **Web 发布活动**窗口显示已发布文件。

    ![Web 发布活动窗口](deploying-a-code-update/_static/image14.png)
5. 浏览器中打开`http://localhost/contosouniversity`URL，，然后按 CTRL + F5 可导致硬刷新才能查看更改的 css 的效果。

    ![正常的 CSS 附带的主页](deploying-a-code-update/_static/image15.png)
6. 关闭浏览器。

## <a name="summary"></a>摘要

现在，你已了解通过多种方式来部署应用程序更新不包含数据库更改，并已了解如何预览更改以验证将更新的内容符合预期。 现在，教师页面包含**课程讲授**部分。

![讲授的课程教师页](deploying-a-code-update/_static/image16.png)

下一教程演示如何将数据库更改部署： 到数据库和教师页，你将添加的出生日期字段。

>[!div class="step-by-step"]
[上一页](deploying-to-production.md)
[下一页](deploying-a-database-update.md)
