---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: "创建自定义 AJAX 控件工具包控件扩展程序 (VB) |Microsoft 文档"
author: microsoft
description: "自定义扩展程序，可以自定义和扩展 ASP.NET 控件的功能而无需创建新类。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3e8fceb3c7570aa1bf085c8e1037736254e74ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>创建自定义 AJAX 控件工具包控件扩展程序 (VB)
====================
通过[Microsoft](https://github.com/microsoft)

> 自定义扩展程序，可以自定义和扩展 ASP.NET 控件的功能而无需创建新类。


在本教程中，您将学习如何创建自定义的 AJAX 控件工具包控件扩展程序。 我们将创建一个简单、 但有用，新的扩展程序从禁用启用时文本框中键入文本更改按钮的状态。 阅读完本教程之后, 你将能够扩展 ASP.NET AJAX 工具包使用你自己的控件扩展器。

你可以创建自定义控件扩展程序使用 Visual Studio 或 Visual Web Developer （请确保你有 Visual Web Developer 的最新版本）。

## <a name="overview-of-the-disabledbutton-extender"></a>DisabledButton 扩展程序的概述

我们新控件扩展程序被命名为 DisabledButton 扩展程序。 此扩展将具有三个属性：

- TargetControlID-控件扩展文本框。
- TargetButtonIID-已禁用或启用的按钮。
- DisabledText-最初显示的按钮中的文本。 当你开始键入时，按钮将显示按钮文本属性的值。

可以将挂接 DisabledButton 扩展程序添加到文本框和按钮控件。 您输入的任何文本之前，将禁用的按钮和文本框和按钮如下所示：


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([单击以查看实际尺寸的图像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))


开始键入文本后，会启用该按钮和文本框和按钮如下所示：


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([单击以查看实际尺寸的图像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))


若要创建我们控件扩展程序，我们需要创建以下三个文件：

- DisabledButtonExtender.vb-此文件是服务器端控件类，以便管理创建您的扩展器，允许你在设计时设置的属性。 它还定义了可以在您的扩展器设置的属性。 这些属性可通过代码和在设计时访问和匹配 DisableButtonBehavior.js 文件中定义的属性。
- DisabledButtonBehavior.js-此文件是你将添加所有你的客户端脚本逻辑。
- DisabledButtonDesigner.vb-此类使设计时功能。 如果你想要正常使用 Visual Studio/Visual Web Developer 设计器的控件扩展程序，你需要此类。

因此控件扩展程序包含服务器端控件、 一个客户端行为和服务器端设计器类。 了解如何在以下各节中创建这些文件的所有三个。

## <a name="creating-the-custom-extender-website-and-project"></a>创建自定义扩展程序网站和项目

第一步是在 Visual Studio/Visual Web Developer 中创建类库项目和网站。 我们 ll 在类库项目中创建的自定义的扩展程序和网站中测试自定义的扩展程序。

让我们来启动与网站。 请按照下列步骤以创建网站：

1. 选择菜单选项**文件、 新的 Web 站点**。
2. 选择**ASP.NET 网站**模板。
3. 将新的网站命名*Website1*。
4. 单击**确定**按钮。

接下来，我们需要创建类库项目将包含该控件扩展程序的代码：

1. 选择菜单选项**文件，添加和新项目**。
2. 选择**类库**模板。
3. 名称命名的新类库**CustomExtenders**。
4. 单击**确定**按钮。

完成这些步骤后，你的解决方案资源管理器窗口应如下所示图 1。


[![与网站和类库项目的解决方案](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**图 01**： 与网站和类库项目的解决方案 ([单击以查看实际尺寸的图像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))


接下来，你需要添加所有对该类库项目的必要的程序集引用：

1. 右键单击 CustomExtenders 项目并选择菜单选项**添加引用**。
2. 选择.NET 选项卡。
3. 添加对下列程序集的引用：

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. 选择浏览选项卡。
5. 添加对 AjaxControlToolkit.dll 程序集的引用。 此程序集位于下载 AJAX 控件工具包的文件夹中。

你可以验证，你已添加所有正确引用通过右键单击项目，选择属性，然后单击引用选项卡 （请参见图 2）。


[![与所需的引用的引用文件夹](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**图 02**： 所需的引用的引用文件夹 ([单击以查看实际尺寸的图像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>创建自定义控件扩展

现在，我们已我们类的库，我们可以开始生成我们扩展程序控件。 允许 s 开头准系统的自定义的扩展程序控件类 （请参阅列表 1）。

**列表 1-MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

有几种您注意到了列表 1 中的控件扩展程序类的方法。 首先，请注意此类从 ExtenderControlBase 基类继承。 所有 AJAX 控件工具包扩展程序控件都派生自这个基类。 例如，基类包括为每个控件扩展程序的必需的属性的 TargetID 属性。

接下来，请注意，此类包含与客户端脚本相关的以下两个属性：

- Web 资源的会导致要作为嵌入资源的程序集中包含的文件。
- ClientScriptResource-会导致脚本资源要从程序集检索。

Web 资源属性用于编译自定义的扩展程序时，将 MyControlBehavior.js JavaScript 文件嵌入到程序集。 ClientScriptResource 属性用于在网页中使用的自定义的扩展程序时的程序集中检索 MyControlBehavior.js 脚本。


在要工作的 web 资源和 ClientScriptResource 属性的顺序，您必须作为嵌入资源将编译 JavaScript 文件。 选择解决方案资源管理器窗口中的文件，打开属性表中，并将值分配*嵌入的资源*到**生成操作**属性。


请注意，该控件扩展程序还包含 TargetControlType 属性。 此属性用于指定由该控件扩展程序扩展的控件的类型。 列出 1，在该控件扩展程序用于扩展文本框。

最后，请注意，自定义扩展包含名为 MyProperty 的属性。 属性用 ExtenderControlProperty 特性标记。 GetPropertyValue() 和 SetPropertyValue() 方法用于将从服务器端控件扩展程序的属性值传递到客户端行为。

让我们来继续操作并实现我们 DisabledButton 扩展程序的代码。 列出 2 中找不到此扩展程序的代码。

**列出 2-DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

列出 2 中的 DisabledButton 扩展程序具有名为 TargetButtonID 和 DisabledText 的两个属性。 应用于 TargetButtonID 属性 IDReferenceProperty 防止将按钮控件的 ID 以外的任何分配给此属性。

Web 资源和 ClientScriptResource 属性将位于此扩展程序的名为 DisabledButtonBehavior.js 文件中的客户端行为相关联。 此 JavaScript 文件在下一部分中的，我们讨论。

## <a name="creating-the-custom-extender-behavior"></a>创建自定义的扩展程序行为

控件扩展程序的客户端组件调用行为。 禁用和启用该按钮的实际逻辑包含在 DisabledButton 行为。 行为的 JavaScript 代码包含在列出 3 中。

**列出 3-DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

列出 3 中的 JavaScript 文件包含一个名为 DisabledButtonBehavior 的客户端类。 此类，如其服务器端双向，包括两个属性名为 TargetButtonID 并使用户能够使用 DisabledText 获取\_TargetButtonID/set\_TargetButtonID 并获取\_DisabledText/set\_DisabledText。

Initialize （） 方法将与行为的目标元素关联的 keyup 事件处理程序。 与此行为关联的文本框中键入字母每次 keyup 处理程序执行。 Keyup 处理程序启用，或禁用的按钮，具体取决于是否包含任何文本与行为关联的文本框。

请记住，必须编译作为嵌入资源清单 3 中的 JavaScript 文件。 选择解决方案资源管理器窗口中的文件，打开属性表中，并将值分配*嵌入的资源*到**生成操作**属性 （请参见图 3）。 此选项是在 Visual Studio 和 Visual Web Developer 中可用。


[![添加一个 JavaScript 文件作为嵌入资源](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**图 03**： 添加一个 JavaScript 文件作为嵌入资源 ([单击以查看实际尺寸的图像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>创建自定义的扩展程序设计器

没有一个我们需要进行创建，以完成我们扩展程序的最后一个类。 我们需要在列出 4 中创建设计器类。 此类才能与 Visual Studio/Visual Web Developer 设计器的正确行为的扩展程序。

**列出 4-DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

你将使用设计器特性的 DisabledButton 扩展程序与关联的设计器中列出的 4。你需要将该设计器属性应用到 DisabledButtonExtender 类如下：

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>使用自定义的扩展程序

现在，我们已完成创建 DisabledButton 控件扩展程序，就可以在我们的 ASP.NET 网站中使用它。 首先，我们需要向工具箱添加自定义的扩展程序。 请执行这些步骤：

1. 通过双击解决方案资源管理器窗口中的网页中打开 ASP.NET 页。
2. 右击工具箱并选择菜单选项**选择项**。
3. 在选择工具箱项对话框中，浏览到 CustomExtenders.dll 程序集。
4. 单击**确定**按钮以关闭对话框。

完成这些步骤后，DisabledButton 控件扩展程序应出现在工具箱 （请参见图 4）。


[![在工具箱 DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**图 04**: 工具箱中的 DisabledButton ([单击以查看实际尺寸的图像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))


接下来，我们需要创建一个新的 ASP.NET 页。 请执行这些步骤：

1. 创建一个名为 ShowDisabledButton.aspx 的新 ASP.NET 页。
2. ScriptManager 拖到页。
3. 将 TextBox 控件拖到该页面。
4. 将按钮控件拖到该页面。
5. 在属性窗口中，将更改按钮 ID 属性的值*btnSave*和值的文本属性*保存\**。
  

我们创建了一个页面，标准 ASP.NET 文本框和按钮控件。

接下来，我们需要扩展带 DisabledButton 扩展程序的 TextBox 控件：

1. 选择**添加扩展程序**任务选项来打开扩展程序向导对话框 （请参见图 5）。 请注意对话框包括我们自定义的 DisabledButton 扩展程序。
2. 选择 DisabledButton 扩展程序，然后单击**确定**按钮。


[![扩展程序向导对话框](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**图 05**: 扩展程序向导对话框 ([单击以查看实际尺寸的图像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))


最后，我们可以设置 DisabledButton 扩展程序的属性。 你可以通过修改文本框控件的属性来修改 DisabledButton 扩展程序的属性：

1. 在设计器中选择文本框。
2. 在属性窗口中，展开扩展节点 （请参阅图 6）。
3. 将值分配*保存*DisabledText 属性和的值*btnSave*到 TargetButtonID 属性。


[![设置扩展程序属性](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**图 06**： 设置扩展程序属性 ([单击以查看实际尺寸的图像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))


（通过点击 F5） 运行页面时，该按钮控件最初处于禁用状态。 只要你开始在文本框中输入文本，按钮控件被启用 （请参阅图 7）。


[![操作 DisabledButton 扩展器](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**图 07**: DisabledButton 扩展程序中操作 ([单击以查看实际尺寸的图像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))


## <a name="summary"></a>摘要

本教程的目的是说明如何将扩展 AJAX 控件工具包带有自定义的扩展程序控件。 在本教程中，我们创建了一个简单 DisabledButton 控件扩展程序添加。 我们通过创建 DisabledButtonExtender 类、 DisabledButtonBehavior JavaScript 行为和 DisabledButtonDesigner 类实现了此扩展程序。 每当创建自定义控件扩展程序遵循一组类似的步骤。

>[!div class="step-by-step"]
[上一篇](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
