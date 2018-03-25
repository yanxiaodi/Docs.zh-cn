---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: "上载文件 (VB) |Microsoft 文档"
author: rick-anderson
description: "了解如何允许用户将 （如 Word 或 PDF 文档） 的二进制文件上载到其中就可能存储在服务器的文件系统网站..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: 69586ade54a40aabb55dd507731a6c2820774c04
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="uploading-files-vb"></a>上载文件 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe)或[下载 PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> 了解如何允许用户将 （如 Word 或 PDF 文档） 的二进制文件上载到其中可能将它们存储在服务器的文件系统或数据库网站。


## <a name="introduction"></a>介绍

所有这些教程我们以独占方式使用文本数据过已检查到目前为止。 但是，许多应用程序必须捕获文本和二进制数据的数据模型。 Online 的约会网站可能允许用户上载图片，以将其配置文件与相关联。 招聘网站可能允许用户上载其恢复为 Microsoft Word 或 PDF 文档。

处理二进制数据将添加新一轮挑战。 我们必须确定应用程序中存储二进制数据的方式。 用于插入新记录的接口必须更新以允许用户从他们的计算机上载文件，并且必须采取额外步骤来显示或提供下载记录 s 的一种方法关联的二进制数据。 本教程中的下一步的三个，我们将探讨如何 hurdle 这些难题。 这些教程末尾我们将生成的完全正常运行的应用程序将图片和 PDF 小册子与每个类别相关联。 在本特定教程中，我们将查看不同的技术来存储二进制数据并探索如何使用户能够上载从其计算机的文件，它保存在 web 服务器的文件系统上。

> [!NOTE]
> 是应用程序的数据模型的一部分的二进制数据有时称为[BLOB](http://en.wikipedia.org/wiki/Binary_large_object)，二进制大型对象的首字母缩写。 这些教程中我已选择要使用的术语的二进制数据，虽然此术语 BLOB 为同义词。


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>步骤 1： 使用二进制数据 Web 页创建工作

我们开始浏览与添加对二进制数据的支持的难题之前，让我们来首先花一些时间，我们需要以及本教程的下一步的三个网站项目中创建 ASP.NET 页。 首先，通过添加一个名为的新文件夹`BinaryData`。 接下来，添加到该文件夹，并确保将每个包含网页相关联的以下 ASP.NET 页面`Site.master`母版页：

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![为与数据相关的二进制教程添加 ASP.NET 页](uploading-files-vb/_static/image1.gif)

**图 1**： 添加 ASP.NET 页的二进制数据相关的教程


在其他文件夹中，如`Default.aspx`中`BinaryData`文件夹将在其部分中列出这些教程。 回想一下，`SectionLevelTutorialListing.ascx`用户控件提供此功能。 因此，此用户将控件添加到`Default.aspx`通过从解决方案资源管理器拖到页面上的设计视图拖动。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**图 2**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](uploading-files-vb/_static/image2.png))


最后，将这些页面添加到的条目为`Web.sitemap`文件。 具体而言，在提高后添加以下标记 GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 在左侧菜单现在包含二进制数据教程使用的项。


![站点图现在包括二进制数据教程使用的项](uploading-files-vb/_static/image3.gif)

**图 3**： 站点图现在包括二进制数据教程使用的项


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>步骤 2： 决定在何处存储二进制数据

与应用程序的数据模型关联的二进制数据可以存储在两个位置之一： 在 web 服务器的文件系统对存储在数据库中; 该文件的引用或直接在数据库本身内 （请参见图 4）。 每种方法具有自己的利与弊集和值得详细的讨论。


[![在文件系统上或直接在数据库中，可以存储二进制数据](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**图 4**： 在文件系统上或直接在数据库中，可以存储二进制数据 ([单击以查看实际尺寸的图像](uploading-files-vb/_static/image4.png))


假设我们想要扩展 Northwind 数据库，以将图片与每个 product 相关联。 一种方法是以这些映像将文件存储在 web 服务器的文件系统并记录中的路径`Products`表。 使用此方法，我们 d 添加`ImagePath`列`Products`表类型`varchar(200)`，可能是。 当用户牛奶上载图片时，可能会在 web 服务器的文件系统上存储该图片`~/Images/Tea.jpg`，其中`~`表示应用程序的物理路径。 也就是说，如果 web 站点所在的物理路径`C:\Websites\Northwind\`，`~/Images/Tea.jpg`将等效于`C:\Websites\Northwind\Images\Tea.jpg`。 在上载后的图像文件，我们 d 更新中的牛奶记录`Products`表，以便其`ImagePath`列引用新映像的路径。 我们无法使用`~/Images/Tea.jpg`或仅仅称为`Tea.jpg`如果我们决定所有产品图像将都置于应用程序的`Images`文件夹。

在文件系统上存储的二进制数据的主要优点是：

- **易于实现**我们将很快看到的如存储和检索二进制数据存储在数据库中直接涉及相比时使用通过文件系统的数据的更多代码。 此外，为了使用户可以查看或下载二进制数据中它们必须提供指向该数据的 url。 如果数据驻留在 web 服务器的文件系统，则 URL 将为简单。 如果在数据库中存储数据，但是，web 页上需要创建将检索数据并从数据库中返回数据。
- **对二进制数据的更宽访问**的二进制数据可能需要与其他服务或应用程序，无法提取数据库中的数据的那些可访问。 例如，与每个 product 相关联的映像可能还需要可供用户通过[FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol)，在这种情况下 d 我们想要将二进制数据存储在文件系统。
- **性能**如果二进制数据存储在文件系统中，数据库服务器和 web 服务器之间的需求和网络拥塞将小于如果直接在数据库内存储的二进制数据。

在文件系统上存储二进制数据的主要缺点是它将分离的数据库中的数据。 如果从删除记录`Products`表，web 服务器的文件系统上关联的文件不会自动删除。 我们必须编写额外代码来删除文件或文件系统将会变得杂乱的未使用的孤立文件。 此外，当备份数据库，我们必须确保在文件系统上进行关联的二进制数据的备份。 将数据库移动到另一个站点或服务器带来类似难题。

或者，二进制数据可以存储在 Microsoft SQL Server 2005 数据库中直接通过创建类型的列`varbinary`。 类似于其他可变长度数据类型，你可以指定可容纳的二进制数据的最大长度此列中。 例如，若要保留最多 5,000 个字节使用`varbinary(5000)`;`varbinary(MAX)`允许的最大存储大小，大约 2 GB。

直接在数据库中存储二进制数据的主要优点是二进制数据和数据库记录之间的紧密耦合。 这极大地简化了数据库管理任务，如备份或将数据库移到不同的站点或服务器。 此外，自动删除一条记录中删除相应的二进制数据。 也有更多的数据库中存储二进制数据的细微优势。 请参阅[存储二进制文件直接在数据库使用 ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx)有关的更深入讨论。

> [!NOTE]
> 在 Microsoft SQL Server 2000 和早期版本中，`varbinary`数据类型具有最大限制为 8000 个字节。 若要存储最多为 2 GB 的二进制数据[`image`数据类型](https://msdn.microsoft.com/library/ms187993.aspx)需要改为使用。 通过添加`MAX`在 SQL Server 2005，但是，`image`已弃用数据类型。 它 s 仍支持向后兼容性，但 Microsoft 已宣布`image`数据类型将删除 SQL Server 的未来版本中。


如果你正在使用的较旧的数据模型可能会看到`image`数据类型。 Northwind 数据库 s`Categories`表具有`Picture`可以用于存储为类别图像文件的二进制数据的列。 由于 Northwind 数据库具有其根在 Microsoft Access 和早期版本的 SQL Server 中，此列的类型是`image`。

对于本教程的下一步的三个，我们将使用这两种方法。 `Categories`表已`Picture`用于存储映像的类别的二进制内容的列。 我们将添加一个额外的列， `BrochurePath`，以用于提供打印质量的完美的类别概述的 web 服务器的文件系统上存储的路径为 PDF。

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>步骤 3： 添加`BrochurePath`列`Categories`表

当前类别表具有只需四个列： `CategoryID`， `CategoryName`， `Description`，和`Picture`。 除了这些字段中，我们需要添加一个 （如果存在），将指向类别的小册子中的新。 若要添加此列，请转到服务器资源管理器，向下钻取到表中，右键单击在上`Categories`表，然后选择打开表定义 （请参见图 5）。 如果看不到服务器资源管理器，使其从视图菜单中，选择的服务器资源管理器选项，或按 Ctrl + Alt + S。

添加新`varchar(200)`列`Categories`名为的表`BrochurePath`，并允许`NULL`s 并单击保存图标 （或按 Ctrl + S）。


[![将 BrochurePath 列添加到类别表](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**图 5**： 添加`BrochurePath`列`Categories`表 ([单击以查看实际尺寸的图像](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>步骤 4： 更新到使用体系结构`Picture`和`BrochurePath`列

`CategoriesDataTable`中数据访问层 (DAL) 当前具有四个`DataColumn`定义的 s: `CategoryID`， `CategoryName`， `Description`，和`NumberOfProducts`。 我们最初设计中的此 DataTable[创建数据访问层](../introduction/creating-a-data-access-layer-vb.md)教程中，`CategoriesDataTable`只有前三个列;`NumberOfProducts`中添加了列[主/从使用项目符号列表的详细信息 DataList Master 记录](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)教程。

中所述*创建数据访问层*，类型化数据集中数据表构成业务对象。 Tableadapter 负责与数据库通信，并包含查询结果将业务对象填充。 `CategoriesDataTable`由填充`CategoriesTableAdapter`，其中有三个数据检索方法：

- `GetCategories()`执行 TableAdapter s 主查询并返回`CategoryID`， `CategoryName`，和`Description`字段中的所有记录`Categories`表。 在主查询是通过自动生成的用途`Insert`和`Update`方法。
- `GetCategoryByCategoryID(categoryID)`返回`CategoryID`， `CategoryName`，和`Description`类别字段其`CategoryID`等于*categoryID*。
- `GetCategoriesAndNumberOfProducts()`-返回`CategoryID`， `CategoryName`，和`Description`中的所有记录的字段`Categories`表。 此外使用子查询返回的每个类别关联的产品数目。

请注意，不能提供的查询返回`Categories`表 s`Picture`或`BrochurePath`列; 也不会`CategoriesDataTable`提供`DataColumn`为这些字段的 s。 若要使用图和`BrochurePath`属性，我们需要先将其添加到`CategoriesDataTable`，然后更新`CategoriesTableAdapter`类以返回这些列。

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>添加`Picture`和`BrochurePath``DataColumn`s

首先，通过添加到这两列`CategoriesDataTable`。 右键单击`CategoriesDataTable`s 标头，从上下文菜单中选择添加，然后选择列选项。 这将创建一个新`DataColumn`位于名为 DataTable `Column1`。 该列重命名为`Picture`。 从属性窗口中，设置`DataColumn`s`DataType`属性`System.Byte[]`（这不是下拉列表中的一个选项; 你需要键入中）。


[![创建 DataColumn 名为图片其数据类型是 System.Byte]](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**图 6**： 创建`DataColumn`命名`Picture`其`DataType`是`System.Byte[]`([单击以查看实际尺寸的图像](uploading-files-vb/_static/image8.png))


添加另一个`DataColumn`到数据表，其命名为`BrochurePath`使用默认`DataType`值 (`System.String`)。

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>返回`Picture`和`BrochurePath`TableAdapter 中的值

与这两个`DataColumn`s 添加到`CategoriesDataTable`，我们已准备好更新重新`CategoriesTableAdapter`。 我们可以对这两个主要的 TableAdapter 查询中, 返回这些列的值，但此时将显示返回的二进制数据每次`GetCategories()`调用方法。 改为让 s 更新主要的 TableAdapter 查询，以使重新`BrochurePath`和创建的其他数据检索方法，返回特定类别的`Picture`列。

若要更新主 TableAdapter 查询，右键单击`CategoriesTableAdapter`s 标头，然后从上下文菜单中选择配置选项。 这将显示表适配器配置向导中，哪些我们已大量过去教程中所示。 更新查询以使重新`BrochurePath`并单击完成。


[![更新要还返回 BrochurePath 的 SELECT 语句中的列列表](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**图 7**： 更新中的列列表`SELECT`语句还返回`BrochurePath`([单击以查看实际尺寸的图像](uploading-files-vb/_static/image10.png))


当使用 TableAdapter 的临时 SQL 语句，更新主查询中的列列表的所有更新的列列表`SELECT`查询 TableAdapter 中的方法。 这意味着`GetCategoryByCategoryID(categoryID)`方法已更新，以返回`BrochurePath`列中，这可能是我们的预期。 但是，它还更新中的列列表`GetCategoriesAndNumberOfProducts()`方法，删除子查询返回的每个类别的产品数目 ！ 因此，我们需要更新此方法的`SELECT`查询。 右键单击`GetCategoriesAndNumberOfProducts()`方法中，选择配置，并还原`SELECT`回其原始值的查询：


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

接下来，创建一个新的 TableAdapter 方法返回的特定类别的`Picture`列的值。 右键单击`CategoriesTableAdapter`s 标头，然后选择添加查询选项以启动 TableAdapter 查询配置向导。 此向导的第一步会我们询问，是否我们要使用的临时 SQL 语句的查询数据，一个新存储过程或一个现有。 选择使用 SQL 语句，然后单击下一步。 由于我们将返回一行，选择第二个步骤中返回行选项选择。


[![选择使用 SQL 语句选项](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**图 8**： 选择使用 SQL 语句选项 ([单击以查看实际尺寸的图像](uploading-files-vb/_static/image12.png))


[![因为查询将从类别表返回一条记录，选择选择其返回行](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**图 9**： 因为查询将从类别表，选择选择返回行返回一条记录 ([单击以查看实际尺寸的图像](uploading-files-vb/_static/image14.png))


在第三个步骤中，输入以下 SQL 查询，然后单击下一步:


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

最后一步是选择新的方法的名称。 使用`FillCategoryWithBinaryDataByCategoryID`和`GetCategoryWithBinaryDataByCategoryID`填充 DataTable 并返回数据表模式，分别。 单击完成以完成向导。


[![选择的名称的 TableAdapter 的方法](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**图 10**： 选择 TableAdapter 的方法的名称 ([单击以查看实际尺寸的图像](uploading-files-vb/_static/image16.png))


> [!NOTE]
> 表适配器查询配置向导完成后可能会看到一个对话框，通知你，新的命令文本返回架构的数据不同于主查询的架构。 该向导简单地说，注意，TableAdapter s 主查询`GetCategories()`返回比我们刚刚创建的一个不同的架构。 但这是我们希望的因此你可以忽略此消息。


此外，请记住，如果你正在使用临时 SQL 语句，并且使用向导来更改时间的 TableAdapter s 在后面某个时间点的主查询，它将修改`GetCategoryWithBinaryDataByCategoryID`方法的`SELECT`语句的列列表，使其包含仅在这些列从主查询 (即，它将删除`Picture`从查询的列)。 你将需要手动更新列列表以返回`Picture`列，类似于我们所做的与`GetCategoriesAndNumberOfProducts()`之前在此步骤中的方法。

添加两个后`DataColumn`到`CategoriesDataTable`和`GetCategoryWithBinaryDataByCategoryID`方法`CategoriesTableAdapter`，这些类型化数据集设计器中的类应如下所示图 11 中的屏幕快照。


![数据集设计器包含的新列和方法](uploading-files-vb/_static/image11.gif)

**图 11**： 数据集设计器包含的新列和方法


## <a name="updating-the-business-logic-layer-bll"></a>更新业务逻辑层 (BLL)

与更新 DAL，剩下的就是来加强业务逻辑层 (BLL) 以囊括新的方法`CategoriesTableAdapter`方法。 添加以下方法`CategoriesBLL`类：


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>步骤 5： 上载到 Web 服务器从客户端文件

当收集二进制数据，通常由最终用户提供此数据。 若要捕获此信息，用户需要能将上载到 web 服务器从其计算机的文件。 上载的数据则需要与数据模型中，这可能意味着将文件保存到 web 服务器的文件系统和将路径添加到在数据库中，文件或直接在数据库中编写的二进制内容集成。 在此步骤中我们将查看如何允许用户从他们的计算机文件上载到服务器。 在下一教程中我们将着重将上载的文件与数据模型相集成。

ASP.NET 2.0 的新增功能[FileUpload Web 控件](https://msdn.microsoft.com/library/ms227677(VS.80).aspx)提供用于将文件从其计算机发送到 web 服务器的用户的机制。 FileUpload 控件呈现为`<input>`元素其`type`属性设置为文件，浏览器显示为带有浏览按钮的文本框。 单击浏览按钮将显示一个对话框，用户可以从中选择一个文件。 窗体回发，所选的文件的内容发送以及回发。 在服务器端上载的文件的信息是通过 FileUpload 控件的属性可访问。

若要演示上载文件，打开`FileUpload.aspx`页面`BinaryData`文件夹中，从工具箱中拖动到设计器中，拖动 FileUpload 控件并设置控制 s`ID`属性`UploadTest`。 接下来，添加按钮 Web 控件设置其`ID`和`Text`属性设置为`UploadButton`并分别上载所选文件。 最后，将在按钮下方的标签 Web 控件放清除其`Text`属性并设置其`ID`属性`UploadDetails`。


[![将 FileUpload 控件添加到 ASP.NET 页](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**图 12**: FileUpload 控件添加到 ASP.NET 页 ([单击以查看实际尺寸的图像](uploading-files-vb/_static/image18.png))


图 13 显示此页查看通过浏览器时。 请注意，单击浏览按钮将显示文件选择对话框中，允许用户选取从其计算机的文件。 一旦选择了一个文件，单击上载所选文件按钮会导致将所选的文件 s 二进制内容发送到 web 服务器的回发。


[![用户可以选择要从其计算机上载到服务器的文件](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**图 13**： 用户可以从他们的计算机到服务器上载到选择一个文件 ([单击以查看实际尺寸的图像](uploading-files-vb/_static/image20.png))


在回发时，上载的文件可以保存到文件系统或直接通过流可以结合其二进制数据。 对于此示例，让我们来创建`~/Brochures`文件夹并保存已上载的文件存在。 首先，通过添加`Brochures`到作为根目录下的子文件夹站点的文件夹。 接下来，创建的事件处理程序`UploadButton`s`Click`事件并添加以下代码：


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

FileUpload 控件提供了各种用于使用已上载的数据的属性。 例如， [ `HasFile`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx)指示是否由用户上载一个文件时[`FileBytes`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx)的字节数组形式提供对上载的二进制数据的访问。 `Click`事件处理程序首先确保已上载文件。 如果已上载文件，该标签将显示已上载的文件，以字节为单位，其大小和其内容类型的名称。

> [!NOTE]
> 若要确保用户将可以检查的文件上载`HasFile`属性并显示一条警告，如果它 s `False`，或者也可以使用[RequiredFieldValidator 控件](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx)改为。


FileUpload s`SaveAs(filePath)`将上载的文件保存到指定*filePath*。 *filePath*必须*物理路径*(`C:\Websites\Brochures\SomeFile.pdf`) 而不是*虚拟**路径*(`/Brochures/SomeFile.pdf`)。 [ `Server.MapPath(virtPath)`方法](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx)采用虚拟路径并返回其相应的物理路径。 在这里的虚拟路径是`~/Brochures/fileName`，其中*fileName*是上载的文件的名称。 请参阅[使用 Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml)有关在虚拟和物理路径和使用的详细信息`Server.MapPath`。

完成后`Click`事件处理程序，需要一段时间来测试浏览器中的页。 单击浏览按钮并选择从硬盘文件，然后单击上载选择文件按钮。 回发将将所选文件的内容发送到 web 服务器，然后会显示有关该文件保存到之前的信息`~/Brochures`文件夹。 后上载文件时，返回到 Visual Studio，然后单击解决方案资源管理器中的刷新按钮。 你会看到刚刚上载 ~/Brochures 文件夹中的文件 ！


[![已将文件 EvolutionValley.jpg 上载到 Web 服务器](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**图 14**： 文件`EvolutionValley.jpg`已上载到 Web 服务器 ([单击以查看实际尺寸的图像](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg 已保存到 ~/Brochures 文件夹](uploading-files-vb/_static/image15.gif)

**图 15**:`EvolutionValley.jpg`已保存到`~/Brochures`文件夹


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>与将上载的文件保存到文件系统的细微部分

有几个保存将文件上载到 web 服务器的文件系统时必须进行寻址的细微部分。 安全的第一个，那里问题。 若要将文件保存到文件系统中，执行 ASP.NET 页所用的安全上下文必须具有写入权限。 在当前用户帐户的上下文中运行 ASP.NET 开发 Web 服务器。 如果使用的 Microsoft 的 Internet 信息服务 (IIS) 作为 web 服务器，安全上下文取决于 IIS 和其配置的版本。

将文件保存到文件系统的另一个难题是围绕命名文件。 目前，我们的页面将保存到已上载的文件的所有`~/Brochures`使用相同的名称作为客户端的计算机上的文件的目录。 如果用户 A 上载同名小册子`Brochure.pdf`，该文件将保存为`~/Brochure/Brochure.pdf`。 但如果一段时间更高版本的用户 B 上载不同小册子文件恰好具有相同的文件名 (`Brochure.pdf`)？ 使用代码我们现在，让的用户的文件将被覆盖用户 B 的上载。

有大量技术来解决文件名冲突问题。 一个选项是禁止上载文件，如果已存在一个具有相同的名称。 使用此方法，当用户 B 尝试上载名为的文件时`Brochure.pdf`，系统将不保存他们的文件并改为显示消息，告知用户 B 重命名文件，然后重试。 另一种方法是使用唯一的文件名，这可能是保存该文件[全局唯一标识符 (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)还是来自相应数据库记录 s 主键列的值 (假定与关联上载中特定行的数据模型）。 在下一步的教程，我们将探讨这些选项中更多详细信息。

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>涉及的二进制数据量非常大的难题

这些教程也假设捕获的二进制数据是适度的大小。 使用量非常大的二进制数据文件，则几兆字节或更大引入了新的挑战，这些教程的范围之外。 例如，默认情况下 ASP.NET 将拒绝上载超过 4 MB，尽管这可以通过配置[`<httpRuntime>`元素](https://msdn.microsoft.com/library/e1f13641.aspx)中`Web.config`。 IIS 有太一定其自身的文件上载大小限制。 请参阅[IIS 上载文件大小](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html)有关详细信息。 此外，所需上载大型文件的时间可能会超出默认值 110 秒 ASP.NET 将等待的请求。 也有会出现在处理大型文件时的内存和性能问题。

FileUpload 控件是不切实际的大型文件上载。 因为文件的内容正在发布到服务器，最终用户必须耐心等待而无需任何确认其上载的进展情况。 当处理较小的文件，可以上载几秒钟后，但问题可能会导致在处理较大，可能需要一分钟才能上载的文件时，这不是如此之多问题。 还有很多第三方文件上载控件更好地适用于处理较大的上载和许多这些供应商提供进度指示器和 ActiveX 上载管理器提供更加完美的用户体验。

如果你的应用程序需要处理较大的文件，你将需要仔细研究面临的挑战和为你的特定需求找到合适的解决方案。

## <a name="summary"></a>摘要

生成的应用程序需要捕获二进制数据引入了许多挑战。 在本教程中我们探讨了前两个： 决定在何处存储二进制数据，并允许用户上载通过网页上的二进制内容。 通过接下来三个教程，我们将了解如何将上载的数据与数据库中记录相关联，以及如何显示其文本数据字段旁边的二进制数据。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [使用大值数据类型](https://msdn.microsoft.com/library/ms178158.aspx)
- [FileUpload 控件快速入门](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [ASP.NET 2.0 FileUpload 服务器控件](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [文件的弊端上载](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨和伯纳黛特 Leigh。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](updating-and-deleting-existing-binary-data-cs.md)
[下一页](displaying-binary-data-in-the-data-web-controls-vb.md)
