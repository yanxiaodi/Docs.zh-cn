---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: "控制在内容页 (C#) 中的 ID 命名 |Microsoft 文档"
author: rick-anderson
description: "说明了如何 ContentPlaceHolder 控件充当命名容器并因此以编程方式使用控件 （通过 FindConrol) 困难..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0c0db7fd76a7a486ff45085329ef7c77b0af5ebe
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="control-id-naming-in-content-pages-c"></a>在内容页 (C#) 中命名的控件 ID
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip)或[下载 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> 演示如何 ContentPlaceHolder 控件充当命名容器和因此请以编程方式使用控件 （通过 FindConrol) 困难。 在此问题和解决方法如下所示。 此外讨论如何以编程方式访问生成的 ClientID 值。


## <a name="introduction"></a>介绍

所有 ASP.NET 服务器控件都包括`ID`属性，用于唯一标识该控件以及是依据此控件以编程方式访问中的代码隐藏类的方法。 同样，HTML 文档中的元素可能包括`id`唯一标识此元素的特性; 这些`id`值通常用于在客户端脚本中以编程方式引用特定的 HTML 元素。 鉴于此，您可能会假定的 ASP.NET 服务器控件呈现到 HTML 时其`ID`值用作`id`呈现的 HTML 元素的值。 这不一定是这种情况因为在某些情况下一个控件替换为单个`ID`值可能在呈现标记中出现多次。 请考虑包括与标签 Web 控件与 TemplateField GridView 控件`ID`的产品名称的值。 当 GridView 绑定到它在运行时的数据源时，此标签被重复一次为每个 GridView 行。 每个呈现标签需求唯一`id`值。

若要处理这种情况下，ASP.NET 允许某些控件表示为命名容器。 命名容器将用作新`ID`命名空间。 出现在命名的容器内所有服务器控件都具有其呈现`id`value 前面带`ID`的命名的容器控件。 例如，`GridView`和`GridViewRow`类都是命名的容器。 因此，在与为 GridView TemplateField 中定义的标签控件`ID`ProductName 给定呈现`id`值`GridViewID_GridViewRowID_ProductName`。 因为*GridViewRowID*是对于每个 GridView 一行，生成唯一`id`值是唯一的。

> [!NOTE]
> [ `INamingContainer`接口](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx)用于指示特定的 ASP.NET 服务器控件应该用作的命名容器。 `INamingContainer`接口不拼写出任何服务器控件必须实现的方法; 相反，它可作为一个标记。 在生成的呈现的标记，如果某个控件实现此接口然后 ASP.NET 引擎自动前缀其`ID`值及其子代的呈现`id`属性值。 此过程将在步骤 2 中的更详细地讨论。


命名容器不只更改呈现`id`属性值，但也会影响如何控件可能会以编程方式引用从 ASP.NET 页的代码隐藏类。 `FindControl("controlID")`方法通常用于以编程方式引用 Web 控件。 但是，`FindControl`不入侵通过命名容器。 因此，不能直接使用`Page.FindControl`方法引用一个 GridView 或其他命名容器内的控件。

如你可能具有 surmised，母版页和 ContentPlaceHolders 同时实现为命名容器。 在本教程中，我们将查看如何主页面影响 HTML 元素`id`值和方法以编程方式引用在中内容页使用的 Web 控件`FindControl`。

## <a name="step-1-adding-a-new-aspnet-page"></a>步骤 1： 添加一个新的 ASP.NET 页

为了演示在本教程中所述的概念，让我们将新的 ASP.NET 页添加到我们的网站。 创建一个名为的新内容页`IDIssues.aspx`在根文件夹中，将其绑定到`Site.master`母版页。


![将内容页 IDIssues.aspx 添加到根文件夹](control-id-naming-in-content-pages-cs/_static/image1.png)

**图 01**： 添加内容页`IDIssues.aspx`的根文件夹


Visual Studio 自动为每个母版页的四个 ContentPlaceHolders 创建内容控件。 中所述[*多个 ContentPlaceHolders 和默认内容*](multiple-contentplaceholders-and-default-content-cs.md)教程，如果内容控件不存在，而是发出主控页的默认 ContentPlaceHolder 内容。 因为`QuickLoginUI`和`LeftColumnContent`ContentPlaceHolders 包含该页的合适的默认标记，请继续并删除其相应内容控件从`IDIssues.aspx`。 此时，内容页的声明性标记应如下所示：


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

在[*母版页中指定的标题、 Meta 标记和其他 HTML 标头*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教程中我们创建了一个自定义的基本页类 (`BasePage`)，如果它是自动配置页的标题未显式设置。 有关`IDIssues.aspx`页上使用此功能，该页面的代码隐藏类必须派生自`BasePage`类 (而不是`System.Web.UI.Page`)。 修改代码隐藏类的定义，使其类似以下所示：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

最后，更新`Web.sitemap`文件以包含此新课适当的项。 添加`<siteMapNode>`元素，并设置其`title`和`url`特性以"控件 ID 命名问题"和`~/IDIssues.aspx`分别。 建立此添加后你`Web.sitemap`文件的标记的外观应与以下类似：


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

如图 2 所示，在新的站点映射条目`Web.sitemap`立即反映在左侧列中的课程部分。


![课程部分现在包括一个指向&quot;控制命名问题的 ID&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**图 02**： 课程部分现在包括"控件 ID 命名问题"的链接


## <a name="step-2-examining-the-renderedidchanges"></a>第 2 步： 检查呈现`ID`更改

若要更好地了解修改 ASP.NET 向呈现引擎发出`id`的服务器的值控制，让我们添加到的几个 Web 控件`IDIssues.aspx`页面，然后查看发送到浏览器的呈现的标记。 具体而言，在文本中的类型"请输入你的年龄:"跟文本框中 Web 控件。 进一步向下的页上添加按钮 Web 控件和标签 Web 控件。 设置文本框的`ID`和`Columns`属性设置为`Age`和 3，分别。 设置按钮的`Text`和`ID`属性设置为"提交"和`SubmitButton`。 清理标签的`Text`属性并设置其`ID`到`Results`。

此时内容控件的声明性标记应类似以下：


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

图 3 显示的页时查看通过 Visual Studio 设计器。


[![该页面包括三个 Web 控件： 文本框、 按钮和标签](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**图 03**: 页面包括三个 Web 控件： 文本框、 按钮和标签 ([单击以查看实际尺寸的图像](control-id-naming-in-content-pages-cs/_static/image5.png))


访问通过浏览器页面，然后查看 HTML 源。 下面所示，标记为`id`文本框、 按钮和标签 Web 控件的 HTML 元素的有效值的组合`ID`的 Web 控件的值与`ID`页中的命名容器的值。


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

本教程中前面所述，母版页和其 ContentPlaceHolders 用作命名容器。 因此，同时参与呈现`ID`其嵌套的控件的值。 需要文本框的`id`特性，例如： `ctl00_MainContent_Age`。 回想一下，TextBox 控件`ID`值`Age`。 这其 ContentPlaceHolder 控件使用的前缀`ID`值， `MainContent`。 此外，此值的前面带母版页的`ID`值， `ctl00`。 净效果是`id`组成的属性值`ID`主控页、 ContentPlaceHolder 控件和文本框本身的值。

图 4 显示了此行为。 若要确定呈现`id`的`Age`文本框中，使用启动`ID`文本框控件中，值`Age`。 接下来，工作沿控制层次结构。 在每个命名容器 （具有桃色颜色的节点），前缀呈现当前`id`与命名容器`id`。


![Rendered id 属性是基于上 ID 值的命名容器](control-id-naming-in-content-pages-cs/_static/image6.png)

**图 04**: 呈现`id`特性都是基于`ID`的命名容器的值


> [!NOTE]
> 如我们所述，`ctl00`部分呈现`id`属性构成`ID`值的主页上，但你可能想知道如何将此`ID`在于生成的值。 我们未指定其任何位置中我们 master 或内容的页面。 页面的声明性标记通过显式添加 ASP.NET 页中的大多数服务器控件。 `MainContent` ContentPlaceHolder 控件的标记中显式指定`Site.master`;`Age`文本框中已定义`IDIssues.aspx`的标记。 我们可以指定`ID`控件通过属性窗口，或从声明性语法，这些类型的值。 其他控件，如即母版页本身，未定义的声明性标记。 因此，其`ID`值必须为我们自动生成。 ASP.NET 引擎集`ID`在其 Id 具有未显式设置这些控件的运行时的值。 它使用的命名模式`ctlXX`，其中*XX*是按顺序递增的整数值。


主页面上本身充当作为命名的容器，因为在母版页中定义的 Web 控件还具有更改呈现`id`属性值。 例如，`DisplayDate`我们将添加到母版页中的标签[*母版页中使用创建站点范围布局*](creating-a-site-wide-layout-using-master-pages-cs.md)教程具有以下呈现标记：


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

请注意，`id`属性包括这两个母版页的`ID`值 (`ctl00`) 和`ID`标签 Web 控件的值 (`DateDisplay`)。

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>步骤 3： 以编程方式引用通过 Web 控件`FindControl`

每个 ASP.NET 服务器控件包含`FindControl("controlID")`搜索名为的控件的控件的后代方法*controlID*。 如果找到这样的控件，则返回;如果不找到任何匹配的控件，则`FindControl`返回`null`。

`FindControl`在其中你需要访问的控件，但不会获得对它的直接引用的方案很有用。 使用 Web 控件，例如 GridView，例如，数据时在 GridView 的字段控件中定义了一次的声明性语法，但是在运行时控件的实例创建的每个 GridView 行。 因此，在运行时生成的控件存在，但我们不提供可从代码隐藏类的直接引用。 因此我们需要使用`FindControl`以编程方式使用特定控件在 GridView 的字段。 (有关详细信息使用`FindControl`若要访问的数据 Web 控件模板中的控件，请参阅[自定义格式设置基于时数据](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md)。)这同一情况下发生时动态地将 Web 控件添加到 Web 窗体，则主题所述[创建动态数据条目的用户界面](https://msdn.microsoft.com/library/aa479330.aspx)。

若要演示如何使用`FindControl`方法搜索控件在内容页中，创建的事件处理程序`SubmitButton`的`Click`事件。 事件处理程序中，添加以下代码，即在以编程方式引用`Age`文本框中和`Results`标签使用`FindControl`方法，然后显示一条消息采用`Results`根据用户的输入。

> [!NOTE]
> 当然，我们不需要使用`FindControl`以引用此示例中的标签和文本框控件。 我们无法引用它们直接通过其`ID`属性值。 我使用`FindControl`此处为了说明使用时，会发生什么情况`FindControl`从内容页。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

尽管用于调用的语法`FindControl`方法略有不同的前两个行中`SubmitButton_Click`，它们在语义上等效。 回想一下，所有 ASP.NET 服务器控件都包括`FindControl`方法。 这包括`Page`从哪些所有 ASP.NET 代码隐藏类必须派生自的类。 因此，调用`FindControl("controlID")`等效于调用`Page.FindControl("controlID")`，假定你尚未重写`FindControl`方法在你的代码隐藏类或中的自定义的基类。

在输入此代码，请访问`IDIssues.aspx`通过浏览器页上，输入你的年龄，然后单击"提交"按钮。 单击"提交"按钮时`NullReferenceException`引发 （请参见图 5）。


[![引发 NullReferenceException](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**图 05**: A`NullReferenceException`引发 ([单击以查看实际尺寸的图像](control-id-naming-in-content-pages-cs/_static/image9.png))


如果在中设置断点`SubmitButton_Click`事件处理程序，你将看到同时调用`FindControl`返回`null`值。 `NullReferenceException`我们尝试访问时，将引发`Age`文本框的`Text`属性。

问题在于`Control.FindControl`仅搜索*控件*的后代的*相同的命名容器中*。 因为主控页构成一个新的命名容器，调用`Page.FindControl("controlID")`永远不会 permeates 母版页对象`ctl00`。 (图 4，若要查看控件层次结构，其中显示了将回指`Page`对象作为母版页对象的父级`ctl00`。)因此，`Results`标签和`Age`找不到文本框中和`ResultsLabel`和`AgeTextBox`为其赋值的`null`。

有两个对此质询的解决方法： 我们可以向下钻取，一个命名的容器，到相应的控件; 次或者我们可以创建我们自己`FindControl`permeates 命名容器的方法。 让我们检验其中每个选项。

### <a name="drilling-into-the-appropriate-naming-container"></a>钻取到相应的命名容器

若要使用`FindControl`引用`Results`标签或`Age`文本框中，我们需要先调用`FindControl`从相同的命名容器中的一个上级控件。 如图 4 显示， `MainContent` ContentPlaceHolder 控件是唯一的祖先`Results`或`Age`，位于相同的命名容器内。 换而言之，调用`FindControl`方法从`MainContent`控件，如下面的代码段中所示正确返回的引用`Results`或`Age`控件。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

但是，我们无法使用`MainContent`ContentPlaceHolder 从使用上面的语法，因为在母版页中定义 ContentPlaceHolder 我们内容页的代码隐藏类。 相反，我们必须使用`FindControl`获取对引用`MainContent`。 中的代码替换`SubmitButton_Click`事件处理程序替换以下修改之处：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

如果你访问通过浏览器页面，输入你的年龄，然后单击"提交"按钮，`NullReferenceException`引发。 如果在中设置断点`SubmitButton_Click`你将看到尝试调用时，会发生此异常的事件处理程序`MainContent`对象的`FindControl`方法。 `MainContent`对象是`null`因为`FindControl`方法找不到名为"主内容"的对象。 基础的原因是与相同`Results`标签和`Age`TextBox 控件：`FindControl`从控件层次结构的顶部启动其搜索，并且不入侵命名容器，但`MainContent`ContentPlaceHolder 是在主页上，这是命名的容器。

我们可以使用之前`FindControl`获取对引用`MainContent`，我们首先需要对母版页控件的引用。 一旦我们有了对主控页的引用我们可以获取对引用`MainContent`通过 ContentPlaceHolder`FindControl`并据此，引用`Results`标签和`Age`文本框中 (同样，通过使用`FindControl`)。 但是，我们如何获取对主控页的引用？ 通过检查`id`中呈现的标记的属性很显然，母版页的`ID`值是`ctl00`。 因此，我们可以使用`Page.FindControl("ctl00")`若要获取对主控页的引用，然后使用该对象获取对引用`MainContent`，依次类推。 下面的代码段说明了此逻辑：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

尽管此代码当然也可以生效，它假定母版页的自动生成`ID`将始终为`ctl00`。 很难做出有关自动生成值的假设一个好办法。

幸运的是，对主控页的引用是可通过访问`Page`类的`Master`属性。 因此，而不必使用`FindControl("ctl00")`来获取主控页的引用，以便访问`MainContent`ContentPlaceHolder，我们可以改用`Page.Master.FindControl("MainContent")`。 更新`SubmitButton_Click`事件处理程序替换为以下代码：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

此时，访问该网页通过浏览器中，输入你的年龄，并单击"提交"按钮显示中的消息`Results`标签，按预期方式。


[![标签中显示用户的年龄](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**图 06**： 标签中显示的用户的年龄 ([单击以查看实际尺寸的图像](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>以递归方式搜索命名容器

原因引用前面的代码示例`MainContent`ContentPlaceHolder 控件从主页上，然后`Results`标签和`Age`文本框控件从`MainContent`，是因为`Control.FindControl`方法仅搜索在*控件*的命名容器。 具有`FindControl`命名容器中的保持在大多数情况下有必要，因为两个不同的命名容器中的两个控件可能具有相同`ID`值。 请考虑一个 GridView，定义一个名为的标签 Web 控件的大小写`ProductName`在其 TemplateFields 之一中。 当将数据绑定到在运行时，GridView`ProductName`会为每个 GridView 行创建标签。 如果`FindControl`搜索通过所有命名容器，我们调用`Page.FindControl("ProductName")`，哪些标签实例应`FindControl`返回？ `ProductName`中第一个 GridView 行标签？ 中的最后一行的一个？

因此具有`Control.FindControl`只搜索*控件*的命名容器有意义在大多数情况下。 但有其他情况下，如面向我们，我们具有一个唯一的那个`ID`所有命名容器，并且想要避免必须认真引用控件层次结构访问的控件中每个命名容器。 具有`FindControl`以递归方式搜索所有命名容器使好的效果，太的变体。 遗憾的是，.NET Framework 不包括这样的方法。

好消息是，我们可以创建我们自己`FindControl`方法，以递归方式搜索所有命名容器。 事实上，使用*扩展方法*我们可以添加`FindControlRecursive`方法`Control`类随附其现有`FindControl`方法。

> [!NOTE]
> 扩展方法是对 C# 3.0 和 Visual Basic 9 附带的.NET Framework 版本 3.5 和 Visual Studio 2008 语言新功能。 简单地说，扩展方法允许开发人员可以创建一个特殊的语法通过现有的类类型的新方法。 有关此非常有用的功能的详细信息，请参阅我文章，[的扩展方法扩展基类型的功能](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)。


若要创建扩展方法，添加新的文件与`App_Code`文件夹名为`PageExtensionMethods.cs`。 添加名为的扩展方法`FindControlRecursive`作为输入采用`string`参数名为`controlID`。 若要正常工作的扩展方法，这至关重要类本身和其扩展方法来标记`static`。 此外，必须接受所有扩展方法，如它们的扩展方法适用的类型的对象的第一个参数和此输入的参数必须前面带有关键字`this`。

以下代码添加到`PageExtensionMethods.cs`类文件以定义此类与`FindControlRecursive`扩展方法：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

此代码中的位置，返回到`IDIssues.aspx`页的代码隐藏类并注释掉当前`FindControl`方法调用。 将它们替换为对调用`Page.FindControlRecursive("controlID")`。 关于扩展方法是，它们显示直接内的 IntelliSense 下拉列表。 如图 7 所示，当你键入页，然后点击期间， `FindControlRecursive` IntelliSense 以及其他的下拉列表中包含方法`Control`类方法。


[![在 IntelliSense 下拉列表包含扩展方法](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**图 07**： 在 IntelliSense 下拉列表包含扩展方法 ([单击以查看实际尺寸的图像](control-id-naming-in-content-pages-cs/_static/image15.png))


输入下面的代码插入`SubmitButton_Click`事件处理程序，然后通过访问的页面，输入你的年龄，然后单击"提交"按钮对其进行测试。 返回在图 6 中所示，则生成的输出将为消息，"就年龄岁 ！"


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> 由于扩展方法不熟悉 C# 3.0 和 Visual Basic 9，如果你使用的 Visual Studio 2005 无法使用扩展方法。 相反，你将需要实现`FindControlRecursive`的帮助器类方法。 [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx)在他的博客文章，具有这样一个示例[ASP.NET 微波激射器页和`FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx)。


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>步骤 4： 使用正确`id`属性在客户端脚本中的值

本教程的简介中所述，Web 控件的呈现`id`属性通常用于在客户端脚本中以编程方式引用特定的 HTML 元素。 例如，以下 JavaScript 引用 HTML 元素由其`id`然后模式的消息框中显示其值：


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

回想一下，在 ASP.NET 页，不包括命名容器，呈现的 HTML 元素的`id`属性等同于 Web 控件的`ID`属性值。 因此，很容易让人想到中的硬编码到`id`到 JavaScript 代码的属性值。 也就是说，如果你知道你想要访问`Age`文本框中 Web 控制通过客户端脚本，这样通过调用`document.getElementById("Age")`。

此方法的问题是，如果使用母版页 （或其他命名的容器控件），呈现的 HTML`id`不是同义词，Web 控件在`ID`属性。 第一个倾角可能访问通过浏览器页并查看源代码来确定实际`id`属性。 一旦你了解了呈现`id`值，你可以将其粘贴到调用`getElementById`访问需要客户端脚本通过使用 HTML 元素。 这种方法是不理想，因为该页面的某些更改控制层次结构或更改为`ID`命名控件的属性将 alter 生成`id`属性，就突破 JavaScript 代码。

好消息是`id`呈现的属性值是可在通过 Web 控件的服务器端代码中访问[`ClientID`属性](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)。 你应使用此属性确定`id`属性在客户端脚本中使用的值。 例如，若要添加到页面的 JavaScript 函数，当调用时，显示的值`Age`模式在消息框中的文本框中添加以下代码到`Page_Load`事件处理程序：


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

上面的代码插入的值`Age`文本框的 ClientID 属性转换为的 JavaScript 调用成`getElementById`。 如果您访问此页面，通过浏览器并查看 HTML 源，你将找到以下 JavaScript 代码：


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

请注意如何正确`id`属性值， `ctl00_MainContent_Age`，对的调用中将显示`getElementById`。 因为此值在运行时计算的所以它适用无论页面控件层次结构的更高版本更改。

> [!NOTE]
> 此 JavaScript 示例只是演示如何添加正确引用呈现服务器控件的 HTML 元素的 JavaScript 函数。 若要使用此函数将需要创作其他 JavaScript 加载文档时或在某些特定用户执行任何操作调查时调用该函数。 有关这些详细信息和相关的主题，阅读[使用客户端脚本](https://msdn.microsoft.com/library/aa479302.aspx)。


## <a name="summary"></a>摘要

某些 ASP.NET 服务器控件充当命名容器，这会影响呈现`id`属性及其子代控件值以及通过 canvassed 的控件的作用域`FindControl`方法。 关于主页面，即母版页本身和其 ContentPlaceHolder 控件命名容器。 因此，我们需要将放规定多做一些工作以编程方式引用内内容页使用的控件`FindControl`。 在本教程中，我们探讨了两种方法： 钻入 ContentPlaceHolder 控制和调用其`FindControl`方法; 和滚动我们自己`FindControl`实现该以递归方式搜索通过所有的命名容器。

除了命名容器引入与引用 Web 控件有关的服务器端问题，还有一些客户端问题。 没有命名容器，Web 控件`ID`属性值和呈现`id`属性值都是一个在同一个。 但命名的容器，呈现增加`id`属性同时包含`ID`的 Web 控件和在其控件层次结构的祖先命名容器的值。 只要你使用 Web 控件的这些命名的内容不在非问题`ClientID`属性来确定呈现`id`属性在客户端脚本中的值。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET 母版页和`FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [创建动态数据条目的用户界面](https://msdn.microsoft.com/library/aa479330.aspx)
- [扩展的扩展方法的基类型功能](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [如何： 引用 ASP.NET 母版页页内容](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [母版页页： 提示、 技巧和陷阱](http://www.odetocode.com/articles/450.aspx)
- [使用客户端脚本](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多个 ASP/ASP.NET 丛书和 4GuysFromRolla.com 创始人，具有已使用 Microsoft Web 技术自 1998 年。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 3.5 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 可以在达到 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Zack Jones 和 Suchi Barnerjee。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一页](urls-in-master-pages-cs.md)
[下一页](interacting-with-the-master-page-from-the-content-page-cs.md)
