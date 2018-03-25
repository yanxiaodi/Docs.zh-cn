---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: "用户控件和 JavaScript (C#) 中使用 DynamicPopulate |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的 DynamicPopulate 控件调用 web 服务 （或页方法），并将生成的值填充到 t 上的目标控件..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d98177561b72ffbe05455f785e156f91e450d36
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>用户控件和 JavaScript (C#) 中使用 DynamicPopulate
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> ASP.NET AJAX 控件工具包中的 DynamicPopulate 控件调用 web 服务 （或页方法），并将生成的值填充到目标控件在页上，而无需页面刷新。 还有可能触发使用自定义客户端 JavaScript 代码的填充。 但是必须格外小心扩展程序驻留在用户控件中时要执行。


## <a name="overview"></a>概述

`DynamicPopulate` ASP.NET AJAX 控件工具包中的控件调用 web 服务 （或页方法），并将生成的值填充到目标控件在页上，而无需页面刷新。 还有可能触发使用自定义客户端 JavaScript 代码的填充。 但是必须格外小心扩展程序驻留在用户控件中时要执行。

## <a name="steps"></a>步骤

首先，你需要 ASP.NET Web 服务实现调用的方法`DynamicPopulateExtender`控件。 Web 服务实现的方法`getDate()`需要一个自变量的类型字符串，调用`contextKey`，因为`DynamicPopulate`控件将发送一条与每个 web 服务调用上下文信息。 以下是代码 (文件`DynamicPopulate.cs.asmx`) 来检索当前日期在三种格式之一：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

在下一步的步骤中，创建一个新的用户控件 (`.ascx`文件)，表示由其第一行中的以下声明：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

A &lt; `label` &gt;元素将用于显示来自服务器的数据。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

此外在用户控件文件中，我们将使用三个单选按钮，表示 web 服务支持的三个可能的日期格式之一的每个组。 当用户单击一个单选按钮时，浏览器将执行 JavaScript 代码将类似如下所示：

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

此代码可访问`DynamicPopulateExtender`（不要担心奇怪 ID，但这将稍后介绍），并触发动态填充的数据。 当前的单选按钮的上下文中`this.value`指其值即`format1`，`format2`或`format3`完全什么 web 方法的要求。

尚未缺少在用户控件中的唯一操作是`DynamicPopulateExtender`链接到 web 服务的单选按钮控件。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

再次你可能注意到控件中使用的奇怪 ID:`mcd1$myDate`而不是`myDate`。 以前，使用的 JavaScript 代码`mcd1_dpe1`访问`DynamicPopulateExtender`而不是`dpe1`。此命名策略是特殊的要求，使用时`DynamicPopulateExtender`用户控件内。 而且，你必须将用户控件中以特定的方式，以使其所有工作。 创建新的 ASP.NET 页并注册刚刚实现用户控件的标记前缀：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

然后，包括 ASP.NET AJAX`ScriptManager`在新页上的控件：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

最后，将用户控件添加到页面中。 你只需设置其`ID`属性 (和`runat="server"`，这是当然的)，但你还必须将其设置为特定的名称：`mcd1`由于这是在用户控件中使用 JavaScript 访问它，使用的前缀。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

就是这么简单！ 页行为与预期相同： 用户单击一个单选按钮，该工具包中的控件调用 web 服务并以所需的格式显示当前日期。


[![用户控件中驻留的单选按钮](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

用户控件中驻留的单选按钮 ([单击以查看实际尺寸的图像](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一页](dynamically-populating-a-control-using-javascript-code-cs.md)
[下一页](dynamically-populating-a-control-vb.md)
