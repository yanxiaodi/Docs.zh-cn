---
title: "使用 ASP.NET Core 的 Visual Studio Tools for Docker"
author: spboyer
description: "了解如何使用 Visual Studio 2017 工具和 Docker for Windows 来容器化 ASP.NET Core 应用。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: b2a3c369a22d50fcefdb96914f5bf84bfafab7cb
ms.sourcegitcommit: 6fa546140575b3eb279eabae12d9acad966f70e0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>使用 ASP.NET Core 的 Visual Studio Tools for Docker

[Visual Studio 2017](https://www.visualstudio.com/)支持生成、 调试和运行面向.NET Core 应用的容器化的 ASP.NET Core。 Windows 和 Linux 容器均受支持。

## <a name="prerequisites"></a>系统必备

* 具有“.NET Core 跨平台开发”工作负载的 [Visual Studio 2017](https://www.visualstudio.com/)
* [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>安装和设置

要安装 Docker，请查看 [Docker for Windows：安装须知](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)了解相关信息，并安装 [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)。

Docker for Windows 中的[共享驱动器](https://docs.docker.com/docker-for-windows/#shared-drives)必须配置为支持卷映射和调试。 右键单击系统托盘 Docker 图标，选择**设置...**，然后选择**共享驱动器**。 选择 Docker 其中存储文件的驱动器。 选择**应用**。

![Shared Drives](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 版本 15.6 及更高版本时提示**共享驱动器**未配置。

## <a name="add-docker-support-to-an-app"></a>向应用添加 Docker 支持

若要将 Docker 支持添加到 ASP.NET 核心项目，项目必须为目标.NET 核心。 支持 Linux 和 Windows 容器。

当将 Docker 支持添加到项目中，选择 Windows 或 Linux 容器。 Docker 主机必须运行类型相同的容器。 要更改正在运行的 Docker 实例中的容器类型，请右键单击系统托盘中的 Docker 图标，再选择“切换到 Windows 容器...”或“切换到 Linux 容器...”。

### <a name="new-app"></a>新应用

使用 ASP.NET Core Web 应用程序项目模板创建新应用时，请选中“启用 Docker 支持”复选框：

![“启用 Docker 支持”复选框](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

如果目标框架是.NET 核心**OS**下拉列表允许针对选择的容器类型。

### <a name="existing-app"></a>现有应用

Visual Studio Tools for Docker 不支持向面向.NET Framework 的现有 ASP.NET Core 项目添加 Docker。 对于面向 .NET Core 的 ASP.NET Core 项目，可通过两个选项使用工具添加 Docker 支持。 在 Visual Studio 中打开项目，然后选择以下选项之一：

* 从“项目”菜单中选择“Docker 支持”。
* 右键单击解决方案资源管理器中的项目，然后选择“添加” > “Docker 支持”。

## <a name="docker-assets-overview"></a>Docker 资产概述

Visual Studio Tools for Docker 向解决方案中添加 docker-compose 项目，包括以下各项：

* .dockerignore：包含文件和目录模式的列表，排除生成生成上下文的时间。
* docker-compose.yml：基本 [Docker Compose](https://docs.docker.com/compose/overview/) 文件，用于定义要分别通过 `docker-compose build` 和 `docker-compose run` 生成和运行的映像集合。
* docker compose.override.yml：一个可选文件，通过 Docker Compose 读取，包含服务的配置替代。 Visual Studio 执行 `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` 以合并这些文件。

Dockerfile，用作创建最终 Docker 映像的方案，添加到项目根目录。 请参阅 [Dockerfile 引用](https://docs.docker.com/engine/reference/builder/)，了解其中的命令。 此特定 Dockerfile 使用[多阶段生成](https://docs.docker.com/engine/userguide/eng-image/multistage-build/)，该生成包含四个不同的命名生成阶段：

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

Dockerfile 基于 [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) 映像。 此基础映像包括 ASP.NET Core NuGet 包，已对此包进行了预实时编译，以提高启动性能。

*Docker-compose.yml*文件包含的项目运行时创建的映像的名称：

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

在前面的示例中，应用在“调试”模式下运行时，`image: hellodockertools` 生成映像 `hellodockertools:dev`。 应用在“发布”模式下运行时，生成 `hellodockertools:latest` 映像。

映像名称加上前缀[Docker Hub](https://hub.docker.com/)用户名 (例如， `dockerhubusername/hellodockertools`) 如果该图像将推送到注册表。 或者，更改要包括私有注册表 URL 的映像名称 (例如， `privateregistry.domain.com/hellodockertools`) 根据配置。

## <a name="debug"></a>调试

在工具栏的调试下拉列表中选择“Docker”，然后开始调试应用。 “输出”窗口的“Docker”视图显示发生的以下操作：

* *Microsoft/aspnetcore* （如果尚未在缓存中） 获取运行时映像。
* *Microsoft/aspnetcore-生成*  /发布编译的映像 （如果尚未在缓存中） 获取。
* ASPNETCORE_ENVIRONMENT 环境变量设置为位于 `Development` 容器内。
* 已公开端口 80，并已映射为 localhost 动态分配的端口。 该端口由 Docker 主机确定，并且可以使用 `docker ps` 进行查询。
* 应用程序复制到容器。
* 使用调试程序附加到使用动态分配端口容器启动默认浏览器。 

生成的 Docker 映像是*开发人员*使用应用程序的映像*microsoft/aspnetcore*作为基本映像的映像。 在“包管理器控制台”(PMC) 窗口中运行 `docker images` 命令。 显示计算机上的映像：

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> 开发人员映像作为缺少的应用程序内容，**调试**配置使用卷装载提供迭代的体验。 要推送映像，请使用“发布”配置。

在 PMC 中运行 `docker ps` 命令。 请注意，应用使用容器运行：

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>编辑并继续

对静态文件和 Razor 视图的更改会自动更新，无需执行编译步骤。 进行更改，保存并在浏览器中刷新，以查看更新。  

对代码文件进行修改需要在容器内进行编译和重启 Kestrel。 更改后，使用 CTRL+F5 执行该过程并在容器内启动应用。 Docker 容器不重新生成或停止。 在 PMC 中运行 `docker ps` 命令。 请注意，截至 10 分钟前，原始容器仍在运行：

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>发布 Docker 映像

应用程序的开发和调试周期完成后，Visual Studio Tools for Docker 帮助创建应用程序的生产映像。 将配置下拉列表更改为“发布”，然后生成应用。 此工具生成的映像*最新*标记，可以推送到的私有注册表或 Docker Hub。 

在 PMC 中运行 `docker images` 命令，查看映像列表：

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> `docker images` 命令返回存储库名称和标记标识为 \<none> （上面未列出）的中间映像。 这些未命名映像由[多阶段生成](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile* 生成。 它们可提高生成最终映像的效率 &mdash; 发生更改时，仅重新生成必要的层。 当不再需要中间的图像时，则删除它们使用[docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/)命令。

可能希望生产或发布映像的大小比开发映像小。 由于卷映射中，调试器和应用程序已运行从本地计算机和不在容器内。 最新映像已打包必要的应用代码，以在主机上运行应用。 因此，增量是应用程序代码的大小。
