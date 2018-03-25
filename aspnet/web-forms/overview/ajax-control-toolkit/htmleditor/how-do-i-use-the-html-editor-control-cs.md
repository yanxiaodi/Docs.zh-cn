---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: "如何使用 HTML 编辑器控件？ (C#) |Microsoft 文档"
author: microsoft
description: "HTMLEditor 是 ASP.NET AJAX 控件，您可以轻松地创建和编辑通过在工具栏中的按钮的 HTML 内容。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 17343660d7bf7aa6210fa9c6c9c0206598d34b18
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-html-editor-control-c"></a>如何使用 HTML 编辑器控件？ (C#)
====================
通过[Microsoft](https://github.com/microsoft)

> HTMLEditor 是 ASP.NET AJAX 控件，您可以轻松地创建和编辑通过在工具栏中的按钮的 HTML 内容。


本教程旨在为你提供附带 AJAX 控件工具包的 HTML 编辑器控件的概述。 HTML 编辑器包括更改字体大小、 选择一种字体、 更改背景色、 修改的前景色，选项添加链接，添加图像，更改文本对齐方式，并执行剪切、 复制和粘贴的操作 （请参见图 1）。


[![HTML 编辑器](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**图 01**: HTML 编辑器 ([单击以查看实际尺寸的图像](how-do-i-use-the-html-editor-control-cs/_static/image2.png))


HTML 编辑器使您能够输入使用设计模式下的内容，也可以直接输入 HTML。 你还附带了用于预览 HTML 内容的选项 （请参见图 2）。


[![设计、 HTML 和预览按钮](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**图 02**： 设计、 HTML 和预览按钮 ([单击以查看实际尺寸的图像](how-do-i-use-the-html-editor-control-cs/_static/image4.png))


在本教程中，你将学习如何显示 HTML 编辑器，如何自定义工具栏按钮显示在 HTML 编辑器中，以及如何避免跨站点脚本攻击。

## <a name="displaying-the-html-editor"></a>显示 HTML 编辑器

你可以在 ASP.NET 页中使用 HTML 编辑器之前，首先必须将 ScriptManager 控件添加到页面中。 ScriptManager 控件位于下 Visual Studio/Visual Web Developer Express 工具箱中的 AJAX Extensions 选项卡。

应将 ScriptManager 控件放在之前页面上的任何其他控件的页面的顶部。 例如，可以将它立即打开服务器端下面&lt;窗体&gt;标记。

HTML 编辑器控件位于工具箱中与 AJAX 控件工具包控件的其余部分。 它名为编辑器控件 （请参见图 3）。


[![HTML 编辑器控件](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**图 03**: HTML 编辑器控件 ([单击以查看实际尺寸的图像](how-do-i-use-the-html-editor-control-cs/_static/image6.png))


HTML 编辑器拖到页面上后，你可以在属性表中设置其属性。 例如，你通常想要设置宽度和高度属性。 列表 1 包含 ASP.NET 页包含 HTML 编辑器源。

**列表 1-SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

列表 1 中的页面包含的 HTML 编辑器控件，按钮控件和文本控件。 文本控件中单击按钮时，显示的 HTML 编辑器中的内容 （请参见图 4）。


[![提交窗体使用 HTML 编辑器](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**图 04**： 提交窗体使用 HTML 编辑器 ([单击以查看实际尺寸的图像](how-do-i-use-the-html-editor-control-cs/_static/image8.png))


HTML 编辑器 Content 属性用于检索输入到 HTML 编辑器中的 HTML 内容。 请注意，此 HTML 内容可以包含 JavaScript。 在下一部分中，我们将讨论如何阻止 JavaScript 注入式攻击。

## <a name="customizing-the-html-editor-toolbar"></a>自定义 HTML 编辑器工具栏

你可以自定义完全哪些按钮出现在编辑器中。 例如，你可能想要删除 HTML 选项卡，以防止用户 HTML 编辑器切换到 HTML 模式。 或者，你可能想要删除要阻止用户在论坛中创建过大的文本的字体大小下拉列表 post 消息 （请参见图 5）。


[![自定义的 HTML 编辑器](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**图 05**： 一个自定义 HTML 编辑器 ([单击以查看实际尺寸的图像](how-do-i-use-the-html-editor-control-cs/_static/image10.png))


你通过派生自的基类的编辑器的新的 HTML 编辑器自定义工具栏按钮。 例如，清单 2 中的自定义编辑器仅包含加粗和倾斜的工具栏按钮。 已删除所有其他工具栏按钮。 此外，HTML 选项卡已从底部的编辑器中 （但设计和预览选项卡仍有）。

**列出 2-应用\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

必须向您的应用程序中列出 2 中添加类\_代码文件夹，以便将自动编译的类。 如果应用程序\_代码文件夹中你的网站不存在，则可以只需将文件夹添加。

创建自定义编辑器后，你可以将其添加到 ASP.NET 页相同的方式添加正常 HTML 编辑器中 （请参阅列出 3）。

**列出 3-ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>避免跨站点脚本 (XSS) 攻击

无论何时你接受来自用户的输入，并重新该输入显示在网站上，你可能打开你受到跨站点脚本 (XSS) 攻击的网站。 从理论上讲，恶意黑客无法提交时将重新显示输入获取执行的 JavaScript 代码。 JavaScript 无法用于窃取用户密码或其他敏感信息。

通常情况下，你可以抵消被 HTML 编码显示在 web 页前从用户检索任何输入 XSS 攻击。 但是，HTML 编码输出的 HTML 编辑器中将不仅编码&lt;脚本&gt;标记，它还对所有的 HTML 标记进行编码。 换而言之，你都将丢失的所有格式如字体类型、 字体大小和背景色。

如果你正在从你的用户-如密码、 信用卡卡号和社会保障号-收集敏感信息然后应不显示来自用户使用 HTML 编辑器中检索的未编码内容。 到你的网站由受信任方，应仅在不重新显示 HTML 内容，或提交所使用的 HTML 内容中使用 HTML 编辑器。

例如，假设你要创建一个博客的应用程序。 在此情况下，它有意义时要使用 HTML 编辑器撰写的博客文章。 是唯一提交博客文章的人，并有可能信任自己无法提交恶意 JavaScript。 但是，它没有意义时要使用 HTML 编辑器允许匿名用户发表评论。 你应在用户将提交密码等敏感信息的情况下特别小心。 可能的恶意用户可以将发布包含右 JavaScript 的窃取密码的注释。

## <a name="summary"></a>摘要

在本教程中，你已提供包括 AJAX 控件工具包中的 HTML 编辑器控件的简要概述。 您学习了如何使用 HTML 编辑器接受来自用户的丰富内容并将其提交到服务器的内容。 我们还讨论了如何自定义工具栏按钮显示由 HTML 编辑器中。 最后，您学习了如何避免跨站点脚本攻击时使用 HTML 编辑器接受潜在的恶意输入。

>[!div class="step-by-step"]
[下一篇](how-do-i-use-the-html-editor-control-vb.md)
