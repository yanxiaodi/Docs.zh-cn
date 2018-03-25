---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: "开始使用 OWIN 和 Katana |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 8922aada723da9b149ec111902fcd883c8241dfb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-owin-and-katana"></a>开始使用 OWIN 和 Katana
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[打开.NET (OWIN) 的 Web 界面](http://owin.org/)定义.NET web 服务器和 web 应用程序之间的抽象。 通过分离从应用程序的 web 服务器，OWIN 使得更轻松地创建.NET web 开发的中间件。 此外，OWIN，从而更便于端口 web 应用程序到其他主机 &#8212; 例如，Windows 服务或其他进程中的自承载。

OWIN 是一个社区拥有规范，未实现。 Katana 项目是一组由 Microsoft 开发的开源 OWIN 组件。 OWIN 和 Katana 的一般概述，请参阅[项目概述 Katana](an-overview-of-project-katana.md)。 在本文中，我将直接到代码开始。

本教程使用[Visual Studio 2013 候选发布版本](https://go.microsoft.com/fwlink/?LinkId=306566)，但你也可以使用 Visual Studio 2012。 在 Visual Studio 2012，我请注意以下几个步骤互不相同。

## <a name="host-owin-in-iis"></a>在 IIS 中的 OWIN 主机

在此部分中，我们将承载在 IIS 中的 OWIN。 此选项可让你的灵活性和可组合性的与 IIS 的成熟的功能集一起 OWIN 管道。 使用此选项，在 ASP.NET 请求管道中运行的 OWIN 应用程序。

首先，创建一个新的 ASP.NET Web 应用程序项目。 （在 Visual Studio 2012 中，使用 ASP.NET 空 Web 应用程序项目类型）。

![](getting-started-with-owin-and-katana/_static/image1.png)

在**新建 ASP.NET 项目**对话框中，选择**空**模板。

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>添加 NuGet 包

接下来，添加所需的 NuGet 包。 从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在 Package Manager Console 窗口中，键入以下命令：

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>添加一个 Startup 类

接下来，添加的 OWIN 启动类。 在解决方案资源管理器，右键单击该项目并选择**添加**，然后选择**新项**。 在**添加新项**对话框中，选择**Owin 启动类**。 有关配置 startup 类的详细信息，请参阅[OWIN 启动类检测](owin-startup-class-detection.md)。

![](getting-started-with-owin-and-katana/_static/image4.png)

将以下代码添加到 `Startup1.Configuration` 方法中：

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

此代码将一个简单的中间件添加到 OWIN 管道，作为函数接收实现**Microsoft.Owin.IOwinContext**实例。 当服务器接收 HTTP 请求时，OWIN 管道时，将调用该中间件。 该中间件将响应的内容类型设置，并将写入响应正文。

> [!NOTE]
> OWIN 启动类模板是在 Visual Studio 2013。 如果你正在使用 Visual Studio 2012，只需添加一个名为的新的空类`Startup1`，然后粘贴以下代码：


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>运行应用程序

按 F5 开始调试。 Visual Studio 将打开一个浏览器窗口以`http://localhost:*port*/`。 页应如下所示：

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>将 OWIN 自承载在控制台应用程序

很容易地将此应用程序转换从到自定义过程中的自承载的 IIS 承载。 使用 IIS 承载，IIS 可以充当 HTTP 服务器和进程该主机服务器。 与自承载，你的应用程序创建过程，并使用**HttpListener**与 HTTP 服务器的类。

在 Visual Studio 中，创建一个新的控制台应用程序。 在 Package Manager Console 窗口中，键入以下命令：

`Install-Package Microsoft.Owin.SelfHost -Pre`

添加`Startup1`从本教程的第 1 部分到项目的类。 你不需要修改此类。

实现应用程序的`Main`方法，如下所示。

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

运行控制台应用程序时，在服务器启动侦听`http://localhost:9000`。 如果你导航到此地址在 web 浏览器中，你将看到"Hello world"页。

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>添加 OWIN 诊断

Microsoft.Owin.Diagnostics 程序包包含中间件捕获未经处理的异常并显示错误详细信息的 HTML 页。 此页函数非常类似有时称为 ASP.NET 错误页"[死亡的黄色屏幕](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)"(YSOD)。 与一样 YSOD，Katana 错误页有用在开发期间，但它是在生产模式中禁用该一个好办法。

若要安装在你的项目中的诊断程序包，请在程序包管理器控制台窗口中键入以下命令：

`install-package Microsoft.Owin.Diagnostics –Pre`

更改中的代码你`Startup1.Configuration`方法，如下所示：

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

现在使用 CTRL + F5 运行应用程序而不进行调试，以便 Visual Studio 将不在异常上中断。 应用程序具有相同的行为和前面一样，直到你导航到`http://localhost/fail`，此时，应用程序会引发异常。 错误页中间件将捕获该异常并显示有关错误的信息的 HTML 页。 你可以单击选项卡来查看堆栈、 查询字符串、 cookie、 请求标头和 OWIN 环境变量。

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>后续步骤

- [OWIN 启动类检测](owin-startup-class-detection.md)
- [使用 OWIN 自承载 ASP.NET Web API](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [使用 OWIN 自承载 SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
