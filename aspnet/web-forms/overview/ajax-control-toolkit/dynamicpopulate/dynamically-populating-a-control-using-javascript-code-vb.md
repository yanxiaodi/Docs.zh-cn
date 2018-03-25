---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: "动态填充控件使用 JavaScript 代码 (VB) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的 DynamicPopulate 控件调用 web 服务 （或页方法），并将生成的值填充到 t 上的目标控件..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b4090b3a785059c8f09de266df79eba0914e9f13
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a>动态填充控件使用 JavaScript 代码 (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)

> ASP.NET AJAX 控件工具包中的 DynamicPopulate 控件调用 web 服务 （或页方法），并将生成的值填充到目标控件在页上，而无需页面刷新。 还有可能触发使用自定义客户端 JavaScript 代码的填充。


## <a name="overview"></a>概述

`DynamicPopulate` ASP.NET AJAX 控件工具包中的控件调用 web 服务 （或页方法），并将生成的值填充到目标控件在页上，而无需页面刷新。 还有可能触发使用自定义客户端 JavaScript 代码的填充。

## <a name="steps"></a>步骤

首先，你需要 ASP.NET Web 服务实现调用的方法`DynamicPopulateExtender`控件。 Web 服务实现的方法`getDate()`需要一个自变量的类型字符串，调用`contextKey`，因为`DynamicPopulate`控件将发送一条与每个 web 服务调用上下文信息。 以下是代码 (文件`DynamicPopulate.vb.asmx`) 来检索当前日期在三种格式之一：

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

在下一步的步骤中，创建一个新的 ASP.NET 站点并启动与 ASP.NET AJAX ScriptManager 控件：

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

然后，添加一个标签控件 (例如使用相同的名称，该 HTML 控件或`<asp:Label />`web 控件) 的更高版本将显示 web 服务调用的结果。

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

接下来，包括`DynamicPopulateExtender`控制并提供 web 服务信息、 目标控件，但不是随即将会触发这将会更高版本，使用自定义 JavaScript 的填充控件的名称 ！

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

现在我们来的 JavaScript 部分。 `$find()`由 ASP.NET AJAX 库中，定义的函数返回的 ASP.NET AJAX 控件工具包的服务器端对象的引用如`DynamicPopulateExtender`。 在当前的文件中，`$find("dpe")`返回到一个引用`DynamicPopulateExtender`页中的控件。 它公开一个方法，该方法调用`populate()`随即将会触发动态填充过程。 `populate()`方法需要一个参数： 作为自变量的上下文项`getDate()`web 方法。 例如，`$find("dpe").populate("format1")`将填充月-日-年格式的当前日期的标签。

为了使示例更具灵活性，用户现在可能有多种日期格式之间进行选择。 其中的每个，将显示一个单选按钮。 一次用户单击一个单选按钮上的，JavaScript 代码动态填充具有所选的日期格式标签。 下面是这些单选按钮：

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

注意，在单选按钮，JavaScript 表达式的上下文中`this.value`当前按钮，这种情况是完全相同的信息的值是指`getDate()`方法可以使用。


[![单击此按钮会检索在服务器上，指定的格式的日期](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)

单击按钮从服务器上，在指定的格式中检索日期 ([单击以查看实际尺寸的图像](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](dynamically-populating-a-control-vb.md)
[下一页](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)
