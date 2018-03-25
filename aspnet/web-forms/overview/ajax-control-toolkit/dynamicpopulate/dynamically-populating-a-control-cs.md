---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: "动态填充控件 (C#) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的 DynamicPopulate 控件调用 web 服务 （或页方法），并将生成的值填充到 t 上的目标控件..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a1868a0e4cec4a95d4175ce255fea2e200692075
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-c"></a>动态填充控件 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> ASP.NET AJAX 控件工具包中的 DynamicPopulate 控件调用 web 服务 （或页方法），并将生成的值填充到目标控件在页上，而无需页面刷新。


## <a name="overview"></a>概述

`DynamicPopulate` ASP.NET AJAX 控件工具包中的控件调用 web 服务 （或页方法），并将生成的值填充到目标控件在页上，而无需页面刷新。 本教程演示如何对此进行设置。

## <a name="steps"></a>步骤

首先，你需要 ASP.NET Web 服务实现调用的方法`DynamicPopulate`。 Web 服务类需要`ScriptService`中定义的特性`Microsoft.Web.Script.Services`; 否则 ASP.NET AJAX 无法创建 web 服务，反过来则需要通过客户端 JavaScript 代理`DynamicPopulate`。

Web 方法就必须预料到一个自变量的类型字符串，调用`contextKey`，因为`DynamicPopulate`控件将发送一条与每个 web 服务调用上下文信息。 以下 web 服务所表示的格式返回当前日期`contextKey`自变量：

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

Web 服务然后另存为`DynamicPopulate.cs.asmx`。 或者，可以实现`getDate()`方法包含实际的 ASP.NET 页中的页方法作为`DynamicPopulate`控件。

在下一步的步骤中，创建一个新的 ASP.NET 文件。 如往常一样，第一步是包括`ScriptManager`当前页来加载 ASP.NET AJAX 库并使控件工具包工作中：

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

然后，添加一个标签控件 (例如使用相同的名称，该 HTML 控件或&lt; `asp:Label`  / &gt; web 控件) 的更高版本将显示 web 服务调用的结果。

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

然后将使用 （作为 HTML 控件，因为我们不需要回发到服务器） HTML 按钮来触发动态填充：

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

最后，我们需要`DynamicPopulateExtender`网络操作的控件。 将设置以下属性 (除了明显的`ID`和`runat` = `"server"`):

- `TargetControlID`从 web 服务调用放置结果
- `ServicePath`web 服务的路径 （如果你想要使用的页方法忽略）
- `ServiceMethod`web 方法或页方法的名称
- `ContextKey`上下文信息发送到 web 服务
- `PopulateTriggerControlID`随即将会触发 web 服务调用的元素
- `ClearContentsDuringUpdate`是否要在 web 服务调用期间将空的目标元素

如你所见，该控件所需的一些信息，但将所有内容放入到位是相当直接。 下面是有关标记`DynamicPopulateExtender`当前应用场景中的控件：

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

在浏览器中运行 ASP.NET 页，然后单击按钮;你将收到月-日-年格式的当前日期。


[![单击按钮从服务器中检索日期](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

单击按钮从服务器中检索日期 ([单击以查看实际尺寸的图像](dynamically-populating-a-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一篇](dynamically-populating-a-control-using-javascript-code-cs.md)
