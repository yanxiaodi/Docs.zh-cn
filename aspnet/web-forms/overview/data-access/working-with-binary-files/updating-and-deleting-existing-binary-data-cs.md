---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: "更新和删除现有的二进制数据 (C#) |Microsoft 文档"
author: rick-anderson
description: "前面的教程中我们已了解如何 GridView 控件可以方便地编辑和删除文本数据。 在本教程中我们看到如何 GridView 控件还建立..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: f2fca1e91720fba0215e12b1a1894a3a31e86b5c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="updating-and-deleting-existing-binary-data-c"></a>更新和删除现有的二进制数据 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe)或[下载 PDF](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> 前面的教程中我们已了解如何 GridView 控件可以方便地编辑和删除文本数据。 在本教程中我们将了解如何 GridView 控件还使它能够编辑和删除二进制数据，该二进制数据是保存到数据库中还是存储在文件系统。


## <a name="introduction"></a>介绍

在过去三个教程我们已添加的处理二进制数据的功能相当多的位。 我们已开始将通过添加`BrochurePath`列`Categories`表并相应地更新体系结构。 我们还将添加数据访问层和业务逻辑层的方法，以便处理与类别表 s 现有`Picture`列，其中包含的图像文件的二进制内容 s。 我们已构建的 web 页面以提供一个 GridView 中的二进制数据小册子中，下载链接具有类别的图中所示`<img>`元素并将其添加说明如何以允许用户添加新类别并上载其小册子和图片的数据。

所有这些剩下实现是能够编辑和删除现有类别，我们将完成在本教程中使用的 GridView s 内置编辑和删除功能。 当编辑某个类别时，用户将能够根据需要上载新图片或继续使用现有的类别。 为小册子中，它们可以选择使用现有小册子中，上载新小册子中，或以指示类别不再具有小册子与之关联。 让我们来开始 ！

## <a name="step-1-updating-the-data-access-layer"></a>步骤 1： 更新的数据访问层

DAL 具有自动生成`Insert`， `Update`，和`Delete`方法，但这些方法基于生成`CategoriesTableAdapter`s 主查询，这不包括`Picture`列。 因此，`Insert`和`Update`方法不包括用于指定类别的图片的二进制数据的参数。 像我们[前面教程](including-a-file-upload-option-when-adding-a-new-record-cs.md)，我们需要创建一个新的 TableAdapter 方法用于更新`Categories`表指定二进制数据时。

打开该类型化数据集，然后从设计器中，右键单击`CategoriesTableAdapter`s 标头，然后选择添加查询从上下文菜单到 launche TableAdapter 查询配置向导。 通过要求我们 TableAdapter 查询访问数据库的方式来启动此向导。 选择使用 SQL 语句，然后单击下一步。 下一步将提示输入查询的类型生成。 由于我们重新创建一个查询来添加到新的记录`Categories`表，选择更新，然后单击下一步。


[![选择更新选项](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**图 1**： 选择更新选项 ([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image2.png))


现在，我们需要指定`UPDATE`SQL 语句。 向导会自动建议使用`UPDATE`语句对应于 TableAdapter s 主查询 (更新的一个`CategoryName`， `Description`，和`BrochurePath`值)。 将该语句更改以便`Picture`列是包含沿`@Picture`参数，如下所示：


[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

在向导的最后一个屏幕可让我们将新的 TableAdapter 方法。 输入`UpdateWithPicture`并单击完成。


[![新的 TableAdapter 方法 UpdateWithPicture 名称](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**图 2**： 将新的 TableAdapter 方法`UpdateWithPicture`([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image4.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>步骤 2： 添加业务逻辑层方法

除了更新 DAL，我们需要更新 BLL 包括更新和删除某一类别的方法。 这些是将与表示层调用的方法。

对于删除类别，我们可以使用`CategoriesTableAdapter`自动生成的 s`Delete`方法。 添加以下方法`CategoriesBLL`类：


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

对于本教程，让 s 创建两种方法来更新类别的一个要求二进制图片数据并调用`UpdateWithPicture`方法我们刚添加到`CategoriesTableAdapter`接受的另一个只需`CategoryName`， `Description`，和`BrochurePath`值，并使用`CategoriesTableAdapter`类自动生成的 s`Update`语句。 使用两种方法背后的基本原理是，在某些情况下，用户可能想要更新的类别的图片以及其其他字段，在该情况下，用户将需要上载新图片。 上载的图片 s 二进制数据然后可在`UPDATE`语句。 在其他情况下，用户可能仅关注中更新，例如，名称和说明。 但是，如果`UPDATE`语句要求的二进制数据`Picture`列，然后我们 d 需要提供该信息以及。 这需要额外经历一次到数据库来取回图片数据正在编辑的记录。 因此，我们想要两个`UPDATE`方法。 业务逻辑层将确定使用哪一个基于图片数据以更新类别时提供。

若要为此，将添加以下两种方法`CategoriesBLL`类，这两名为`UpdateCategory`。 第一个应接受三个`string`s，`byte`数组，和`int`作为其输入参数; 第二个，只需三个`string`s 和`int`。 `string`输入的参数只用于类别的名称、 说明和小册子文件路径，`byte`数组为类别的图的二进制内容和`int`标识`CategoryID`要更新的记录。 请注意，第一个重载时，将调用第二个如果传入的`byte`数组是`null`:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>步骤 3： 将某个复制 Insert 和视图功能

在[前面教程](including-a-file-upload-option-when-adding-a-new-record-cs.md)我们创建一个名为页`UploadInDetailsView.aspx`，列出一个 GridView 中的所有类别，提供说明如何向系统添加新类别。 在本教程中我们将扩展以包括编辑和删除支持 GridView。 而不是继续使用`UploadInDetailsView.aspx`，让 s 改为将放在此教程的更改`UpdatingAndDeleting.aspx`来自同一文件夹中，页`~/BinaryData`。 复制并粘贴声明性的标记，然后从代码`UploadInDetailsView.aspx`到`UpdatingAndDeleting.aspx`。

首先打开`UploadInDetailsView.aspx`页。 将所有中的声明性语法复制`<asp:Content>`元素，如图 3 中所示。 接下来，打开`UpdatingAndDeleting.aspx`和粘贴在此标记其`<asp:Content>`元素。 同样，复制中的代码`UploadInDetailsView.aspx`页上 s 代码隐藏类`UpdatingAndDeleting.aspx`。


[![从 UploadInDetailsView.aspx 复制的声明性的标记](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**图 3**： 复制中的声明性标记`UploadInDetailsView.aspx`([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image6.png))


复制后通过声明性的标记和代码，请访问`UpdatingAndDeleting.aspx`。 你应看到相同输出，并且具有相同的用户体验与`UploadInDetailsView.aspx`来自前面的教程页。

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>步骤 4： 添加删除支持添加到的对象数据源和 GridView

中所述我们回[概述的插入、 更新和删除数据](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)教程中，GridView 提供内置的删除功能，并且可以在一个复选框的刻度线启用这些功能，如果网格 s 基础数据源支持删除。 当前 ObjectDataSource GridView 绑定到 (`CategoriesDataSource`) 不支持删除。

若要解决此问题，请单击从 ObjectDataSource s 智能标记的配置数据源选项，以启动向导。 第一个屏幕显示 ObjectDataSource 配置为使用`CategoriesBLL`类。 点击下一步。 目前，仅 ObjectDataSource s`InsertMethod`和`SelectMethod`指定属性。 但是，向导自动填充使用更新和删除选项卡中的下拉列表`UpdateCategory`和`DeleteCategory`方法，分别。 这是因为在`CategoriesBLL`我们标记使用这些方法的类`DataObjectMethodAttribute`与更新和删除的默认方法。

现在，设置为 （无） 的更新选项卡的下拉列表，但将删除选项卡的下拉列表设置为保持为`DeleteCategory`。 我们会返回到步骤 6 以添加更新的支持在此向导。


[![配置对象数据源以使用 DeleteCategory 方法](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**图 4**： 配置使用 ObjectDataSource`DeleteCategory`方法 ([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image8.png))


> [!NOTE]
> 完成向导之后，Visual Studio 可能会要求是否你要刷新字段和密钥，这将重新生成数据 Web 控制字段。 因为选择是将覆盖您所做的任何字段自定义，请选择否，。


ObjectDataSource 现在将包括的值其`DeleteMethod`属性以及`DeleteParameter`。 回想一下，当使用向导指定的方法，Visual Studio 将设置 ObjectDataSource s`OldValuesParameterFormatString`属性`original_{0}`，这会导致出现问题更新和删除方法调用。 因此，完全清除此属性，或将它重置为默认情况下， `{0}`。 如果您需要刷新你在此 ObjectDataSource 属性上的内存，请参阅[概述的插入、 更新和删除数据](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)教程。

完成该向导并修复后`OldValuesParameterFormatString`，ObjectDataSource s 声明性标记应类似如下所示：


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

在配置对象数据源之后, 删除通过将功能添加到 GridView 检查从 GridView s 智能标记启用删除复选框。 这会将 CommandField 添加到 GridView 其`ShowDeleteButton`属性设置为`true`。


[![为在 GridView 中删除启用支持](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**图 5**： 启用对 GridView 中删除的支持 ([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image10.png))


花些时间测试的删除功能。 没有间的外键`Products`表 s`CategoryID`和`Categories`表的`CategoryID`，因此如果你尝试删除的任何前八个类别，你将收到外键约束冲突异常。 若要测试出此功能，请添加一个新类别，提供小册子和图片。 图 6 所示我测试类别包括一个名为的测试小册子文件`Test.pdf`和测试图片。 图 7 显示 GridView 后添加测试类别。


[![添加具有小册子和图像的测试类别](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**图 6**： 添加具有小册子和图像的测试类别 ([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image12.png))


[![插入后测试类别，它将显示在 GridView](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**图 7**： 插入后测试类别，它将显示在 GridView ([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image14.png))


在 Visual Studio 中，刷新解决方案资源管理器。 你现在应该看到的新文件中`~/Brochures`文件夹， `Test.pdf` （请参阅图 8）。

接下来，单击删除链接在测试类别行中，导致页后，可以回发和`CategoriesBLL`类的`DeleteCategory`方法激发。 这将调用 DAL s`Delete`方法，使相应`DELETE`语句发送到数据库。 然后，重新将数据绑定到 GridView 和标记发送回客户端使用测试类别不再存在。

而删除工作流已成功删除中的测试类别记录`Categories`表，而未从 web 服务器的文件系统中删除其小册子文件。 刷新解决方案资源管理器，你将看到，`Test.pdf`仍位于`~/Brochures`文件夹。


![Test.pdf 文件未删除从 Web 服务器的文件系统](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**图 8**:`Test.pdf`不从 Web 服务器的文件系统中删除文件


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>步骤 5： 删除已删除的类别的小册子文件

存储到数据库外部的二进制数据的缺点之一是，必须采取额外步骤来清理这些文件，删除关联的数据库记录时。 GridView 和 ObjectDataSource 提供激发之前和之后已执行 delete 命令的事件。 我们实际上需要创建前和操作后事件的事件处理程序。 之前`Categories`删除记录，我们需要确定其 PDF 文件的路径，但我们不想删除 PDF 类别中删除某些异常并且不删除类别之前。

GridView s [ `RowDeleting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)激发已调用 ObjectDataSource s delete 命令之前，而其[`RowDeleted`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx)之后激发。 创建事件处理程序使用下面的代码这两个事件：


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

在`RowDeleting`事件处理程序，`CategoryID`的行在从 GridView s 抓取正在删除`DataKeys`集合，这可通过此事件处理程序中访问`e.Keys`集合。 接下来，`CategoriesBLL`类的`GetCategoryByCategoryID(categoryID)`调用以返回有关要删除的记录的信息。 如果返回`CategoriesDataRow`对象具有非`NULL``BrochurePath`值则存储在该页面变量`deletedCategorysPdfPath`，以便可以在删除该文件`RowDeleted`事件处理程序。

> [!NOTE]
> 而不是检索`BrochurePath`详细信息`Categories`记录中被删除`RowDeleting`事件处理程序中，我们无法或者添加`BrochurePath`到 GridView 的`DataKeyNames`属性和访问记录的值通过`e.Keys`集合。 这样将略有增加 GridView 的视图状态大小，但会减少所需的代码量并将行程保存到数据库。


已调用 s 基础 delete 命令，ObjectDataSource GridView s 后`RowDeleted`事件处理程序将触发。 如果不没有中删除数据的任何异常，并且没有值`deletedCategorysPdfPath`，然后从文件系统中删除 PDF。 请注意，则不需要此额外代码以清理其图片与关联的类别 s 二进制数据。 该 s 因为直接在数据库中存储的图片数据，因此删除`Categories`行也会删除该类别的图片数据。

添加两个事件处理程序后, 再次运行此测试用例。 当删除类别时，也将删除其关联的 PDF。

更新现有的关联记录 s 二进制数据提供了一些有趣的难题。 本教程的其余部分深入介绍了将更新功能添加到的手册和图片。 步骤 6 探讨了用于在步骤 7 考察更新图更新小册子信息技术。

## <a name="step-6-updating-a-category-s-brochure"></a>步骤 6： 更新类别的手册

中所述[概述的插入、 更新和删除数据](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)教程中，GridView 提供其基础数据源是否可以通过一个复选框的刻度线的内置行级编辑支持适当地配置。 目前， `CategoriesDataSource` ObjectDataSource 尚未配置以包括更新支持，因此让 s 添加中。

单击从 ObjectDataSource 的向导的配置数据源链接，转到第二个步骤。 由于`DataObjectMethodAttribute`中使用`CategoriesBLL`，更新下拉列表应自动用填充`UpdateCategory`接受四个输入参数的重载 (所有列，但`Picture`)。 更改此步骤，以便它使用五个参数的重载。


[![配置对象数据源以使用包含一个参数为图片 UpdateCategory 方法](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**图 9**： 配置使用 ObjectDataSource`UpdateCategory`包括的参数的方法`Picture`([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image16.png))


ObjectDataSource 现在将包括的值其`UpdateMethod`属性以及对应`UpdateParameter`s。 Visual Studio 在步骤 4 中所述，设置 ObjectDataSource s`OldValuesParameterFormatString`属性`original_{0}`时使用配置数据源向导。 这将导致问题的更新和删除方法调用。 因此，完全清除此属性，或将它重置为默认情况下， `{0}`。

完成该向导并修复后`OldValuesParameterFormatString`，ObjectDataSource s 声明性标记应如下所示：


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

若要打开 s 内置编辑功能的 GridView，选中从 GridView s 智能标记启用编辑选项。 这便会设置 CommandField s`ShowEditButton`属性`true`，这会导致其他部分的编辑按钮 （和正在编辑的行的更新和取消按钮）。


[![配置为支持编辑 GridView](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**图 10**： 配置为支持编辑 GridView ([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image18.png))


访问通过浏览器页面，单击一个行的编辑按钮。 `CategoryName`和`Description`BoundFields 呈现为文本框。 `BrochurePath` TemplateField 缺少`EditItemTemplate`，因此它将继续显示其`ItemTemplate`小册子中的链接。 `Picture` ImageField 呈现文本框中为其`Text`属性被赋值 ImageField s`DataImageUrlField`值，在这种情况下`CategoryID`。


[![GridView 为 BrochurePath 缺少编辑接口](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**图 11**: GridView 缺少用于的编辑接口`BrochurePath`([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image20.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>自定义`BrochurePath`s 编辑接口

我们需要创建的编辑接口`BrochurePath`TemplateField，允许用户或者直接：

- 将类别的小册子中作为-，
- 通过上载新小册子中，更新类别的手册或
- （在类别不再具有关联的小册子的情况下） 完全移除类别的手册。

我们还需要更新`Picture`ImageField s 编辑接口，但我们将获取与此步骤 7 中。

从 GridView s 智能标记中，单击编辑模板链接，然后选择`BrochurePath`TemplateField 的`EditItemTemplate`从下拉列表。 说明如何 Web 控件添加到此模板中，设置其`ID`属性`BrochureOptions`及其`AutoPostBack`属性`true`。 从属性窗口中，单击省略号以`Items`属性，它会弹出`ListItem`集合编辑器。 添加的以下三个选项`Value`的 1、 2 和 3，分别：

- 使用当前手册
- 删除当前手册
- 上载新的手册

设置第一个`ListItem`s`Selected`属性`true`。


![说明如何向中添加三个 ListItems](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**图 12**： 添加三个`ListItem`到说明如何


下面说明如何，添加一个名为的 FileUpload 控件`BrochureUpload`。 设置其`Visible`属性`false`。


[![将说明如何和 FileUpload 控件添加到 EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**图 13**： 添加说明如何与 FileUpload 控制`EditItemTemplate`([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image22.png))


此说明如何提供用户的三个选项。 思路是仅当选择最后一个选项，上载新小册子中，将显示 FileUpload 控件。 若要实现此目的，创建的事件处理程序说明如何的`SelectedIndexChanged`事件并添加以下代码：


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

由于如何和 FileUpload 控件是在模板中，我们必须编写的代码以编程方式访问这些控件。 `SelectedIndexChanged`事件处理程序传递的引用中说明如何`sender`输入的参数。 若要获取 FileUpload 控件，我们需要先获取说明如何的父控件并使用`FindControl("controlID")`从那里的方法。 一旦我们有了说明如何和 FileUpload 控件的引用，FileUpload 控制 s`Visible`属性设置为`true`才说明如何 s`SelectedValue`等于 3，这是`Value`上载新手册`ListItem`.

与此代码中的位置，需要花费一段时间来测试编辑界面。 单击编辑按钮的行。 最初，应选择使用当前小册子选项。 更改所选的索引会导致回发。 如果选择第三个选项，则显示 FileUpload 控件，否则其处于隐藏状态。 图 14 首先单击编辑按钮; 时显示的编辑接口图 15 演示接口之后选择上载新小册子选项。


[![最初，使用当前小册子中选择选项](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**图 14**： 最初，使用当前小册子选项 ([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image24.png))


[![选择上载新小册子中此选项可以显示 FileUpload 控件](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**图 15**： 选择上载新小册子中此选项可以显示 FileUpload 控件 ([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image26.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>保存小册子中文件和更新`BrochurePath`列

单击 GridView s 更新按钮后，其`RowUpdating`事件激发。 调用 s 更新命令 ObjectDataSource，然后选择 GridView 的`RowUpdated`事件激发。 像借助删除工作流，我们需要为这两种事件创建事件处理程序。 在`RowUpdating`事件处理程序，我们需要确定要执行的操作基于`SelectedValue`的`BrochureOptions`说明如何：

- 如果`SelectedValue`为 1，我们想要继续使用相同`BrochurePath`设置。 因此，我们需要将设置 ObjectDataSource s`brochurePath`到现有的参数`BrochurePath`正在更新的记录的值。 ObjectDataSource s`brochurePath`参数可以设置使用`e.NewValues["brochurePath"] = value`。
- 如果`SelectedValue`为 2，则我们想要设置记录 s`BrochurePath`值赋给`NULL`。 这可以通过设置 ObjectDataSource s`brochurePath`参数`Nothing`，这将导致数据库`NULL`中正在使用`UPDATE`语句。 如果没有正在删除现有小册子文件，我们需要删除现有文件。 但是，我们只想要执行此操作，如果更新完成而不会引发异常。
- 如果`SelectedValue`为 3，则我们想要确保用户已上载 PDF 文件，然后将其保存到文件系统并更新记录的`BrochurePath`列的值。 此外，如果没有将被替换的现有小册子文件，我们需要删除以前的文件。 但是，我们只想要执行此操作，如果更新完成而不会引发异常。

当完成所需的步骤说明如何 s`SelectedValue`是 3 几乎是相同的所使用的说明如何的`ItemInserting`事件处理程序。 此事件处理程序执行时从我们添加中的说明如何控件添加新类别记录[以前一教程](including-a-file-upload-option-when-adding-a-new-record-cs.md)。 因此，它 behooves 我们可以重构出到单独的方法，此功能。 具体而言，我移出的常见功能为两个方法：

- `ProcessBrochureUpload(FileUpload, out bool)`FileUpload 控件实例和一个输出布尔值，指定是否应继续删除或编辑操作或如果不应将其取消由于某个验证错误，则接受作为输入。 此方法返回的已保存文件的路径或`null`如果没有文件已保存。
- `DeleteRememberedBrochurePath`删除指定的页面变量中的路径的文件`deletedCategorysPdfPath`如果`deletedCategorysPdfPath`不`null`。

遵循这两种方法的代码。 请注意之间的相似性`ProcessBrochureUpload`和说明如何的`ItemInserting`从前面的教程的事件处理程序。 在本教程中，我已更新说明的事件处理程序以使用这些新方法。 下载与本教程，若要查看对说明的事件处理程序的修改关联的代码。


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

GridView s`RowUpdating`和`RowUpdated`事件处理程序使用`ProcessBrochureUpload`和`DeleteRememberedBrochurePath`方法，如以下代码所示：


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

请注意如何`RowUpdating`事件处理程序使用一系列条件语句来执行相应的操作基于`BrochureOptions`说明如何的`SelectedValue`属性值。

使用此代码中的位置，你可以编辑某一类别，并将其使用其当前小册子，请使用没有小册子中，或上载一个新。 请继续并对其进行测试。在中设置断点`RowUpdating`和`RowUpdated`事件处理程序以获取工作流的意义。

## <a name="step-7-uploading-a-new-picture"></a>步骤 7： 上载新图片

`Picture` ImageField s 编辑接口呈现为文本框中，使用从值填充其`DataImageUrlField`属性。 在编辑工作流，GridView 将参数传递给参数 s 同名 ObjectDataSource ImageField 的值`DataImageUrlField`属性和参数 s 值编辑界面中，在文本框中输入的值。 映像保存为文件系统上的文件时，此行为是适合和`DataImageUrlField`包含图像的完整 URL。 与此类情况下，编辑界面显示图像的 URL 在文本框中，用户可以更改和保存回数据库。 授予，此默认接口不允许用户上载一个新的映像，但它确实允许它们将图像的 URL 与当前值更改为另一个。 对于本教程，但是，编辑接口 ImageField 的默认不满足需要因为`Picture`直接在数据库中存储二进制数据和`DataImageUrlField`属性包含仅`CategoryID`。

若要更好地了解时会发生什么情况在我们的教程用户编辑具有 ImageField 的行，请考虑下面的示例： 用户编辑某一行而该行`CategoryID`10，导致`Picture`ImageField 以将呈现为文本框中，值为 10。 假设用户在此文本框中的值更改为 50，并单击更新按钮。 产生的回发和 GridView 最初创建一个名为参数`CategoryID`值 50。 但是，GridView 发送此参数之前 (和`CategoryName`和`Description`参数)，它会从值内添加`DataKeys`集合。 因此，它将覆盖`CategoryID`参数与当前行 s 基础`CategoryID`值 10。 简单地说，编辑接口 ImageField s 没有任何影响编辑工作流出于本教程因为 ImageField 的名称`DataImageUrlField`属性和网格的`DataKey`值都相同。

尽管 ImageField 使得变得更容易地显示基于数据库数据的映像，我们不想提供编辑界面中的文本框。 相反，我们想要提供最终用户可用于更改类别的图片 FileUpload 控件。 与不同`BrochurePath`值，这些教程我们已决定需要每个类别，必须具有图片。 因此，我们不需要让用户指示是否有任何关联的图片用户可能将.vhd 文件的新图片或保留当前图片作为-是。

若要自定义 ImageField s 编辑界面，我们需要将其转换转换为 TemplateField。 从 GridView s 智能标记，单击编辑列链接、 选择 ImageField，和 TemplateField 链接到单击转换此字段。


![将 ImageField 转换转换为 TemplateField](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**图 16**: ImageField 转换为 TemplateField


将 ImageField 转换为以这种方式为 TemplateField 生成两个模板与 TemplateField。 如下面的声明性语法所示，`ItemTemplate`包含映像 Web 控件`ImageUrl`属性分配使用数据绑定语法基于 ImageField s`DataImageUrlField`和`DataImageUrlFormatString`属性。 `EditItemTemplate`包含一个文本框其`Text`属性绑定到指定的值`DataImageUrlField`属性。


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

我们需要更新`EditItemTemplate`若要使用 FileUpload 控件。 从 GridView s 智能标记单击编辑模板链接，然后选择`Picture`TemplateField 的`EditItemTemplate`从下拉列表。 在模板中，你应该看到删除此文本框。 接下来，将 FileUpload 控件从工具箱拖到该模板后，设置其`ID`到`PictureUpload`。 此外添加文本以更改类别的图片，指定新图片。 若要保留相同类别的图片，该字段留空到模板。


[![FileUpload 控件添加到 EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**图 17**: FileUpload 将控件添加到`EditItemTemplate`([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image28.png))


自定义的编辑界面之后, 在浏览器中查看进度。 在查看行在只读模式下时，类别的映像将显示为之前，但单击编辑按钮将图片列呈现为与 FileUpload 控件的文本。


[![编辑接口包含 FileUpload 控件](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**图 18**： 编辑接口包含 FileUpload 控件 ([单击以查看实际尺寸的图像](updating-and-deleting-existing-binary-data-cs/_static/image30.png))


回想一下，ObjectDataSource 配置为调用`CategoriesBLL`类 s`UpdateCategory`接受作为输入图片作为的二进制数据的方法`byte`数组。 如果此数组有`null`值，但是，备用`UpdateCategory`调用重载，哪些问题`UPDATE`不会修改的 SQL 语句`Picture`列，从而使类别 s 当前按原样图片。 因此，在 GridView s`RowUpdating`我们需要以编程方式引用的事件处理程序`PictureUpload`FileUpload 控制，并确定未上载文件。 如果一个未上载，那么我们要做*不*想要为指定值`picture`参数。 另一方面，如果未在上载文件`PictureUpload`FileUpload 控件，我们想要确保它是一个 JPG 文件。 如果是，则我们可以将其二进制内容发送到通过 ObjectDataSource`picture`参数。

像在步骤 6 中使用的代码中，大部分已此处需要的代码中存在对具有说明的`ItemInserting`事件处理程序。 因此，我遇到重构成新方法的常见功能`ValidPictureUpload`，并且更新`ItemInserting`事件处理程序以使用此方法。

将以下代码添加到的 GridView 的开头`RowUpdating`事件处理程序。 它非常重要的是此代码将放在保存小册子文件，因为我们不 t 的代码之前想要将小册子中保存到 web 服务器的文件系统中，如果上载无效图片文件。


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

`ValidPictureUpload(FileUpload)`方法接受 FileUpload 控件作为其唯一的输入参数中，并检查已上载的文件的扩展，以确保已上载的文件的 JPG; 如果上载图片文件，则仅调用。 如果不上载任何文件，则将图片的参数未设置，并因此使用其默认值为`null`。 如果已上载图片和`ValidPictureUpload`返回`true`、`picture`参数分配的二进制数据的已上载的图像中; 如果该方法返回`false`、 取消更新工作流和事件处理程序退出。

`ValidPictureUpload(FileUpload)`从说明 s 重构的方法代码`ItemInserting`事件处理程序遵循：


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>步骤 8： 替换 Jpg 原始类别图片

记得原始的八个类别图片的位图文件包装在一个 OLE 标头。 现在，我们已添加的功能，若要编辑现有的记录的图片，花些时间将替换为 Jpg 这些位图。 如果你想要继续使用当前的类别图片，可以直接将它们转换为 Jpg，通过执行以下步骤：

1. 将位图图像保存到硬盘空间。 请访问`UpdatingAndDeleting.aspx`页在浏览器和每个前八个类别上，在图像上右键单击，选择将保存图。
2. 在选择的图像编辑器中打开的图像。 可以使用 Microsoft 画图，例如所示。
3. 将位图另存为的 JPG 图像。
4. 更新通过编辑界面、 使用 JPG 文件的类别的图片。

后编辑类别并上传的 JPG 图像，图像将不会呈现在浏览器因为`DisplayCategoryPicture.aspx`页去除前 78 个字节从前八个类别的图片。 通过删除执行 OLE 标头去除的代码来解决此问题。 执行此操作，操作之后`DisplayCategoryPicture.aspx``Page_Load`事件处理程序应具有只会将以下代码：


[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx`页 s 插入和编辑接口无法使用多做一些工作。 `CategoryName`和`Description`中的说明和 GridView BoundFields 应转换为 TemplateFields。 由于`CategoryName`不允许`NULL`值，应添加 RequiredFieldValidator。 与`Description`文本框中应可能转换为多行文本框。 我为你作为练习保留这些完成收尾工作。


## <a name="summary"></a>摘要

本教程中完成我们看处理二进制数据。 在本教程和以前的三个，我们已了解如何二进制数据可以存储在文件系统或直接在数据库内。 用户提供的选择从其硬盘的文件并将其上载到 web 服务器，其中可以存储在文件系统或插入到数据库到系统的二进制数据。 ASP.NET 2.0 包括 FileUpload 控件，可提供此类接口一样简单，如拖放。 但是，如中另有说明[上载文件](uploading-files-cs.md)教程中，FileUpload 控件是仅适合于相对较小的文件上载，理想情况下不超过一兆字节。 我们还介绍了如何将上载的数据与基础数据模型中，关联，以及如何编辑和删除来自现有记录的二进制数据。

我们接下来的教程介绍了各种缓存技术。 缓存提供了一种方式来提高应用程序 s 通过采用来自成本高昂的操作的结果，并将它们存储在可以更快地访问的位置的整体性能。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](including-a-file-upload-option-when-adding-a-new-record-cs.md)
[下一页](uploading-files-vb.md)
