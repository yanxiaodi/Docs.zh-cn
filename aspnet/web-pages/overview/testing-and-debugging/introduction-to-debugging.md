---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: "调试 ASP.NET Web 简介页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "调试是查找和修复错误，在你的代码页中的过程。 本章展示的一些工具和技术可用于调试和 analyz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: 0b6b5a886efe515b434948dade1ae840ddaecd42
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>调试 ASP.NET Web 简介页 (Razor) 站点
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了调试在 ASP.NET Web 页 (Razor) 网站中的页的各种方法。 调试是查找和修复错误，在你的代码页中的过程。
> 
> **你将学习：** 
> 
> - 如何显示信息，可帮助分析和调试页。
> - 如何使用调试工具在 Visual Studio 中。
>   
> 
> 这些是文章中引入的 ASP.NET 功能：
> 
> - `ServerInfo`帮助器。
> - `ObjectInfo`帮助器。
>   
> 
> ## <a name="software-versions"></a>软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
> - Visual Studio 2013
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。 你可以使用 WebMatrix 3，但不是支持集成的调试器。


排除的错误和问题在代码中的一个重要方面是在第一个位置中避免它们。 你可以执行该操作通过将放在有可能导致到的错误代码部分`try/catch`块。 有关详细信息，请参阅部分处理中的错误[ASP.NET Web 编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkId=202890)。

`ServerInfo`帮助程序是一个诊断工具，将概述有关承载你的页面的 web 服务器环境的信息。 它还显示当浏览器请求页时发送的 HTTP 请求信息。 `ServerInfo`帮助器将显示当前的用户标识，发出了请求、 的浏览器的类型，依此类推。 这种信息可以帮助你解决常见的问题。

1. 创建一个名为的新 web 页*ServerInfo.cshtml*。
2. 在页结束时，只在关闭前`</body>`标记中，添加`@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    你可以添加`ServerInfo`页中的任意位置的代码。 但将其添加在结束会保留其输出独立于其他页面内容，这样，就更易读。

    > [!NOTE] 
    > 
    > **重要**网页移到生产服务器之前，应从你的 web 页面删除诊断的任何代码。 这适用于`ServerInfo`帮助程序，以及这篇文章涉及将代码添加到页上的其他诊断技术。 因为这种类型的信息可能会给了恶意企图的人员有用，你不希望您的网站访问者，以在服务器和类似详细信息，查看有关你的服务器名称、 用户名称、 路径信息。
3. 保存页并在浏览器中运行它。

    ![调试 1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo`帮助器页中显示的信息的四个表：

    - 服务器配置。 本部分提供有关托管的 web 服务器，包括计算机名称、 你正在运行的 ASP.NET、 域名和服务器时间的版本信息。
    - ASP.NET 服务器变量。 本部分提供有关的许多的 HTTP 协议详细信息 （调用 HTTP 变量） 的详细信息并值是每个网页请求的一部分。
    - HTTP 运行时信息。 本部分提供有关的详细信息的 Microsoft.NET Framework 下运行的 web 页、 路径、 有关缓存中，依次类推的详细信息的版本。 (正如你在中学到[ASP.NET Web 编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkId=202890)、 使用 Razor 语法基于 Microsoft 的 ASP.NET web 服务器技术，它本身基于广泛的软件的 ASP.NET Web Pages开发库中调用.NET Framework。）
    - 环境变量。 本部分提供的 web 服务器上的所有本地环境变量及其值的列表。

    所有服务器和请求信息的完整说明不在本文的范围，但你可以看到，`ServerInfo`帮助程序返回了大量的诊断信息。 有关值的详细信息，`ServerInfo`返回时，请参阅[识别环境变量](https://technet.microsoft.com/library/dd560744(WS.10).aspx)Microsoft TechNet 网站上和[IIS 服务器变量](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)MSDN 网站上。

## <a name="embedding-output-expressions-to-display-page-values"></a>嵌入输出表达式，以显示页值

若要查看你的代码中发生的情况的另一种方法是将输出表达式嵌入在页中。 如您所知，可以通过添加类似于直接输出变量的值`@myVariable`或`@(subTotal * 12)`到页。 以进行调试，你可以在代码中，在关键点将这些输出表达式。 这可以运行你的页面时，请参阅关键变量或计算的结果的值。 当你已完成调试，可以删除表达式或注释掉这些。此过程说明了使用嵌入式的表达式来帮助调试页面的典型方式。

1. 创建名为的新 WebMatrix 页*OutputExpression.cshtml*。
2. 将页面内容替换为以下内容：

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    该示例使用`switch`用于检查的值的语句`weekday`变量，然后显示它是不同的输出消息，具体取决于一周中的哪一天。 在示例中，`if`中第一个代码块的块任意更改通过将一天添加到当前周日期值的日期是星期几。 这是用于说明目的引入错误。
3. 保存页并在浏览器中运行它。

    为错误的日期是星期几，该页显示消息。 任何日期是星期几它实际上是，将看到更高版本的一天的消息。 尽管在这种情况下你知道为什么消息处于关闭状态 （因为代码有意设置不正确的日期值），实际上通常很难知道其中事情的进展状况错误代码中。 若要调试，你需要了解所发生的情况的键的对象和变量的值如`weekday`。
4. 将输出表达式添加的方法是插入`@weekday`在代码中的注释指示的两个位置所示。 这些输出表达式将显示变量的值在该点在代码执行。

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. 保存并在浏览器中运行页面。

    页面显示实际日期是星期几首先，然后将结果添加一天，然后从生成的消息的更新日期是星期几`switch`语句。 从两个变量的表达式输出 (`@weekday`) 具有天之间没有空格，因为您没有添加任何 HTML`<p>`输出; 标记表达式可以只供测试。

    ![调试 2](introduction-to-debugging/_static/image2.jpg)

    现在，你可以查看错误所在。 当你第一次显示`weekday`变量中的代码，它显示正确的日期。 当你显示它的第二个时间之后,`if`在代码块中，在一天处于关闭状态的一个。 因此，您知道工作日变量的第一个和第二个外观之间发生了内容。 如果这是一个真正的 bug，这种方法可帮助你缩小问题的原因的代码的位置。
6. 修复在页中的代码，删除你添加的两个输出表达式，并删除更改日期是星期几的代码。 完成剩余块的代码类似于下面的示例：

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. 在浏览器中运行页面。 此时会显示正确的消息显示为实际日期是星期几。

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>使用 ObjectInfo 帮助器来显示对象值

`ObjectInfo`帮助器显示类型和每个对象传递给它的值。 可用来在代码中 （如你在前面的示例的输出表达式），查看变量和对象的值加上你可以看到的数据类型有关对象的信息。

1. 打开名为的文件*OutputExpression.cshtml*之前创建。
2. 将替换为以下代码块中页的所有代码：

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. 保存并在浏览器中运行页面。

    ![调试-4](introduction-to-debugging/_static/image3.jpg)

    在此示例中，`ObjectInfo`帮助器将显示两个项：

    - 类型。 对于第一个变量，该类型是`DayOfWeek`。 对于第二个变量，该类型是`String`。
    - 值。 在这种情况下，因为你已在页中显示的问候语变量的值，值再次显示当你将变量传递到`ObjectInfo`。

    对于更复杂的对象，`ObjectInfo`帮助器可以显示详细信息 &#8212; 基本上，它可以在其中显示的类型和的所有对象的属性的值。

## <a name="using-debugging-tools-in-visual-studio"></a>使用 Visual Studio 中的调试工具

对于更全面的调试体验，使用 Visual Studio 2013 或免费[Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)。 使用 Visual Studio，可以在你想要检查的行在代码中设置断点。

![设置断点](introduction-to-debugging/_static/image1.png)

当你测试 web 站点时，正在执行的代码都将在断点处暂停。

![到达断点](introduction-to-debugging/_static/image2.png)

你可以检查变量和单步执行代码--逐行的当前值。

![请参阅值](introduction-to-debugging/_static/image3.png)

有关使用 Visual Studio 中集成的调试器来调试 ASP.NET Razor 页的信息，请参阅[编程 ASP.NET Web 页 (Razor) 使用 Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)。

## <a name="additional-resources"></a>其他资源

- [使用 Visual Studio 编程 ASP.NET Web 页 (Razor)](https://go.microsoft.com/fwlink/?LinkId=205854)
- [IIS 服务器变量](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)(MSDN)
- [识别环境变量](https://technet.microsoft.com/library/dd560744(WS.10).aspx)(TechNet)
