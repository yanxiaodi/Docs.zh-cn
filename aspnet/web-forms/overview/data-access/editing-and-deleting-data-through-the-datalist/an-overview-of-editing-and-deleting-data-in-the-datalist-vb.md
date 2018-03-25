---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: "编辑和删除 DataList (VB) 中的数据的概述 |Microsoft 文档"
author: rick-anderson
description: "DataList 缺少内置编辑和删除功能，而在本教程中我们将了解如何创建支持编辑和删除 o DataList..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: e08b55f763677a40a03503e54a23dc77a10a34f5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>编辑和删除 DataList (VB) 中的数据的概述
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe)或[下载 PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> DataList 缺少内置编辑和删除功能，而在本教程中我们将了解如何创建支持编辑和删除其基础数据的 DataList。


## <a name="introduction"></a>介绍

在[概述的插入、 更新和删除数据](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)我们介绍了如何插入、 更新和删除数据使用应用程序体系结构时，对象数据源，以及 GridView、 说明，和 FormView 的教程控件。 使用对象数据源和这些三个数据 Web 控件，实现简单的数据修改接口已管理单元和涉及仅计时从智能标记的复选框。 需要编写任何代码。

遗憾的是，DataList 缺少内置编辑和删除 GridView 控件中的固有的功能。 此缺少的功能是在某种由于 DataList 是从以前版本的 ASP.NET，relic，当声明性数据源控件和无需编码的数据修改页不可用。 虽然在 ASP.NET 2.0 DataList 不提供现成的相同数据修改功能作为 GridView，我们可以使用 ASP.NET 1.x 技术包括这样的功能。 此方法需要的代码，但 DataList 的位置，以帮助在此过程中我们将在本教程中看到的具有某些事件和属性。

在本教程中我们将了解如何创建支持编辑和删除其基础数据的 DataList。 更高级的编辑和删除方案 （包括输入的字段验证），适当地处理从数据访问层或业务逻辑层和等等引发的异常，将检查将来的教程。

> [!NOTE]
> DataList，如中继器控件缺少外插入、 更新或删除的框功能。 虽然可以添加此类功能，但是 DataList 包括属性和简化添加此类功能的事件中转发器找不到。 因此，本教程将来的以及查看编辑和删除将焦点严格 DataList。


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>步骤 1： 创建编辑和删除教程网页

我们开始浏览如何更新和删除数据从 DataList 之前，让我们来首先花一些时间，我们需要用于本教程和下一步的几个的网站项目中创建 ASP.NET 页。 首先，通过添加一个名为的新文件夹`EditDeleteDataList`。 接下来，添加到该文件夹，并确保将每个包含网页相关联的以下 ASP.NET 页面`Site.master`母版页：

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![本教程中添加的 ASP.NET 页面](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**图 1**： 本教程中添加的 ASP.NET 页面


在其他文件夹中，如`Default.aspx`中`EditDeleteDataList`文件夹其部分中列出这些教程。 回想一下，`SectionLevelTutorialListing.ascx`用户控件提供此功能。 因此，此用户将控件添加到`Default.aspx`通过从解决方案资源管理器拖到页面上的设计视图拖动。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**图 2**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


最后，将页添加到的条目为`Web.sitemap`文件。 具体而言，在主/详细信息报表后添加以下标记，使用 DataList 和转发器`<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 在左侧菜单现在包括 DataList 编辑和删除教程项。


![站点图现在包括 DataList 编辑和删除教程的条目](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**图 3**： 站点图现在包括 DataList 编辑和删除教程的条目


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>第 2 步： 检查更新和删除数据的技术

编辑和删除与 GridView 的数据是非常简单，因为这背后的 GridView 和 ObjectDataSource 协同工作。 中所述[检查与插入、 更新和删除的事件相关联](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)教程中，单击行的更新按钮时，GridView 自动分配使用双向数据绑定到其字段`UpdateParameters`其对象数据源的集合，然后调用该 ObjectDataSource 的`Update()`方法。

遗憾的是，DataList 不提供任何此内置的功能。 我们有责任以确保用户 s 将值赋给 ObjectDataSource 的参数且其`Update()`调用方法。 为了帮助我们在此任务，DataList 提供的以下属性和事件：

- **[ `DataKeyField`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**当更新或删除时，我们需要能够唯一识别 DataList 中的每一项。 将此属性设置为显示的数据的主键字段。 这样将填充 DataList s [ `DataKeys`集合](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx)具有指定`DataKeyField`DataList 的每个项的值。
- **[ `EditCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**按钮、 LinkButton 或 ImageButton 时激发其`CommandName`属性设置为单击编辑。
- **[ `CancelCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**按钮、 LinkButton 或 ImageButton 时激发其`CommandName`属性设置为单击取消。
- **[ `UpdateCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**按钮、 LinkButton 或 ImageButton 时激发其`CommandName`属性设置为在单击更新。
- **[ `DeleteCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**按钮、 LinkButton 或 ImageButton 时激发其`CommandName`属性设置为单击删除。

使用这些属性和事件，有四种方法可以用于更新和删除数据从 DataList:

1. **使用 ASP.NET 1.x 技术**DataList ASP.NET 2.0 和 ObjectDataSources 之前存在并已能够更新和删除数据完全通过编程方式。 此方法完全 ditches ObjectDataSource 和要求，我们将数据绑定到 DataList 直接从业务逻辑层，不管是在检索要显示的数据和更新或删除记录时。
2. **用于选择、 更新和删除的页上的单个 ObjectDataSource 控件**时 DataList 缺少 GridView s 固有编辑和删除功能，这里 s 无端我们可以 t 以添加，并自行。 使用此方法，我们在 GridView 示例中，就像使用对象数据源，但必须为 DataList s 创建事件处理程序`UpdateCommand`事件中： 我们在其中设置 ObjectDataSource 的参数和调用其`Update()`方法。
3. **ObjectDataSource 控件用于选择，但更新和删除直接针对 BLL**使用选项 2 时，我们需要编写的代码中`UpdateCommand`事件，依次类推分配参数值。 相反，我们可以停在与使用对象数据源进行选择，但进行更新和删除直接针对 BLL （例如，使用选项 1） 调用。 在我看来，通过直接接触 BLL 更新数据，就会导致更具可读性代码比分配 ObjectDataSource s`UpdateParameters`并调用其`Update()`方法。
4. **使用声明性方式通过多个 ObjectDataSources**以前所有的三种方法需要的代码。 如果你 d 而继续使用如尽可能得多的声明性语法，最后一个选项是包括在页上的多个 ObjectDataSources。 第一个对象数据源从 BLL 中检索数据，并将其绑定到 DataList。 为了更新，添加，但直接在 DataList s 内添加另一个 ObjectDataSource `EditItemTemplate`。 若要包括删除支持，尚未另一个对象数据源需要在`ItemTemplate`。 使用此方法，这些嵌入 ObjectDataSource 的使用`ControlParameters`若要以声明方式将 ObjectDataSource 的参数绑定到用户输入控件 (而不是无需在 DataList s 以编程方式指定`UpdateCommand`事件处理程序)。 此方法仍要求的代码，我们需要调用嵌入的 ObjectDataSource s`Update()`或`Delete()`命令，但需要远远小于与其他三种方法。 此处的缺点是，多个 ObjectDataSources 执行混乱不堪页上，使整体可读性。

如果强制永远只有使用其中一种方法，我选择选项 1，因为它提供了最大的灵活性和因为 DataList 最初设计用于容纳此模式。 DataList 已扩展到使用 ASP.NET 2.0 数据源控件，但它没有的所有扩展性点或官方 ASP.NET 2.0 数据 （GridView，说明如何和 FormView） 的 Web 控件的功能。 选项 2 至 4 但是，也不值得，没有。

这和将来编辑和删除教程将使用对象数据源用于检索数据可以显示并将调用定向到 BLL 更新和删除数据 （选项 3）。

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>步骤 3： 添加 DataList 和配置其 ObjectDataSource

在本教程中我们将创建 DataList 列出产品信息，并为每个产品，提供用户编辑名称和价格，并完全删除该产品的功能。 具体而言，我们将检索的记录，以显示使用对象数据源，但执行更新和删除操作通过直接与 BLL 进行连接。 我们担心实现 DataList 的编辑和删除功能之前，让我们来首先要获取页后，可以在只读的界面中显示的产品。 因为我们已将检查这些步骤在上一个教程中，我将继续访问这些快速。

首先打开`Basics.aspx`页面`EditDeleteDataList`文件夹，然后从设计视图中，添加 DataList 到页。 接下来，从 DataList s 智能标记，创建新对象数据源。 由于我们正在使用产品数据，将其配置为使用`ProductsBLL`类。 若要检索*所有*产品，选择`GetProducts()`中选择的选项卡上的方法。


[![配置对象数据源以使用 ProductsBLL 类](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**图 4**： 配置使用 ObjectDataSource`ProductsBLL`类 ([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![返回使用 GetProducts() 方法的产品信息](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**图 5**： 返回产品信息使用`GetProducts()`方法 ([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


DataList，如 GridView，不适用于插入新数据;因此，选择插入选项卡中的下拉列表从 (None) 选项。此外选择 （无） 为 UPDATE 和 DELETE 选项卡由于将通过 BLL 以编程方式执行更新和删除操作。


[![确认下拉列表中 ObjectDataSource 的插入、 更新和删除选项卡设置为 （无）](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**图 6**： 确认，ObjectDataSource 的插入、 更新和删除选项卡中的下拉列表设置为 （无） ([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


在配置对象数据源后，单击完成返回到设计器。 正如我们看到在过去的示例中，当自动完成对象数据源配置，Visual Studio 的已创建`ItemTemplate`的下拉列表中，显示每个数据字段。 将此替换`ItemTemplate`具有一个显示产品的名称和价格。 此外，还要设置`RepeatColumns`属性设置为 2。

> [!NOTE]
> 中所述*概述的插入、 更新和删除数据*教程中，修改数据使用我们的体系结构需要我们删除 ObjectDataSource 时`OldValuesParameterFormatString`从 ObjectDataSource 的属性声明性的标记 (或其重置为其默认值， `{0}`)。 在本教程中，但是，我们将使用 ObjectDataSource 仅用于检索数据。 因此，我们不需要修改 ObjectDataSource 的`OldValuesParameterFormatString`属性值 (尽管它不会降低，若要这样做)。


替换默认 DataList 后`ItemTemplate`用一个自定义在页面上的声明性标记应类似于以下：


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

花些时间查看我们通过浏览器的进度。 如图 7 所示，DataList 两个列中显示每个产品的产品名称和单位价格。


[![在两个列 DataList 中显示的产品名称和价格](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**图 7**: 产品名称和产品价格显示在两个列 DataList ([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> DataList 具有多个更新和删除过程中，所需的属性和视图状态中存储这些值。 因此，当生成 DataList，该支持编辑或删除数据，很关键，启用 DataList 的视图状态。  
>   
>  敏锐读取器可能还记得我们能够在创建可编辑 GridViews、 DetailsViews 和 FormViews 时禁用视图状态。 这是因为 ASP.NET 2.0 Web 控件可以包含*控制状态*，后者在类似于视图状态，但被视为基本的回发保持状态。


正在禁用视图中 GridView 状态只是未包含普通的状态信息，但维护 （其中包括用于编辑和删除所需的状态） 的控件状态。 DataList，具有在 ASP.NET 1.x 的时间范围内，已创建不使用控件状态，因此必须具有启用视图状态。 请参阅[控件状态 vs。视图状态](https://msdn.microsoft.com/library/1whwt1k7.aspx)有关详细信息的控件状态和与视图状态的不同目的。

## <a name="step-4-adding-an-editing-user-interface"></a>步骤 4： 添加用于编辑的用户界面

GridView 控件组成的字段 （BoundFields、 CheckBoxFields、 TemplateFields，等） 的集合。 这些字段可以调整其呈现的标记，具体取决于其模式。 例如，在只读模式下，BoundField 显示其数据字段值作为文本;当处于编辑模式呈现文本框中 Web 控件`Text`属性分配数据字段值。

DataList，另一方面，呈现其使用模板的项。 只读项呈现使用`ItemTemplate`而处于编辑模式项呈现通过`EditItemTemplate`。 此时，我们 DataList 只有`ItemTemplate`。 若要支持项目级编辑功能，我们需要添加`EditItemTemplate`，其中包含要为可编辑的项显示的标记。 对于本教程，我们将使用以进行编辑产品的名称和单价的文本框中 Web 控件。

`EditItemTemplate`可以 （通过选择编辑模板选项中 DataList s 智能标记） 以声明方式或通过设计器创建。 若要使用编辑模板选项，请首先单击智能标记中的编辑模板链接，然后选择`EditItemTemplate`从下拉列表的项。


[![选择以使用 DataList 的 EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**图 8**： 选择以使用 DataList s `EditItemTemplate` ([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


接下来，在产品名称中键入: 和价格： 然后将两个文本框控件拖动到工具箱从`EditItemTemplate`设计器上的接口。 设置文本框中`ID`属性设置为`ProductName`和`UnitPrice`。


[![添加产品的名称和价格的文本框](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**图 9**： 添加文本框的产品名称和价格 ([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


我们需要将绑定到的相应产品数据字段值`Text`的两个文本框中的属性。 从文本框中的智能标记，单击编辑数据绑定链接，然后将关联具有的适当的数据字段`Text`属性，如图 10 中所示。

> [!NOTE]
> 在绑定时`UnitPrice`到文本框中 s 的价格数据字段`Text`字段中，您可能将其格式化为货币值 (`{0:C}`)，常规数 (`{0:N}`)，或将其保留为未格式化。


![将产品名称和单价数据字段绑定到文本框中的文本属性](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**图 10**： 绑定`ProductName`和`UnitPrice`数据字段设置为`Text`的文本框中的属性


请注意图 10 中的编辑数据绑定对话框的工作原理*不*包括编辑在 GridView 或说明，为 TemplateField 或 FormView 中的模板时显示的双向数据绑定复选框。 允许文本框输入的 Web 控件，若要自动分配到相应的 ObjectDataSource s 中输入的值使用双向数据绑定功能`InsertParameters`或`UpdateParameters`插入或更新数据时。 DataList 不支持双向数据绑定，因为我们将有更高版本上，在本教程中，了解用户使她更改和已准备好更新数据后，我们将需要以编程方式访问这些文本框中`Text`属性和传入到其值适当`UpdateProduct`中的方法`ProductsBLL`类。

最后，我们需要添加更新和取消按钮`EditItemTemplate`。 正如我们看到在[母版/详细介绍 Master 记录项目符号列表使用详细信息 DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)教程中，按钮、 LinkButton 或 ImageButton 时其`CommandName`属性设置中的转发器或 DataList，单击从转发器或 DataList 的`ItemCommand`引发事件。 有关 DataList，如果`CommandName`属性设置为某个值，则可能也引发事件的附加事件。 这两个特殊`CommandName`内的其他属性值包括：

- 取消引发`CancelCommand`事件
- 编辑引发`EditCommand`事件
- 更新引发`UpdateCommand`事件

请记住，会引发这些事件*除了*`ItemCommand`事件。

将添加到`EditItemTemplate`两个按钮 Web 控件、 一个其`CommandName`设置为更新和其他 s 设置为取消。 后添加这两个按钮 Web 控件设计器应看起来类似于以下：


[![添加更新和取消按钮 EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**图 11**： 添加更新和取消按钮添加到`EditItemTemplate`([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


与`EditItemTemplate`完成 DataList s 声明性标记应类似于以下：


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>步骤 5： 添加联结进入编辑模式

此时我们 DataList 具有编辑界面通过定义其`EditItemTemplate`; 但是，此处 s 当前无法访问我们的页面的用户指示他想要编辑产品的信息。 我们需要将一个编辑按钮添加到每个产品，单击时，呈现该 DataList 项处于编辑模式。 首先，通过添加到一个编辑按钮`ItemTemplate`，通过设计器或以声明方式。 请务必设置编辑按钮的`CommandName`编辑的属性。

已添加此编辑按钮后，需要一段时间来查看通过浏览器的页。 添加此元素后，每个产品列表应包含编辑按钮。


[![添加更新和取消按钮 EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**图 12**： 添加更新和取消按钮添加到`EditItemTemplate`([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


单击按钮会导致回发，但不*不*使产品列表置于编辑模式。 若要使产品可编辑，我们需要：

1. 设置 DataList s [ `EditItemIndex`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)的索引`DataListItem`只需单击其编辑按钮。
2. 重新绑定数据到 DataList。 DataList 时都会重新生成时`DataListItem`其`ItemIndex`对应 DataList s`EditItemIndex`将呈现使用其`EditItemTemplate`。

由于 DataList s`EditCommand`单击编辑按钮时激发事件，创建`EditCommand`事件处理程序替换为以下代码：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

`EditCommand`事件处理程序传递的类型对象中`DataListCommandEventArgs`作为其第二个输入参数，其中包括对引用`DataListItem`其编辑按钮被单击 (`e.Item`)。 事件处理程序首先设置 DataList s`EditItemIndex`到`ItemIndex`的可编辑`DataListItem`然后将数据转换为 DataList 重新绑定通过调用 DataList 的`DataBind()`方法。

在添加此事件处理程序之后, 重新访问浏览器中的页。 现在单击编辑按钮使单击的产品可编辑 （请参阅图 13）。


[![单击编辑按钮使可编辑产品](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**图 13**： 单击编辑按钮使产品可编辑 ([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>步骤 6： 保存用户的更改

单击编辑的产品 s 更新或取消按钮此时; 未执行任何操作若要添加此功能，我们需要创建的事件处理程序 DataList s`UpdateCommand`和`CancelCommand`事件。 首先创建`CancelCommand`单击编辑的产品 s 取消按钮，且它担负使 DataList 恢复为其预编辑状态时将执行的事件处理程序。

若要让 DataList 呈现其所有项中的只读模式，我们需要：

1. 设置 DataList s [ `EditItemIndex`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)的不存在的索引`DataListItem`索引。 `-1`由于是安全的选择，`DataListItem`索引开始`0`。
2. 重新绑定数据到 DataList。 由于否`DataListItem` `ItemIndex` es 对应于 DataList 的`EditItemIndex`，整个 DataList 将呈现在只读模式下。

可以使用下面的事件处理程序代码完成这些步骤：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

添加此元素后，单击取消按钮返回 DataList 为其预编辑状态。

我们需要完成最后一个事件处理程序是`UpdateCommand`事件处理程序。 此事件处理程序需要：

1. 以编程方式访问的用户输入的产品名称和价格，以及编辑的产品的`ProductID`。
2. 启动更新过程，通过调用适当`UpdateProduct`重载中`ProductsBLL`类。
3. 设置 DataList s [ `EditItemIndex`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)的不存在的索引`DataListItem`索引。 `-1`由于是安全的选择，`DataListItem`索引开始`0`。
4. 重新绑定数据到 DataList。 由于否`DataListItem` `ItemIndex` es 对应于 DataList 的`EditItemIndex`，整个 DataList 将呈现在只读模式下。

步骤 1 和 2 负责保存 s 更改; 的用户步骤 3 和 4 DataList 后返回到其预编辑状态更改已保存，并且在中执行的步骤相同`CancelCommand`事件处理程序。

若要获取的更新的产品名称和价格，我们需要使用`FindControl`为以编程方式引用文本框中 Web 控件中的方法`EditItemTemplate`。 我们还需要获取编辑的产品的`ProductID`值。 Visual Studio 时我们最初绑定到 DataList ObjectDataSource，分配 DataList s`DataKeyField`到的主键值从数据源的属性 (`ProductID`)。 此值可以然后从 DataList s 检索`DataKeys`集合。 花一些时间，请确保`DataKeyField`属性确实设置为`ProductID`。

下面的代码实现的四个步骤：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

通过编辑产品 s 中读取的事件处理程序启动`ProductID`从`DataKeys`集合。 下一步中的两个文本框`EditItemTemplate`引用及其`Text`属性存储在本地变量，`productNameValue`和`unitPriceValue`。 我们使用`Decimal.Parse()`方法来读取值`UnitPrice`文本框中，因此，如果输入的值具有货币符号，它可仍将正确转换为`Decimal`值。

> [!NOTE]
> 从值`ProductName`和`UnitPrice`文本框中仅分配给的 productNameValue 和 unitPriceValue 变量，如果文本框中文本属性具有指定的值。 否则为值为`Nothing`用于变量，它具有与数据库中更新的数据的效果`NULL`值。 即我们的代码将转换为空字符串转换为数据库`NULL`值，这是 GridView、 说明，和 FormView 控件中的编辑界面的默认行为。


在读取值之后,`ProductsBLL`类 s`UpdateProduct`调用方法、 传入产品的名称，价格和`ProductID`。 事件处理程序完成通过为预先编辑状态，使用完全相同的逻辑中返回 DataList`CancelCommand`事件处理程序。

与`EditCommand`， `CancelCommand`，和`UpdateCommand`事件处理程序完成，访问者可以编辑名称和产品价格。 图 14-16 在操作中展示此编辑工作流。


[![第一个中访问的页面，所有产品时处于只读模式](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**图 14**： 时第一次访问该页面，所有产品都处于只读模式 ([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![若要更新的产品名称的价格，请单击编辑按钮](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**图 15**： 若要更新的产品名称或价格，单击编辑按钮 ([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![更改值之后，请单击更新返回到只读模式](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**图 16**： 后更改的值，单击更新返回到只读模式 ([单击以查看实际尺寸的图像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>步骤 7： 添加删除功能

将删除功能添加到 DataList 的步骤是类似于添加编辑功能。 简单地说，我们需要添加到删除按钮`ItemTemplate`，单击时：

1. 相应的产品 s 中读取`ProductID`通过`DataKeys`集合。
2. 通过调用执行删除`ProductsBLL`类的`DeleteProduct`方法。
3. 重新绑定次数 DataList 到的数据。

让我们来通过添加到删除按钮来启动`ItemTemplate`。

单击按钮时其`CommandName`为编辑，更新或取消引发 DataList s`ItemCommand`以及其他事件的事件 (例如，当使用编辑`EditCommand`事件也会产生)。 同样，任何按钮、 LinkButton 或 DataList 中的 ImageButton 其`CommandName`属性设置为删除原因`DeleteCommand`事件激发 (连同`ItemCommand`)。

添加中的编辑按钮旁边的删除按钮`ItemTemplate`，设置其`CommandName`删除的属性。 在添加后，此按钮会控制你 DataList 的`ItemTemplate`声明性语法，应如下所示：


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

接下来，为 DataList s 创建事件处理程序`DeleteCommand`事件，使用下面的代码：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

单击删除按钮会导致回发并激发 DataList 的`DeleteCommand`事件。 事件处理程序中，单击产品 s`ProductID`值访问从`DataKeys`集合。 接下来，通过调用删除产品`ProductsBLL`类的`DeleteProduct`方法。

删除该产品后它 s 重要我们重新绑定数据到 DataList (`DataList1.DataBind()`)，否则 DataList 将继续显示的产品，只需删除。

## <a name="summary"></a>摘要

DataList 缺少点，并单击编辑和删除享受的 GridView 的支持，而进行少量的代码的短它可以得到增强，包括这些功能。 在本教程中我们已了解如何创建的产品，无法删除，无法编辑其名称和价格的两列列表。 添加编辑和删除支持是一种包括在适当的 Web 控件`ItemTemplate`和`EditItemTemplate`、 创建相应的事件处理程序、 读取用户输入和主键值，和业务进行交互逻辑层。

虽然我们已添加基本编辑和删除 DataList 的功能，但它缺少更高级的功能。 例如，没有任何输入的字段验证-如果用户输入指价格为太占用大量资源，将引发异常的`Decimal.Parse`时尝试转换太昂贵到`Decimal`。 同样，如果更新的数据在业务逻辑或数据访问层中的问题用户将看到与标准错误屏幕。 而无需任何种类的删除按钮确认消息，是所有太可能意外删除产品。

在将来教程，我们将了解如何改善用户的编辑体验。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Zack Jones、 Ken Pespisa 和徐 Schmidt。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](customizing-the-datalist-s-editing-interface-cs.md)
[下一页](performing-batch-updates-vb.md)
