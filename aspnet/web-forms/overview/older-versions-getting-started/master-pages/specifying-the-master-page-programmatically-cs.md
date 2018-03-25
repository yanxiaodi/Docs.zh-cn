---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: "以编程方式指定 Master 页 (C#) |Microsoft 文档"
author: rick-anderson
description: "查看设置内容页的母版页以编程方式通过 PreInit 事件处理程序。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 15efb8e2f38b7a405da0c0e12e447e5c3146f025
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="specifying-the-master-page-programmatically-c"></a>以编程方式指定 Master 页 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip)或[下载 PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> 查看设置内容页的母版页以编程方式通过 PreInit 事件处理程序。


## <a name="introduction"></a>介绍

中的篇示例以来[*创建站点范围布局使用 Master 页*](creating-a-site-wide-layout-using-master-pages-cs.md)，所有内容页引用以声明方式通过其母版页`MasterPageFile`中属性`@Page`指令。 例如，以下`@Page`指令链接到的主页面的内容页`Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

[ `Page`类](https://msdn.microsoft.com/library/system.web.ui.page.aspx)中`System.Web.UI`命名空间包括[`MasterPageFile`属性](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx)，将路径返回到内容页的母版页; 它是由设置此属性`@Page`指令。 此属性还可以用于以编程方式指定内容页的母版页。 此方法非常有用，如果你想要动态分配母版页基于外部因素，例如访问的页面的用户。

在本教程中我们将第二个母版页添加到我们的网站，并动态决定要在运行时使用的主页面。

## <a name="step-1-a-look-at-the-page-lifecycle"></a>步骤 1： 查看页面生命周期

每当请求到达 web 服务器是内容页的 ASP.NET 页时，ASP.NET 引擎必须保险丝页面的内容控件添加到母版页的对应 ContentPlaceHolder 控件。 此合成创建然后可以继续完成典型的页面生命周期的单个控件层次结构。

图 1 说明此合成。 步骤 1 中图 1 显示了初始内容和母版页控件层次结构。 末端的 PreInit 阶段内容页中的控件将添加到相应 ContentPlaceHolders 母版页 (步骤 2) 中。 在此合成后母版页用作融合的控件层次结构的根。 这在打印控件层次结构随后添加到页后，可以生成完成的控件层次结构 (步骤 3)。 最终结果是页的控件层次结构包含融合的控件层次结构。


[![母版页和内容页的控件层次结构排列 Fused PreInit 阶段](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**图 01**： 在母版页和内容页的控件层次结构排列 Fused PreInit 阶段 ([单击以查看实际尺寸的图像](specifying-the-master-page-programmatically-cs/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>步骤 2： 设置`MasterPageFile`代码中的属性

在此合成 partakes 哪些母版页依赖于的值`Page`对象的`MasterPageFile`属性。 设置`MasterPageFile`属性中`@Page`指令有分配的净效果`Page`的`MasterPageFile`在初始化阶段，即该页面的生命周期的第一个阶段的属性。 或者，我们可以以编程方式设置此属性。 但是，它是命令性图 1 中的合成发生之前，设置此属性。

PreInit 阶段开始时`Page`对象引发其[`PreInit`事件](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx)并调用其[`OnPreInit`方法](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx)。 若要以编程方式设置主控页，然后，我们可以创建的事件处理程序`PreInit`事件或替代`OnPreInit`方法。 让我们看一下这两种方法。

首先打开`Default.aspx.cs`，我们站点的主页的代码隐藏类文件。 添加事件处理程序为页的`PreInit`通过键入下面的代码中的事件：


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

从此处我们可以设置`MasterPageFile`属性。 更新代码，以便它将的值"~ / Site.master"到`MasterPageFile`属性。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

如果设置断点并开始调试你将看到，每当`Default.aspx`网页访问，或者每当没有回发到此页上，`Page_PreInit`事件处理程序执行和`MasterPageFile`属性分配给"~ / Site.master"。

或者，你可以重写`Page`类的`OnPreInit`方法和组`MasterPageFile`那里属性。 对于此示例中，让我们不设置母版页中的特定页，而从`BasePage`。 回想一下，我们创建了一个自定义的基本页类 (`BasePage`) 返回[*母版页中指定的标题、 Meta 标记和其他 HTML 标头*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教程。 当前`BasePage`重写`Page`类的`OnLoadComplete`方法，它可以将设置页面的`Title`属性基于站点地图数据。 让我们更新`BasePage`还重写`OnPreInit`方法以编程方式指定母版页。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

因为我们的所有内容页派生自`BasePage`，所有这些现在都以编程方式分配其母版页。 此时`PreInit`中的事件处理程序`Default.aspx.cs`是多余的; 请尝试将其删除。

### <a name="what-about-thepagedirective"></a>呢`@Page`指令？

新增功能可能有点令人困惑在于内容页的`MasterPageFile`属性现在在两个位置中指定： 以编程方式在`BasePage`类的`OnPreInit`方法以及通过`MasterPageFile`每个内容页面中的属性`@Page`指令。

页生命周期中的第一个阶段是初始化阶段。 在此阶段`Page`对象的`MasterPageFile`属性分配的值`MasterPageFile`属性中`@Page`指令 （如果提供）。 PreInit 阶段遵循初始化阶段中，并就在这里，我们以编程方式设置`Page`对象的`MasterPageFile`属性，从而覆盖从分配的值`@Page`指令。 因为我们设置`Page`对象的`MasterPageFile`属性以编程方式，我们无法删除`MasterPageFile`属性从`@Page`指令而不会影响最终用户体验。 来说服您自己的这种情况，请继续并删除`MasterPageFile`属性从`@Page`指令`Default.aspx`，然后访问通过浏览器页面。 如你所料，输出之前相同的属性已被删除。

是否`MasterPageFile`通过设置属性`@Page`指令或以编程方式向最终用户体验无关紧要。 但是，`MasterPageFile`属性中`@Page`指令由 Visual Studio 在设计时用于生成所见即所得视图设计器中的。 如果返回到`Default.aspx`Visual Studio 中，然后导航到设计器中，你将看到消息，"母版页错误： 页了控件需要母版页引用，但是未指定"（请参见图 2）。

简单地说，您需要离开`MasterPageFile`属性中`@Page`指令以享受 Visual Studio 中丰富的设计时体验。


[![Visual Studio 使用@Page指令的 MasterPageFile 属性呈现设计视图](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**图 02**: Visual Studio 将使用`@Page`指令的`MasterPageFile`属性呈现到设计视图 ([单击以查看实际尺寸的图像](specifying-the-master-page-programmatically-cs/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>步骤 3： 创建的替代项母版页

因为在运行时以编程方式设置内容页面的母版页则可以动态加载特定母版页根据某些外部标准。 此功能可以在其中站点的布局需要改变根据用户的情况下有用。 例如，博客引擎 web 应用程序可能允许其用户选择其博客的布局其中每个布局是与不同的母版页相关联。 在运行时，当访问者查看用户的博客，web 应用程序需要确定博客的布局和动态将相应的主控页的内容页与相关联。

让我们看一下如何动态加载在运行时根据某些外部标准母版页。 我们的网站目前包含只在一个母版页 (`Site.master`)。 我们需要另一个主页面，以说明选择母版页在运行时。 此步骤重点介绍创建和配置新的主控页。 步骤 4 来看待确定哪些主页后，可以在运行时使用。

名为的根文件夹中创建新的母版页`Alternate.master`。 此外将新的样式表添加到名为的网站`AlternateStyles.css`。


[![添加另一个母版页和 CSS 文件到网站](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**图 03**： 添加另一个母版页和 CSS 文件到网站 ([单击以查看实际尺寸的图像](specifying-the-master-page-programmatically-cs/_static/image9.png))


我已设计`Alternate.master`母版页能够在顶部的页上，居中和深蓝色背景上显示的标题。 我已分配的左侧列，移动下的该内容`MainContent`ContentPlaceHolder 控件，现在跨越页面的整个宽度。 此外，我 nixed 未经排序的课程列表并替换水平上列`MainContent`。 我也已更新的字体和颜色的母版页 （和使用，通过扩展，其内容页）。 图 4 显示`Default.aspx`时使用`Alternate.master`母版页。

> [!NOTE]
> ASP.NET 包括能够定义*主题*。 主题是图像、 CSS 文件和与样式有关的 Web 控件属性设置。 可以应用于在运行时的页的集合。 主题是如果你的站点布局各不相同，这是仅在显示的图像和由其 CSS 规则的方式。 如果布局有所不同更大体上，例如使用不同的 Web 控件或具有完全不同的布局，你将需要使用单独的主控页。 在本教程末尾的有关主题的详细信息，请查阅其他阅读材料部分。


[![我们内容页现在可以使用新的外观和感觉](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**图 04**： 我们内容页现在可以使用新的外观和感觉 ([单击以查看实际尺寸的图像](specifying-the-master-page-programmatically-cs/_static/image12.png))


当 fused master 和内容页的标记时，`MasterPage`类检查，以确保每个内容控件在内容页中的引用在母版页中的 ContentPlaceHolder。 如果找到了引用不存在 ContentPlaceHolder 内容控件，将引发异常。 换而言之，它是命令性主控页分配给内容页具有 ContentPlaceHolder，每个内容控件在内容页中的。

`Site.master`母版页包括四个 ContentPlaceHolder 控件：

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

一些我们的网站中的内容页包括只是一个或两个内容控件;其他每个可用 ContentPlaceHolders 包括内容控件。 如果我们新的母版页 (`Alternate.master`) 可能曾分配给具有内容控件中 ContentPlaceHolders 的所有这些内容页`Site.master`然后很重要，`Alternate.master`还包括与相同ContentPlaceHolder控制`Site.master`.

若要获取你`Alternate.master`母版页，看起来像要挖掘 （请参见图 4），请首先定义中的主控页的样式`AlternateStyles.css`样式表。 添加到以下规则`AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

接下来，添加到以下的声明性标记`Alternate.master`。 如你所见，`Alternate.master`包含具有相同的四个 ContentPlaceHolder 控件`ID`值中的 ContentPlaceHolder 控件作为`Site.master`。 此外，它包括 ScriptManager 控件，我们的网站中使用 ASP.NET AJAX 框架这些页才需要。


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>测试新母版页

若要测试此新的母版页更新`BasePage`类的`OnPreInit`方法，以便`MasterPageFile`属性分配值"~ / Alternate.maser"，然后访问该网站。 每一页应正常情况下，除了两个函数：`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`。 在说明如何向中添加产品`~/Admin/AddProduct.aspx`导致`NullReferenceException`从尝试设置主控页的代码行`GridMessageText`属性。 当来访`~/Admin/Products.aspx``InvalidCastException`会在页面加载并显示消息上引发:"找不到类型的对象强制转换 ASP.alternate\_master 类型 ASP.site\_master。"

发生这些错误是因为`Site.master`代码隐藏类包括公共事件、 属性和方法中未定义`Alternate.master`。 这些两个页面的标记部分有`@MasterType`引用的指令`Site.master`母版页。


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

此外，说明的`ItemInserted`中的事件处理程序`~/Admin/AddProduct.aspx`包括将强制转换松散类型的代码`Page.Master`类型的对象属性`Site`。 `@MasterType`指令 （使用这种方式） 和中的转换`ItemInserted`事件处理程序紧密标`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`到页`Site.master`母版页。

若要中断，可以有此紧密耦合`Site.master`和`Alternate.master`派生自公共基类，它包含的公共成员的定义。 接下来，我们可以更新`@MasterType`指令以引用此通用的基类型。

### <a name="creating-a-custom-base-master-page-class"></a>创建自定义基本 Master 页类

添加新的类文件与`App_Code`文件夹名为`BaseMasterPage.cs`，并将其派生自`System.Web.UI.MasterPage`。 我们需要定义`RefreshRecentProductsGrid`方法和`GridMessageText`中的属性`BaseMasterPage`，但我们不能只需将它们移动存在从`Site.master`因为这些成员使用特定于的 Web 控件的工作`Site.master`母版页 ( `RecentProducts`GridView 和`GridMessage`标签)。

我们需要做什么是配置`BaseMasterPage`这些成员，定义，但由实际实现的方式`BaseMasterPage`的派生类 (`Site.master`和`Alternate.master`)。 这种类型的继承可通过将标记的类并为其成员`abstract`。 简单地说，添加`abstract`到这两个成员的关键字宣布`BaseMasterPage`尚未实现`RefreshRecentProductsGrid`和`GridMessageText`，但该及其派生的类将。

我们还需要定义`PricesDoubled`中的事件`BaseMasterPage`通过并提供了一种方法来引发事件的派生类。 .NET Framework 中用于促进此行为的模式是以基类中创建的公共事件，添加一个受保护`virtual`方法名为`OnEventName`。 派生的类可以然后调用此方法来引发事件，或可重写它之前或之后引发事件时，才执行代码。

更新你`BaseMasterPage`类以使其包含以下代码：


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

接下来，请转到`Site.master`代码隐藏类，并将其派生自`BaseMasterPage`。 因为`BaseMasterPage`是`abstract`我们需要重写那些`abstract`中的成员此处`Site.master`。 添加`override`关键字的方法和属性定义。 此外更新引发的代码`PricesDoubled`中的事件`DoublePrice`按钮的`Click`通过调用基类的事件处理程序`OnPricesDoubled`方法。

这些修改后`Site.master`代码隐藏类应包含以下代码：


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

我们还需要更新`Alternate.master`的代码隐藏类以派生自`BaseMasterPage`并重写两个`abstract`成员。 但是，由于`Alternate.master`不包含一个 GridView，最新的产品和新产品后显示一条消息的标签都添加到数据库的列表，这些方法不需要执行任何操作。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>引用基 Master 页类

现在，我们已完成`BaseMasterPage`类并且没有将其扩展我们两个母版页，我们最后一步是更新`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`页来指代此通用类型。 通过更改启动`@MasterType`指令从这两个页中：


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

到:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

而不是引用文件路径，`@MasterType`属性现在引用的基类型 (`BaseMasterPage`)。 因此，对强类型`Master`在这两个页的代码隐藏类中使用的属性现在是类型的`BaseMasterPage`(而不是类型`Site`)。 进行此更改后就地重新访问`~/Admin/Products.aspx`。 以前，这导致强制转换错误因为页面已配置为使用`Alternate.master`主页上，但`@MasterType`指令引用`Site.master`文件。 但现在没有错误呈现页面。 这是因为`Alternate.master`母版页可以强制转换为类型的对象`BaseMasterPage`（因为它进行扩展）。

没有需要在中所做的一个小更改`~/Admin/AddProduct.aspx`。 说明如何控制`ItemInserted`事件处理程序使用这两个强类型`Master`属性和松散类型`Page.Master`属性。 我们解决的强类型的引用，我们更新时`@MasterType`指令，但我们仍需要更新的松散类型化的引用。 将以下代码行：


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

使用以下内容，这会强制`Page.Master`为基类型：


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>步骤 4： 确定哪些母版页绑定到内容页

我们`BasePage`类当前设置所有内容页的`MasterPageFile`属性设置为在页面生命周期的 PreInit 阶段硬编码的值。 我们可以更新此代码，以根据某些外部因素的主控页。 可能要加载的主页面取决于当前登录用户的首选项。 在这种情况下，我们将需要在中编写代码`OnPreInit`中的方法`BasePage`，查找当前正在访问用户的母版页首选项。

让我们创建一个网页，允许用户选择的主控页后，可以使用-`Site.master`或`Alternate.master`-并将此选择保存的会话变量中。 通过在名为的根目录中创建新的 web 页启动`ChooseMasterPage.aspx`。 创建此页 （或任何其他内容页之后） 时无需将其绑定到母板页中，因为主控页以编程方式设置`BasePage`。 但是，如果你不会绑定的新页到母版页然后新页面的默认声明性标记包含 Web 窗体和母版页由提供的其他内容。 你将需要手动将此标记替换为适当的内容控件。 为此，我发现更轻松地将新的 ASP.NET 页绑定到母版页。

> [!NOTE]
> 因为`Site.master`和`Alternate.master`具有组相同的 ContentPlaceHolder 控件并不重要时创建新的内容页选择哪些母版页。 为了保持一致性，建议使用`Site.master`。


[![将新的内容页添加到网站](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**图 05**： 向网站添加新的内容页 ([单击以查看实际尺寸的图像](specifying-the-master-page-programmatically-cs/_static/image15.png))


更新`Web.sitemap`文件以包含本课程的适当的项。 添加以下标记下的`<siteMapNode>`母版页和 ASP.NET AJAX 课后生成：


[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

然后再添加任何内容分发至`ChooseMasterPage.aspx`页面需要一段时间来更新页的代码隐藏类，使它派生自`BasePage`(而非`System.Web.UI.Page`)。 接下来，将 DropDownList 控件添加到页，设置其`ID`属性`MasterPageChoice`，并添加具有两个 ListItems`Text`的值"~ / Site.master"和"~ / Alternate.master"。

向页面添加按钮 Web 控件并设置其`ID`和`Text`属性设置为`SaveLayout`和"保存布局选择"，分别。 此时页面的声明性标记应类似以下：


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

当首次访问页时，我们需要显示用户的当前所选的母版页选择。 创建`Page_Load`事件处理程序并添加以下代码：


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

上面的代码执行仅在第一次的页访问 （而不在后续回发）。 它首先检查是否会话变量`MyMasterPage`存在。 如果是这样，它尝试查找列表中的项匹配`MasterPageChoice`DropDownList。 如果找到匹配的 ListItem，则其`Selected`属性设置为`true`。

我们还需要将保存到的用户的选择的代码`MyMasterPage`会话变量。 创建的事件处理程序`SaveLayout`按钮的`Click`事件并添加以下代码：


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> 按时间`Click`事件处理程序执行回发时，主控页已被选择。 因此，用户的下拉列表中选择不会生效直到下一个页面访问。 `Response.Redirect`强制浏览器将重新请求`ChooseMasterPage.aspx`。


与`ChooseMasterPage.aspx`完成的页上，我们最后一项任务是让`BasePage`分配`MasterPageFile`属性基于的值`MyMasterPage`会话变量。 如果未设置会话变量具有`BasePage`默认为`Site.master`。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> 我已分配的代码将移动`Page`对象的`MasterPageFile`外的属性`OnPreInit`事件处理程序，并放入两个不同方法。 此第一种方法， `SetMasterPageFile`，将分配`MasterPageFile`到第二种方法，返回的值的属性`GetMasterPageFileFromSession`。 我进行了`SetMasterPageFile`方法`virtual`以便将来的课程可扩展`BasePage`可以选择重写以使其实现自定义逻辑，如果需要。 我们将看到举例说明重写`BasePage`的`SetMasterPageFile`下一教程中的属性。


使用此代码中的位置，请访问`ChooseMasterPage.aspx`页。 最初，`Site.master`母版页是所选 （请参阅图 6），但用户均可选择不同的母版页，从下拉列表。


[![内容页将显示使用 Site.master 母版页](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**图 06**： 内容页将显示使用`Site.master`母版页 ([单击以查看实际尺寸的图像](specifying-the-master-page-programmatically-cs/_static/image18.png))


[![内容页现在显示使用 Alternate.master 母版页](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**图 07**： 内容页是现在显示使用`Alternate.master`母版页 ([单击以查看实际尺寸的图像](specifying-the-master-page-programmatically-cs/_static/image21.png))


## <a name="summary"></a>摘要

时访问内容页，其内容的控件使用其母版页 ContentPlaceHolder 控件融合。 内容页的主控页由表示`Page`类的`MasterPageFile`属性，该值将赋给`@Page`指令的`MasterPageFile`在初始化阶段的属性。 与本教程介绍了，我们可以将值赋给`MasterPageFile`只要我们这样 PreInit 阶段结束之前的属性。 能够以编程方式指定主控页将打开用于更高级的方案，如动态绑定到外部因素所基于的母版页的内容页的门。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET 页面生命周期关系图](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [ASP.NET 页面生命周期概述](https://msdn.microsoft.com/library/ms178472.aspx)
- [ASP.NET 主题和皮肤概述](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [母版页： 提示、 技巧和陷阱](http://www.odetocode.com/articles/450.aspx)
- [在 ASP.NET 中的主题](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多个 ASP/ASP.NET 丛书和 4GuysFromRolla.com 创始人，具有已使用 Microsoft Web 技术自 1998 年。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 3.5 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 可以在达到 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Suchi Banerjee。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](master-pages-and-asp-net-ajax-cs.md)
[下一页](nested-master-pages-cs.md)
