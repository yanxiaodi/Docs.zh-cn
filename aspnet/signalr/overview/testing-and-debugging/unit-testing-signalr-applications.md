---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: "单元测试 SignalR 应用程序 |Microsoft 文档"
author: pfletcher
description: "本文介绍如何使用 SignalR 2.0 的单元测试功能。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: d767e1a9d27670387133e5a48a8f92f5bdd39d9e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-signalr-applications"></a>单元测试 SignalR 应用程序
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

> 本文介绍如何使用 SignalR 2 的单元测试功能。 
> 
> ## <a name="software-versions-used-in-this-topic"></a>本主题中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 版本 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>单元测试 SignalR 应用程序

可以使用 SignalR 2 中的单元测试功能 SignalR 应用程序中创建单元测试。 SignalR 2 包括[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx)接口，可以用于创建用于模拟你的中心方法用于测试的模拟的对象。

在本部分中，你将添加在中创建的应用程序的单元测试[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)使用[XUnit.net](https://github.com/xunit/xunit)和[Moq](https://github.com/Moq/moq4)。

XUnit.net 将可用于控制测试;Moq 将用于创建[模拟](http://en.wikipedia.org/wiki/Mock_object)用于测试的对象。 如果需要; 可以使用其他模拟框架[NSubstitute](http://nsubstitute.github.io/)也是一个不错的选择。 本教程演示如何设置两种方式模拟对象： 首先，使用`dynamic`对象 （在.NET Framework 4 中引入） 和第二，使用接口。

### <a name="contents"></a>内容

本教程包含以下各节。

- [使用动态进行单元测试](#dynamic)
- [单元测试的类型](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>使用动态进行单元测试

在本部分中，你将添加在中创建的应用程序的单元测试[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)使用动态对象。

1. 安装[XUnit 运行程序扩展](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099)for Visual Studio 2013。
2. 完成[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)，或下载完成的应用程序从[MSDN 代码库](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)。
3. 如果你正在使用的快速入门应用程序的下载版本，打开**程序包管理器控制台**单击**还原**将 SignalR 包添加到项目。

    ![还原包](unit-testing-signalr-applications/_static/image1.png)
4. 将项目添加到单元测试的解决方案。 右键单击你的解决方案中**解决方案资源管理器**和选择**添加**，**新项目...**.下**C#**节点中，选择**Windows**节点。 选择**类库**。 将新项目**TestLibrary**单击**确定**。

    ![创建测试库](unit-testing-signalr-applications/_static/image2.png)
5. 到 SignalRChat 项目测试库项目中添加的引用。 右键单击**TestLibrary**项目，然后选择**添加**，**引用...**.选择**项目**节点下的**解决方案**节点，并选中**SignalRChat**。 单击 **“确定”**。

    ![添加项目引用](unit-testing-signalr-applications/_static/image3.png)
6. 添加 SignalR、 Moq 和 XUnit 程序包**TestLibrary**项目。 在**程序包管理器控制台**，将其设置**默认项目**下拉列表中到**TestLibrary**。 在控制台窗口中运行以下命令：

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![安装包](unit-testing-signalr-applications/_static/image4.png)
7. 创建测试文件。 右键单击**TestLibrary**项目，然后单击**添加...**，**类**。 将新类**Tests.cs**。
8. Tests.cs 的内容替换为以下代码。

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    在上面的代码中，使用创建测试客户端`Mock`对象[Moq](https://github.com/Moq/moq4)的类型库[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (在 SignalR 2.1 中，将分配`dynamic`类型参数。）`IHubCallerConnectionContext`接口是代理对象与其调用客户端上的方法。 `broadcastMessage`函数然后定义模拟客户端，以便它可以由调用`ChatHub`类。 则测试引擎会调用`Send`方法`ChatHub`类，该类又会调用模拟`broadcastMessage`函数。
9. 生成解决方案，按**F6**。
10. 运行单元测试。 在 Visual Studio 中，选择**测试**， **Windows**，**测试资源管理器**。 在测试资源管理器窗口中，右键单击**HubsAreMockableViaDynamic**和选择**运行选定的测试**。

    ![测试资源管理器](unit-testing-signalr-applications/_static/image5.png)
11. 验证该测试已通过通过检查下部窗格中，在测试资源管理器窗口中。 窗口将显示该测试已通过。

    ![已通过测试](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>单元测试的类型

在本部分中，你将添加应用程序中创建的测试[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)使用包含要测试的方法的接口。

1. 完成步骤 1-7[单元测试与动态](#dynamic)上述教程。
2. Tests.cs 的内容替换为以下代码。

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    在上面的代码中，定义的签名创建接口`broadcastMessage`测试引擎将为其创建模拟的客户端的方法。 然后使用创建模拟的客户端`Mock`类型的对象[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (在 SignalR 2.1 中，将分配`dynamic`为类型参数。)`IHubCallerConnectionContext`接口是代理对象与其调用客户端上的方法。

    测试然后创建的实例`ChatHub`，然后创建的模型版本`broadcastMessage`方法，反过来调用通过调用`Send`集线器上的方法。
3. 生成解决方案，按**F6**。
4. 运行单元测试。 在 Visual Studio 中，选择**测试**， **Windows**，**测试资源管理器**。 在测试资源管理器窗口中，右键单击**HubsAreMockableViaDynamic**和选择**运行选定的测试**。

    ![测试资源管理器](unit-testing-signalr-applications/_static/image7.png)
5. 验证该测试已通过通过检查下部窗格中，在测试资源管理器窗口中。 窗口将显示该测试已通过。

    ![已通过测试](unit-testing-signalr-applications/_static/image8.png)
