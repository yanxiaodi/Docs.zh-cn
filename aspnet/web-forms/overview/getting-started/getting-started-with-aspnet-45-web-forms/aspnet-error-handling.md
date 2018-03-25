---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: ASP.NET 错误处理 |Microsoft 文档
author: Erikre
description: 本系列教程将教您生成有关我们使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 的 ASP.NET Web 窗体应用程序的基础知识...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 3f732ae6f1b7845bcae88912b4a4fe26574c10de
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="aspnet-error-handling"></a>ASP.NET 错误处理
====================
通过[艾力克 Reitan](https://github.com/Erikre)

[下载 Wingtip Toys 示例项目 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下载电子书 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教程将教您构建使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web 窗体应用程序的基础知识。 Visual Studio 2013[包含 C# 源代码项目](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)是可以附带本系列教程。


在本教程中，你将要修改 Wingtip Toys 示例应用程序，包括错误处理和错误日志记录。 错误处理将允许应用程序适当地处理错误，并相应地显示错误消息。 错误日志记录将允许你查找并修复已发生的错误。 本教程以前一教程"URL 路由"上构建，并为 Wingtip Toys 教程系列的一部分。

## <a name="what-youll-learn"></a>你将学习：

- 如何添加全局错误处理的应用程序的配置。
- 如何添加在应用程序、 页和代码级别处理的错误。
- 如何记录供以后查看的错误。
- 如何显示不会危及安全性的错误消息。
- 如何实现错误日志记录模块和处理程序 (ELMAH) 错误日志记录。

## <a name="overview"></a>概述

ASP.NET 应用程序必须能够处理以一致的方式在执行期间发生的错误。 ASP.NET 使用公共语言运行时 (CLR)，它提供一种向应用程序的错误通知以统一的方式。 当发生错误时，则会引发异常。 异常是任何错误、 条件或应用程序遇到的意外的行为。

在 .NET Framework 中，异常是从 `System.Exception` 类继承的对象。 异常引发自发生问题的代码区域。 该异常调用堆栈中向上传递到其中的应用程序提供用于处理异常的代码的位置。 如果应用程序不处理异常，则会强制浏览器以显示错误详细信息。

作为最佳做法是，处理在中的代码级别中的错误`Try` / `Catch` / `Finally`内你的代码块。 请尝试将这些块，以便用户可以更正它们出现的上下文中的问题。 如果错误处理块太远来自发生错误，会变得更困难向用户提供所需若要解决此问题的信息。

### <a name="exception-class"></a>异常类

异常类是从其继承异常的基类。 大多数异常对象是异常类中，一些派生类的实例如`SystemException`类，`IndexOutOfRangeException`类，或`ArgumentNullException`类。 此异常类具有属性，如`StackTrace`属性，`InnerException`属性，与`Message`属性，提供有关的错误的发生的特定信息。

### <a name="exception-inheritance-hierarchy"></a>异常继承层次结构

运行时具有一组基本的异常派生自`SystemException`运行时会引发时遇到异常的类。 从异常类，如继承的类的大多数`IndexOutOfRangeException`类和`ArgumentNullException`类中，未实现其他成员。 因此，最重要的信息，用于处理的异常可以找到异常、 异常名称和包含在异常中的信息的层次结构中。

### <a name="exception-handling-hierarchy"></a>异常处理层次结构

在 ASP.NET Web 窗体应用程序，可以基于特定处理层次结构处理异常。 可以在以下级别上处理的异常：

- 应用程序级别
- 页面级别
- 代码级别

当应用程序处理异常时，可以通常检索和向用户显示有关异常的异常类继承的其他信息。 除了应用程序、 页上，和代码级别，你还可以处理在 HTTP 模块级别和通过使用 IIS 自定义处理程序的异常。

### <a name="application-level-error-handling"></a>应用程序级别错误处理

你可以通过修改你的应用程序配置或添加处理应用程序级别的默认错误`Application_Error`中的处理程序*Global.asax*应用程序文件。

你可以通过添加处理默认错误和 HTTP 错误`customErrors`部分到*Web.config*文件。 `customErrors`部分允许您指定将重定向到用户，发生错误时的默认页。 它还允许你指定的特定状态代码错误的各个页。

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

遗憾的是，当你使用配置将用户重定向到其他页面，你没有发生的错误的详细信息。

但是，你可以捕获的任何位置发生的错误在你的应用程序通过将代码添加到`Application_Error`中的处理程序*Global.asax*文件。

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>页面级别错误事件处理

页级别处理程序使用户返回到页上，而发生错误，但由于没有维持控件的实例，将不再有任何内容页上。 为了向应用程序的用户提供的错误详细信息，你必须专门写入错误详细信息页。

日志未处理的错误或将用户定向到可以显示有用信息的页面，你通常将使用页级错误处理程序。

此代码示例演示在 ASP.NET Web 页中的错误事件的处理程序。 此处理程序捕获不内已处理的所有异常`try` / `catch`页中的块。

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

处理错误后，你必须通过调用清除它`ClearError`Server 对象的方法 (`HttpServerUtility`类)，否则你将看到先前已发生错误。

### <a name="code-level-error-handling"></a>代码级别错误处理

Try-catch 语句包含的 try 块后跟一个或多个 catch 子句，指定不同异常的处理程序。 当引发异常时，公共语言运行时 (CLR) 查找处理此异常的 catch 语句。 如果当前正在执行的方法不包含 catch 块，则 CLR 会查看当前的方法，依次类推，在调用堆栈中向上调用的方法。 如果找到没有任何 catch 块，则 CLR 向用户显示一条未处理的异常消息，并停止执行该程序。

下面的代码示例演示使用的常用方式`try` / `catch` / `finally`来处理错误。

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

在上述代码中，try 块包含需要防范可能发生的异常的代码。 直到引发异常或块已成功完成，将执行此块。 如果任一`FileNotFoundException`异常或`IOException`则会发生异常，执行传输到其他页面。 然后中, 包含的代码块最后执行的无论是否出错。

## <a name="adding-error-logging-support"></a>添加错误日志记录支持

在添加之前 Wingtip Toys 示例应用程序处理的错误，可以将错误日志记录通过添加支持添加`ExceptionUtility`类到*逻辑*文件夹。 通过执行此操作，请在每次应用程序处理错误，错误详细信息将添加到错误日志文件。

1. 右键单击*逻辑*文件夹，然后选择**添加** - &gt; **新项**。   
 随即出现“添加新项”对话框。
2. 选择**Visual C#**  - &gt; **代码**左侧的模板组。 然后，选择**类**从中间列表并将其命名**ExceptionUtility.cs**。
3. 选择“添加”。 将显示新的类文件。
4. 将现有代码替换为以下代码：  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

发生异常时，可以通过调用写入到的异常错误日志文件异常`LogException`方法。 此方法采用两个参数的异常对象，包含有关异常的源的详细信息的字符串。 异常日志将写入到*errorlog.txt 中*文件中*应用\_数据*文件夹。

### <a name="adding-an-error-page"></a>添加一个错误页面

Wingtip Toys 示例应用程序，在一页将用于显示错误。 错误页旨在向站点的用户显示一条安全错误消息。 但是，如果用户是开发人员进行的 HTTP 请求的计算机上本地提供代码所在的位置，将在错误页上显示其他错误详细信息。

1. 右键单击项目名称 (**Wingtip Toys**) 中**解决方案资源管理器**和选择**添加** - &gt; **新项**.   
 随即出现“添加新项”对话框。
2. 选择**Visual C#**  - &gt; **Web**左侧的模板组。 从中间列表中选择**包含母版页的 Web 窗体**，并将其命名**ErrorPage.aspx**。
3. 单击 **“添加”**。
4. 选择*Site.Master*文件作为主页上，，然后选择**确定**。
5. 现有的标记替换为以下代码：   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. 替换现有代码的代码隐藏 (*ErrorPage.aspx.cs*) 以使其显示，如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

错误页显示时，`Page_Load`执行事件处理程序。 在`Page_Load`处理程序中，确定第一次处理错误情况的位置。 然后，所发生的最后一个错误由调用`GetLastError`Server 对象的方法。 如果异常不再存在，将创建一般异常。 然后，如果本地发出 HTTP 请求，会显示所有错误详细信息。 在这种情况下，仅运行 web 应用程序的本地计算机将看到这些错误详细信息。 已显示的错误信息后，错误将添加到日志文件，并从服务器清除错误。

### <a name="displaying-unhandled-error-messages-for-the-application"></a>显示应用程序的未处理的错误消息

通过添加`customErrors`部分到*Web.config*文件，可以快速处理发生在整个应用程序的简单错误。 你还可以指定如何处理基于其状态代码值，如 404-找不到文件的错误。

#### <a name="update-the-configuration"></a>更新配置

通过添加更新的配置`customErrors`部分到*Web.config*文件。

1. 在**解决方案资源管理器**，找到并打开*Web.config* Wingtip Toys 示例应用程序根目录的文件。
2. 添加`customErrors`部分到*Web.config*文件内`<system.web>`节点，如下所示：   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. 保存*Web.config*文件。

`customErrors`部分指定的模式，设置为"开"。 它还指定`defaultRedirect`，它指示应用程序的页后，可以导航到，发生错误时。 此外，你添加一个指定如何处理 404 错误，找不到页面时的特定错误元素。 稍后在本教程中，你将添加额外的错误处理，将捕获的应用程序级的错误详细信息。

#### <a name="running-the-application"></a>运行应用程序

你可以运行应用程序现在以查看更新的路由。

1. 按**F5**运行 Wingtip Toys 示例应用程序。  
 浏览器将打开并显示*Default.aspx*页。
2. 到浏览器中输入以下 URL (请务必使用**你**端口号):  
    `https://localhost:44300/NoPage.aspx`
3. 查看*ErrorPage.aspx*浏览器中显示。 

    ![ASP.NET 错误处理-找不到页面错误](aspnet-error-handling/_static/image1.png)

当请求*NoPage.aspx*页上，不存在，错误页将显示简单的错误消息和详细的错误信息如果提供了其他详细信息。 但是，如果从远程位置，则用户要求不存在页，错误页将只会以红色显示的错误消息。

### <a name="including-an-exception-for-testing-purposes"></a>出于测试目的包括异常

若要验证你的应用程序出错时的运行方式发生，你故意在 ASP.NET 中创建错误条件。 在 Wingtip Toys 示例应用程序，将加载默认页面以查看会发生什么情况时引发的测试异常。

1. 打开的代码隐藏*Default.aspx* Visual Studio 中的页。   
 *Default.aspx.cs*将显示代码隐藏页。
2. 在`Page_Load`处理程序中，将代码添加，以便处理程序显示方式，如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

它是可以创建各种不同类型的异常。 在上述代码中，创建`InvalidOperationException`时*Default.aspx*加载页。

#### <a name="running-the-application"></a>运行应用程序

你可以运行应用程序以查看应用程序如何处理异常。

1. 按**CTRL + F5**运行 Wingtip Toys 示例应用程序。  
 应用程序会引发 InvalidOperationException。 

    > [!NOTE] 
    > 
    > 必须按**CTRL + F5**以显示页面，而无需中断到代码以在 Visual Studio 中查看错误根源。
2. 查看*ErrorPage.aspx*浏览器中显示。 

    ![ASP.NET 错误处理的错误页](aspnet-error-handling/_static/image2.png)

如你所见错误详细信息中，已捕获异常`customError`主题中*Web.config*文件。

### <a name="adding-application-level-error-handling"></a>添加应用程序级别错误处理

而不是异常使用的陷阱`customErrors`主题中*Web.config*文件，其中获取有关异常的少信息时，你可以捕获应用程序级别错误，并检索错误详细信息。

1. 在**解决方案资源管理器**，找到并打开*Global.asax.cs*文件。
2. 添加**应用程序\_错误**处理程序，使其显示，如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

在应用程序中，发生错误时`Application_Error`调用处理程序。 此处理程序中检索并评审中的最后一个异常。 如果未经处理异常，且该异常包含内部异常详细信息 (即，`InnerException`不为 null)，应用程序来将执行传输到错误页并显示异常详细信息的位置。

#### <a name="running-the-application"></a>运行应用程序

你可以运行应用程序以查看通过处理此异常的应用程序级别提供的其他错误详细信息。

1. 按**CTRL + F5**运行 Wingtip Toys 示例应用程序。  
 应用程序会引发`InvalidOperationException`。
2. 查看*ErrorPage.aspx*浏览器中显示。 

    ![ASP.NET 错误处理的应用程序级别错误](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>添加页级错误处理

你可以添加页面级别错误处理到页上通过使用添加`ErrorPage`属性设为`@Page`指令的页上，或通过添加`Page_Error`到代码隐藏页的事件处理程序。 在本部分中，你将添加`Page_Error`事件处理程序将传输到执行*ErrorPage.aspx*页。

1. 在**解决方案资源管理器**，找到并打开*Default.aspx.cs*文件。
2. 添加`Page_Error`处理程序，以便代码隐藏显示为遵循：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

在页上，发生错误时`Page_Error`调用事件处理程序。 此处理程序中检索并评审中的最后一个异常。 如果`InvalidOperationException`发生，`Page_Error`事件处理程序来将执行传输到错误页并显示异常详细信息的位置。

#### <a name="running-the-application"></a>运行应用程序

你可以运行应用程序现在以查看更新的路由。

1. 按**CTRL + F5**运行 Wingtip Toys 示例应用程序。  
 应用程序会引发`InvalidOperationException`。
2. 查看*ErrorPage.aspx*浏览器中显示。 

    ![ASP.NET 错误处理的页级别的错误](aspnet-error-handling/_static/image4.png)
3. 关闭浏览器窗口。

### <a name="removing-the-exception-used-for-testing"></a>删除用于测试的异常

若要允许 Wingtip Toys 示例应用程序的功能而不引发本教程中前面添加的异常，删除异常。

1. 打开的代码隐藏*Default.aspx*页。
2. 在`Page_Load`处理程序中，删除代码引发异常，以便处理程序显示，如下所示：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>添加代码级错误日志记录

本教程中前面所述，你可以添加 try/catch 语句尝试运行的代码部分并处理发生的第一个错误。 在此示例中，你将仅错误详细信息写入错误日志文件，以便可以更高版本查看错误。

1. 在**解决方案资源管理器**中*逻辑*文件夹中，找到并打开*PayPalFunctions.cs*文件。
2. 更新`HttpCall`方法，以便代码显示为遵循：   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

在上面的代码调用`LogException`中包含的方法`ExceptionUtility`类。 你添加*ExceptionUtility.cs*类文件*逻辑*本教程中前面的文件夹。 `LogException` 方法采用两个参数。 第一个参数是异常对象。 第二个参数是用于识别错误的源的字符串。

### <a name="inspecting-the-error-logging-information"></a>检查错误日志记录信息

如前所述，你可以使用错误日志以确定应首先修复你的应用程序中的哪些错误。 当然，将记录已捕获并写入错误日志的错误。

1. 在**解决方案资源管理器**，找到并打开*errorlog.txt 中*文件中*应用\_数据*文件夹。   
 你可能需要选择"**显示所有文件**"选项或"**刷新**"顶部的选项**解决方案资源管理器**若要查看*errorlog.txt 中*文件。
2. 查看显示在 Visual Studio 中的错误日志： 

    ![ASP.NET 错误处理-errorlog.txt 中](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>安全的错误消息

它是**特别要注意**，当你的应用程序显示错误消息时，它不应该泄露恶意用户可能会发现有助于攻击你的应用程序的信息。 例如，如果你的应用程序未成功尝试写入到的数据库，它应该不会显示错误消息，其中包含正在使用的用户名。 出于此原因，是向用户显示为红色的一般性错误消息。 向开发人员在本地计算机上仅显示所有其他错误详细信息。

## <a name="using-elmah"></a>使用 ELMAH

ELMAH （错误日志记录模块和处理程序） 是所插入作为 NuGet 程序包 ASP.NET 应用程序错误日志记录功能。 ELMAH 提供以下功能：

- 日志记录的未经处理的异常。
- 要查看整个日志的记录的未处理异常的 web 页面。
- 要查看每个的完整详细信息的 web 页面记录异常。
- 电子邮件通知的每个错误在它发生的时间。
- RSS 源从日志的最后 15 个错误。

你可以使用 ELMAH 之前，必须安装它。 此操作会简单使用*NuGet*程序包安装程序。 本系列教程中前面所述，NuGet 是可以轻松地安装和更新开放源代码库和 Visual Studio 中的工具的 Visual Studio 扩展。

1. 在 Visual Studio 中，从**工具**菜单上，选择**库程序包管理器** - &gt; **管理解决方案的 NuGet 包**。 

    ![ASP.NET 错误处理的管理解决方案的 NuGet 包](aspnet-error-handling/_static/image6.png)
2. **管理 NuGet 包**对话框中将显示在 Visual Studio 内。
3. 在**管理 NuGet 包**对话框框中，展开**联机**在左侧，然后选择**nuget.org**。然后，查找并安装**ELMAH**从联机可用包列表的包。 

    ![ASP.NET 错误处理-ELMA NuGet 包](aspnet-error-handling/_static/image7.png)
4. 你将需要 internet 连接才能下载包。
5. 在**选择项目**对话框框中，请确保**WingtipToys**选择已选中，然后单击**确定**。 

    ![ASP.NET 错误处理的选择项目对话框](aspnet-error-handling/_static/image8.png)
6. 单击**关闭**中**管理 NuGet 包**对话框中，如果需要。
7. 如果 Visual Studio 请求重新加载任何打开的文件，选择"**全是**"。
8. ELMAH 程序包本身中添加条目*Web.config*在你的项目的根目录的文件。 如果 Visual Studio 会要求你如果你想要重新加载修改后*Web.config*文件中，单击**是**。

ELMAH 现已准备好存储出现任何未处理的错误。

### <a name="viewing-the-elmah-log"></a>查看 ELMAH 日志

查看 ELMAH 日志变得简单，但首先你将创建将 ELMAH 日志中记录的未处理的异常。

1. 按**CTRL + F5**运行 Wingtip Toys 示例应用程序。
2. 若要写入的 ELMAH 日志未处理的异常，导航到以下 URL （使用端口号） 浏览器中：  
    `https://localhost:44300/NoPage.aspx` 将显示错误页。
3. 若要显示 ELMAH 日志，请导航到以下 URL （使用端口号） 浏览器中：  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET 错误处理-ELMAH 错误日志](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>摘要

在本教程中，你已学习了如何处理在应用程序级别、 页面级别中，而代码级别的错误。 你已学习如何记录处理和未经处理的错误，供以后查看。 添加 ELMAH 实用程序，以提供异常日志记录和通知到应用程序使用 NuGet。 此外，你已了解有关安全的错误消息的重要性。

## <a name="tutorial-series-conclusion"></a>系列教程结束

*感谢以下沿。我希望此组的教程帮助你更熟悉 ASP.NET Web 窗体。如果你需要有关在 ASP.NET 4.5 和 Visual Studio 2013 中可用的 Web 窗体功能的详细信息，请参阅* [*ASP.NET 和 Web Tools for Visual Studio 2013 发行说明*](../../../../visual-studio/overview/2013/release-notes.md)*.此外，一定要看一看本教程中所述* ***后续步骤****部分和 defintely 试用* [*免费 Azure 试用版*](https://azure.microsoft.com/pricing/free-trial/)*.*

![谢谢-艾力克](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>后续步骤

了解有关部署到 Microsoft Azure web 应用程序的详细信息，请参阅[将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET Web 窗体应用部署到 Azure 网站](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)。

## <a name="free-trial"></a>免费试用版

[Microsoft Azure 的免费试用版](https://azure.microsoft.com/pricing/free-trial/)  
 你的网站发布到 Microsoft Azure 将为你节省时间、 维护和开销。 它是一个到你的 web 应用部署到 Azure 的快速过程。 当你需要进行维护和监视你的 web 应用时，Azure 提供了各种各样的工具和服务。 管理数据、 通信、 标识、 备份，消息传递、 媒体和在 Azure 中的性能。 并且，所有这些提供在成本效率很高的方法。

## <a name="additional-resources"></a>其他资源

[与 ASP.NET 运行状况监视的日志记录错误详细信息](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>致谢

我要特别感谢以下人员执行了巨大的内容的本系列教程的贡献：

- [Alberto Poblacion、 MVP &amp; MCT、 西班牙](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen，荷兰](http://blog.alexthissen.nl/)(twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier，USA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik，斯洛文尼亚](http://twitter.com/bvrhovnik)
- [Bruno Sonnino，巴西](http://msmvps.com/blogs/bsonnino)(twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos dos Santos 巴西](http://www.carloscds.net/)
- [Dave Campbell，USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway、 Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael 升音号、 美国](http://www.930solutions.com/)(twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike Pope
- [Mitchel 卖方、 美国](http://www.mitchelsellers.com/)(twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba Microsoft](http://linqto.me/Links/pcociuba)
- [圣保罗 Morgado，葡萄牙](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>社区贡献

- 格雷厄姆 · Mendick ([@grahammendick](http://twitter.com/grahammendick))  
 Visual Studio 2012 相关 MSDN 上的代码示例：[导航 Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
 Visual Studio 2012 相关 MSDN 上的代码示例：[在 Visual Basic 中的 ASP.NET 4.5 Web 窗体教程系列](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo-Microsoft 技术受众参与者 (twitter: @driazevedo)  
 Visual Studio 2012 翻译： [Iniciando com Visão Geral 的 ASP.NET Web 窗体 4.5-Parte 1-Introdução e](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

>[!div class="step-by-step"]
[上一篇](url-routing.md)
