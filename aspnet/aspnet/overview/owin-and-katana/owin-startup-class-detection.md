---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: "OWIN 启动类检测 |Microsoft 文档"
author: Praburaj
description: "本教程演示如何配置加载哪些 OWIN startup 类。 有关 OWIN 的详细信息，请参阅项目 Katana 概述。 本教程已..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 618f8fa23630dcf9821a54415766dc015694e535
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="owin-startup-class-detection"></a>OWIN 启动类检测
====================
通过[Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)

> 本教程演示如何配置加载哪些 OWIN startup 类。 有关 OWIN 的详细信息，请参阅[项目概述 Katana](an-overview-of-project-katana.md)。 本教程编写由 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )，Praburaj Thiagarajan 和 Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。
> 
> ## <a name="prerequisites"></a>系统必备
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>OWIN 启动类检测

 每个 OWIN 应用程序具有一个 startup 类，其中您可以指定应用程序管道的组件。 有不同的方式可以与运行时连接 startup 类，具体取决于宿主模型选择 （OwinHost、 IIS 和 IIS Express）。 在本教程中所示的启动类可在每个托管的应用程序。 与托管运行时使用下列任一方法连接 startup 类：  

1. **命名约定**: Katana 查找名为的类`Startup`匹配的程序集名称或全局命名空间的命名空间中。
2. **OwinStartup 属性**： 这是大多数开发人员需要指定启动类的方法。 以下属性将设置为 startup 类`TestStartup`类`StartupDemo`命名空间。 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 `OwinStartup`特性将重写的命名约定。 也可以指定一个友好名称，使用此属性，但是，使用的友好名称要求你还使用`appSetting`配置文件中的元素。
3. **配置文件中的 appSetting 元素**:`appSetting`元素会替代`OwinStartup`属性和命名约定。 你可以有多个启动类 (每个使用`OwinStartup`属性) 和配置将使用类似于以下的标记的配置文件中加载哪些 startup 类：  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 此外可以使用显式指定的启动类和程序集的以下项： 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 下面的 XML 配置文件中指定的友好的启动类名称`ProductionConfiguration`。  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 上面的标记都必须使用以下`OwinStartup`属性指定的友好名称，并导致`ProductionStartup2`类运行。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. 若要禁用 OWIN 启动发现，请添加`appSetting owin:AutomaticAppStartup`值为`"false"`web.config 文件中。

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>创建 ASP.NET Web 应用使用 OWIN 启动

1. 创建空的 Asp.Net web 应用程序并将其命名**StartupDemo**。 -安装`Microsoft.Owin.Host.SystemWeb`使用 NuGet 包管理器。 从**工具**菜单上，选择**库程序包管理器**，，然后**程序包管理器控制台**。 输入以下命令：  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- 添加的 OWIN 启动类。 在 Visual Studio 2013 中右键单击项目，然后选择**添加类**。-在**添加新项**对话框框中，输入*OWIN*在搜索字段和更改该名称与 Startup.cs，然后单击**添加**。  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 你想要添加的下一步时间*Owin 启动类*，则将为可从**添加**菜单。  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 或者，你可以右键单击项目并选择**添加**，然后选择**新项**，然后选择**Owin 启动类**。  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- 中的生成的代码替换*Startup.cs*替换为以下文件：  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 `app.Use` Lambda 表达式用于注册指定的中间件组件向 OWIN 管道。 在这种情况下，我们要设置的传入请求的响应传入的请求之前的日志记录。 `next`参数为的委托 ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [任务](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) 到管道中的下一个组件。 `app.Run` Lambda 表达式挂钩到传入的请求管线，并提供响应机制。
     > [!NOTE]
     > 在上面的代码情况下，我们已注释掉`OwinStartup`属性，我们要依赖于正在运行名为的类的约定`Startup`。-按***F5***运行该应用程序。 命中刷新几次。  
  
    ![](owin-startup-class-detection/_static/image4.png)  
注意： 在本教程中的映像显示的数字将不匹配看到的选项数目。 毫秒字符串用于刷新页面时显示新的响应。  
 你可以看到中的跟踪信息**输出**窗口。  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>添加更多的启动类

在本部分中，我们将添加另一个 Startup 类。 你可以将多个 OWIN 启动类添加到你的应用程序。 例如，你可能想要创建用于开发、 测试和生产的启动类。

1. 创建一个新的 OWIN 启动类并将其命名`ProductionStartup`。
2. 将生成的代码替换为以下代码：

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. 按控件 F5 运行应用程序。 `OwinStartup`属性指定运行生产 startup 类。  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. 创建另一个 OWIN 启动类并将其命名`TestStartup`。
5. 将生成的代码替换为以下代码：  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 `OwinStartup`上面的属性重载指定`TestingConfiguration`作为*友好*Startup 类的名称。
6. 打开*web.config*文件并添加指定的启动类的友好名称的 OWIN 应用程序启动密钥：

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. 按控件 F5 运行应用程序。 应用程序设置元素采用引用单元格以及测试运行配置。  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. 删除*友好*名称从`OwinStartup`属性中`TestStartup`类。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. 替换中的 OWIN 应用程序启动密钥*web.config*替换为以下文件：

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. 还原`OwinStartup`在由 Visual Studio 生成的默认属性代码的每个类的属性：  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 每个以下的 OWIN 应用程序的启动密钥将导致要运行的生产类。 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 最后一个的启动密钥指定的启动配置方法。 以下的 OWIN 应用程序启动密钥允许您更改配置类的名称`MyConfiguration`。

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>使用 Owinhost.exe

1. Web.config 文件替换为以下标记：  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 最后一个键 wins，因而在这种情况下`TestStartup`指定。
2. 从 PMC 安装 Owinhost: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. 导航到应用程序文件夹 (此文件夹包含*Web.config*文件) 并在命令提示符并键入： 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 命令窗口将显示： 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. 启动浏览器的 url `http://localhost:5000/`。  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 OwinHost 遵循上面列出的启动约定。
5. 在命令窗口中，按 Enter 以退出 OwinHost。
6. 在`ProductionStartup`类中，添加以下 OwinStartup 属性指定的友好名称的*ProductionConfiguration*。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. 在命令提示符并键入： 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 加载生产 startup 类。  
    ![](owin-startup-class-detection/_static/image9.png)  
 我们的应用程序具有多个启动类，并在此示例中我们具有延迟到运行时加载哪些 startup 类。
8. 测试以下运行时启动选项：

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
