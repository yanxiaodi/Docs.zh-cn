---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: "ASP.NET 2.0 页模型 |Microsoft 文档"
author: microsoft
description: "在 ASP.NET 中 1.x，开发人员必须内联代码模型与代码隐藏代码模型之间选择。 无法使用任一 Src attr 实现代码隐藏..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: e008f197cf08bec81c560018f2d42306598f9e6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="the-aspnet-20-page-model"></a>ASP.NET 2.0 页模型
====================
通过[Microsoft](https://github.com/microsoft)

> 在 ASP.NET 中 1.x，开发人员必须内联代码模型与代码隐藏代码模型之间选择。 无法使用 Src 属性或的代码隐藏文件属性来实现代码隐藏@Page指令。 在 ASP.NET 2.0 中，开发人员仍之间进行选择内联代码和代码隐藏，但已对代码隐藏模型的重大增强功能。


在 ASP.NET 中 1.x，开发人员必须内联代码模型与代码隐藏代码模型之间选择。 无法使用 Src 属性或的代码隐藏文件属性来实现代码隐藏@Page指令。 在 ASP.NET 2.0 中，开发人员仍之间进行选择内联代码和代码隐藏，但已对代码隐藏模型的重大增强功能。

## <a name="improvements-in-the-code-behind-model"></a>代码隐藏模型中的改进

若要完全了解在 ASP.NET 2.0 中的代码隐藏模型中的更改，其最大努力快速查看模型存在于 ASP.NET 1.x。

## <a name="the-code-behind-model-in-aspnet-1x"></a>在 ASP.NET 中的代码隐藏模型 1.x

在 ASP.NET 中 1.x，代码隐藏模型包含个的 ASPX 文件 （该 web 窗体） 和包含程序代码的代码隐藏文件。 两个文件连接使用@Page指令 ASPX 文件中。 ASPX 页上的每个控件具有相应声明的代码隐藏文件中为实例变量。 代码隐藏文件还包含事件绑定的代码，并生成必需的 Visual Studio 设计器代码。 此模型效果相当不错，但因为 ASPX 页中的每个 ASP.NET 元素所需的代码隐藏文件中的相应代码，没有代码和内容没有真正分离。 例如，如果设计器添加到 Visual Studio IDE 之外的 ASPX 文件新的服务器控件，该应用程序将会破坏由于缺少该控件的声明，因此在代码隐藏文件中。

## <a name="the-code-behind-model-in-aspnet-20"></a>ASP.NET 2.0 中的代码隐藏模型

ASP.NET 2.0 大大改进了此模型。 在 ASP.NET 2.0 中，代码隐藏实现使用新*分部类*ASP.NET 2.0 中提供。 在 ASP.NET 2.0 中的代码隐藏类是定义为分部类，这意味着它包含仅的类定义的一部分。 使用 ASPX 页，在运行时或在预编译网站的 ASP.NET 2.0 动态生成的类定义的其余部分。 代码隐藏文件和 ASPX 页面之间的链接则仍会建立使用 @ Page 指令。 但是，而不是一个代码隐藏或 Src 特性中，ASP.NET 2.0 现在使用同属性。 Inherits 特性也用于指定页的类名称。

典型的 @ Page 指令可能如下所示：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

ASP.NET 2.0 代码隐藏文件中的典型类定义可能如下所示：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# 和 Visual Basic 是唯一的托管的语言当前支持分部类。 因此，使用 J# 开发人员将不能在 ASP.NET 2.0 中使用代码隐藏模型。


新模型可增强代码隐藏模型，因为开发人员现在将具有包含他们创建的代码的代码文件。 它还提供的代码和内容 true 分离因为有代码隐藏文件中的没有实例变量声明。

> [!NOTE]
> ASPX 页的分部类是事件绑定发生，因为 Visual Basic 开发人员可以使用代码隐藏文件中的句柄关键字将事件绑定实现略微的性能增加。 C# 包含没有等效的关键字。


## <a name="new--page-directive-attributes"></a>新的 @ 页面指令属性

ASP.NET 2.0 将许多新特性添加到 @ Page 指令。 以下属性是 ASP.NET 2.0 中的新增功能。

## <a name="async"></a>Async

异步属性，可配置页后，可以以异步方式执行。 也包括此模块的更高版本中的异步页。

## <a name="asynctimeout"></a>AsyncTimeout

指定异步页的超时值。 默认值为 45 秒。

## <a name="codefile"></a>同

同属性是对 Visual Studio 2002/2003 中的代码隐藏属性替代。

### <a name="codefilebaseclass"></a>CodeFileBaseClass

在要从单个基类派生的多个页的情况下使用 CodeFileBaseClass 属性。 由于在 ASP.NET 中，如果没有此特性的分部类的实现使用共享的公共字段来引用在 ASPX 页面中声明的控件的基类将不正常工作，因为 ASP。网编译引擎将自动创建基于页中的控件的新成员。 因此，如果你需要一个公共基类在 ASP.NET 中的两个或多个页面，你将需要定义 CodeFileBaseClass 属性中指定基类，然后是每个页类派生自该基类。 使用此特性时，则也需要同属性。

## <a name="compilationmode"></a>compilationMode

此属性允许你设置的 ASPX 页 CompilationMode 属性。 CompilationMode 属性是一个包含的值的枚举**始终**，**自动**，和**从不**。 默认值是**始终**。 **自动**设置将阻止 ASP.NET 动态尽可能编译页面。 从动态编译排除页提高性能。 但是，如果不对此页包含必须编译该代码，将引发错误时浏览页面。

## <a name="enableeventvalidation"></a>EnableEventValidation

此属性指定验证回发和回调事件。 启用后，自变量以回发或回调事件会检查以确保它们源自最初呈现这些服务器控件。

## <a name="enabletheming"></a>EnableTheming

此属性指定在页面上使用了 ASP.NET 主题。 默认值为 false。 中介绍了 ASP.NET 主题[模块 10](profiles-themes-and-web-parts.md)。

## <a name="linepragmas"></a>LinePragmas

此属性指定是否应在编译过程中添加行杂注。 行杂注是调试器用于标记代码的特定部分的选项。

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

此属性指定 JavaScript 注入到页以便保持之间回发的滚动位置。 此属性是**false**默认情况下。

此属性时**true**，ASP.NET 将添加&lt;脚本&gt;块在回发时，如下所示：

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

请注意，此脚本块 src WebResource.axd。 此资源不是物理路径。 当请求此脚本时，ASP.NET 将动态生成脚本。

### <a name="masterpagefile"></a>MasterPageFile

此属性指定当前页的主控页文件。 路径可以是相对或绝对。 中介绍了母版页[模块 4](master-pages.md)。

## <a name="stylesheettheme"></a>StyleSheetTheme

此属性允许你重写由 ASP.NET 2.0 主题定义的用户界面外观属性。 主题中介绍了[模块 10](profiles-themes-and-web-parts.md)。

## <a name="theme"></a>主题

指定的页面主题。 如果没有为 StyleSheetTheme 属性指定一个值，主题属性将替代应用于这些控件在页上的所有样式。

## <a name="title"></a>标题

设置页的标题。 此处指定的值将出现在&lt;标题&gt;呈现页面上的元素。

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

设置 ViewStateEncryptionMode 枚举的值。 可用值有**始终**，**自动**，和**从不**。 默认值是**自动**。当此属性设置为值为**自动**，加密视图状态是一个请求，则通过调用**RegisterRequiresViewStateEncryption**方法。

## <a name="setting-public-property-values-via-the--page-directive"></a>设置公共属性的值通过 @ 页指令

ASP.NET 2.0 中的 @ Page 指令的另一个新功能是能够设置基类的公共属性的初始值。 假设，例如，你具有的公共属性调用**SomeText**在您的基类和你意愿它，以初始化为**Hello**加载页时。 你可以完成此操作通过只需在 @ Page 指令中设置的值如下所示：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**SomeText** @ Page 指令的特性到基类中设置 SomeText 属性的初始值*Hello ！*。 下面的视频是基类使用 @ 页面指令中设置的公共属性的初始值的演练。


![](the-asp-net-2-0-page-model/_static/image1.png)


[打开全屏幕视频](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>页类的新公共属性

以下公共属性是 ASP.NET 2.0 中的新增功能。

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

返回的页面或控件的相对于应用程序路径。 例如，为位于 http://app/folder/page.aspx 页，该属性返回 ~ / 文件夹 /。

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

返回的页面或控件的相对虚拟目录路径。 例如位于 http://app/folder/page.aspx 的页面，该属性返回 ~ / folder/page.aspx。

## <a name="asynctimeout"></a>AsyncTimeout

获取或设置用于异步页处理的超时值。 （异步页将在后面进行介绍此模块。）

## <a name="clientquerystring"></a>ClientQueryString

返回所请求 URL 的查询字符串部分的只读属性。 此值是 URL 编码。 HttpServerUtility 类的 UrlDecode 方法可用于对其进行解码。

## <a name="clientscript"></a>ClientScript

此属性返回可以用于管理的客户端脚本的 ASP.NETs 发出 ClientScriptManager 对象。 （ClientScriptManager 类介绍此模块中的更高版本中）。

## <a name="enableeventvalidation"></a>EnableEventValidation

此属性控制启用事件验证回发和回调事件。 启用时，自变量以回发或回调事件进行验证以确保它们源自最初呈现这些服务器控件。

## <a name="enabletheming"></a>EnableTheming

此属性获取或设置一个布尔值，指定将 ASP.NET 2.0 主题应用到页。

## <a name="form"></a>窗体

此属性为 HtmlForm 对象返回 ASPX 页上的 HTML 窗体。

## <a name="header"></a>Header

此属性返回对包含页眉的 HtmlHead 对象的引用。 可以使用返回的 HtmlHead 对象用于获取/设置样式表、 元标记，等等。

## <a name="idseparator"></a>IdSeparator

此只读属性获取用于分隔控件标识符，当 ASP.NET 生成页面上的控件的唯一 ID 的字符。 它不可直接通过代码使用。

## <a name="isasync"></a>IsAsync

此属性允许异步页。 在此模块的后面部分讨论了异步页。

## <a name="iscallback"></a>IsCallback

此只读属性返回**true**如果页是回调的结果。 在此模块的后面部分讨论了回拨。

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

此只读属性返回**true**如果页是跨页回发的一部分。 在此模块的后面介绍跨页回发。

## <a name="items"></a>项

返回到 IDictionary 实例，其中包含存储在页上下文中的所有对象的引用。 你可以将项添加到此 IDictionary 对象，它们将可供你的整个生存期内上下文。

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

此属性控制 ASP.NET 发出维护页回发发生后滚动浏览器中的位置的 JavaScript。 （此属性的详细信息已前面所述此模块。）

## <a name="master"></a>master

此只读属性返回到已应用母版页的页面的母版页实例的引用。

## <a name="masterpagefile"></a>MasterPageFile

获取或设置页面的母版页文件名。 仅可以 PreInit 方法中设置此属性。

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

此属性获取或设置页状态的最大长度以字节为单位。 如果该属性设置为正数，页面视图状态将分解成多个隐藏的字段，使其不超过指定的字节数。 如果该属性是负数，则的视图状态将不划分为区块中。

## <a name="pageadapter"></a>PageAdapter

返回对修改请求的浏览器的页 PageAdapter 对象的引用。

## <a name="previouspage"></a>PreviousPage

Server.transfer 的调用或跨页回发的情况下均返回到以前的页面的引用。

## <a name="skinid"></a>SkinID

指定要应用于页的 ASP.NET 2.0 外观。

## <a name="stylesheettheme"></a>StyleSheetTheme

此属性获取或设置应用于页的样式表。

## <a name="templatecontrol"></a>TemplateControl

返回到页包含控件的引用。

## <a name="theme"></a>主题

获取或设置 ASP.NET 2.0 主题应用于页面的名称。 在 PreInit 方法之前，必须设置此值。

## <a name="title"></a>标题

此属性获取或设置页的标题，从页面标头中获得。

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

获取或设置页的 ViewStateEncryptionMode。 请参阅本模块前面的此属性的详细的讨论。

## <a name="new-protected-properties-of-the-page-class"></a>页类的新的受保护的属性

以下是 ASP.NET 2.0 中的页类的新的受保护的属性。

## <a name="adapter"></a>适配器

返回对呈现在设备上的页 ControlAdapter 的引用请求它。

## <a name="asyncmode"></a>AsyncMode

此属性指示以异步方式处理页。 它旨在用于由运行时并不直接在代码中。

## <a name="clientidseparator"></a>ClientIDSeparator

此属性返回时为控件创建唯一的客户端 Id 用作分隔符的字符。 它旨在用于由运行时并不直接在代码中。

## <a name="pagestatepersister"></a>PageStatePersister

此属性返回的页面 PageStatePersister 对象。 此属性主要由 ASP.NET 控件开发人员使用。

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

此属性返回唯一 suffic 追加到缓存浏览器的文件路径。 默认值是\_ \_ufps = 和 6 位数。

## <a name="new-public-methods-for-the-page-class"></a>页类的新公共方法

以下的公共方法不熟悉 ASP.NET 2.0 中的页类。

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

此方法注册异步页执行的事件处理程序委托。 在此模块的后面部分讨论了异步页。

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

适用于页的页样式表中的属性。

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

此方法朋友异步任务。

### <a name="getvalidators"></a>GetValidators

如果未指定，则返回的验证程序指定的验证组或默认验证组的集合。

## <a name="registerasynctask"></a>RegisterAsyncTask

此方法注册新的异步任务。 在此模块的后面介绍异步页。

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

此方法是告诉 ASP.NET 必须保留的页控件状态。

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

此方法会告知 ASP.NET 页面视图状态，需要加密。

## <a name="resolveclienturl"></a>ResolveClientUrl

返回可以用于图像，等等的客户端请求的相对 URL。

## <a name="setfocus"></a>SetFocus

此方法将焦点设置到最初加载页面时指定的控件。

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

此方法将为不再需要控制状态持久性传递给它的控件中注销。

## <a name="changes-to-the-page-lifecycle"></a>对页生命周期的更改

在 ASP.NET 2.0 中的页生命周期尚未发生显著变化，但有一些你应注意的新方法。 下面列出了 ASP.NET 2.0 页面生命周期。

## <a name="preinit-new-in-aspnet-20"></a>PreInit （新在 ASP.NET 2.0)

PreInit 事件是开发人员可以访问的生命周期中的最早阶段。 此事件添加使可以以编程方式更改 ASP.NET 2.0 主题、 母版页、 访问 ASP.NET 2.0 配置文件等的属性。如果在回发状态，一定要认识到，Viewstate 已不尚未应用于控件此时生命周期中。 因此，如果在此阶段，开发人员更改控件的属性，它将可能覆盖页生命周期中更高版本。

## <a name="init"></a>Init

从 ASP.NET 未更改 Init 事件 1.x。 这是你想要读取或初始化在页面上的控件的属性。 在此阶段、 母版页、 主题，等已应用到页。

## <a name="initcomplete-new-in-20"></a>InitComplete （中的新增 2.0)

在页初始化阶段的末尾，调用 InitComplete 事件。 此时在其生命周期，可以访问控件在页上，但尚未填充其状态。

## <a name="preload-new-in-20"></a>（2.0 中的新） 预加载

在应用所有的回发数据后和页之前，将调用此事件\_负载。

## <a name="load"></a>Load

从 ASP.NET 未更改的 Load 事件 1.x。

## <a name="loadcomplete-new-in-20"></a>LoadComplete （中的新增 2.0)

LoadComplete 事件是页负载阶段中的最后一个事件。 在此阶段，所有的回发和视图状态数据已应用到页。

## <a name="prerender"></a>PreRender

如果您要用于 viewstate 正确维护的动态添加到页面的控件，PreRender 事件将是将它们添加的最后机会。

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete （中的新增 2.0)

在 PreRenderComplete 阶段中，所有控件已都添加到页，而该页已准备好呈现。 PreRenderComplete 事件是保存的页面视图状态之前引发的最后一个事件。

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete （中的新增 2.0)

所有的页面视图状态和控制状态已保存后立即调用 SaveStateComplete 事件。 这是最后一个事件之前页面实际呈现到浏览器。

## <a name="render"></a>呈现

Render 方法不自从 ASP.NET 1.x。 这是其中初始化 HtmlTextWriter 和页呈现到浏览器。

## <a name="cross-page-postback-in-aspnet-20"></a>在 ASP.NET 2.0 中的跨页回发

在 ASP.NET 中 1.x，需要为发送到相同的页的回发。 不允许跨页回发。 ASP.NET 2.0 增加了能够回发到 IButtonControl 接口通过不同的页。 实现新的 IButtonControl 接口 （按钮、 LinkButton 和 ImageButton 除了第三方自定义控件） 的任何控件可以利用通过 PostBackUrl 属性使用此新功能。 下面的代码演示回发到第二个页的按钮控件。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

回发页面，启动回发页面时，可通过第二页上的 PreviousPage 属性访问。 通过新的 web 窗体中实现此功能\_控件回发到其他页面时，ASP.NET 2.0 呈现到页的 DoPostBackWithOptions 客户端函数。 通过发出将脚本保存到客户端新 WebResource.axd 处理程序提供了此 JavaScript 函数。

下面的视频是跨页回发的演练。


![](the-asp-net-2-0-page-model/_static/image2.png)


[打开全屏幕视频](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>在跨页回发的更多详细信息

### <a name="viewstate"></a>视图状态

您可能已要求自己已发生 viewstate 从在跨页回发方案中的第一页。 毕竟，不实现 IPostBackDataHandler 任何控件将持久保存其状态通过视图状态，因此有权在跨页回发的第二页上该控件的属性，你必须有权的页面的视图状态。 ASP.NET 2.0 负责的这种情况下，使用调用的第二页中的新隐藏的字段\_ \_PREVIOUSPAGE。 \_ \_PREVIOUSPAGE 窗体字段包含的第一页的视图状态，以便你可以访问的所有控件的属性中的第二页。

### <a name="circumventing-findcontrol"></a>也可以避开 FindControl

在跨页回发的视频演练中，我将使用 FindControl 方法来获取对第一页上文本框控件的引用。 该方法非常适用于该目的，但 FindControl 将占用大量资源，并且它需要编写附加代码。 幸运的是，ASP.NET 2.0 提供 FindControl 的替代项为此目的，将在许多方案中工作。 PreviousPageType 指令允许你通过使用类型名称或 VirtualPath 属性具有到以前的页面的强类型引用。 TypeName 属性，可指定前一页的类型，同时 VirtualPath 属性允许您以引用使用的虚拟路径的前一页。 设置 PreviousPageType 指令后，就必须公开控件，你想要允许使用公共属性的访问等。

## <a name="lab-1-cross-page-postback"></a>实验室 1 跨页回发

在本实验中，你将创建使用新的 ASP.NET 2.0 跨页面回发的功能的应用程序。

1. 打开 Visual Studio 2005 并创建新的 ASP.NET Web 站点。
2. 添加调用 page2.aspx 的新 web 窗体。
3. 在设计视图中打开 Default.aspx 并添加一个按钮控件和文本框控件。 

    1. 为按钮控件指定的 ID**提交按钮**和文本框控件的 ID**用户名**。
    2. 设置为 page2.aspx 的按钮 PostBackUrl 属性。
4. 在源视图中打开 page2.aspx。
5. 添加 @ PreviousPageType 指令，如下所示：
6. 将以下代码添加到页\_page2.aspx 的隐藏代码的负载： 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. 通过在生成菜单上单击生成中生成项目。
8. 将以下代码添加到 Default.aspx 代码隐藏： 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. 更改页\_中所示的 page2.aspx 负载： 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. 生成项目。
11. 运行该项目。
12. 在文本框中输入你的名称，然后单击按钮。
13. 结果是什么？

## <a name="asynchronous-pages-in-aspnet-20"></a>在 ASP.NET 2.0 中的异步页

在 ASP.NET 中的许多争用问题而引起的 （如 Web 服务或数据库调用） 的外部调用、 文件 IO 延迟等的延迟。当针对 ASP.NET 应用程序发出请求时，ASP.NET 将使用一种与其工作线程来处理该请求。 该请求拥有该线程，直到完成该请求并发送响应。 ASP.NET 2.0 试图通过添加功能以异步方式执行页解决这些类型的问题的延迟问题。 这意味着工作线程可以启动请求，然后移交其他执行的另一个线程，从而快速返回到可用线程池。 完成后的文件 IO、 数据库调用等，从要完成该请求的线程池获得新线程。

在进行异步执行的页的第一步是设置**异步**页面指令属性如下所示：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

此属性是告诉 ASP.NET，若要实现页 IHttpAsyncHandler。

下一步是要在之前 PreRender 页生命周期中的点调用 AddOnPreRenderCompleteAsync 方法。 (通常在页中调用此方法\_负载。)AddOnPreRenderCompleteAsync 方法采用两个参数;BeginEventHandler 和 EndEventHandler。 BeginEventHandler 返回 IAsyncResult 然后传递给 EndEventHandler 作为参数。

以下视频是异步页请求的演练。


![](the-asp-net-2-0-page-model/_static/image3.png)


[打开全屏幕视频](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> 直到完成 EndEventHandler 异步页不会呈现到浏览器中。 毫无疑问，但一些开发人员将将视为类似于异步回调的异步请求。 请务必意识到它们不是。 异步请求到的好处是，可以为新请求提供服务，从而减少了由于 IO 绑定的争用，等等的线程池返回第一个工作线程。


## <a name="script-callbacks-in-aspnet-20"></a>在 ASP.NET 2.0 中的脚本回调

Web 开发人员一直始终在寻找种方法可避免闪烁与回调相关联。 在 ASP.NET 中 1.x，SmartNavigation 避免闪烁，最常用方法但 SmartNavigation 由于客户端上其实现的复杂性为一些开发人员带来问题。 ASP.NET 2.0，解决此问题与脚本回调。 脚本回调利用 XMLHttp 针对通过 JavaScript 的 Web 服务器发出请求。 XMLHttp 请求返回 XML 数据然后可操纵通过浏览器的 dom。 用户从新 WebResource.axd 处理程序的情况下，XMLHttp 代码处于隐藏状态。

有几个步骤所需配置 ASP.NET 2.0 中的脚本回调。

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>步骤 1： 实现 ICallbackEventHandler 接口

为了使 ASP.NET 可以将你的页面识别为参与脚本回调，则必须实现 ICallbackEventHandler 接口。 你可以执行此操作的代码隐藏文件中如下所示：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

你也可以这样做因此使用 @ 实现指令类似：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

使用内联 ASP.NET 代码时，你通常将使用 @ 实现指令。

## <a name="step-2--call-getcallbackeventreference"></a>步骤 2： 调用 GetCallbackEventReference

如前所述，XMLHttp 调用将封装在 WebResource.axd 处理程序。 ASP.NET 页面呈现时，将添加对 web 窗体的调用\_DoCallback，WebResource.axd 由提供的客户端脚本。 该 web 窗体\_DoCallback 函数将替换\_ \_doPostBack 回调函数。 请记住， \_ \_doPostBack 以编程方式提交页上的表单。 你希望在回调方案中，因此阻止回发， \_ \_doPostBack 不满足要求。

> [!NOTE]
> \_\_doPostBack 仍呈现到客户端脚本回调方案中的页。 但是，它不用于回调。


该 web 窗体的自变量\_DoCallback 客户端函数提供通过服务器端功能通常会在页中调用的 GetCallbackEventReference\_负载。 典型调用 GetCallbackEventReference 可能如下所示：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> 在这种情况下，cm 是 ClientScriptManager 的一个实例。 ClientScriptManager 类将在此模块的后面进行介绍。


有多个重载的版本的 GetCallbackEventReference。 在这种情况下，自变量如下所示：

`this`

对调用 GetCallbackEventReference 控件的引用。 在这种情况下，它是本身的页。

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

将从客户端代码中向服务器端事件传递一个字符串参数。 在这种情况下，将下拉列表中的值传递的 Im 调用 ddlCompany。

`ShowCompanyName`

将在从服务器端回调事件返回的值接受 （作为字符串） 的客户端函数名称。 服务器端回调成功后，将仅调用此函数。 因此，为了可靠性，通常建议使用 GetCallbackEventReference 采用指定要执行发生错误时的客户端函数的名称的附加字符串自变量的重载的版本。

`null`

表示它到服务器在回调之前启动的客户端函数的字符串。 在这种情况下，没有此类脚本，因此该参数为 null。

`true`

一个布尔值，指定以异步方式执行回调。

Web 窗体调用\_DoCallback 客户端上的会将这些参数传递。 因此，客户端上呈现此页面，该代码将看起来如下所示：

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

请注意，客户端上函数的签名稍有不同。 客户端函数会将 5 个字符串和一个布尔值传递。 （这是 null，在上面的示例中） 的其他字符串包含客户端函数将处理从服务器端回调的任何错误。

## <a name="step-3--hook-the-client-side-control-event"></a>步骤 3： 挂钩的客户端控件事件

请注意，上面的 GetCallbackEventReference 的返回值分配给字符串变量。 该字符串用于挂钩启动回调的控件的客户端事件。 在此示例中，回调可由下拉列表中的页上，因此我想要挂钩*OnChange*事件。

若要将连接客户端事件，只需添加一个处理程序到客户端标记，如下所示：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

回想一下， *cbRef*是 GetCallbackEventReference 调用的返回值。 它包含对 web 窗体的调用\_DoCallback 上面所示。

## <a name="step-4--register-the-client-side-script"></a>步骤 4： 注册客户端脚本

回想一下，GetCallbackEventReference 调用指定客户端脚本调用**ShowCompanyName**服务器端回调成功时将执行。 该脚本需要添加到页面使用的 ClientScriptManager 实例。 （ClientScriptManager 类将是此模块中的更高版本的讨论。）因此不要该类似：

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>步骤 5： 调用的方法的 ICallbackEventHandler 接口

ICallbackEventHandler 包含两个方法，你需要在你的代码中实现。 它们是**RaiseCallbackEvent**和**GetCallbackEvent**。

**RaiseCallbackEvent**采用字符串作为自变量和返回任何内容。 字符串自变量传递与 web 窗体的客户端调用\_DoCallback。 在这种情况下，该值是*值*调用 ddlCompany 下拉列表中的属性。 应将你的服务器端代码置于 RaiseCallbackEvent 方法。 例如，如果你的回调 WebRequest 针对外部资源，该代码应放置在 RaiseCallbackEvent。

**GetCallbackEvent**负责处理返回给客户端的回调。 它不采用任何参数并返回一个字符串。 它将返回的字符串将作为参数传递给客户端函数中，在这种情况下*ShowCompanyName*。

完成上述步骤后，你就可以在 ASP.NET 2.0 中执行脚本回调。


![](the-asp-net-2-0-page-model/_static/image4.png)


[打开全屏幕视频](the-asp-net-2-0-page-model/_static/callback1.wmv)


中支持使 XMLHttp 调用任何浏览器支持在 ASP.NET 中的脚本回调。 中包括所有现代浏览器中使用今天。 Internet Explorer 使用 XMLHttp ActiveX 对象，而其他浏览器 （包括即将到来的 IE 7） 使用的内部 XMLHttp 对象。 若要以编程方式确定是否浏览器支持回调，则可以使用**Request.Browser.SupportCallback**属性。 此属性将返回**true**如果请求的客户端支持脚本回调。

## <a name="working-with-client-script-in-aspnet-20"></a>使用 ASP.NET 2.0 中的客户端脚本

通过使用 ClientScriptManager 类情况下，ASP.NET 2.0 中的客户端脚本进行管理。 将跟踪 ClientScriptManager 类的使用类型和名称的客户端脚本。 这可以防止同一个脚本以编程方式插入页面上一次以上。

> [!NOTE]
> 脚本已成功注册在页面上后，任何后续尝试注册同一脚本只需将导致不正在注册第二次的脚本。 添加任何重复的脚本，并且未发生任何异常。 若要避免不必要的计算，还有一些可用于确定，以便不尝试多次注册，是否已注册脚本的方法。


应为所有当前的 ASP.NET 开发人员熟悉 ClientScriptManager 的方法：

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

此方法将脚本添加到所呈现的页面的顶部。 这可用于添加将在客户端显式调用的函数。

有两个重载的版本，此方法。 三个四个自变量是在它们之间常见。 它们是：

`type (string)`

***类型***自变量标识脚本的类型。 它通常是一个好办法使用页面的类型 （这。类型的 GetType())。

`key (string)`

***密钥***自变量是脚本的用户定义键。 这应是唯一的每个脚本。 如果你尝试添加具有相同的键和类型的已添加脚本的脚本，将不会添加它。

`script (string)`

***脚本***自变量是一个包含要添加的实际脚本字符串。 建议使用 StringBuilder 创建脚本，然后使用 StringBuilder 上的 tostring （） 方法来分配***脚本***自变量。

如果你使用重载的 RegisterClientScriptBlock 仅使用三个参数，则必须包含脚本元素 (&lt;脚本&gt;和&lt;/&gt;) 在你的脚本。

你可以选择使用 RegisterClientScriptBlock 采用第四个参数的重载。 第四个参数是一个布尔值，指定 ASP.NET 应为您添加脚本元素。 如果此参数为**true**，你的脚本不应包含的脚本元素显式。

IsClientScriptBlockRegistered 方法用于确定是否已注册的脚本。 这样您就可以避免尝试重新注册已注册的脚本。

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude （中的新增 2.0)

RegisterClientScriptInclude 标记创建的脚本块链接到外部脚本文件。 它具有两个重载。 其中一个使用密钥和 URL。 第二个将添加指定的类型的第三个自变量。

例如，下面的代码生成一个脚本块链接到 jsfunctions.js 在应用程序的脚本文件夹的根目录中：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

此代码生成所呈现的页面中的以下代码：

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> 脚本块呈现在页的底部。


IsClientScriptIncludeRegistered 方法用于确定是否已注册的脚本。 这样您就可以避免尝试重新注册的脚本。

## <a name="registerstartupscript"></a>RegisterStartupScript

RegisterStartupScript 方法采用与 RegisterClientScriptBlock 方法相同的参数。 在页面加载后但 OnLoad 客户端事件之前，将执行注册 RegisterStartupScript 脚本。 在 1.X RegisterStartupScript 与已注册的脚本已被放只需在关闭前&lt;窗体窗体&gt;标记时与 RegisterClientScriptBlock 已注册的脚本已被放在打开后立即&lt;窗体&gt;标记。 在 ASP.NET 2.0 中，同时位于立即在关闭前&lt;窗体窗体&gt;标记。

> [!NOTE]
> 如果对 RegisterStartupScript 注册了一个函数，该函数将不会执行，直到显式需在客户端代码中调用它即可。


使用 IsStartupScriptRegistered 方法确定是否已注册脚本，从而避免尝试重新注册的脚本。

## <a name="other-clientscriptmanager-methods"></a>其他 ClientScriptManager 方法

以下是一些 ClientScriptManager 类的其他有用方法。

| **GetCallbackEventReference** | 请参阅本模块前面的脚本回调。 |
| --- | --- |
| **GetPostBackClientHyperlink** | 获取 JavaScript 参考 (javascript:&lt;调用&gt;) 可以用于从客户端事件发布。 |
| **GetPostBackEventReference** | 获取可用于启动重新从客户端 post 的字符串。 |
| **GetWebResourceUrl** | 返回到嵌入到程序集中的资源的 URL。 必须与结合使用**RegisterClientScriptResource**。 |
| **RegisterClientScriptResource** | 注册页的 Web 资源。 这些是资源嵌入程序集中并由新 WebResource.axd 处理程序处理。 |
| **RegisterHiddenField** | 注册页的隐藏的表单域。 |
| **RegisterOnSubmitStatement** | 注册在提交 HTML 表单时执行的客户端代码。 |
