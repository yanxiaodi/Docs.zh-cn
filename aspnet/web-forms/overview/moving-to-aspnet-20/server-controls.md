---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: "服务器控件 |Microsoft 文档"
author: microsoft
description: "ASP.NET 2.0 增强了在许多方面的服务器控件。 在此模块中，我们将介绍一些方式 ASP.NET 2.0 和 Visual Studio 200 体系结构更改..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 72e9cac7cf9a01791c30783fa56ad7ea205a5a11
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="server-controls"></a>服务器控件
====================
by [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 增强了在许多方面的服务器控件。 在此模块中，我们将介绍一些方式 ASP.NET 2.0 的体系结构更改和 Visual Studio 2005 处理服务器控件。


ASP.NET 2.0 增强了在许多方面的服务器控件。 在此模块中，我们将介绍一些方式 ASP.NET 2.0 的体系结构更改和 Visual Studio 2005 处理服务器控件。

## <a name="view-state"></a>视图状态

主要的不同的视图状态中 ASP.NET 2.0 在于大大缩短了大小。 请考虑一个日历控件将在其上的页。 这是 ASP.NET 1.1 中的视图状态。

[!code-css[Main](server-controls/samples/sample1.css)]

现在这是视图状态在相同页面上中 ASP.NET 2.0。

[!code-css[Main](server-controls/samples/sample2.css)]

这是一个非常重要的变化，并考虑将视图状态来回转入网络时，此更改可让开发人员显著地提高性能。 视图状态的大小减小很大程度上取决于我们内部处理的方式。 请记住视图状态是一个 Base64 编码字符串。 若要更好地了解在 ASP.NET 2.0 视图状态更改，让我们看一下上面的示例中的已解码值。

下面是已解码的 1.1 的视图状态：

[!code-css[Main](server-controls/samples/sample3.css)]

这可能看起来有点像不起作用，但此处的模式。 在 ASP.NET 中 1.x，我们单个字符用于标识数据类型和分隔值使用&lt;&gt;字符。 上面的视图状态示例中的"t"表示拉斯三元数组。 三元数组包含的成对的 Arraylist （"l"表示一个 ArrayList。）这些 Arraylist 之一包含 Int32 ("i") 值为 1，另一个包含另一个斯三元数组。 三元数组包含的成对的 Arraylist，等等。要记住的重要事项是，我们使用三个命令包含对中，我们确定通过字母的数据类型，而我们使用&lt;和&gt;作为分隔符的字符。

在 ASP.NET 2.0 中，已解码的视图状态看起来稍有不同。

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

你应注意到大的更改中的已解码的视图状态的外观。 此项更改具有多个体系结构的基础。 查看状态在 ASP.NET 中 1.x 用于 LosFormatter 序列化数据。 在 2.0 中，我们将使用新的 ObjectStateFormatter 类。 此类已专门用于帮助进行序列化和反序列化的视图状态和控件状态。 （控件状态将在下一节。）有很多更改的序列化和反序列化发生的方法中获得的好处。 最显著之一是与它使用一个 textwriter，使 LosFormatter，不同 ObjectStateFormatter 使用 BinaryWriter 事实。 这允许 ASP.NET 2.0，以存储视图状态一系列字节而不是字符串。 例如，采用一个整型。 在 ASP.NET 1.1 整数所需的视图状态的 4 个字节。 在 ASP.NET 2.0 中，该相同整数仅需要 1 个字节。 其他增强功能已减少的存储的视图状态。 日期时间值，例如，现在存储使用 TickCount 而不字符串。

就像所有的不足够，特别注意已支付给 1.x 中的视图状态的最大使用者之一是相似的控件与 DataGrid 的事实。 例如视图状态致力 DataGrid 控件的主要缺点是信息的它通常包含大量重复。 在 ASP.NET 中 1.x，重复信息只需通过和重新生成臃肿的视图状态中存储的。 在 ASP.NET 2.0 中，我们将使用新的 IndexedString 类来存储此类数据。 如果字符串重复发生，我们只需将该令牌存储 IndexedString 和正在运行的 IndexedString 对象表中的索引。

## <a name="control-state"></a>控件状态

开发人员必须与视图状态主要 gripes 之一是它添加到 HTTP 负载的大小。 如前所述，视图状态的最大使用者之一是 DataGrid 控件。 若要避免由 DataGrid 生成的视图状态的庞大，许多开发人员只需禁用该控件的视图状态。 遗憾的是，该解决方案不始终是一个很好。 查看状态在 ASP.NET 1.x 包含不仅进行该控件的正确功能所需的数据。 它还包含有关控件的 UI 的状态信息。 这意味着如果你想要允许在 DataGrid 的分页必须启用视图状态，即使你不需要的所有查看 UI 信息，包含状态。 它是一个全盘接受或者全盘方案。

在 ASP.NET 2.0 中，控件状态可解决该问题很好地通过控件状态的简介。 控件状态包含绝对必要的正确功能的控件的数据。 与不同的视图状态，无法禁用控件状态。 因此，务必仔细控制存储在控件状态的数据。

> [!NOTE]
> 以及的视图状态中保持控件状态\_ \_VIEWSTATE 隐藏的表单字段。


此视频是的视图状态和控件状态的演练。


![](server-controls/_static/image1.png)


[打开全屏幕视频](server-controls/_static/state1.wmv)


按顺序读取和写入控制状态的服务器控件，你必须执行三个步骤。

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>步骤 1： 调用 RegisterRequiresControlState 方法

RegisterRequiresControlState 方法将通知 ASP.NET 控件需要保持控件状态。 它采用一个参数的类型是要注册的控件的控件。

请务必注意注册不会保留请求以请求。 因此，此方法必须对每个请求调用如果控件是保留控件状态。 建议在 OnInit 调用该方法。

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>步骤 2： 重写 SaveControlState

SaveControlState 方法保存自最后一个回发控件的控件的状态更改。 它返回一个表示控件的状态对象。

## <a name="step-3-override-loadcontrolstate"></a>步骤 3： 重写 LoadControlState

LoadControlState 方法将已保存的状态加载到控件。 该方法采用一个类型自变量的对象，其中包含控件保存的状态。

## <a name="full-xhtml-compliance"></a>完整 XHTML 法规遵从性

任何 Web 开发人员知道 Web 应用程序中的标准的重要性。 为了维护基于标准的开发环境，ASP.NET 2.0 是完全符合 XHTML。 因此，所有标记都将呈现在浏览器支持 HTML 4.0 中 XHTML 标准根据或更高版本。

ASP.NET 1.1 中的文档类型定义时，如下所示：

[!code-html[Main](server-controls/samples/sample6.html)]

在 ASP.NET 2.0 中，默认文档类型定义如下所示：

[!code-html[Main](server-controls/samples/sample7.html)]

如果选择，你可以更改通过配置文件中的 xhtmlConformance 节点的默认 XHML 符合性。 例如，web.config 文件中的以下节点将更改为 XHTML 1.0 Strict XHTML 法规遵从性：

[!code-xml[Main](server-controls/samples/sample8.xml)]

如果选择，你还可以配置 ASP.NET 用于在 ASP.NET 中使用的旧配置 1.x，如下所示：

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>自适应呈现使用适配器

在 ASP.NET 中包含的配置文件 1.x &lt;browserCaps&gt;填充 HttpBrowserCapabilities 对象的部分。 此对象允许开发人员确定哪些设备正在特定的请求并相应地呈现代码。 在 ASP.NET 2.0 中，模型了改进，并且现在使用新的 ControlAdapter 类。 ControlAdapter 类重写控件的生命周期中的事件，并控制的基于用户代理的功能的控件的呈现。 由浏览器定义文件 （带有.browser 文件扩展名的文件） 存储在 c:\windows\microsoft.net\framework\v2.0 定义特定用户代理的功能。\* \* \* \*\CONFIG\Browsers 文件夹。

> [!NOTE]
> ControlAdapter 类是一个抽象类。


非常类似&lt;browserCaps&gt; 1.x，浏览器定义文件中的部分使用正则表达式分析以便确定请求的浏览器的用户代理字符串。 它它们为该用户代理定义特定的功能。 ControlAdapter 呈现 Render 方法通过控件。 因此，如果你替代 Render 方法，你不应在基的类调用呈现器。 这样可能导致呈现发生两次，一次为适配器，一次针对该控件本身。

## <a name="developing-a-custom-adapter"></a>开发自定义适配器

你可以通过从 ControlAdapter 继承开发你自己的自定义适配器。 此外，你可以从在其中页需要适配器的情况下的抽象类 PageAdapter 继承。 通过实现映射到自定义适配器控件&lt;controlAdapters&gt;浏览器定义文件中的元素。 例如，浏览器定义文件中的以下 XML 将菜单控件映射到 MenuAdapter 类：

[!code-html[Main](server-controls/samples/sample10.html)]

使用此模型，它将成为非常便于控件开发人员可以针对某个特定设备或浏览器。 它也是为开发人员可以完全控制页在每个设备上的呈现方式非常简单。

## <a name="per-device-rendering"></a>每个设备呈现

在 ASP.NET 2.0 的服务器控件属性可以指定每个设备使用的浏览器特定前缀。 例如，下面的代码将更改取决于哪个设备用于浏览页面的标签的文本。

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

从 Internet 资源管理器浏览包含此标签页面后，该标签将显示文本"您在浏览从 Internet 资源管理器。" 从 Firefox 浏览页面后，该标签将显示文本"你将浏览 Firefox。" 如果从任何其他设备中浏览页面后，它将显示"您在浏览从未知的设备。" 可以使用此特殊的语法指定任何属性。

## <a name="setting-focus"></a>设置焦点

有关如何在一个特定的控件上设置初始焦点经常要求 ASP.NET 1.x 开发人员。 例如，在登录页上，它可用于具有首先加载页面时获得焦点的用户 ID 文本框。 在 ASP.NET 1.x，执行此操作需要编写一些客户端的脚本。 虽然此类脚本是一项重要任务，它不再需要 ASP.NET 2.0 中感谢 SetFocus 方法。 SetFocus 方法采用一个参数，该值指示应接收焦点的控件。 此参数可以是字符串的形式控件的客户端 ID 或控件对象作为服务器控件的名称。 例如，若要将初始焦点设置到文本框控件调用 txtUserID 首先加载页面时，将以下代码添加到页\_负载：

[!code-csharp[Main](server-controls/samples/sample12.cs)]

-或

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 使用 Webresource.axd 处理程序 （前面所述） 来呈现将焦点设置的客户端函数。 客户端函数的名称是 web 窗体\_AutoFocus 如下所示：

[!code-html[Main](server-controls/samples/sample14.html)]

或者，可以使用控件的焦点方法初始焦点设置到该控件。 焦点方法从控件类派生，并可供 ASP.NET 2.0 的所有控件。 还有可能发生验证错误时，将焦点设置到一个特定的控件。 将在更高版本的模块进行介绍。

## <a name="new-server-controls-in-aspnet-20"></a>在 ASP.NET 2.0 中的新服务器控件

以下是 ASP.NET 2.0 中的新服务器控件。 在更高版本的模块中的其中一些，我们将转到更多详细信息。

## <a name="imagemap-control"></a>ImageMap 控件

ImageMap 控件可用于添加对可以启动一种回发或导航到 URL 的图像的热点。 有可用的三种类型的热点;CircleHotSpot、 RectangleHotSpot 和 PolygonHotSpot。 通过 Visual Studio 中或以编程方式在代码中的集合编辑器添加热点。 不没有可用于在图像上绘制热点的任何用户界面。 必须以声明方式指定的坐标和大小或作用点的半径。 此外，还有设计器中的热点没有可视表示形式。 如果热点配置为导航到的 URL，是通过热点 NavigateUrl 属性指定的 URL。 对于 post 回热点，PostBackValue 属性允许你在服务器端代码中传递回发可检索的字符串。


![Visual Studio 中的作用点集合编辑器](server-controls/_static/image1.jpg)

**图 1**： 在 Visual Studio 中的作用点集合编辑器


## <a name="bulletedlist-control"></a>如何

如何控制是可以轻松地进行数据绑定的项目符号列表。 列表可以进行排序 （编号） 或通过 BulletStyle 属性未排序。 由 ListItem 对象表示列表中的每个项。


![Visual Studio 中的如何控件](server-controls/_static/image1.gif)

**图 2**: Visual Studio 中的如何控件


## <a name="hiddenfield-control"></a>HiddenField 控件

HiddenField 控件将隐藏的表单字段添加到你的页面上，其值是服务器端代码中提供。 隐藏的表单字段的值通常被应该保持开机自检备份之间保持不变。 但是，很可能的恶意用户能够更改发送回值之前。 如果发生这种情况，HiddenField 控件将引发 ValueChanged 事件。 如果你有 HiddenField 控件中的敏感信息和你想要确保它保持不变，则应在代码中处理 ValueChanged 事件。

## <a name="fileupload-control"></a>FileUpload 控件

在 ASP.NET 2.0 FileUpload 控件使可以将文件上载到通过 ASP.NET 页的 Web 服务器。 此控件是为 ASP.NET 1.x HtmlInputFile 类具有几种例外情况非常相似。 在 ASP.NET 中 1.x，建议检查 PostedFile 属性为 null 以确定是否完好的文件。 在 ASP.NET 2.0 FileUpload 控件将添加一个新的 HasFile 属性可以用于相同的目的，并且会稍微更加高效。

PostedFile 属性仍可用于访问 HttpPostedFile 对象，但某些 HttpPostedFile 的功能现已推出本质上与 FileUpload 控件。 例如，若要将已上载的文件保存在 ASP.NET 1.x，你的另存为方法对 HttpPostedFile 对象调用。 在 ASP.NET 2.0 中使用 FileUpload 控件，将在 FileUpload 控件自身上调用 Save 方法。

2.0 行为 （和可能的最显著的更改） 中的另一个重要的变化是，已不再需要在保存它之前将整个上载的文件加载到内存。 在 1.x，任何已上载的文件将保存完全读入内存之前正在写入到磁盘。 此体系结构可防止大型文件上载。

在 ASP.NET 2.0 中，httpRuntime 元素的 requestLengthDiskThreshold 特性，可配置多少千字节为单位之前正在写入的内存中缓冲区中保留到磁盘。

**重要**: MSDN 文档 （和其他位置的文档） 指定此值是以字节为单位 （不千字节为单位），且默认值为 256。 实际中千字节为单位指定的值和默认值为 80。 通过使用默认值为 80 K，我们可以确保，缓冲区不结束，大型对象堆上。

## <a name="wizard-control"></a>向导控件

它是相当容易遇到困扰尝试收集一系列中的信息的使用面板"页"或通过转移页面之间的 ASP.NET 开发人员。 时常，任务是一个很令人沮丧，并使用的时间长。 新的向导控件中一个向导界面，用户熟悉的线性和非线性步骤，从而解决了问题。 向导控件显示输入的窗体中的一系列步骤。 每个步骤都由该控件的 StepType 属性指定的特定类型。 可用的步骤类型如下所示：

| **步骤类型** | **Explanation** |
| --- | --- |
| 自动 | 此向导自动确定根据其位置步骤层次结构中的步骤的类型。 |
| Start | 第一步，通常用于展示的介绍性语句。 |
| 步骤 | 正常的步骤。 |
| 完成 | 最后一步，通常用于呈现按钮以完成向导。 |
| 完成 | 显示通信成功或失败的消息。 |

> [!NOTE]
> 向导控件将跟踪的其使用 ASP.NET 控件状态的状态。 因此，应属性可以设置为 false，而无需任何双刃剑。


此视频是向导控件的演练。


![](server-controls/_static/image2.png)


[打开全屏幕视频](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>本地化控件

Localize 控件是类似于文本控件。 但是，Localize 在控件有**模式**属性，用于控制标记添加到其中的呈现方式。 模式属性支持以下值：

| **模式** | **Explanation** |
| --- | --- |
| Transform | 标记根据发出请求的浏览器的协议进行转换。 |
| 传递 | 标记呈现为-是。 |
| 编码 | 使用 HtmlEncode 编码添加到控件的标记。 |

## <a name="multiview-and-view-controls"></a>多视图和视图控件

多视图控件充当视图控件的容器和视图控件充当其他控件 （很像面板控件） 的容器。 多视图控件中的每个视图由单个视图控件表示。 多视图中的第一个视图控件是视图 0，第二个是视图 1，等等。你可以通过指定多视图控件的 ActiveViewIndex 切换视图。

## <a name="substitution-control"></a>Substitution 控件

Substitution 控件与 ASP.NET 缓存结合使用。 在其中你想要利用的缓存，但具有必须在每个请求 （换而言之，页的部分，将从缓存中免除） 更新的页的部分的情况下，替换组件提供的理想解决方案。 该控件不会实际呈现其自身上的任何输出。 相反，它所绑定到服务器端代码中的方法。 当请求页时，调用该方法并返回的标记呈现在替换控件的位置。

通过指定 Substitution 控件绑定到方法**MethodName**属性。 该方法必须满足以下条件：

- 它必须是静态 (共享中 VB) 方法。
- 它接受一个类型 HttpContext 的参数。
- 它将返回一个字符串，表示应替换页面上的控件的标记。

替换控件不具有能够修改任何其他控件在页上，但它确实有权访问当前 HttpContext 通过其参数。

## <a name="gridview-control"></a>GridView 控件

GridView 控件是 DataGrid 控件的替代。 此控件将在更高版本的模块中的更详细地介绍。

## <a name="detailsview-control"></a>说明如何控制

说明如何控制可以显示来自数据源的单个记录并将编辑或删除它。 中更高版本的模块中的更详细地介绍了它。

## <a name="formview-control"></a>FormView 控件

FormView 控件用于在可配置界面中显示来自数据源的单个记录。 中更高版本的模块中的更详细地介绍了它。

## <a name="accessdatasource-control"></a>AccessDataSource Control

AccessDataSource 控件是用于将数据绑定的 Access 数据库。 中更高版本的模块中的更详细地介绍了它。

## <a name="objectdatasource-control"></a>ObjectDataSource 控件

ObjectDataSource 控件用于支持三层体系结构，以便控件可以是数据绑定到中间层业务对象而不是两层模型其中控件直接绑定到数据源。 它将更高版本的模块中的更详细地讨论。

## <a name="xmldatasource-control"></a>XmlDataSource 控件

XmlDataSource 控件用于数据绑定到 XML 数据源。 中更高版本的模块中的更详细地介绍了它。

## <a name="sitemapdatasource-control"></a>SiteMapDataSource 控件

SiteMapDataSource 控件提供基于站点地图上的站点导航控件的数据绑定。 它将更高版本的模块中的更详细地讨论。

## <a name="sitemappath-control"></a>SiteMapPath 控件

说明如何显示一系列通常称为痕迹导航链接。 中更高版本的模块中的更详细地介绍了它。

## <a name="menu-control"></a>Menu 控件

菜单控件显示使用 DHTML 的动态菜单。 中更高版本的模块中的更详细地介绍了它。

## <a name="treeview-control"></a>TreeView 控件

TreeView 控件用于显示数据的分层树视图。 中更高版本的模块中的更详细地介绍了它。

## <a name="login-control"></a>登录控件

登录控件提供一种机制来登录到网站。 中更高版本的模块中的更详细地介绍了它。

## <a name="loginview-control"></a>LoginView 控件

LoginView 控件允许基于用户的登录状态的不同模板的显示。 中更高版本的模块中的更详细地介绍了它。

## <a name="passwordrecovery-control"></a>取回控件

说明用于 ASP.NET 应用程序的用户检索忘记的密码。 中更高版本的模块中的更详细地介绍了它。

## <a name="loginstatus"></a>LoginStatus

LoginStatus 控件将显示用户的登录状态。 中更高版本的模块中的更详细地介绍了它。

## <a name="loginname"></a>LoginName

登录到 ASP.NET 应用程序后，LoginName 控件显示用户的用户名。 中更高版本的模块中的更详细地介绍了它。

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard 是一个可配置向导，使用户能够创建 ASP.NET 应用程序中使用 ASP.NET 成员身份的帐户。 中更高版本的模块中的更详细地介绍了它。

## <a name="changepassword"></a>ChangePassword

ChangePassword 控件允许用户更改其密码 ASP.NET 应用程序。 中更高版本的模块中的更详细地介绍了它。

## <a name="various-webparts"></a>各种 web 部件

ASP.NET 2.0 随附了各种 Web 部件。 这些将详细介绍了更高版本的模块中。
