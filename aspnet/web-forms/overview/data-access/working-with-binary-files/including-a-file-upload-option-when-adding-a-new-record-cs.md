---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: "添加一条新记录 (C#) 时包括文件上载选项 |Microsoft 文档"
author: rick-anderson
description: "本教程演示如何创建一个 Web 界面，允许用户同时输入文本数据，并将二进制文件上载。 为了说明选项可用 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: 384251e5d0d72c6d1cc014c929a5d504be11d1d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>包括文件上载选项，在添加新的记录 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe)或[下载 PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> 本教程演示如何创建一个 Web 界面，允许用户同时输入文本数据，并将二进制文件上载。 为了说明可用于存储二进制数据的选项，一个文件将保存在数据库中时的其他存储在文件系统。


## <a name="introduction"></a>介绍

在前面的两个教程中我们探讨了用于存储与应用程序的数据模型，介绍了如何使用 FileUpload 控件将文件从客户端发送到 web 服务器，并已了解如何将此二进制数据呈现数据 W 中关联的二进制数据的技术eb 控件。 我们遇到有待讨论如何通过将上载的数据与数据模型中，关联。

在本教程中我们将创建一个用于添加新类别 web 页。 除类别的名称和说明的文本框中，此页需要包括两个 FileUpload 控件另一个用于新的类别的图片，另一个用于小册子中。 将新记录 s 中直接存储上载的图片`Picture`列，而小册子将保存到`~/Brochures`替换为新记录 s 中保存的文件的路径的文件夹`BrochurePath`列。

然后再创建此新的 web 页面，我们将需要更新体系结构。 `CategoriesTableAdapter` S 主查询未检索`Picture`列。 因此，自动生成`Insert`方法只有输入`CategoryName`， `Description`，和`BrochurePath`字段。 因此，我们需要在所有四个提示的 TableAdapter 中创建的其他方法`Categories`字段。 `CategoriesBLL`业务逻辑层中的类还将需要更新。

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>步骤 1： 添加`InsertWithPicture`方法`CategoriesTableAdapter`

当我们创建`CategoriesTableAdapter`进来[创建数据访问层](../introduction/creating-a-data-access-layer-cs.md)教程中，我们将其配置为自动生成`INSERT`， `UPDATE`，和`DELETE`语句基于主查询。 此外，我们已指示要采用的 DB 直接的方法，创建方法的 TableAdapter `Insert`， `Update`，和`Delete`。 这些方法来执行自动生成`INSERT`， `UPDATE`，和`DELETE`语句，因此，接受基于主查询返回的列的输入的参数。 在[上载文件](uploading-files-cs.md)教程我们扩充`CategoriesTableAdapter`s 要使用的主查询`BrochurePath`列。

由于`CategoriesTableAdapter`s 主查询未引用`Picture`列中，我们不能添加新的记录或使用的值更新现有记录`Picture`列。 为了捕获此信息，我们可以创建一个新方法中专门用于插入二进制数据的记录的 TableAdapter 或者我们可以自定义自动生成`INSERT`语句。 自定义自动生成的问题`INSERT`语句是我们风险具有覆盖由向导我们自定义项。 例如，假设我们自定义`INSERT`语句以包含使用`Picture`列。 这将更新 TableAdapter 的`Insert`方法以包括类别的图片 s 二进制数据的其他输入的参数。 我们然后可以在业务逻辑层，若要使用此 DAL 方法并且调用表示层，通过此 BLL 方法中创建一种方法并的所有内容的方式非常工作。 也就是说，只有在下次时我们配置了通过 TableAdapter 配置向导 TableAdapter。 向导已完成，到我们自定义项时，就会立即`INSERT`语句，都会覆盖，`Insert`方法将恢复到原来的形式，并且我们的代码将无法再编译 ！

> [!NOTE]
> 使用存储的过程而不临时的 SQL 语句时，此干扰将是一个非问题。 将来的教程将探讨在数据访问层中使用而不是临时的 SQL 语句的存储的过程。


若要避免此潜在难题，而不是自定义自动生成 SQL 语句，请让 s 改为 TableAdapter 创建新方法。 此方法中，名为`InsertWithPicture`，将接受的值`CategoryName`， `Description`， `BrochurePath`，和`Picture`列和执行`INSERT`将所有四个值存储在一条新记录中的语句。

打开该类型化数据集，然后从设计器中，右键单击`CategoriesTableAdapter`s 标头，然后从上下文菜单中选择添加查询。 这将启动 TableAdapter 查询配置向导的说明进行操作，它首先会询问我们 TableAdapter 查询访问数据库的方式。 选择使用 SQL 语句，然后单击下一步。 下一步将提示输入查询的类型生成。 由于我们重新创建一个查询来添加到新的记录`Categories`表、 选择插入，然后单击下一步。


[![选择插入选项](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**图 1**： 选择插入选项 ([单击以查看实际尺寸的图像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


现在，我们需要指定`INSERT`SQL 语句。 向导会自动建议使用`INSERT`语句对应于 TableAdapter s 主查询。 在这种情况下，它 s`INSERT`插入语句`CategoryName`， `Description`，和`BrochurePath`值。 Update 语句，以便`Picture`列是包含沿`@Picture`参数，如下所示：


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

在向导的最后一个屏幕可让我们将新的 TableAdapter 方法。 输入`InsertWithPicture`并单击完成。


[![新的 TableAdapter 方法 InsertWithPicture 名称](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**图 2**： 将新的 TableAdapter 方法`InsertWithPicture`([单击以查看实际尺寸的图像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>步骤 2： 更新的业务逻辑层

由于表示层应仅与业务逻辑层进行交互，而不是我们绕过它以直接转至数据访问层，需要创建调用我们刚刚创建的 DAL 方法的 BLL 方法 (`InsertWithPicture`)。 对于本教程中，创建中的方法`CategoriesBLL`类名为`InsertWithPicture`接受作为输入的三个`string`s 和`byte`数组。 `string`输入的参数只用于类别的名称、 说明和小册子文件路径时`byte`数组为类别的图片的二进制内容。 如下面的代码所示，此 BLL 方法将调用相应的 DAL 方法：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> 请确保在添加之前保存类型化数据集`InsertWithPicture`为 BLL 的方法。 由于`CategoriesTableAdapter`类代码是自动生成基于类型化数据集，如果 don t 首先将所做的更改保存到类型化数据集`Adapter`属性获胜 t 了解`InsertWithPicture`方法。


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>步骤 3： 列出现有类别和其二进制数据

在本教程中我们将创建一个页面，让最终用户能够将新类别添加到系统提供的新类别的图片和手册。 在[前面教程](displaying-binary-data-in-the-data-web-controls-cs.md)我们用于 GridView TemplateField ImageField 与显示每个类别的名称，说明、 图片和链接以下载其手册。 让我们来为本教程中，创建一个页，同时列出所有现有的类别，并允许创建新的复制该功能。

首先打开`DisplayOrDownload.aspx`来自页`BinaryData`文件夹。 请转到源视图并将复制的 GridView 和 ObjectDataSource s 声明性语法，粘贴在`<asp:Content>`中的元素`UploadInDetailsView.aspx`。 此外，不要忘记复制通过`GenerateBrochureLink`方法的代码隐藏类从`DisplayOrDownload.aspx`到`UploadInDetailsView.aspx`。


[![复制并粘贴到 UploadInDetailsView.aspx 从 DisplayOrDownload.aspx 的声明性语法](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**图 3**： 复制并粘贴从声明性语法`DisplayOrDownload.aspx`到`UploadInDetailsView.aspx`([单击以查看实际尺寸的图像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


复制的声明性语法后和`GenerateBrochureLink`方法转移到`UploadInDetailsView.aspx`页上，查看通过浏览器以确保所有内容已复制通过正确的页。 你应看到列出的八个类别一个 GridView，包含要下载小册子，以及类别的图片的链接。


[![现在，你应看到每个类别以及其二进制数据](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**图 4**： 你应现在请参阅每个类别沿对其的二进制数据 ([单击以查看实际尺寸的图像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>步骤 4： 配置`CategoriesDataSource`支持插入

`CategoriesDataSource` ObjectDataSource 由`Categories`GridView 目前不提供插入数据的能力。 若要支持通过此数据源控件插入，我们需要映射其`Insert`到在其基础对象中方法的方法`CategoriesBLL`。 具体而言，我们想要将其映射到`CategoriesBLL`返回在步骤 2 中，我们添加的方法`InsertWithPicture`。

通过单击从 ObjectDataSource s 智能标记的配置数据源链接启动。 第一个屏幕中显示数据源配置要使用的对象`CategoriesBLL`。 保留此设置为-前进到定义数据方法屏幕旁边单击。 将移动到插入选项卡，选取`InsertWithPicture`从下拉列表的方法。 单击完成以完成向导。


[![配置对象数据源以使用 InsertWithPicture 方法](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**图 5**： 配置对象数据源以使用`InsertWithPicture`方法 ([单击以查看实际尺寸的图像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> 完成向导之后，Visual Studio 可能会要求是否你要刷新字段和密钥，这将重新生成数据 Web 控制字段。 因为选择是将覆盖您所做的任何字段自定义，请选择否，。


完成向导后，ObjectDataSource 将现在包含的值其`InsertMethod`属性，以及`InsertParameters`对于四个类别列中，如下面的声明性标记所阐释：


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>步骤 5： 创建插入接口

在首次涵盖[概述的插入、 更新和删除数据](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)，说明提供一个内置的插入界面，可以使用支持插入的数据源控件时使用。 让我们来说明如何控件添加到此页面上方 GridView 将永久呈现其插入的界面，允许用户快速添加新类别。 在说明中添加新类别，在其下的 GridView 将自动刷新并显示新类别。

通过从工具箱中拖动到上面 GridView，设置设计器中拖动说明如何开始其`ID`属性`NewCategory`和清除`Height`和`Width`属性值。 从说明 s 智能标记，请将其绑定到现有`CategoriesDataSource`，然后选中启用插入复选框。


[![将说明如何绑定到 CategoriesDataSource 并启用插入](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**图 6**： 绑定到说明`CategoriesDataSource`和启用插入 ([单击以查看实际尺寸的图像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


若要永久呈现在其插入界面说明，设置其`DefaultMode`属性`Insert`。

请注意，说明有五个 BoundFields `CategoryID`， `CategoryName`， `Description`， `NumberOfProducts`，和`BrochurePath`尽管`CategoryID`因为插入接口不呈现 BoundField 其`InsertVisible`属性设置为`false`。 因为它们是返回的列，所以存在这些 BoundFields`GetCategories()`方法，即 ObjectDataSource 调用以检索其数据。 用于插入，但是，我们不想让用户指定的值`NumberOfProducts`。 此外，我们需要以允许它们上载为新的类别图片以及上载小册子 PDF。

删除`NumberOfProducts`从说明如何一起删除，然后更新 BoundField`HeaderText`属性`CategoryName`和`BrochurePath`BoundFields 为类别并小册子中，分别。 接下来，将转换`BrochurePath`转换为 TemplateField BoundField 和添加图片，为新 TemplateField 提供此新 TemplateField`HeaderText`图片的值。 移动`Picture`TemplateField 使之之间`BrochurePath`TemplateField 和 CommandField。


![将说明如何绑定到 CategoriesDataSource 并启用插入](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**图 7**： 绑定到说明`CategoriesDataSource`并启用插入


如果你转换`BrochurePath`转换通过编辑字段对话框中为 TemplateField BoundField TemplateField 包括`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`。 仅`InsertItemTemplate`是需要但是，因此请尝试删除了其他两个模板。 此时你说明如何 s 声明性语法应如下所示：


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>手册和图片字段添加 FileUpload 控件

目前， `BrochurePath` TemplateField s`InsertItemTemplate`包含文本框中，虽然`Picture`TemplateField 不包含任何模板。 我们需要更新这些两个 TemplateField 的`InsertItemTemplate`以使用 FileUpload 控件。

从说明 s 智能标记中，选择编辑模板选项，然后选择`BrochurePath`TemplateField 的`InsertItemTemplate`从下拉列表。 删除文本框中，然后将 FileUpload 控件从工具箱拖入模板。 设置 FileUpload 控件 s`ID`到`BrochureUpload`。 同样，FileUpload 将控件添加到`Picture`TemplateField 的`InsertItemTemplate`。 将此 FileUpload 控制 s 设置`ID`到`PictureUpload`。


[![FileUpload 控件添加到 InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**图 8**: FileUpload 将控件添加到`InsertItemTemplate`([单击以查看实际尺寸的图像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


完成后这些添加的内容，请将为两个 TemplateField s 声明性语法：


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

当用户将添加新类别时，我们想要确保的手册和图片的文件类型正确。 对于小册子中，用户必须提供 PDF。 对于本图中，我们需要用户上载一个图像文件，但我们允许*任何*图像文件或仅图像文件的特定类型，例如 Gif 或 Jpg？ 若要允许不同的文件类型，d 我们需要扩展`Categories`架构以包括捕获的文件类型，以便可以将此类型发送到客户端通过列`Response.ContentType`中`DisplayCategoryPicture.aspx`。 由于我们不具有此类的列，它明智的做法是以将用户限制到仅提供特定的图像文件类型。 `Categories`表 s 现有的映像是位图，但 Jpg 图像提供通过 web 服务的更合适文件格式。

如果用户上载不正确的文件类型时，我们需要取消插入并显示一条消息，指示的问题。 添加下面说明的标签 Web 控件。 设置其`ID`属性`UploadWarning`，清除出其`Text`属性，设置`CssClass`警告，属性和`Visible`和`EnableViewState`属性设置为`false`。 `Warning` CSS 类中定义`Styles.css`并呈现大型、 红色、 斜体化、 加粗字体中的文本。

> [!NOTE]
> 理想情况下，`CategoryName`和`Description`BoundFields 将转换为 TemplateFields 和自定义其插入接口。 `Description`插入接口，例如，将可能会更好地适合通过多行文本框。 并且由于`CategoryName`列不接受`NULL`值，应添加 RequiredFieldValidator 以确保用户的新类别的名称提供一个值。 这些步骤作为练习从左到读取器。 将回指[自定义的数据修改界面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)的深入探讨了补充的数据修改接口。


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>步骤 6： 将上载的小册子保存到 Web 服务器的文件系统

当用户输入的值的新类别，然后单击插入按钮时，产生的回发和插入工作流展开。 首先，说明如何 s [ `ItemInserting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx)激发。 接下来，ObjectDataSource s`Insert()`调用方法时，这将导致新记录添加到`Categories`表。 在此之后，说明如何 s [ `ItemInserted`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx)激发。

之前 ObjectDataSource 的`Insert()`调用方法，我们必须首先确保用户已上载的相应文件类型，然后将小册子 PDF 保存到 web 服务器的文件系统。 创建的事件处理程序说明如何的`ItemInserting`事件并添加以下代码：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

事件处理程序启动通过引用`BrochureUpload`FileUpload 控件从说明的模板。 然后，如果已上载小册子，上载的文件的扩展检查。 如果不扩展。显示 PDF，然后一条警告，插入被取消时，并在事件处理程序执行结束。

> [!NOTE]
> 依赖于上载的文件的扩展名不是确保上载的文件的 PDF 文档攻击性技术。 用户可能具有有效的 PDF 文档扩展名`.Brochure`，或无法拍摄非 PDF 文档并为它起`.pdf`扩展。 文件 s 二进制内容将需要以编程方式检查以便更最终验证文件类型。 此类全面的方法，不过，往往太过严厉;检查扩展足以满足大多数方案。


中所述[上载文件](uploading-files-cs.md)教程，因此该一个用户的上载不会覆盖另一个 s 保存文件到文件系统时必须小心。 对于本教程中我们将尝试使用相同的名称作为上载的文件。 如果已存在的文件中`~/Brochures`directory 与该文件同名，但是，我们将在附加号结束直到找到一个唯一的名称。 例如，如果在用户上载一个名为的小册子文件`Meats.pdf`，但已存在名为的文件`Meats.pdf`中`~/Brochures`文件夹中，我们将保存的文件名称更改为`Meats-1.pdf`。 如果存在，我们将尝试`Meats-2.pdf`，依此类推，直到找到唯一的文件名。

下面的代码使用[`File.Exists(path)`方法](https://msdn.microsoft.com/library/system.io.file.exists.aspx)以确定文件是否已存在具有指定的文件名称。 如果是这样，它将继续尝试小册子中的新文件名称，直到找到不会发生冲突。


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

找到有效的文件名称后, 该文件必须保存到文件系统和 ObjectDataSource 的`brochurePath``InsertParameter`值需要进行更新，以便该文件名写入到数据库。 正如我们所看到的进来*上载文件*教程中，保存该文件可以使用 FileUpload 控件的`SaveAs(path)`方法。 若要更新 ObjectDataSource s`brochurePath`参数，使用`e.Values`集合。


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>步骤 7： 将上载的图片保存到数据库

若要将已上载的图片存储在新`Categories`记录，我们需要将上载的二进制内容分配给 ObjectDataSource s`picture`中说明如何的参数`ItemInserting`事件。 我们进行此分配之前，但是，我们需要首先请确保已上载的图片是 JPG 和不某些其他图像类型。 如下所示步骤 6，让我们来使用已上载的图片的文件扩展名来确定其类型。

虽然`Categories`表允许`NULL`值`Picture`列中，当前所有类别有图片。 让我们来强制用户在添加新类别通过此页时提供图片。 下面的代码检查以确保已上载图片，并且它具有相应的扩展。


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

应将此代码放置*之前*步骤 6 中的代码，以便如果图片上载问题，事件处理程序将终止之前小册子文件保存到文件系统。

假设已上载了相应的文件，将已上载的二进制内容分配到图片参数的值与下面的代码行：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>完整`ItemInserting`事件处理程序

为了保持完整性，下面是`ItemInserting`其完整的事件处理程序：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>步骤 8： 修复`DisplayCategoryPicture.aspx`页

允许 s 花片刻时间以测试插入接口和`ItemInserting`创建在过去几个步骤的事件处理程序。 请访问`UploadInDetailsView.aspx`页上通过浏览器并尝试添加一个类别，但省略图片，或者指定非 JPG 图片或非 PDF 手册。 在任何这些情况下，将显示一条错误消息和插入工作流取消。


[![显示如果上载无效的文件类型是一条警告消息](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**图 9**: 警告消息是显示如果上载无效的文件类型 ([单击以查看实际尺寸的图像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


一次你已验证的页面要求图片要上载和获胜的 t 接受非 PDF 或非 JPG 文件，添加具有有效的 JPG 图片的新类别将小册子字段留空。 单击插入按钮后，页面将回发，一条新记录将添加到`Categories`表直接在数据库中存储的已上载的图像 s 二进制内容。 GridView 更新，并显示新添加的类别中，行，但如图 10 所示，新的类别的图片不正确呈现。


[![新的类别图不显示的 s](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**图 10**： 图不显示的新类别 s ([单击以查看实际尺寸的图像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


不显示新图片的原因是因为`DisplayCategoryPicture.aspx`返回指定的类别的图片的页面已配置为处理 OLE 标头的位图。 从中提取出来此 78 字节的标头`Picture`列 s 二进制内容在发送之前回客户端。 但没有此 OLE 标题，则我们只需上载为新的类别的 JPG 文件。因此，根据需要，有效字节即将从映像 s 二进制数据中删除。

由于现在有两个位图与 OLE 标头中的 Jpg`Categories`表，我们需要更新`DisplayCategoryPicture.aspx`，以便其不去除原始八个类别的 OLE 标头，绕过较新的类别记录此去除。 在我们的下一教程中，我们将查看如何更新现有记录的映像，并且我们将更新所有旧的类别图片，以便它们 Jpg。 现在，不过，使用下面的代码`DisplayCategoryPicture.aspx`去除仅用于这些原始的八个分类的 OLE 标头：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

进行此更改后，JPG 图像在 GridView 的现在正确呈现。


[![新的类别的 JPG 图像是正确呈现](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**图 11**： 新的类别的 JPG 图像是正确呈现 ([单击以查看实际尺寸的图像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>步骤 9： 删除小册子中的遇到了异常

在 web 服务器的文件系统上存储二进制数据的挑战之一是它引入了断开连接之间的数据模型和其二进制数据。 因此，只要记录被删除，必须还删除文件系统上的相应二进制数据。 这可能会在插入时，也发挥。 考虑以下方案： 用户添加新类别中，指定有效的图片和手册。 单击插入按钮，产生的回发加以说明的`ItemInserting`事件激发，将小册子中保存到 web 服务器的文件系统。 接下来，ObjectDataSource s`Insert()`调用方法，从而调用`CategoriesBLL`类 s`InsertWithPicture`方法，后者将调用`CategoriesTableAdapter`s`InsertWithPicture`方法。

现在，如果数据库处于脱机状态，会发生什么情况，或如果中的错误`INSERT`SQL 语句？ 清楚地插入将失败，因此没有新的类别行将添加到数据库。 但我们仍有坐在 web 服务器的文件系统上的已上载的小册子文件 ！ 此文件需要将删除在遇到时插入工作流期间出现的异常。

中所述以前[处理 BLL-和 ASP.NET 页中的 DAL 级别异常](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教程中，在它通过各层上传的体系结构的深度内从引发异常时。 在演示文稿层上，我们可以确定是否从说明 s 发生了异常`ItemInserted`事件。 此事件处理程序还提供的值 ObjectDataSource 的`InsertParameters`。 因此，我们可以创建的事件处理程序`ItemInserted`事件检查是否出现异常，如果是这样，删除 ObjectDataSource s 指定的文件`brochurePath`参数：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>摘要

有多个要添加包含二进制数据的记录提供基于 web 的界面，必须执行的步骤。 如果正在直接到数据库中存储二进制数据，则很有你将需要更新体系结构中，添加特定的方法以处理二进制数据正在插入的位置的情况。 一旦体系结构已更新下, 一步创建插入接口，这可以使用自定义要包括的每个二进制数据字段 FileUpload 控制说明如何完成。 上载的数据然后可以保存到 web 服务器的文件系统或分配给说明 s 中的数据源参数`ItemInserting`事件处理程序。

将二进制数据保存到文件系统需要多个计划，而不直接在数据库中保存数据。 为了避免覆盖另一个 s 的一个用户的上载，必须选择命名方案。 此外，必须采取额外步骤来删除已上载的文件，如果数据库插入操作失败。

我们现在有能够将新类别添加到具有小册子和状况，但我们系统遇到有待看一下如何更新现有类别 s 二进制数据或如何正确删除已删除的类别的二进制数据。 在下一教程中，我们将探讨这两个主题。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Dave Gardner、 Teresa 墨和伯纳黛特 Leigh。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](displaying-binary-data-in-the-data-web-controls-cs.md)
[下一页](updating-and-deleting-existing-binary-data-cs.md)
