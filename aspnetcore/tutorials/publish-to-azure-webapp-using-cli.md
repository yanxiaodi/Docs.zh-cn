---
title: "使用命令行工具将 ASP.NET Core 应用发布到 Azure"
author: camsoper
description: "了解如何使用 Git 命令行客户端将 ASP.NET Core 应用发布到 Azure 应用服务。"
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0418a2695d3afb6dc2c55b8f694a97d62239835f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a>从命令行将 ASP.NET Core 应用程序部署到 Azure App Service

作者：[Cam Soper](https://twitter.com/camsoper)

本教程将为你介绍如何使用命令行工具生成 ASP.NET Core 应用程序并将其部署到 Microsoft Azure App Service。  完成后，你将拥有一个在 ASP.NET MVC Core 中构建并作为 Azure App Service Web App 托管的 Web 应用程序。  本教程是使用 Windows 命令行工具编写的，但也可以应用于 macOS 和 Linux 环境。  

在本教程中，你将了解：

> [!div class="checklist"]
> * 如何使用 Azure CLI 创建 Azure App Service 网站
> * 如何使用 Git 命令行工具将 ASP.NET Core 应用程序部署到 Azure App Service

## <a name="prerequisites"></a>系统必备

若要完成本教程，你需要：

* [Microsoft Azure 订阅](https://azure.microsoft.com/free/)
* [.NET Core](https://www.microsoft.com/net/download/core)
* [Git](https://www.git-scm.com/) 命令行客户端

## <a name="create-a-web-application"></a>创建 Web 应用程序

为 Web 应用程序创建新目录，创建新的 ASP.NET Core MVC 应用程序，然后在本地运行该网站。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[其他](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![命令行输出](publish-to-azure-webapp-using-cli/_static/new_prj.png)

通过浏览到 http://localhost:5000 测试应用程序。

![本地运行的网站](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a>创建 Azure App Service 实例

使用 [Azure Cloud Shell](/azure/cloud-shell/quickstart)，创建资源组、App Service 计划和 App Service Web 应用。

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

在部署之前，使用以下命令设置帐户级部署凭据：

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

需要使用部署 URL 来使用 Git 部署应用程序。  检索类似如下的 URL。

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
请注意以 `.git` 结尾的已显示 URL。 它在下一步中使用。

## <a name="deploy-the-application-using-git"></a>使用 Git 部署应用程序

你已准备好使用 Git 从本地计算机部署。

> [!NOTE]
> 忽略 Git 中任何关于行尾的警告是安全的。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[其他](#tab/other)
```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```
---

Git 提示前面设置的部署凭据。 身份验证后，将应用程序推送到远程位置，然后生成并部署。

![Git 部署输出](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a>测试应用程序

通过浏览到 `https://<web app name>.azurewebsites.net` 测试应用程序。  若要在 Cloud Shell（或 Azure CLI）中显示地址，请使用以下方法：

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![在 Azure 中运行的应用](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>清理

完成测试应用并检查代码和资源后，通过删除资源组来删除 Web 应用并制定计划。

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：

> [!div class="checklist"]
> * 如何使用 Azure CLI 创建 Azure App Service 网站
> * 如何使用 Git 命令行工具将 ASP.NET Core 应用程序部署到 Azure App Service

接下来，了解如何使用命令行来部署使用 CosmosDB 的现有 Web 应用。

> [!div class="nextstepaction"]
> [通过 .NET Core 从命令行部署到 Azure](/dotnet/azure/dotnet-quickstart-xplat)
