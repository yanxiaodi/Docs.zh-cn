---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: "与验证控件 (C#) 使用 TextBoxWatermark |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 TextBoxWatermark 控件扩展的文本框中，使得在框中显示文本。 当用户单击到框中，它我..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 61fa55c8c4580800de1097b7242c7077cda27115
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>TextBoxWatermark 使用验证控件 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> AJAX 控件工具包中的 TextBoxWatermark 控件扩展的文本框中，使得在框中显示文本。 当用户单击到框中时，它将被清空。 如果用户离开而无需输入文本框中，再次显示已预先的文本。 这可能与在同一页上，ASP.NET 验证控件发生冲突，但可以解决这些问题。


## <a name="overview"></a>概述

`TextBoxWatermark` AJAX 控件工具包中的控件扩展文本框中，以便在框中显示文本。 当用户单击到框中时，它将被清空。 如果用户离开而无需输入文本框中，再次显示已预先的文本。 这可能与在同一页上，ASP.NET 验证控件发生冲突，但可以解决这些问题。

## <a name="steps"></a>步骤

此示例的基本设置为以下：`TextBox`控件打水印使用`TextBoxWatermarkExtender`控件。 按钮触发回发，并将更高版本可用于触发页上的验证控件。 此外，`ScriptManager`初始化 ASP.NET AJAX 所需的控件：

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

现在添加`RequiredFieldValidator`检查是否存在文本字段中提交表单时的控件。 `InitialValue`验证程序的属性必须设置为相同的值中使用`TextBoxWatermarkExtender`控件： 提交表单时，不变的文本框中的值时，在其中的水印值：

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

但是没有使用此方法的一个问题： 如果客户端禁用 JavaScript，文本字段不预先填入使用的水印文本，因此`RequiredFieldValidator`不会触发一条错误消息。 因此，第二个`RequiredFieldValidator`需要进行控制的检查的空文本框 (省略`InitialValue`属性)。

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

因为这两个验证程序使用`Display` = `"Dynamic"`，最终用户不能区分这两个验证程序已激发的可视外观从; 相反，它似乎没有其中只有一。

最后，添加一些服务器端代码以输出字段中的文本，如果没有验证程序发出一条错误消息：

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![验证程序错误报告的字段中没有任何文本](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

验证程序错误报告的字段中没有任何文本 ([单击以查看实际尺寸的图像](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一页](using-textboxwatermark-in-a-formview-cs.md)
[下一页](using-textboxwatermark-in-a-formview-vb.md)
