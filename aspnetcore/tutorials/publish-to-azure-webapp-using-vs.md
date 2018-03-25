---
title: "使用 Visual Studio 将 ASP.NET Core 应用发布到 Azure"
author: rick-anderson
description: "了解如何使用 Visual Studio 将 ASP.NET Core 应用发布到 Azure 应用服务。"
manager: wpickett
ms.author: riande
ms.date: 12/16/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 14d8dd0a5e6a99bacce3bf50b0468b20e0dddb96
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>使用 Visual Studio 将 ASP.NET Core Web 应用发布到 Azure App Service

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 、[Cesar Blum Silveira](https://github.com/cesarbs) 和 [Rachel Appel](https://twitter.com/rachelappel)

如果你在 Mac 上工作，请参阅[从 Visual Studio for Mac 发布到 Azure](https://blog.xamarin.com/publish-azure-visual-studio-mac/)。

## <a name="set-up"></a>设置

* 如果没有[免费 Azure 帐户](https://aka.ms/K5y5yh)，请开设一个。 

## <a name="create-a-web-app"></a>创建 Web 应用

在 Visual Studio 起始页中，选择“文件”>“新建”>“项目...”

![“文件”菜单](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

完成“新建项目”对话框：

* 在左侧窗格中，选择“.NET Core”。
* 在中间窗格中，选择“ASP.NET Core Web 应用程序”。
* 选择“确定”。

![“新建项目”对话框](publish-to-azure-webapp-using-vs/_static/new_prj.png)

在“新建 ASP.NET Core Web 应用程序”对话框中：

* 选择“Web 应用程序”。
* 选择“更改身份验证”。

![“新建项目”对话框](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

“更改身份验证”对话框随即出现。 

* 选择“个人用户帐户”。
* 选择“确定”返回到“新建 ASP.NET Core Web 应用程序”，然后再次选择“确定”。

![“新建 ASP.NET Core Web 身份验证”对话框](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Visual Studio 随即创建解决方案。

## <a name="run-the-app"></a>运行应用

* 按 Ctrl+F5 运行项目。
* 测试“关于”和“联系”链接。

![localhost 上的 Microsoft Edge 中打开的 Web 应用程序](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a>注册用户

* 选择“注册”并注册新用户。 可使用虚构电子邮件地址。 提交时，页面上会显示以下错误：

    “内部服务器错误: 处理请求时，数据库操作失败。SQL 异常: 无法打开数据库。可通过向应用程序数据库上下文应用现有迁移解决此问题。”*
* 选择“应用迁移”，并在页面更新后刷新页面。

![内部服务器错误: 处理请求时，数据库操作失败。 SQL 异常: 无法打开数据库。 可能可通过向应用程序数据库上下文应用现有迁移解决此问题。](publish-to-azure-webapp-using-vs/_static/mig.png)

应用将显示用于注册新用户的电子邮件和一个“注销”链接。

![Microsoft Edge 中打开的 Web 应用程序。 注册链接会替换为文本“Hello email@domain.com!”](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>将应用部署到 Azure

在解决方案资源管理器中右键单击该项目，然后选择“发布...”。

![打开且突出显示“发布”链接的上下文菜单](publish-to-azure-webapp-using-vs/_static/pub.png)

在“发布”对话框中：

* 选择“Microsoft Azure 应用服务”。
* 选择齿轮图标，然后选择“创建配置文件”。
* 选择“创建配置文件”。

![“发布”对话框](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a>创建 Azure 资源

“创建应用服务”对话框随即显示：

* 输入订阅。
* “应用名称”、“资源组”和“应用服务计划”输入字段已填充。 可以保留这些名称，也可以进行更改。

![“应用服务”对话框](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* 选择“服务”选项卡以创建新的数据库。

* 选择绿色的 + 图标以创建新的 SQL 数据库

![新建 SQL 数据库](publish-to-azure-webapp-using-vs/_static/sql.png)

* 在“配置 SQL 数据库”对话框中选择“新建...”以创建新的数据库。

![新建 SQL 数据库和服务器](publish-to-azure-webapp-using-vs/_static/conf.png)

“配置 SQL Server”对话框随即出现。

* 输入管理员用户名和密码，然后选择“确定”。 可保留默认的“服务器名称”。 

> [!NOTE]
> 不可使用“admin”作为管理员用户名。

![“配置 SQL Server”对话框](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* 选择“确定”。

Visual Studio 将返回到“创建应用服务”对话框。

* 选择“创建应用服务”对话框上的“创建”。

![“配置 SQL 数据库”对话框](publish-to-azure-webapp-using-vs/_static/conf_final.png)

Visual Studio 在 Azure 上创建 Web 应用和 SQL Server。 此步骤可能需要几分钟。 有关创建的资源的信息，请参阅[其他资源](#additonal-resources)。

部署完成时，选择“设置”：

![“配置 SQL Server”对话框](publish-to-azure-webapp-using-vs/_static/set.png)

在“发布”对话框的“设置”页面上：

  * 展开“数据库”并选中“在运行时使用此连接字符串”。
  * 展开“Entity Framework 迁移”并选中“在发布时应用此迁移”。

* 选择“保存”。 Visual Studio 将返回到“发布”对话框。 

![“发布”对话框：设置面板](publish-to-azure-webapp-using-vs/_static/pubs.png)

单击“发布” 。 Visual Studio 将应用发布到 Azure。 部署完成时，应用在浏览器中打开。

### <a name="test-your-app-in-azure"></a>在 Azure 中测试应用

* 测试“关于”和“联系”链接

* 注册新用户

![Microsoft Edge 中 Azure App Service 上打开的 Web 应用程序](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>更新应用

* 编辑“Pages/About.cshtml”Razor 页面并更改其内容。 例如，可以将段落修改为显示“Hello ASP.NET Core!”：[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* 右键单击项目，然后再次选择“发布...”。

![打开且突出显示“发布”链接的上下文菜单](publish-to-azure-webapp-using-vs/_static/pub.png)

* 应用发布后，验证所做的更改在 Azure 上是否可用。

![验证任务已完成](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>清理

完成应用测试后，转到 [Azure 门户](https://portal.azure.com/)并删除该应用。

* 选择“资源组”，然后选择所创建的资源组。

![Azure 门户：侧边栏菜单中的资源组](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* 在“资源组”页面中，选择“删除”。

![Azure 门户：“资源组”页面](publish-to-azure-webapp-using-vs/_static/rgd.png)

* 输入资源组的名称并选择“删除”。 现已从 Azure 中删除了本教程中创建的应用和其他所有资源。

### <a name="next-steps"></a>后续步骤

* [使用 Visual Studio 和 Git 持续部署到 Azure](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a>其他资源

* [Azure 应用服务](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [Azure 资源组](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [Azure SQL 数据库](https://docs.microsoft.com/azure/sql-database/)
