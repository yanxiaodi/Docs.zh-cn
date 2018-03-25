---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: "在数据 Web 中显示二进制数据控制 (VB) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们查看的选项显示在网页上，包括图像文件的显示和的下载链接 f 预配的二进制数据..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: df79748bf5734ffcb9eb81ca089aeded0e63bdc5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>在数据 Web 控件 (VB) 中显示二进制数据
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe)或[下载 PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> 在本教程中我们查看一下在网页上，包括显示的图像文件和 PDF 文件的下载链接设置上显示二进制数据的选项。


## <a name="introduction"></a>介绍

在前面的教程，我们探讨了用于将二进制数据与应用程序 s 基础数据模型相关联的两个技术和 FileUpload 控件用于从浏览器文件上载到 web 服务器的文件系统。 我们遇到尚未若要了解如何将上载的二进制数据与数据模型相关联。 也就是说，上载文件并将其保存到文件系统后，文件的路径必须存储在相应的数据库记录。 如果正在直接在数据库中存储数据，上载的二进制数据不需要保存到文件系统，但必须插入到数据库。

我们将看看将数据与数据模型相关联之前，不过，让我们来首先看一下如何向最终用户提供的二进制数据。 提供文本数据非常简单，但如何二进制数据将显示？ 它取决于，当然，二进制数据的类型。 对于映像，我们可能想要显示图像;对于 pdf 文件，Microsoft Word 文档、 ZIP 文件和其他类型的二进制数据，提供下载链接是可能更合适。

在本教程中我们将探讨如何提供与一起使用的数据及其关联的文本数据的二进制数据 Web 控制如 GridView 和说明。 在下一教程中我们将着重将上载的文件与数据库相关联。

## <a name="step-1-providingbrochurepathvalues"></a>步骤 1： 提供`BrochurePath`值

`Picture`中的列`Categories`表已包含二进制数据的各种类别映像。 具体而言，`Picture`为每个记录的列包含粒状、 质量较低的、 16 颜色位图图像的二进制内容。 每个类别图像为 172 像素、 高宽和 120 像素，并使用大致 11 KB。 新增功能详细、 中的二进制内容`Picture`列包括 78 字节[OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding)必须之前显示的图像中提取出来的标头。 因为 Northwind 数据库存在其根在 Microsoft Access 中，此标头信息不存在。 在 Access 中，使用此标头采取的 OLE 对象数据类型存储二进制数据。 现在，我们将了解如何以显示该图片数据包从这些质量较低的映像的标头。 在将来的教程中我们生成了一个接口，使更新类别的`Picture`列，并将这些 OLE 标头使用的不必要的 OLE 标头没有等效 JPG 图像的位图图像。

在前面的教程中，我们已了解如何使用 FileUpload 控件。 因此，你可以继续，并将小册子文件添加到 web 服务器的文件系统。 这样做，但是，不会更新`BrochurePath`中的列`Categories`表。 在下一步的教程，我们将了解如何实现此目的，但现在，我们需要手动为此列提供值。

在此教程的下载你会发现中的七个 PDF 小册子文件`~/Brochures`文件夹中，一个用于每个数据项除类别。 我有意省略添加数据项小册子说明如何处理具有关联并不是所有记录的二进制数据的方案。 若要更新`Categories`表具有以下值中，右键单击`Categories`节点从服务器资源管理器，然后选择显示表数据。 然后，输入具有小册子中，如图 1 所示的每个类别的小册子文件的虚拟路径。 由于没有为数据项类别没有小册子，将保留其`BrochurePath`s 形式列值`NULL`。


[![手动输入类别表的 BrochurePath 列的值](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**图 1**： 手动输入的值`Categories`表 s`BrochurePath`列 ([单击以查看实际尺寸的图像](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>步骤 2： 为在 GridView 小册子中提供的下载链接

与`BrochurePath`为提供的值`Categories`表，我们重新已准备好创建一个 GridView，列出每个类别以及用于下载类别的小册子中的链接。 在步骤 4 中，我们将扩展此 GridView 还显示类别的图像。

通过从工具箱中拖动到设计器的拖动一个 GridView 启动`DisplayOrDownloadData.aspx`页面`BinaryData`文件夹。 设置 GridView s`ID`到`Categories`GridView s 智能标记，通过选择将其绑定到新的数据源。 具体而言，将其绑定到名为 ObjectDataSource`CategoriesDataSource`检索数据使用`CategoriesBLL`对象的`GetCategories()`方法。


[![创建名为 CategoriesDataSource 新对象数据源](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**图 2**： 创建新对象数据源命名`CategoriesDataSource`([单击以查看实际尺寸的图像](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![配置对象数据源以使用 CategoriesBLL 类](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**图 3**： 配置使用 ObjectDataSource`CategoriesBLL`类 ([单击以查看实际尺寸的图像](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![检索类别使用 GetCategories() 方法的列表](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**图 4**： 检索列表的类别使用`GetCategories()`方法 ([单击以查看实际尺寸的图像](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


完成配置数据源向导后，Visual Studio 将自动添加到 BoundField`Categories`的 GridView `CategoryID`， `CategoryName`， `Description`， `NumberOfProducts`，和`BrochurePath` `DataColumn` s。 请继续并删除`NumberOfProducts`自 BoundField`GetCategories()`方法的查询未检索此信息。 此外会删除`CategoryID`BoundField 和重命名`CategoryName`和`BrochurePath`BoundFields`HeaderText`为类别和小册子中，属性分别。 进行这些更改后，你 GridView 和 ObjectDataSource s 声明性标记应如下所示：


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

查看此页通过浏览器 （请参见图 5）。 列出每个八个类别。 使用七种类别`BrochurePath`值具有`BrochurePath`各自 BoundField 中显示的值。 数据项，具有`NULL`值，则为其`BrochurePath`，将显示空单元格。


[![列出每个类别的名称、 说明和 BrochurePath 值](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**图 5**： 每个类别的名称、 说明，和`BrochurePath`列出值 ([单击以查看实际尺寸的图像](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


而不是显示的文本`BrochurePath`列中，我们想要创建指向小册子中的链接。 若要完成此操作，请删除`BrochurePath`BoundField 并将其替换 HyperLinkField。 设置新的 HyperLinkField s`HeaderText`属性小册子中，其`Text`属性视图小册子中，并将其`DataNavigateUrlFields`属性`BrochurePath`。


![为 BrochurePath 添加 HyperLinkField](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**图 6**： 添加为 HyperLinkField`BrochurePath`


如图 7 所示，这将添加到 GridView，一列链接。 单击视图小册子链接将在浏览器中直接显示 PDF，或者提示用户下载的文件，具体取决于是否安装了 PDF 阅读器和浏览器的设置。


[![可以通过单击查看小册子链接查看类别的手册](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**图 7**： 通过单击查看小册子链接的类别 s 可以查看小册子 ([单击以查看实际尺寸的图像](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![显示类别的小册子 PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**图 8**: The Category s 显示小册子 PDF ([单击以查看实际尺寸的图像](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>隐藏视图小册子文本，而无需小册子类别

如图 7 所示， `BrochurePath` HyperLinkField 显示其`Text`属性值 （视图小册子） 的所有记录，而不管是否那里 s 非`NULL`值`BrochurePath`。 当然，如果`BrochurePath`是`NULL`，则链接将显示为纯文本是与数据项类别如此 （回头参考图 7 中）。 而不是显示的文本视图小册子，可能很棒而无需这些类别`BrochurePath`值显示一些备用文本，如没有小册子可用。

为了提供此行为，我们需要使用其内容生成通过对发出相应的输出基于页方法的调用为 TemplateField`BrochurePath`值。 我们首先介绍了此格式设置方法返回[GridView 控件中使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)教程。

选择将转换为 TemplateField HyperLinkField `BrochurePath` HyperLinkField，再单击转换此域转换为 TemplateField 链接在编辑列对话框中。


![将 HyperLinkField 转换转换为 TemplateField](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**图 9**: HyperLinkField 转换为 TemplateField


这将创建与 TemplateField`ItemTemplate`包含超链接 Web 控件`NavigateUrl`属性绑定到`BrochurePath`值。 将此标记替换为对方法的调用`GenerateBrochureLink`，并传递的值中`BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

接下来，创建`Protected`方法在 ASP.NET 页上 s 代码隐藏类名为`GenerateBrochureLink`返回`String`并接受`Object`作为输入参数。


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

此方法可确定如果传入的`Object`值是一个数据库`NULL`，并且如果是这样，则返回一个消息，表明类别缺少小册子。 否则为如果没有`BrochurePath`值，它显示超链接中的 s。 请注意，如果`BrochurePath`值是将其提交 s 传入[`ResolveUrl(url)`方法](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx)。 此方法会解析传入的*url*，并替换`~`字符与相应的虚拟路径。 例如，如果应用程序位于`/Tutorial55`，`ResolveUrl("~/Brochures/Meats.pdf")`将返回`/Tutorial55/Brochures/Meat.pdf`。

在应用这些更改后，图 10 显示的页。 请注意，数据项类别的`BrochurePath`字段现在显示的文本没有小册子可用。


[![显示有关这些类别而无需小册子文本没有小册子可用](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**图 10**： 为这些类别而无需小册子显示的文本没有小册子可用 ([单击以查看实际尺寸的图像](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>步骤 3： 添加网页上显示类别的图片

当用户访问 ASP.NET 页时，他们会收到 ASP.NET 页的 HTML。 收到的 HTML 只是文本，并且不包含任何二进制数据。 任何其他的二进制数据，如图像、 声音文件、 Macromedia Flash 应用程序、 嵌入式 Windows Media Player 视频和等，存在为 web 服务器上的单独资源。 HTML 包含对这些文件的引用，但不包括文件的实际内容。

例如，在 HTML`<img>`元素用来与引用图片，`src`指向图像文件的属性如下所示：


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

当浏览器收到此 HTML 时，它会发出到 web 服务器以检索图像文件，并将结果显示在浏览器中的二进制内容的另一个请求。 相同的概念适用于任何二进制数据。 在步骤 2 中，小册子中未发送到浏览器页面的 HTML 标记的一部分。 相反，呈现的 HTML 时，提供超链接，单击，导致浏览器直接请求 PDF 文档。

若要显示或允许用户下载位于数据库内的二进制数据，我们需要创建单独的 web 页面上，返回的数据。 为使应用程序，这里 s 只有一个直接存储在数据库类别的图片的二进制数据字段。 因此，我们需要一个页面，在调用时，返回特定类别的图像数据。

添加到新的 ASP.NET 页`BinaryData`文件夹名为`DisplayCategoryPicture.aspx`。 这样做，将保持为选择母版页复选框未选中状态。 此页需要`CategoryID`中查询字符串并返回该类别 s 的二进制数据值`Picture`列。 由于此页返回二进制数据和其他任何内容，因此它不需要任何 HTML 部分中的标记。 因此，请单击左下角的源选项卡上并删除所有除页面的标记`<%@ Page %>`指令。 也就是说， `DisplayCategoryPicture.aspx` s 声明性标记应包含单个行：


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

如果你看到`MasterPageFile`属性中`<%@ Page %>`指令，将其删除。

在页面的代码隐藏类中，添加以下代码`Page_Load`事件处理程序：


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

此代码启动通过读取`CategoryID`查询字符串值转换为一个名为变量`categoryID`。 接下来，通过调用检索图片数据`CategoriesBLL`类的`GetCategoryWithBinaryDataByCategoryID(categoryID)`方法。 此数据就会归还客户端通过使用`Response.BinaryWrite(data)`方法，但在此情况称作之前`Picture`必须删除列值的 OLE 标头。 这通过创建实现`Byte`名为数组`strippedImageData`，将存放精确 78 小于在中的字符`Picture`列。 [ `Array.Copy`方法](https://msdn.microsoft.com/library/z50k9bft.aspx)用于中的数据复制`category.Picture`位置 78 开始转移到`strippedImageData`。

`Response.ContentType`属性指定[MIME 类型](http://en.wikipedia.org/wiki/MIME)以便浏览器知道如何使其返回的内容。 由于`Categories`表的`Picture`列位图图像，使用位图 MIME 类型此处 (图像/bmp)。 如果省略的 MIME 类型，大多数浏览器仍将显示该图像正确因为它们可以推断基于图像文件 s 二进制数据的内容的类型。 但是，它比较明智的做法包括 MIME s 键入在可能的情况。 请参阅[Internet 号码分配机构的网站](http://www.iana.org/)有关的完整列表[MIME 媒体类型](http://www.iana.org/assignments/media-types/)。

使用创建此页上，可以通过访问查看特定类别的图片`DisplayCategoryPicture.aspx?CategoryID=categoryID`。 图 11 显示饮料类别的图中，可以从查看`DisplayCategoryPicture.aspx?CategoryID=1`。


[![图片显示饮料类别 s](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**图 11**: 饮料类别的图片显示 ([单击以查看实际尺寸的图像](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


如果是，在访问时`DisplayCategoryPicture.aspx?CategoryID=categoryID`，收到异常，并显示无法为类型的强制转换对象设置为 System.DBNull 类型 System.Byte []，这可能导致两件事情。 首先，`Categories`表 s`Picture`允许列`NULL`值。 `DisplayCategoryPicture.aspx`页上，但是，假定有一个非`NULL`存在的值。 `Picture`属性`CategoriesDataTable`如果它具有不能直接对其进行访问`NULL`值。 如果你想要允许`NULL`值`Picture`列，d 你想要包括满足以下条件：


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

上面的代码假定某些映像名为的文件： s`NoPictureAvailable.gif`中`Images`想要显示无图片这些类别的文件夹。

此外会引发此异常，如果`CategoriesTableAdapter`s`GetCategoryWithBinaryDataByCategoryID`方法的`SELECT`语句已恢复回主查询的列列表中，如果你正在使用临时 SQL 语句并且你已重新运行该向导 TableAdapter s 可以发生这种情况主查询。 检查以确保`GetCategoryWithBinaryDataByCategoryID`方法 s`SELECT`语句仍包含`Picture`列。

> [!NOTE]
> 每次`DisplayCategoryPicture.aspx`是访问时，将访问该数据库并返回指定的类别的图片数据。 如果自上次具有查看更改类别的图片做 t，不过，这是浪费的工作量。 幸运的是，HTTP 允许*条件获取*。 条件性 GET 发出 HTTP 请求的客户端中将发送沿[ `If-Modified-Since` HTTP 标头](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)，它提供的日期和时间客户端上次从 web 服务器检索此资源。 如果内容后未发生更改这指定日期，web 服务器可能响应与[不会修改状态代码 (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)并且放弃发回的请求的资源的内容。 简单地说，此技术缓解无需发送内容的资源，如果它尚未修改由于客户端上次访问的 web 服务器。


若要实现此行为，但是，要求你添加`PictureLastModified`列`Categories`表捕获时`Picture`和代码以检查上次更新列`If-Modified-Since`标头。 有关详细信息`If-Modified-Since`标头和条件 GET 工作流，请参阅[HTTP 条件 GET RSS 黑客](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers)和[更深入地看一下 ASP.NET 页中执行 HTTP 请求](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx)。

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>步骤 4： 在一个 GridView 中显示的类别图片

现在，我们已网页上要显示的特定类别的图片，我们可以使用显示[映像 Web 控件](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx)或 HTML`<img>`元素指向`DisplayCategoryPicture.aspx?CategoryID=categoryID`。 可以在 GridView 或说明如何使用 ImageField 中显示其 URL 将由数据库数据的映像。 ImageField 包含`DataImageUrlField`和`DataImageUrlFormatString`工作方式与 HyperLinkField s 类似的属性`DataNavigateUrlFields`和`DataNavigateUrlFormatString`属性。

让我们来增加`Categories`中的 GridView`DisplayOrDownloadData.aspx`通过添加 ImageField 若要显示每个类别的图片。 只需添加 ImageField 并设置其`DataImageUrlField`和`DataImageUrlFormatString`属性设置为`CategoryID`和`DisplayCategoryPicture.aspx?CategoryID={0}`分别。 这将创建将呈现的 GridView 列`<img>`元素其`src`属性引用`DisplayCategoryPicture.aspx?CategoryID={0}`、 {0} 将替换 GridView 行的`CategoryID`值。


![将 ImageField 添加到 GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**图 12**: ImageField 添加 GridView


在添加 ImageField 之后, 你 GridView s 声明性语法应类似 soothe 以下：


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

花些时间查看通过浏览器的此页。 请注意如何每条记录现在包括为类别图片。


[![为每个行显示类别的图片](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**图 13**: The Category 的图片显示每个行 ([单击以查看实际尺寸的图像](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>摘要

在本教程中，我们将探讨如何显示二进制数据。 数据显示方式取决于数据的类型。 对于 PDF 小册子文件中，我们向用户提供视图小册子链接，单击时，用户直接到达 PDF 文件。 类别的图片，我们首先创建一个页面来检索，从数据库中返回的二进制数据，并且随后用于该页面在一个 GridView 中显示每个类别的图片。

现在我们已介绍了如何显示二进制数据，我们重新已准备好检查如何执行插入、 更新和删除针对包含二进制数据的数据库。 在下一教程中，我们将了解如何将已上载的文件与其相应的数据库记录相关联。 在这之后的教程中，我们将了解如何更新现有的二进制数据，以及如何删除其相关联的记录时删除的二进制数据。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨和 Dave Gardner。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](uploading-files-vb.md)
[下一页](including-a-file-upload-option-when-adding-a-new-record-vb.md)
