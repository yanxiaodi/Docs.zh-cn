---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: "了解调试功能的 ASP.NET AJAX |Microsoft 文档"
author: scottcate
description: "调试代码的能力是在其集中而不管他们正在使用的技术应该具有每个开发人员技能。 许多开发人员时..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 426d0182978faf7fc7516203fcc84ef0152790ba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>了解 ASP.NET AJAX 调试功能
====================
通过[Scott 类别](https://github.com/scottcate)

[下载 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> 调试代码的能力是在其集中而不管他们正在使用的技术应该具有每个开发人员技能。 许多开发人员都习惯于使用 Visual Studio.NET 或 Web Developer Express 调试 ASP.NET 应用程序使用 VB.NET 或 C# 代码，而一些并不了解，它也是对于调试 JavaScript 之类的客户端代码非常有用。 也可以是相同类型的使用调试.NET 应用程序的技术适用于启用了 AJAX 的应用程序和更具体地说就是 ASP.NET AJAX 应用程序。


## <a name="debugging-aspnet-ajax-applications"></a>调试 ASP.NET AJAX 应用程序

Dan Wahlin

调试代码的能力是在其集中而不管他们正在使用的技术应该具有每个开发人员技能。 毫无疑问了解可用的不同的调试选项可以保存大量时间上的项目和甚至可能是几个将来的麻烦。 许多开发人员都习惯于使用 Visual Studio.NET 或 Web Developer Express 调试 ASP.NET 应用程序使用 VB.NET 或 C# 代码，而一些并不了解，它也是对于调试 JavaScript 之类的客户端代码非常有用。 也可以是相同类型的使用调试.NET 应用程序的技术适用于启用了 AJAX 的应用程序和更具体地说就是 ASP.NET AJAX 应用程序。

在本文中，你将看到如何使用 Visual Studio 2008 和几个其他工具来调试 ASP.NET AJAX 应用程序快速找到 bug 和其他问题。 此讨论将包含有关启用 Internet Explorer 6 或更高的调试、 使用 Visual Studio 2008 和脚本资源管理器来单步执行代码，以及使用其他可用的工具，如 Web 开发帮助器的信息。 你还将学习如何调试在 Firefox 使用扩展命名 Firebug 这样在逐句通过直接在浏览器中没有任何其他工具的 JavaScript 代码中的 ASP.NET AJAX 应用程序。 最后，将向你介绍 ASP.NET AJAX 库，可帮助与各种的调试任务，如跟踪和代码断言语句中的类。

你尝试调试在 Internet Explorer 中查看的页之前有几个基本步骤，你将需要执行以便为调试启用它。 让我们看看需要执行若要开始一些基本安装程序要求。

## <a name="configuring-internet-explorer-for-debugging"></a>为调试配置 Internet Explorer

大多数人不感兴趣使用 Internet Explorer 查看网站上遇到 JavaScript 问题。 事实上，平均用户无法甚至知道要执行的操作如果它们看到一条错误消息。 因此，默认情况下，在浏览器的关闭状态是调试选项。 但是，它是非常简单，开启调试并将其用于开发新的 AJAX 应用程序。

若要启用调试功能，请转到 Internet 资源管理器菜单上的工具 Internet 选项，然后选择高级选项卡。在浏览部分内确保以下各项未选中状态：

- 禁用脚本调试 （Internet 资源管理器）
- 禁用脚本调试 （其他）

尽管不是必需的如果你尝试调试的应用程序，你可能想要立即可见和明显的页中的任何 JavaScript 错误。 你可以强制所有错误都以通过选中"显示关于每个脚本错误的通知"复选框显示消息框。 虽然这是一个实用的选项，若要打开在开发应用程序时，它可以很快就会烦人如果由于遇到 JavaScript 错误的可能性是可以实现相当不错，只需在浏览其他网站。

已针对调试正确配置后，应如下图 1 显示哪些 Internet Explorer 高级对话框。


[![为调试配置 Internet Explorer。](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**图 1**： 用于调试配置 Internet Explorer。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


一旦已开启调试，你将看到新菜单项显示在视图菜单上名为脚本调试器。 在下一条语句，它具有两个选项可用包括打开，并会破坏。 当选中打开时将提示您进行调试在 Visual Studio 2008 （请注意，Visual Web Developer Express 还可用于调试） 页。 如果当前正在运行 Visual Studio.NET 可以选择使用该实例或者若要创建的新实例。 选择下一个语句处中断时系统将提示你时执行的 JavaScript 代码调试页。 如果在页的 onLoad 事件中执行的 JavaScript 代码可以刷新页后，可以触发调试会话。 如果单击的按钮后运行 JavaScript 代码则单击该按钮后立即时，调试器会运行。

> *>[!NOTE]如果运行的在 Windows Vista 与用户访问控制 (UAC) 启用，并且必须设置为以管理员身份运行 Visual Studio 2008，Visual Studio 将无法附加到进程时系统会提示你附加。若要解决此问题，首先，启动 Visual Studio，并使用该实例来调试。*


已打开的页和你想要更全面检查它时，尽管下一步部分将演示如何调试的 ASP.NET AJAX 页上直接从 Visual Studio 2008 中，使用 Internet Explorer 脚本调试器选项非常有用。

## <a name="debugging-with-visual-studio-2008"></a>使用 Visual Studio 2008 进行调试

Visual Studio 2008 提供世界各地的开发人员依靠每天向调试.NET 应用程序的调试功能。 内置调试器可以单步执行代码，对象数据，为特定变量监视监视调用堆栈以及其他更多的视图。 除了调试 VB.NET 或 C# 代码，调试器也是用于调试 ASP.NET AJAX 应用程序有帮助，并且将允许你单步调试 JavaScript 代码行的行。 按照焦点可用于调试客户端脚本文件，而不是提供的调试应用程序使用 Visual Studio 2008 的完整过程论文的技术详细信息。

可以通过多种不同方式启动调试 Visual Studio 2008 中的页的过程。 首先，你可以使用上一节中所述的 Internet 资源管理器脚本调试器选项。 这适用于页已加载到浏览器和你想要开始调试它。 或者，可以右键单击解决方案资源管理器中的.aspx 页，并从菜单中选择设为起始页。 如果你习惯于调试 ASP.NET 页然后你已可能执行此操作之前。 按下 F5 后可以调试页面。 但是，通常可以任意位置设置断点时您可能希望在 VB.NET 或 C# 代码中，并不总是使用 JavaScript 的情况下正如你将看到的下一步。

*嵌入字体和外部脚本*

Visual Studio 2008 调试器将嵌入到网页不同于外部 JavaScript 文件中的 JavaScript。 外部脚本文件，你可以打开该文件并在你选择的任意行上设置断点。 可以通过单击代码编辑器窗口左侧的灰色栏区域中设置断点。 当 JavaScript 嵌入直接到页使用`<script>`标记，通过单击灰色栏区域中设置断点不是一个选项。 尝试在嵌入的脚本的行上设置断点会表明"这不是有效的断点位置"的警告。

你可以通过将代码移到外部.js 文件并引用它使用的 src 属性获得解决此问题&lt;脚本&gt;标记：


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

如果将代码移到外部文件不是一个选项或要求比它更多的工作是值得？ 尽管你无法设置断点使用编辑器，可以添加直接到你想要开始调试的代码调试器语句。 你可以使用 ASP.NET AJAX 库中的可用 Sys.Debug 类强制调试启动。 你将了解有关在本文后面的 Sys.Debug 类的详细信息。

使用的示例`debugger`关键字列出 1 所示。 此示例将强制调试器中断正确进行对更新函数的调用之前。

**列表 1。使用调试器关键字来强制 Visual Studio.NET 调试器中断。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Debugger 语句命中后你将提示您调试使用 Visual Studio.NET 的页，可以开始逐句通过代码。 虽然执行此，你可能会遇到访问 ASP.NET AJAX 库脚本文件，因此，让我们使用在页中的问题需要查看使用 Visual Studio。NET 的脚本资源管理器。

## <a name="using-visual-studio-net-windows-to-debug"></a>使用 Visual Studio.NET Windows 调试

在后启动调试会话并开始遍历通过代码使用默认 F11 键，可能会遇到错误对话框中所示请参见图 2，除非页中使用的所有脚本文件都是开放的且可用于调试。


[![可用于调试没有源代码时显示错误对话框。](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**图 2**： 可用于调试没有源代码时显示的错误对话框。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


此对话框会显示，因为 Visual Studio.NET 不确定如何访问某些被的页引用的脚本的源代码。 虽然这可能是非常令人沮丧首先，存在是简单的修复程序。 启动调试会话并命中断点后，请转到调试 Windows 脚本资源管理器窗口上的 Visual Studio 2008 菜单或使用 Ctrl + Alt + N 热键。

> *>[!NOTE]如果看不到脚本资源管理器菜单上列出，请转到工具**自定义* *Visual Studio.NET 菜单上的命令。在类别部分中找到调试条目并单击它以显示所有可用菜单项。在命令列表中，向下滚动到脚本资源管理器，然后将它拖到调试* *Windows 菜单中的前面所述。执行此操作会使脚本资源管理器菜单项可每次运行 Visual Studio.NET。*


脚本资源管理器可以用于查看用于页中的所有脚本和在代码编辑器中打开它们。 脚本资源管理器打开后，双击当前正在调试在代码编辑器窗口中打开它的.aspx 页。 为所有脚本资源管理器中所示的其他脚本中都执行相同操作。 一旦所有脚本处于打开代码窗口可以按 F11 （并使用其他调试热键） 逐句通过代码。 图 3 显示脚本资源管理器的示例。 它列出正在调试的当前文件 (Demo.aspx) 以及两个自定义脚本和动态由 ASP.NET AJAX ScriptManager 插入到页中的两个脚本。


[![脚本资源管理器提供轻松访问用于页中的脚本。](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**图 3**。 脚本资源管理器提供轻松访问用于页中的脚本。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


多个其他 windows 还可以用于单步执行在页中的代码时提供有用的信息。 例如，可以使用局部变量窗口以查看不同页上，若要计算特定变量或条件并查看输出即时窗口中使用的变量的值。 输出窗口还可用于查看跟踪语句写出使用的 Sys.Debug.trace 函数 （将在本文中的更高版本） 或 Internet Explorer Debug.writeln 函数。

逐句通过代码使用调试器时你可以将鼠标放在代码中以查看它们分配的值的变量。 但是，脚本调试程序有时不会显示任何内容如你将鼠标移到给定的 JavaScript 变量。 若要查看的值，突出显示的语句或你正在尝试在代码编辑器窗口中查看，然后将鼠标移到它的变量。 尽管在任何情况下，此方法不起作用，但很多时候你将能够查看而无需在不同的调试窗口，如局部变量窗口中查找的值。

可以在查看演示的一些功能此处讨论的视频教程[http://www.xmlforasp.net](http://www.xmlforasp.net)。

## <a name="debugging-with-web-development-helper"></a>使用 Web 开发帮助程序进行调试

尽管 Visual Studio 2008 （和 Visual Web Developer Express 2008） 是非常有用的调试工具，有一些其他也可用于更轻量的选项。 要释放的最新工具之一是 Web 开发帮助器。 Microsoft 的 Nikhil Kothari （一个 microsoft 密钥的 ASP.NET AJAX 架构师） 已写入该极好的工具，它可以从简单的调试查看 HTTP 请求和响应消息执行许多不同的任务。 Web 开发帮助器可以从以下网址下载[http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx)。

直接在其中可以方便地使用 Internet Explorer 内，可以使用 web 开发帮助器。 它通过从 Internet 资源管理器菜单中选择工具 Web 开发帮助程序启动。 这将在浏览器是很好，因为无需离开浏览器来执行多个任务，如 HTTP 请求和响应消息日志记录的下半部分中打开该工具。 图 4 显示 Web 开发帮助程序中操作的外观。


[![Web 开发帮助器](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**图 4**: Web 开发帮助器 ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Web 开发帮助器不是你将使用的工具逐句通过代码逐行作为随 Visual Studio 2008。 但是，使用它来查看跟踪输出、 轻松地计算脚本中的变量或浏览数据是在一个 JSON 对象内部。 它也是用于查看的 ASP.NET AJAX 页上和服务器来回传递的数据非常有用。

在 Internet Explorer 中打开 Web 开发帮助器后，则必须通过脚本启用脚本调试从菜单中选择 Web 开发帮助器更早版本中图 4 所示启用脚本调试。 这使发生为在运行页面时的截距错误的工具。 它还允许轻松访问的页中的输出的跟踪消息。 若要查看跟踪信息或执行脚本命令，以测试在页面内的不同功能，请从 Web 开发帮助程序菜单中选择脚本显示脚本控制台。 这提供对命令窗口并简单即时窗口访问。

*查看跟踪消息和 JSON 对象数据*

即时窗口可以用于执行脚本命令或甚至加载或保存用于在页中测试不同的函数的脚本。 命令窗口显示跟踪或调试消息写出正在查看的页面。 列出 2 演示如何编写使用 Internet Explorer Debug.writeln 函数的跟踪消息。

**列出 2。写出使用 Debug 类的客户端跟踪消息。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Web 开发帮助程序如果 LastName 属性包含 Doe 一个值，将显示消息"人员姓名： Doe"在脚本控制台命令窗口 （假设，已启用调试）。 Web 开发帮助器还将顶级 debugService 对象添加到用于写出跟踪信息或查看 JSON 对象的内容的页。 列出 3 显示使用 debugService 类跟踪函数的示例。

**列出 3。使用 Web 开发帮助器 debugService 类编写跟踪消息。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

DebugService 类的一个不错的功能是，使之能够即使调试未在从而便于时运行 Web 开发帮助器始终访问跟踪数据的 Internet Explorer 中启用。 时未被该工具使用以调试页面，跟踪语句将被忽略，因为对 window.debugService 的调用将返回 false。

DebugService 类还允许 JSON 对象数据，您可以使用 Web 开发帮助器的检查器窗口。 列出 4 创建一个简单的 JSON 对象，包含个人数据。 在创建对象后进行调用到 debugService 类的检查函数，以允许直观地检查的 JSON 对象。

**列出 4。使用 debugService.inspect 函数查看 JSON 对象数据。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

在页中或通过即时窗口调用 GetPerson() 函数将导致出现在图 5 所示的对象检查器对话框窗口。 通过突出显示，可以动态更改在该对象的属性更改的值文本框中显示的值，然后单击更新链接。 使用对象检查器，可以直观地查看 JSON 对象数据，试用将不同的值应用于属性。

*调试错误*

还允许跟踪数据和 JSON 对象要显示，Web 开发帮助器可还帮助调试在页中的错误。 如果遇到错误，将会提示你继续到下一行代码或调试脚本 （请参阅图 6）。 脚本错误对话框窗口中显示完整的调用堆栈以及行号，以便你可以轻松地识别问题所在脚本中。


[![使用的对象检查器窗口以查看一个 JSON 对象。](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**图 5**： 使用的对象检查器窗口以查看一个 JSON 对象。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


选择调试选项，你可以直接在要查看的变量的值，写出 JSON 对象，还需要更多的 Web 开发帮助器即时窗口中执行脚本语句。 如果重新执行触发错误的操作相同，并且 Visual Studio 2008 的计算机上可用，将提示您启动调试会话，以便你可以单步执行代码行的行中上一节所述。


[![Web 开发帮助程序脚本错误对话框](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**图 6**: Web 开发帮助程序脚本错误对话框 ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*检查请求和响应消息*

调试 ASP.NET AJAX 页时通常很有用，请参阅页和服务器之间发送的请求和响应消息。 查看消息中的内容允许你查看是否正确的数据将传入的消息大小以及。 Web 开发帮助器提供绝佳 HTTP 消息记录器功能，可以轻松地查看数据，为原始文本或更易读的格式。

若要查看 ASP.NET AJAX 请求和响应消息，必须通过选择 Web 开发帮助程序菜单中的 HTTP 启用 HTTP 日志记录启用 HTTP 记录器。 启用后，可以在可通过选择 HTTP 显示 HTTP 日志访问的 HTTP 日志查看器中查看发送从当前页的所有消息。

尽管查看每个请求/响应消息中发送的原始文本是当然很有用 （和一个的 Web 开发帮助程序中的选项），它通常是更轻松地查看多图形格式的消息数据。 一旦启用 HTTP 日志记录，并已记录消息，则可以通过双击 HTTP 日志查看器中的消息查看消息数据。 执行此操作将允许你查看与一条消息，以及实际的消息关联的所有标头内容。 图 7 显示请求消息和响应消息在 HTTP 日志查看器窗口中查看的示例。


[![使用 HTTP 日志查看器查看请求和响应消息数据。](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**图 7**： 使用 HTTP 日志查看器查看请求和响应消息数据。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


HTTP 日志查看器自动分析 JSON 对象，并显示它们使用变得更加快速、 轻松地查看对象的属性数据的树视图。 当 UpdatePanel 正在使用中的 ASP.NET AJAX 页上时，查看器将中断出到各个部分的消息的每个部分中，如图 8 中所示。 这是一项强大功能，这样可以更轻松地查看并了解什么是相比查看原始消息数据消息中。


[![使用 HTTP 日志查看器查看 UpdatePanel 响应消息。](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**图 8**： 使用 HTTP 日志查看器查看 UpdatePanel 响应消息。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


有几种可用来查看除了 Web 开发帮助器的请求和响应消息的其他工具。 另一个不错的选择是免费提供的 Fiddler [http://www.fiddlertool.com](http://www.fiddlertool.com)。尽管不将此处讨论 Fiddler，它也是一个不错的选择时需要全面检查消息标头和数据。

## <a name="debugging-with-firefox-and-firebug"></a>使用 Firefox 和 Firebug 进行调试

虽然 Internet 资源管理器仍是最广泛使用的浏览器，例如 Firefox 其他浏览器将变得非常流行，并且正在多地使用。 因此，你将想要查看和调试 ASP.NET AJAX 网页中 Firefox，以及 Internet 资源管理器以确保你的应用程序正常工作。 Firefox 不能直接与 Visual Studio 2008 以进行调试，尽管它具有调用可用于调试页的 Firebug 扩展。 可以通过转到免费下载 firebug [http://www.getfirebug.com](http://www.getfirebug.com)。

Firebug 提供了可用于单步执行代码行的行，访问在 page 中使用的所有脚本、 查看 DOM 结构，在页中显示 CSS 样式和甚至是跟踪事件发生一个全面的调试环境。 安装完成后，可以通过从 Firefox 菜单中选择工具 Firebug 打开 Firebug 访问 Firebug。 Web 开发帮助器，如 Firebug 但也可以作为独立的应用程序使用浏览器中直接使用。

Firebug 运行后，可以设置断点在 JavaScript 文件的任何行是否脚本嵌入到网页中，或不。 若要设置断点，首先加载相应的页面你想要在 Firefox 中进行调试。 一旦加载页面，选择要从下拉列表中 Firebug 的脚本调试的脚本。 将显示页面使用的所有脚本。 通过单击数据行上 Firebug 的灰色栏区域断点应该必须像就像在 Visual Studio 2008 中设置断点。

断点设置在 Firebug 后可以执行执行需要例如单击按钮或刷新浏览器中触发 onLoad 事件进行调试的脚本所需的操作。 包含断点的行上，将自动停止执行。 图 9 显示中 Firebug 已触发断点的示例。


[![处理 Firebug 中的断点。](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**图 9**： 处理 Firebug 中的断点。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


命中断点后你可以单步执行、 逐过程执行或跳出代码使用箭头按钮。 单步执行代码，在调试器使你可以看到值并深化到对象的右半部分显示脚本变量。 Firebug 还包括一个调用堆栈下拉列表，以查看导致当前正在调试的行的脚本的执行步骤。

Firebug 还包括可用来测试不同的脚本语句、 计算的变量和查看跟踪输出的控制台窗口。 它是通过单击 Firebug 窗口顶部的控制台选项卡上进行访问。 正在调试的页可以还"检查"以通过单击检查选项卡中查看其 DOM 结构和内容。当你将突出显示轻松地查看在页中使用元素的位置鼠标悬停在检查器窗口中显示页上的相应部分的不同 DOM 元素。 "实时"尝试使用将不同的宽度、 样式等应用于元素，可以更改与给定元素相关联的属性值。 这是一个不错的功能，无需在源代码编辑器和 Firefox 浏览器以查看如何简单更改影响页之间不断地切换为你节省。

图 10 显示使用 DOM 检查器来查找名为 txtCountry 在页中的文本框示例。 Firebug 检查器还可以用于查看使用在页以及事件会发生例如跟踪鼠标移动、 按钮单击、 还需要更多的 CSS 样式。


[![使用 Firebug 的 DOM 检查器。](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**图 10**： 使用 Firebug DOM 检查器。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug 使您能够快速调试直接在 Firefox 以及检查页面中的不同元素的理想工具页面轻量。

## <a name="debugging-support-in-aspnet-ajax"></a>调试在 ASP.NET AJAX 的支持

ASP.NET AJAX 库包含许多不同的类可用于简化将添加到网页的 AJAX 功能的过程。 这些类可用于查找在页面内的元素和对其进行处理、 添加新控件、 调用 Web 服务和甚至处理事件。 ASP.NET AJAX 库还包含可以用于增强调试页的过程的类。 在本部分将介绍与 Sys.Debug 类，并请参阅如何可以在应用程序中使用它。

*使用 Sys.Debug 类*

Sys.Debug 类 （位于 Sys 命名空间中的 JavaScript 类） 可以用于执行几个不同的函数包括写入跟踪输出、 执行代码的断言和强制失败，以便可调试的代码。 它用于广泛在 ASP.NET AJAX 库的调试文件 （默认情况下安装在 C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0） 执行条件检测 （调用断言），确保传递的参数正确对象，包含所需的数据的功能，以及将写入跟踪语句。

Sys.Debug 该类会公开可用于处理跟踪、 代码断言或故障表 1 中所示的几个不同函数。

**表 1。Sys.Debug 类函数。**

| **函数名** | **描述** |
| --- | --- |
| 断言 （条件、 消息、 displayCaller） | 断言的条件参数为 true。 如果所测试的条件为 false，则将使用一个消息框来显示消息参数值。 如果 displayCaller 参数为 true，该方法还会显示有关调用方的信息。 |
| clearTrace() | 清除语句从跟踪操作的输出。 |
| fail(message) | 会导致程序停止执行并中断调试器。 消息参数可以用于提供失败的原因。 |
| trace(message) | 消息参数写入跟踪输出。 |
| traceDump （对象、 名称） | 将输出以可读格式的对象的数据。 可以使用 name 参数可为跟踪转储提供标签。 默认情况下，正在转储的对象中的所有子对象将写出。 |

客户端跟踪可在 ASP.NET 中提供的跟踪功能的方式大致相同。 它允许不同的消息，以轻松看到而不会中断应用程序流。 列出 5 演示使用 Sys.Debug.trace 函数来写入跟踪日志的示例。 此函数只需将作为一个参数应写出消息。

**列出 5。使用 Sys.Debug.trace 函数。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

如果执行列出 5 所示的代码不会看到在页中的任何跟踪输出。 若要查看它的唯一方法是使用在 Visual Studio.NET、 Web 开发帮助器或 Firebug 中可用的控制台窗口。 如果您想要查看的页中的跟踪输出，然后你需要添加 TextArea 标记并授予它 TraceConsole id 如下所示：


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

在页中的任何 Sys.Debug.trace 语句将写入 TraceConsole TextArea。

在想要查看的 JSON 对象中包含的数据的情况下，你可以使用 Sys.Debug 类 traceDump 函数。 此函数采用两个参数，包括应转储到跟踪控制台的对象和可以用于标识跟踪输出中的对象的名称。 列出 6 显示使用 traceDump 函数的示例。

**列出 6。使用 Sys.Debug.traceDump 函数。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

图 11 显示的调用 Sys.Debug.traceDump 函数的输出。 请注意，除了写出 Person 对象的数据，它还将写出地址子对象的数据。

除了跟踪，Sys.Debug 类还可以使用执行代码断言。 断言用于测试应用程序运行时满足特定条件。 ASP.NET AJAX 库脚本的调试版本包含几个 assert 语句来测试各种条件。

列出 7 显示使用 Sys.Debug.assert 函数对条件进行测试的示例。 代码测试在更新 Person 对象之前地址对象为 null。


[![Sys.Debug.traceDump 函数的输出。](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**图 11**: Sys.Debug.traceDump 函数的输出。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**列出 7。使用 debug.assert 函数。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

三个参数传递包括要计算，显示如果断言返回 false，并且应显示有关调用方信息的消息的条件。 在其中断言失败的情况下，将显示消息以及调用方信息，如果第三个参数为 true。 图 12 显示出现如果列出 7 所示则断言失败的失败对话框的示例。

最终的函数，以涵盖是 Sys.Debug.fail。 当你想要强制代码无法在脚本中的特定行时可以添加 Sys.Debug.fail 调用而不是通常使用 JavaScript 应用程序中的调试器语句。 Sys.Debug.fail 函数接受单个字符串参数表示失败的原因，如下所示：


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Sys.Debug.assert 失败消息。](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**图 12**: Sys.Debug.assert 失败消息。  ([单击以查看实际尺寸的图像](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


当遇到 Sys.Debug.fail 语句时执行脚本时，消息参数的值将显示在控制台中调试应用程序，例如 Visual Studio 2008 并且将会提示你调试应用程序。 这可以是非常有用的一种情况是当你无法在内联脚本上设置具有 Visual Studio 2008 的断点，但想要在特定的行上停止，因此你可以检查变量的值的代码。

*了解 ScriptManager 控件的 ScriptMode 属性*

ASP.NET AJAX 库包括调试和发布脚本版本安装在 C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 默认情况下。 调试脚本格式设置精美、 易于读取以及具有多个 Sys.Debug.assert 调用遍布在整个它们同时发布脚本会去掉空格 Sys.Debug 类应谨慎使用尽量减小其总体大小。

ScriptManager 控件添加到 ASP.NET AJAX 页读取 web.config 以确定哪些版本的库脚本加载中的编译元素的调试属性。 但是，可以控制如果调试或发布脚本可以加载 （的库脚本或你自己的自定义脚本） 通过更改 ScriptMode 属性。 ScriptMode 接受其成员包括自动、 调试、 版本和继承的 ScriptMode 枚举。

ScriptMode 默认为自动，这意味着 ScriptManager 将检查在 web.config 中的调试属性的值。当为 false 时调试 ScriptManager 将加载 ASP.NET AJAX 的库脚本的版本。 调试为 true 时将加载脚本的调试版本。 更改要释放或调试的 ScriptMode 属性将强制 ScriptManager 加载合适的脚本，而不考虑什么样的调试属性已在 web.config 中的值。列出 8 演示使用 ScriptManager 控件从 ASP.NET AJAX 库加载调试脚本的示例。

**列出 8。加载调试脚本使用 ScriptManager**。


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

你也可以通过列出 9 中所示使用 ScriptManager 的脚本属性以及 ScriptReference 组件加载不同版本 （调试或发布） 的自定义脚本。

**列出 9。使用 ScriptManager 加载自定义脚本。**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> 如果你正在加载使用 ScriptReference 组件的自定义脚本时通过在脚本的底部添加下面的代码加载完脚本必须通知 ScriptManager:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

列出 9 中所示的代码指示 ScriptManager 若要寻找人员脚本的调试版本，因此它将自动查找而不是 Person.js Person.debug.js。 如果 Person.debug.js 文件未找到将引发错误。

在你希望调试的位置，或要加载的自定义脚本的版本取决于在 ScriptManager 控件上设置 ScriptMode 属性的值的情况下，你可以将 ScriptReference 控件的 ScriptMode 属性设置为继承。 这将导致要加载的自定义脚本的适当版本基于 ScriptManager ScriptMode 属性列出 10 中所示。 因为 ScriptManager 控件的 ScriptMode 属性设置为调试，则将加载 Person.debug.js 脚本，并将其在页中使用。

**列出 10。从自定义脚本的 ScriptManager 继承 ScriptMode。**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

通过适当地使用 ScriptMode 属性可以更轻松地调试应用程序和简化的整个过程。 ASP.NET AJAX 库的发布脚本是相当困难，单步执行和读取由于代码格式设置调试脚本格式化专门以便进行调试时已删除。

## <a name="conclusion"></a>结束语

Microsoft ASP.NET AJAX 技术用于构建启用了 AJAX 的应用程序，可以增强最终用户的总体体验提供坚实的基础。 但是，作为与任何编程技术 bug 和其他应用程序问题，将当然出现。 了解有关可用的不同的调试选项可以将大量的时间和结果保存在更稳定的产品。

这篇文章中你已向你介绍了用于调试 ASP.NET AJAX 页包括 Internet Explorer 使用 Visual Studio 2008、 Web 开发帮助器和 Firebug 多种不同的方法。 这些工具可以简化了整体的调试过程，因为你可以访问变量数据、 遍历代码行的行，并查看跟踪语句。 除了讨论各种调试工具，你还了解了如何在应用程序中使用 ASP.NET AJAX 库的 Sys.Debug 类以及如何使用 ScriptManager 类加载调试或发布脚本的版本。

## <a name="bio"></a>简介

Dan Wahlin （Microsoft 最有价值专家 ASP.NET 和 XML Web 服务） 是在接口技术培训的.NET 开发教师和体系结构顾问 ([www.interfacett.com)](http://www.interfacett.com)。 Dan 成立 ASP.NET 开发人员网站的 XML ([www.XMLforASP.NET](http://www.XMLforASP.NET))、 位于 INETA 扬声器局和在几个有关参加会议讲话时。 Dan 共同撰写 Professional Windows dna 的解码 (Wrox) ASP.NET： 提示、 教程和代码 (Sam)、 ASP.NET 1.1 内部解决方案、 Professional ASP.NET 2.0 AJAX (Wrox)、 ASP.NET 2.0 MVP Hacks 和编写的 XML for ASP.NET 开发人员 (Sam)。 当他不编写代码、 项目或丛书时，Dan 将喜欢编写和录制音乐和播放高尔夫球和儿童他妻子与支篮球队。

Scott 类别自 1997 年以来处理与 Microsoft Web 技术，并且是 myKB.com 总裁 ([www.myKB.com](http://www.myKB.com)) 其中他专注于编写 ASP.NET 基于侧重于知识库软件解决方案的应用程序。 可以通过在电子邮件联系 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在其博客地址[ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[上一篇](understanding-asp-net-ajax-web-services.md)
