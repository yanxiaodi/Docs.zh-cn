---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: "多个 ContentPlaceHolders 和默认内容 (VB) |Microsoft 文档"
author: rick-anderson
description: "检查如何将多个内容占位符添加到母版页，以及如何在内容的占位符中指定默认内容。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: ccb65f0b2f16e0c7a67787f7dfab14303daeca1d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>多个 ContentPlaceHolders 和默认内容 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip)或[下载 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> 检查如何将多个内容占位符添加到母版页，以及如何在内容的占位符中指定默认内容。


## <a name="introduction"></a>介绍

在前面的教程，我们探讨母版页启用 ASP.NET 开发人员创建一致的站点范围布局。 母版页定义其内容页中的所有公用的标记和是按页基于可自定义的区域。 在以前的教程，我们创建了一个简单的母版页 (`Site.master`) 和两个内容页 (`Default.aspx`和`About.aspx`)。 我们母版页由名为的两个 ContentPlaceHolders 构成`head`和`MainContent`，其位于`<head>`元素和 Web 窗体，分别。 当内容页每个具有两个内容控件时, 我们仅指定对应于一个标记`MainContent`。

中的两个 ContentPlaceHolder 控件如`Site.master`，母版页可能包含多个 ContentPlaceHolders。 更重要的是，主控页可以指定 ContentPlaceHolder 控件的默认标记。 内容页中，然后，可以根据需要指定其自己的标记或使用默认标记。 在本教程中我们将查看在母版页中使用多个内容控件，并请参阅如何在 ContentPlaceHolder 控件中定义默认标记。

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>步骤 1： 将其他 ContentPlaceHolder 控件添加到 Master 页

许多网站设计包含在屏幕上的自定义按页基于几个方面。 `Site.master`在前面的教程中，我们创建的主页面包含在名为 Web 窗体单个 ContentPlaceHolder `MainContent`。 具体而言，此 ContentPlaceHolder 位于中`mainContent``<div>`元素。

图 1 显示`Default.aspx`通过浏览器查看时。 以红色圆圈显示的区域是对应的特定于页的标记`MainContent`。


[![带圆圈的区域上显示区域当前可自定义按页基础](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**图 01**: Circled 区域显示区域当前可自定义上按页基础 ([单击以查看实际尺寸的图像](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


假设，除了在地区中图 1 所示，我们还需要将特定于页的项添加到左侧列下方的课程和新闻部分。 若要实现此目的，我们将另一个 ContentPlaceHolder 控件添加到母版页。 若要执行此操作，打开`Site.master`母版页在 Visual Web Developer，然后 ContentPlaceHolder 控件从工具箱中拖动到设计器的新闻部分之后。 设置 ContentPlaceHolder`ID`到`LeftColumnContent`。


[![ContentPlaceHolder 控件添加到母版页的左侧列](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**图 02**: ContentPlaceHolder 控件添加到母版页的左侧列 ([单击以查看实际尺寸的图像](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


通过添加`LeftColumnContent`到母版页 ContentPlaceHolder，我们可以定义此区域的内容按页基于通过包含内容控件在页中`ContentPlaceHolderID`设置为`LeftColumnContent`。 我们检查在步骤 2 中此过程。

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>步骤 2： 在内容页中定义新 ContentPlaceHolder 的内容

在新的内容页添加到网站时，Visual Web Developer 会自动创建一个内容控件中选定的母版页中的每个 ContentPlaceHolder 的页。 具有添加`LeftColumnContent`到在步骤 1 中，现在新 ASP.NET 页将我们母版页 ContentPlaceHolder 具有三个内容控件。

为了说明这一点，请将新的内容页添加到名为根目录`MultipleContentPlaceHolders.aspx`，它绑定到`Site.master`母版页。 Visual Web Developer 将创建具有以下声明性标记此页：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

输入到内容控件引用的某些内容`MainContent`ContentPlaceHolders (`Content2`)。 接下来，添加以下标记到`Content3`内容控件 (后者引用`LeftColumnContent`ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

在添加此标记之后, 访问通过浏览器页面。 如图 3 所示，标记放置在`Content3`（以红色圆圈显示） 新闻节下方左侧列中显示内容控件。 标记放置在`Content2`（用蓝色圆形） 页的右侧部分中显示。


[![左侧的列现在包括新闻节下方的特定于页的内容](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**图 03**: 左侧列现在包括特定于页的内容下新闻部分 ([单击以查看实际尺寸的图像](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>在现有的内容页中定义的内容

自动创建一个新的内容页包含我们在步骤 1 中增加 ContentPlaceHolder 控件。 但我们两个现有内容页-`About.aspx`和`Default.aspx`-没有用于的内容控件`LeftColumnContent`ContentPlaceHolder。 若要为此 ContentPlaceHolder 这些两个现有页中指定的内容，我们需要自行添加内容控件。

不同于大多数 ASP.NET Web 控件，Visual Web 开发人员工具箱不包括的内容控件项。 我们可以手动键入内容控件的声明性标记中的源视图中，但更方便和快捷的方法是使用设计视图。 打开`About.aspx`页上，并切换到设计视图。 如图 4 所示， `LeftColumnContent` ContentPlaceHolder 将显示在设计视图中; 如果鼠标悬停时，显示的标题将显示为:"LeftColumnContent (Master)。" 标题中的包含"Master"指示没有内容控件在页中为此 ContentPlaceHolder 定义。 如果存在 ContentPlaceHolder，如下所示的情况有关的内容控件`MainContent`，将读取标题:"*ContentPlaceHolderID* （自定义）。"

若要添加的内容控件`LeftColumnContent`到 ContentPlaceHolder `About.aspx`，展开 ContentPlaceHolder 的智能标记，然后单击创建自定义内容链接。


[![About.aspx 的设计视图显示 LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**图 04**: 设计视图`About.aspx`显示`LeftColumnContent`ContentPlaceHolder ([单击以查看实际尺寸的图像](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


单击创建自定义内容链接生成所需的内容控件中的页和集其`ContentPlaceHolderID`属性 ContentPlaceHolder `ID`。 例如，单击创建自定义内容链接`LeftColumnContent`在区域`About.aspx`将以下声明性标记添加到页：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>省略内容控件

ASP.NET 不需要所有内容页为在母版页中定义的每个 ContentPlaceHolder 包含内容控件。 如果省略内容控件，则 ASP.NET 引擎将使用在母版页中 ContentPlaceHolder 中定义的标记。 此标记称为 ContentPlaceHolder*默认内容*和在其中内容的某些区域是常见的大部分页，但需要进行自定义对于少量的页面的方案中十分有用。 步骤 3 探讨在母版页中的指定默认内容。

目前，`Default.aspx`包含两个内容控件以`head`和`MainContent`ContentPlaceHolders; 它不具有的内容控件`LeftColumnContent`。 因此，当`Default.aspx`呈现`LeftColumnContent`使用 ContentPlaceHolder 的默认内容。 因为我们尚未为此 ContentPlaceHolder 定义默认的任何内容的净效果是，没有标记，将发出此区域。 若要验证此行为，请访问`Default.aspx`通过浏览器。 如图 5 所示，在左侧列下方的新闻区不发出任何标记。


[![为 LeftColumnContent ContentPlaceHolder 不呈现任何内容](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**图 05**： 无内容呈现为`LeftColumnContent`ContentPlaceHolder ([单击以查看实际尺寸的图像](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>步骤 3： 在 Master 页中指定默认内容

某些网站设计都包括其内容是除了一个或两个异常站点中的所有页相同的区域。 请考虑支持用户帐户的网站。 此类站点需要可以在其中访问者输入其凭据以登录到网站的登录页。 若要加快登录过程，网站设计器可能包含每个页后，可以允许用户登录而无需显式访问登录页的左上角中的用户名和密码的文本框。 虽然这些用户名和密码文本框中很有用在大多数页中，它们是冗余在登录页中，已包含用户的凭据的文本框。

若要实现这种设计，无法在母版页左上角创建 ContentPlaceHolder 控件。 其左上角显示的用户名和密码文本框所需的每一页将为此 ContentPlaceHolder 创建内容控件并添加所需的接口。 登录页上，另一方面，或者将忽略此 ContentPlaceHolder 为添加内容控件，或将创建内容与定义未标记的控件。 此方法的缺点是，我们需要记住要添加到我们将添加到 （除外的登录页中） 站点的每一页的用户名和密码文本框。 这请求时遇到问题。 我们可能会忘记这些文本框中添加一个页面或两个或更糟的是，我们可能会不实现接口正确 （可能添加一个文本框中，而不是两个）。

更好的解决方案是为 ContentPlaceHolder 的默认内容定义用户名和密码文本框。 通过此操作，我们只需重写此默认内容中不显示的用户名和密码文本框这些几个页面 （例如登录页面）。 若要说明指定的默认内容 ContentPlaceHolder 控件，让我们实现刚刚讨论的方案。

> [!NOTE]
> 本教程的其余部分更新我们的网站，登录界面包括所有页，但登录页的左侧列中。 但是，本教程将不检查如何配置网站以支持用户帐户。 有关本主题的详细信息，请参阅我[窗体身份验证、 授权、 用户帐户和角色](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)教程。


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>添加 ContentPlaceHolder 并指定自己的默认内容

打开`Site.master`母版页并将以下标记添加到左侧列之间`DateDisplay`标签和课程部分：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

添加此标记后母版页的设计视图应类似于图 6。


[![母版页包括登录控件](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**图 06**: 母版页包括登录控件 ([单击以查看实际尺寸的图像](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


此 ContentPlaceHolder `QuickLoginUI`，已登录 Web 控件作为其默认内容。 登录控件将显示一个用户界面，提示用户输入其用户名和密码，以及一个登录按钮。 如果单击登录按钮，Login 控件内部验证针对成员资格 API 的用户的凭据。 若要使用此登录控件在实践中，然后，需要配置站点，以使用成员资格。 本主题不在本教程; 的作用域请参阅我[窗体身份验证、 授权、 用户帐户和角色](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)有关生成的 web 应用程序支持用户帐户的详细信息的教程。

随意自定义登录控件的行为或外观。 我已经设置其两个属性：`TitleText`和`FailureAction`。 `TitleText`属性值，该值默认为"Log In"，显示在控件的用户界面的顶部。 我已经设置此属性，以便它显示为"登录"文本`<h3>`元素。 `FailureAction`属性指示要执行的操作如果用户的凭据无效。 它将默认值为`Refresh`，这导致用户能在同一页上，并显示登录控件内的失败消息。 我已更改到`RedirectToLoginPage`，这会将用户转至登录页出现时凭据无效。 我愿意向用户发送到登录页上，当用户尝试从其他页面上，但会失败，登录由于登录页可能包含其他说明和无法轻松地适应左侧列的选项。 例如，登录页可能会包含检索忘记了的密码或创建新的帐户的选项。

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>创建登录页和重写默认内容

与完整母版页后下, 一步是创建登录页。 将 ASP.NET 页添加到名为你的站点根目录`Login.aspx`，将其绑定到`Site.master`母版页。 这样将创建具有四个内容控件的页，在定义的每个 ContentPlaceHolders `Site.master`。

登录将控件添加到`MainContent`内容控件。 同样，随意添加任何内容分发至`LeftColumnContent`区域。 但是，请确保将保留的内容控件`QuickLoginUI`ContentPlaceHolder 空。 这将确保控件不显示在登录页的左侧列中的登录名。

定义的内容后`MainContent`和`LeftColumnContent`区域，您的登录页的声明性标记应类似于以下：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

图 7 显示此页查看通过浏览器时。 因为此页上指定的内容控件`QuickLoginUI`ContentPlaceHolder，它将替代在母版页中指定的默认内容。 净效果是，Login 控件显示在母版页的设计视图 （请参阅图 6） 将不呈现在此页中。


[![登录页 Represses QuickLoginUI ContentPlaceHolder 默认内容](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**图 07**: 登录页 Represses `QuickLoginUI` ContentPlaceHolder 的默认内容 ([单击以查看实际尺寸的图像](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>在新页中使用的默认内容

我们想要在除登录页的所有页面左侧列中显示 Login 控件。 若要实现此目的，登录页除外的所有内容页应省略的内容控件`QuickLoginUI`ContentPlaceHolder。 通过省略内容控件，将改为使用 ContentPlaceHolder 的默认内容。

我们现有内容页- `Default.aspx`， `About.aspx`，和`MultipleContentPlaceHolders.aspx`-不包括的内容控件`QuickLoginUI`因为我们将该 ContentPlaceHolder 控件添加到母版页之前创建它们。 因此，这些现有的页面不需要更新。 但是，新页面添加到该网站包含的内容控件`QuickLoginUI`ContentPlaceHolder，默认情况下的。 因此，我们需要请记住删除这些内容控制每次我们添加一个新的内容页 （除非我们想要重写 ContentPlaceHolder 的默认内容，如登录页）。

若要删除内容控件，您可以手动从源视图中删除其声明性的标记或时，从设计视图中，选择默认为母版页的内容链接从其智能标记。 这两种方法中删除内容控件从页上，然后生成相同的净效果。

图 8 显示`Default.aspx`通过浏览器查看时。 回想一下，`Default.aspx`仅有两个内容控件中其声明性的标记的另一个用于指定`head`，另一个用于`MainContent`。 因此，内容的默认`LeftColumnContent`和`QuickLoginUI`ContentPlaceHolders 会显示。


[![显示内容默认 LeftColumnContent 和 QuickLoginUI ContentPlaceHolders](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**图 08**: 默认内容`LeftColumnContent`和`QuickLoginUI`显示 ContentPlaceHolders ([单击以查看实际尺寸的图像](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>摘要

ASP.NET 母版页模型允许在母版页中任意数量的 ContentPlaceHolders。 更多的是什么，ContentPlaceHolders 包括默认的内容，它将发出在情况下，没有对应的内容在内容页中的控件。 在本教程中我们已了解如何在母版页中包括其他 ContentPlaceHolder 控件以及如何为新的和现有的 ASP.NET 页中的这些新 ContentPlaceHolders 定义内容控件。 我们还了解了指定默认值 ContentPlaceHolder 中的内容即其中只有极少的页需要自定义否则标准化在某些区域内的内容的方案中十分有用。

在下一教程中我们将检查`head`ContentPlaceHolder 更详细地了解如何以声明方式和以编程方式定义的标题、 meta 标记和其他 HTML 标头按页基础上。

尽情享受编程 ！

### <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多个 ASP/ASP.NET 丛书和 4GuysFromRolla.com 创始人，具有已使用 Microsoft Web 技术自 1998 年。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 3.5 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 可以在达到 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Suchi Banerjee。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一页](creating-a-site-wide-layout-using-master-pages-vb.md)
[下一页](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
