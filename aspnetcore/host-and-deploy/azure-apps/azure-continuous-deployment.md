---
title: "使用 Visual Studio 的 Azure 和 ASP.NET Core 使用 Git 进行持续部署"
author: rick-anderson
description: "了解如何使用 Visual Studio 创建 ASP.NET Core Web 应用并使用 Git 将它部署到 Azure 应用服务以实现持续部署。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 7302de1ace62dba53b317039aac7f4763314aa19
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a>使用 Visual Studio 的 Azure 和 ASP.NET Core 使用 Git 进行持续部署

作者：[Erik Reitan](https://github.com/Erikre)

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

本教程演示如何创建使用 Visual Studio 的 ASP.NET 核心 web 应用并将其从 Visual Studio 部署到 Azure App Service，使用连续部署。

另请参阅 [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)（使用 VSTS 生成并通过持续部署发布到 Azure Web 应用），其中演示如何使用 Visual Studio Team Services 为 [Azure App Service](/azure/app-service/app-service-web-overview) 配置持续交付 (CD) 工作流。 在 Team Services 的 azure 持续交付简化了可靠的部署管道的设置以发布托管在 Azure App Service 中的应用程序的更新。 可以从 Azure 门户中生成、 运行测试，将部署到过渡槽中，以及然后部署到生产环境中配置管道。

> [!NOTE]
> 若要完成本教程，Microsoft Azure 帐户是必需的。 若要获取帐户，[激活 MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F)或[注册一个免费试用版](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>系统必备

本教程假定安装以下软件：

* [Visual Studio](https://www.visualstudio.com)
* [.NET 核心 SDK](https://www.microsoft.com/net/download/core) (运行时和工具)
* 用于 Windows 的 [Git](https://git-scm.com/downloads)

## <a name="create-an-aspnet-core-web-app"></a>创建 ASP.NET Core Web 应用

1. 启动 Visual Studio。

1. 从“文件”菜单中选择“新建” > “项目”。

1. 选择“ASP.NET Core Web 应用程序”项目模板。 它出现在“已安装” > “模板” > “Visual C#” > “.NET Core”下。 将项目命名为 `SampleWebAppDemo`。 选择**创建新 Git 存储库**选项，然后单击**确定**。

   ![“新建项目”对话框](azure-continuous-deployment/_static/01-new-project.png)

1. 在“新建 ASP.NET Core 项目”对话框中，选择 ASP.NET Core 的“空”模板，然后单击“确定”。

   ![“新建 ASP.NET 项目”对话框](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> .NET 核心的最新版本为 2.0。

### <a name="running-the-web-app-locally"></a>本地运行 Web 应用

1. Visual Studio 完成创建应用后，请选择“调试” > “启动调试”以运行该应用。 作为替代方法，按**F5**。

   可能需要一些时间对 Visual Studio 和新应用进行初始化。 完成后，浏览器将显示正在运行的应用。

   ![显示正在运行的应用程序（显示“Hello World!”）的浏览器窗口](azure-continuous-deployment/_static/04-browser-runapp.png)

1. 之后查看正在运行的 Web 应用，关闭浏览器，然后在 Visual Studio 以停止应用的工具栏中选择"停止调试"图标。

## <a name="create-a-web-app-in-the-azure-portal"></a>在 Azure 门户中创建 Web 应用

以下步骤在 Azure 门户中创建 web 应用：

1. 登录到[Azure 门户](https://portal.azure.com)。

1. 选择**新建**在左上角的门户界面。

1. 选择**Web + 移动** > **Web 应用**。

   ![Microsoft Azure 门户：“新建”按钮：Marketplace 下的“Web + 移动”：“特别推荐的应用”下的“Web 应用”按钮](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. 在“Web 应用”边栏选项卡中，输入“应用服务名称”的唯一值。

   ![“Web 应用”边栏选项卡](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > **App Service 名称**名称必须唯一。 门户执行此规则时提供的名称。 如果提供不同的值，将替代的每个匹配项的值**SampleWebAppDemo**在本教程。

   另外，在“Web 应用”边栏选项卡中，选择现有“应用服务计划/位置”或新建一个。 如果创建新的计划，请选择定价层、 位置和其他选项。 App Service 计划的详细信息，请参阅[Azure App Service 计划深入概述](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。

1. 选择“创建”。 Azure 将设置并启动 web 应用。

   ![Azure 门户：示例 Web 应用演示 01 Essentials 边栏选项卡](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>为新 Web 应用启用 Git 发布

Git 是一个分布式的版本控制系统，可用来部署 Azure App Service web 应用。 Web 应用程序代码存储在本地 Git 存储库，并且该代码通过将推送到远程存储库部署到 Azure。

1. 登录到[Azure 门户](https://portal.azure.com)。

1. 选择**应用程序服务**以查看与 Azure 订阅关联的应用程序服务的列表。

1. 选择在本教程的上一节中创建的 web 应用。

1. 在“部署”边栏选项卡中，选择“部署选项” > “选择源” > “本地 Git 存储库”。

   ![“设置”边栏选项卡：“部署源”边栏选项卡：“选择源”边栏选项卡](azure-continuous-deployment/_static/deployment-options.png)

1. 选择“确定”。

1. 如果以前尚未设置部署凭据用于发布 web 应用或其他 App Service 应用，它们现在设置：

   * 选择**设置** > **部署凭据**。 **设置部署凭据**显示边栏选项卡。
   * 创建用户名和密码。 设置 Git 时，请保存以供将来使用的密码。
   * 选择“保存”。

1. 在**Web 应用**边栏选项卡，选择**设置** > **属性**。 要将部署到远程 Git 存储库的 URL 将显示在**GIT URL**。

1. 复制“GIT URL”的值，稍后将在本教程中用到。

   ![Azure 门户：应用程序“属性”边栏选项卡](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>将 web 应用发布到 Azure App Service

在本部分中，创建使用 Visual Studio 和推送从该存储库到 Azure 来部署 web 应用的本地 Git 存储库。 涉及到的步骤如下：

* 添加使用 GIT URL 值，因此可以将本地存储库部署到 Azure 的远程存储库设置。
* 提交项目进行的更改。
* 在 Azure 上，项目进行的更改从本地存储库推送到远程存储库。

1. 在“解决方案资源管理器”中右键单击“解决方案 ‘SampleWebAppDemo’”并选择“提交”。 **团队资源管理器**显示。

   ![“团队资源管理器连接”选项卡](azure-continuous-deployment/_static/10-team-explorer.png)

1. 在“团队资源管理器”中，选择“主页”（主页图标）>“设置” > “存储库设置”。

1. 在**远程网站**部分**存储库设置**，选择**添加**。 **添加远程**对话框随即显示。

1. 将远程的“名称”设置为“Azure-SampleApp”。

1. 将的值设置**提取**到**Git URL** ，在本教程前面部分中从 Azure 复制。 请注意，此 URL 是以 .git 结尾的。

   ![“编辑远程”对话框](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > 作为替代方法，指定从远程存储库**命令窗口**通过打开**命令窗口**、 将更改为项目目录，然后输入命令。 示例:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. 选择“主页”（主页图标）>“设置” > “全局设置”。 确认设置的名称和电子邮件地址。 选择**更新**必要情况。

1. 选择“主页” > “更改”以返回到“更改”视图。

1. 输入提交消息，如**初始推送 #1**和选择**提交**。 此操作将创建*提交*本地。

   ![“团队资源管理器连接”选项卡](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > 作为替代方法中的更改提交**命令窗口**通过打开**命令窗口**、 将更改为项目目录，然后输入 git 命令。 示例:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. 选择“主页” > “同步” > “操作” > “打开命令提示符”。 命令提示打开的项目目录。

1. 在命令窗口中输入以下命令：

   `git push -u Azure-SampleApp master`

1. 输入 Azure**部署凭据**之前在 Azure 中创建的密码。

   此命令启动将本地项目文件推送到 Azure 的过程。 上面的命令的输出由部署成功消息结尾。

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > 如果需要在项目上的协作，请考虑在推送到[GitHub](https://github.com)之前将推送到 Azure。
 
### <a name="verify-the-active-deployment"></a>验证活动的部署

验证与本地环境到 Azure web 应用程序传输成功。

在[Azure 门户](https://portal.azure.com)，选择 web 应用。 选择**部署** > **部署选项**。

![Azure 门户：“设置”边栏选项卡：显示成功部署的“部署”边栏选项卡](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>在 Azure 中运行此应用

现在，web 应用部署到 Azure，运行该应用。

这可以通过两种方式实现：

* 在 Azure 门户中，找到 web 应用的 web 应用边栏选项卡。 选择**浏览**在默认浏览器中查看应用。
* 打开浏览器并输入 web 应用的 URL。 示例：`http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>更新 web 应用程序并将重新发布

对本地代码进行更改后, 重新发布：

1. 在 Visual Studio 的“解决方案资源管理器”中，打开 Startup.cs 文件。

1. 在 `Configure` 方法中，修改 `Response.WriteAsync` 方法，使它显示以下内容：

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. 保存对*Startup.cs*。

1. 在“解决方案资源管理器”中右键单击“解决方案 ‘SampleWebAppDemo’”并选择“提交”。 **团队资源管理器**显示。

1. 输入提交消息，如`Update #2`。

1. 按“提交”按钮以提交项目更改。

1. 选择“主页” > “同步” > “操作” > “推送”。

> [!NOTE]
> 作为替代方法，将从更改推送**命令窗口**通过打开**命令窗口**、 将更改为项目目录，然后输入 git 命令。 示例:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>在 Azure 中查看更新的 Web 应用

通过选择查看更新的 web 应用**浏览**从 web 应用边栏选项卡在 Azure 门户或通过打开浏览器并输入 web 应用的 URL。 示例：`http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>其他资源

* [VSTS 用于生成并发布到 Azure Web 应用程序使用连续部署](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [项目 Kudu](https://github.com/projectkudu/kudu/wiki)
