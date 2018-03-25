---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: "显示与 ObjectDataSource (C#) 的数据 |Microsoft 文档"
author: rick-anderson
description: "本教程考察 ObjectDataSource 控件使用此控件，你可以将数据从以前的教程，而无需 havi 中创建 BLL 检索绑定..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bd6534b652735e657aa71cdf07dac48f20a549c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="displaying-data-with-the-objectdatasource-c"></a>显示与 ObjectDataSource (C#) 的数据
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe)或[下载 PDF](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> 本教程可以使用此控件，你可以将数据从前面的教程中创建无需编写一行代码 BLL 检索绑定 ObjectDataSource 控件去 ！


## <a name="introduction"></a>介绍

与我们的应用程序体系结构和网站页面布局完成，我们已准备好开始探索如何完成的各种常见数据和报表相关的任务。 在前面的教程，我们已了解如何以编程方式将数据从 DAL 和 BLL 绑定到数据 ASP.NET 页中的 Web 控件。 分配数据 Web 控件的此语法`DataSource`数据到显示，然后再调用控件的属性`DataBind()`方法已在 ASP.NET 1.x 应用程序，所使用的模式，并且可以继续在 2.0 应用程序中使用。 但是，ASP.NET 2.0 的新数据源控件提供以声明性方式处理数据。 使用这些控件你可以将数据从 BLL 检索的绑定在中创建[以前一教程](../introduction/creating-a-business-logic-layer-cs.md)而无需编写一行代码 ！

ASP.NET 2.0 附带有五个内置的数据源控件[SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx)， [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx)， [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx)， [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx)，和[SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx)尽管可以创建你自己[自定义数据源控件](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp)在需要时。 由于我们已为我们的教程应用程序开发一种体系结构，我们将使用 ObjectDataSource 针对我们 BLL 类。


![ASP.NET 2.0 包含五个内置的数据源控件](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**图 1**: ASP.NET 2.0 包含五个内置的数据源控件


ObjectDataSource 用作代理使用一些其他对象。 若要配置 ObjectDataSource 我们指定这基础对象和其方法如何映射到 ObjectDataSource `Select`， `Insert`， `Update`，和`Delete`方法。 后尚未指定此基础对象，其方法映射到对象数据源的我们然后可以将对象数据源绑定到数据 Web 控件。 ASP.NET 附带有很多数据 Web 控件，包括 GridView、 说明、 说明如何和的 DropDownList，等等。 在页面生命周期，Web 控件的数据可能需要访问的数据绑定到了，它将通过调用其 ObjectDataSource 完成`Select`方法; 如果数据 Web 控件支持插入、 更新或删除，调用可能会对其ObjectDataSource 的`Insert`， `Update`，或`Delete`方法。 这些调用然后路由到适当的基础对象的方法 ObjectDataSource 通过中，如下面的关系图所示。


[![ObjectDataSource 充当的代理](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**图 2**: ObjectDataSource 用作代理的 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image4.png))


尽管可以使用对象数据源来调用用于插入方法，更新，或删除数据，让我们只需专注于返回的数据;将来的教程将探讨使用对象数据源和数据修改数据的 Web 控件。

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>步骤 1： 添加和配置 ObjectDataSource 控件

首先打开`SimpleDisplay.aspx`页面`BasicReporting`文件夹中，切换到设计视图中，并从工具箱拖到页面的设计图面将 ObjectDataSource 控件。 ObjectDataSource 将显示为灰色的框，在设计图面上，因为它不会生成任何标记;通过调用一种方法从指定的对象，它只需访问数据。 返回对象数据源的数据可以显示由数据 Web 控件，如 GridView、 说明、 FormView，依次类推。

> [!NOTE]
> 或者，你可能会首先向页面添加 Web 控件的数据，然后，从其智能标记上，选择&lt;新数据源&gt;从下拉列表的选项。


若要指定对象数据源的基础对象和该对象的方法映射到对象数据源的方式，请单击从 ObjectDataSource 的智能标记的配置数据源链接。


[![单击配置中的智能标记的数据源链接](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**图 3**： 单击智能标记中的配置数据源链接 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image7.png))


这会打开配置数据源向导。 首先，我们必须指定要使用对象数据源的对象。 如果"显示仅数据组件"复选框后，此屏幕上的下拉列表将列出已修饰的那些对象`DataObject`属性。 目前我们的列表在类型化数据集和我们在前面的教程中创建 BLL 类中包括 Tableadapter。 如果你忘记添加`DataObject`属性到的业务逻辑层类你看不到他们此列表中。 在这种情况下，取消选中"显示仅数据组件"复选框以查看所有对象，其中应包括 BLL 类 （以及类型化数据集数据表、 Datarow，等中的其他类）。

在此第一个屏幕中选择`ProductsBLL`类从下拉列表并单击下一步。


[![通过 ObjectDataSource 控件指定使用对象](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**图 4**： 通过 ObjectDataSource 控件指定使用对象 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image10.png))


该向导的下一个屏幕会提示你选择哪种 ObjectDataSource 应调用的方法。 下拉列表将列出在从上一屏幕中选择的对象中返回数据的那些方法。 在这里我们看到`GetProductByProductID`， `GetProducts`， `GetProductsByCategoryID`，和`GetProductsBySupplierID`。 选择`GetProducts`方法从该下拉列表并单击完成 (如果你添加`DataObjectMethodAttribute`到`ProductBLL`的前面的教程中，此选项中所示的方法将选择默认情况下)。


[![选择将数据还原从选择的选项卡上的方法](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**图 5**： 返回的数据选择的方法，从选择选项卡 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>手动配置对象数据源

对象数据源的配置数据源向导提供了一个快速方法来指定它使用的对象并将关联对象的哪些方法调用。 但是，可以配置其属性，通过属性窗口或直接在声明性的标记中通过对象数据源。 只需设置`TypeName`为要使用的基础对象的类型的属性和`SelectMethod`到要在检索数据时调用的方法。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

即使你希望配置数据源向导可能有些时候当你需要手动配置对象数据源，因为此向导只列出开发人员创建的类。 如果你想要将对象数据源绑定到.NET Framework 中的类如[成员资格类](https://msdn.microsoft.com/library/system.web.security.membership.aspx)、 访问用户帐户信息或[Directory 类](https://msdn.microsoft.com/library/system.io.directory.aspx)要使用文件系统信息你将需要手动设置对象数据源的属性。

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>步骤 2： 添加数据 Web 控件，并将其绑定到 ObjectDataSource

添加到页面并配置对象数据源后，我们已准备好将数据 Web 控件添加到页后，可以显示所返回的对象数据源的数据`Select`方法。 Web 控件的任何数据可以绑定到 ObjectDataSource;让我们看一下在 GridView、 说明如何和 FormView 中显示对象数据源的数据。

## <a name="binding-a-gridview-to-the-objectdatasource"></a>绑定到对象数据源的 GridView

从工具箱拖到添加 GridView 控件`SimpleDisplay.aspx`的设计图面。 从 GridView 的智能标记，选择我们在步骤 1 中增加 ObjectDataSource 控件。 这将自动在每个属性返回的数据从 ObjectDataSource 的 GridView 中创建 BoundField`Select`方法 （即，由产品 DataTable 定义的属性）。


[![GridView 已添加到页面并将其绑定到 ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**图 6**: GridView 已添加到页上，然后绑定到 ObjectDataSource ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image16.png))


然后可以自定义、 重新排列，或通过单击智能标记中的编辑列选项移除 GridView BoundFields。


[![管理通过编辑列对话框中的 GridView BoundFields](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**图 7**： 管理 GridView BoundFields 通过编辑列对话框 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image19.png))


花一些时间来修改 GridView BoundFields，删除`ProductID`， `SupplierID`， `CategoryID`， `QuantityPerUnit`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`BoundFields。 只需从列表中左下角选择 BoundField，然后单击删除按钮 (红色的 X) 可将其删除。 接下来，重新排列 BoundFields 以便`CategoryName`和`SupplierName`BoundFields 之前`UnitPrice`BoundField 通过选择这些 BoundFields 并单击向上箭头。 设置`HeaderText`属性到剩余 BoundFields `Products`， `Category`， `Supplier`，和`Price`分别。 接下来，让`Price`BoundField 通过设置 BoundField 的情况下的格式设为货币`HtmlEncode`属性设置为 False 并将其`DataFormatString`属性`{0:c}`。 最后，水平对齐`Price`向右和`Discontinued`通过中心中的复选框`ItemStyle` / `HorizontalAlign`属性。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]


[![GridView BoundFields 已自定义](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**图 8**: GridView BoundFields 已自定义 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>使用一致的外观的主题

这些教程致力于删除任何控件级别样式设置，而不使用级联样式表定义尽可能与外部文件中。 `Styles.css`文件包含`DataWebControlStyle`， `HeaderStyle`， `RowStyle`，和`AlternatingRowStyle`应该用于规定数据的外观的 CSS 类 Web 这些教程中使用的控件。 若要实现此目的，我们无法设置 GridView`CssClass`属性`DataWebControlStyle`，并将其`HeaderStyle`， `RowStyle`，和`AlternatingRowStyle`属性的`CssClass`属性相应地。

如果我们将这些`CssClass`Web 控件，我们将需要显式设置的每个数据这些属性值，请务必在属性 Web 控件添加到我们的教程。 更易于管理的方法是为 GridView，说明如何，定义的默认 CSS 相关的属性和 FormView 控件使用的主题。 主题是控件级别的属性设置、 图像、 和到页可以应用跨站点来强制执行常见的外观和感觉的 CSS 类的集合。

我们主题不包含任何图像或 CSS 文件 (我们将其保留样式表`Styles.css`作为-在 web 应用程序的根文件夹中，定义)，但会包括两个外观。 外观是定义 Web 控件的默认属性的文件。 具体而言，我们将已外观为文件 GridView 和说明控件中，默认值，该值指示`CssClass`-与相关的属性。

通过将新的外观文件添加到名为的项目启动`GridView.skin`通过右键单击解决方案资源管理器中的项目名称并选择添加新项。


[![添加一个名为 GridView.skin 的外观文件](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**图 9**： 添加外观文件命名为`GridView.skin`([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image25.png))


外观文件需要放置在一个主题，其中位于`App_Themes`文件夹。 我们还没有这样的文件夹，因为 Visual Studio 中将请先将其提供为创建一个我们添加我们的第一个外观时。 单击是以创建`App_Theme`文件夹并将新`GridView.skin`文件存在。


[![让 Visual Studio 创建 App_Theme 文件夹](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**图 10**： 让 Visual Studio 创建`App_Theme`文件夹 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image28.png))


这将创建新主题中的`App_Themes`文件夹以外观文件命名 GridView `GridView.skin`。


![GridView 主题都有已添加到 App_Theme 文件夹](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**图 11**: GridView 主题都有已添加到`App_Theme`文件夹


将 GridView 主题重命名为 DataWebControls (右键单击 GridView 文件夹`App_Theme`文件夹，然后选择重命名)。 接下来，输入以下标记到`GridView.skin`文件：


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

这将定义的默认属性`CssClass`-与属性相关的任何页中使用 DataWebControls 主题中的任何 GridView。 有关说明，我们将很快使用的 Web 控件的数据，让我们添加另一个的外观。 添加到名为 DataWebControls 主题的新外观`DetailsView.skin`并添加以下标记：


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

与我们定义的主题，最后一步是将主题应用到我们 ASP.NET 页。 可以将主题应用基于按页或在网站中的所有页面。 让我们在网站中的所有页中都使用此主题。 若要完成此操作，将添加到以下标记`Web.config`的`<system.web>`部分：


[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

这就是所有到它 ！ `styleSheetTheme`设置指示主题中指定的属性应*不*重写控件级别指定的属性。 若要指定主题设置应能带来控制设置，使用`theme`属性代替了`styleSheetTheme`; 遗憾的是，通过指定的主题设置`theme`属性不显示在 Visual Studio 设计视图。 请参阅[ASP.NET 主题和皮肤概述](https://msdn.microsoft.com/library/ykzx33wh.aspx)和[服务器端样式使用主题](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)主题和皮肤; 的详细信息请参阅[How To： 应用 ASP.NET 主题](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx)有关的详细信息配置页后，可以使用主题。


[![GridView 显示产品的名称、 类别、 供应商、 价格和停用的信息](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**图 12**: GridView 显示产品的名称、 类别、 供应商、 价格和停用信息 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>在说明中一次显示一条记录

GridView 显示个返回的数据源控件绑定到每个记录都占一行。 有的时候，但是，我们可能想要一次显示唯一记录或仅一条记录。 [说明](https://msdn.microsoft.com/library/s3w1w7t4.aspx)提供此功能，从而呈现为 HTML`<table>`与两个列和每个列或属性绑定到控件的一个行。 可以将说明如何视为与单个记录的旋转 90 度 GridView。

首先，通过添加说明如何控制*上面*中的 GridView `SimpleDisplay.aspx`。 接下来，将其绑定到 GridView 作为相同 ObjectDataSource 控件。 如与 GridView，BoundField 将添加到对象数据源返回的对象中每个属性说明`Select`方法。 唯一的区别是水平而不是垂直说明的 BoundFields 的布局方式。


[![向页面添加说明并将其绑定到 ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**图 13**： 向页面添加说明并将其绑定到 ObjectDataSource ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image35.png))


GridView，如说明的 BoundFields 可以进行修改，以提供更多自定义的显示的对象数据源返回的数据。 图 14 显示说明后其 BoundFields 和`CssClass`已配置属性以使其外观类似于 GridView 示例。


[![说明如何显示单个记录](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**图 14**: 说明如何显示单个记录 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image38.png))


请注意，说明如何仅显示其数据源返回的第一个记录。 若要允许用户来单步执行的所有记录，一次一个地我们必须启用分页功能的说明。 为此，请返回到 Visual Studio，并检查中说明的智能标记的启用分页复选框。


[![在说明中启用分页](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**图 15**： 在说明中启用分页 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image41.png))


[![说明如何使用启用了分页，允许用户查看的任何产品](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**图 16**: 说明如何使用分页已启用，允许用户查看的任何产品 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image44.png))


我们将讨论有关分页在将来的教程的详细信息。

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>一次显示一条记录更灵活的布局

说明如何是非常刚性中从 ObjectDataSource 返回每个记录中的显示方式。 我们可能想更灵活的数据视图。 例如，而不是在单独的一行上显示产品的名称、 类别、 供应商、 价格和停用的信息，我们可能想要显示的产品名称并以定价`<h4>`标题下，显示的类别和供应商信息下面的名称和较小的字体大小的价格。 我们可能不满足要显示的值旁边的属性名称 （产品、 类别和等等）。

[FormView 控件](https://msdn.microsoft.com/library/fyf1dk77.aspx)提供此级别的自定义项。 FormView 而不是使用字段 （如的 GridView 和说明如何执行操作），使用允许的 Web 控件，静态 HTML 混合的模板和[数据绑定语法](http://www.15seconds.com/issue/040630.htm)。 如果你熟悉中继器控件从 ASP.NET 1.x，你可以将 FormView 视为显示单个记录转发器。

FormView 将控件添加到`SimpleDisplay.aspx`页的设计图面。 最初 FormView 将显示为灰色块，通知我们，我们需要至少情况下，提供控件的`ItemTemplate`。


[![FormView 必须包括 ItemTemplate](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**图 17**: FormView 必须包括`ItemTemplate`([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image47.png))


可以对通过 FormView 的智能标记，将创建的默认数据源控件直接绑定 FormView`ItemTemplate`自动 (连同`EditItemTemplate`和`InsertItemTemplate`，如果 ObjectDatatSource 控件的`InsertMethod`和`UpdateMethod`设置属性)。 但是，对于此示例中我们将数据绑定到 FormView 并指定其`ItemTemplate`手动。 通过设置 FormView 启动`DataSourceID`属性`ID`的 ObjectDataSource 控件， `ObjectDataSource1`。 接下来，创建`ItemTemplate`，使其显示产品的名称和价格中的`<h4>`元素和类别和发货方中的名称下的较小的字体大小。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]


[![首次产品 （牛奶） 显示在自定义格式](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**图 18**: 首次产品 （牛奶） 显示在自定义格式 ([单击以查看实际尺寸的图像](displaying-data-with-the-objectdatasource-cs/_static/image50.png))


`<%# Eval(propertyName) %>`是数据绑定的语法。 `Eval`方法返回绑定到 FormView 控件的当前对象的指定属性的值。 请参阅 Alex 荷马的文章[简体和扩展数据绑定中的语法 ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)有关数据绑定的详细信息细节。

说明如何，如 FormView 只显示从 ObjectDataSource 返回第一条记录。 你可以在中启用分页 FormView，允许访问者遍历一次一个的产品。

## <a name="summary"></a>摘要

而无需编写一行代码感谢 ASP.NET 2.0 ObjectDataSource 控件，可以实现访问，以及如何显示从业务逻辑层的数据。 ObjectDataSource 调用指定的方法的类，并返回结果。 这些结果可以显示在数据绑定到对象数据源的 Web 控件。 在本教程中我们将讨论在将 GridView、 说明如何和 FormView 控件绑定到对象数据源。

到目前为止我们仅已了解如何使用对象数据源来调用的参数的方法，但如果我们想要调用的方法，要求输入参数，如`ProductBLL`类的`GetProductsByCategoryID(categoryID)`？ 为了调用方法，要求一个或多个参数，我们必须配置对象数据源指定这些参数的值。 我们将了解如何完成此中我们[下一教程](declarative-parameters-cs.md)。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [创建你自己的数据源控件](https://msdn.microsoft.com/library/ms364049.aspx)
- [ASP.NET 2.0 的 GridView 示例](https://msdn.microsoft.com/library/aa479339.aspx)
- [简化和扩展数据绑定 ASP.NET 2.0 中的语法](http://www.15seconds.com/issue/040630.htm)
- [在 ASP.NET 2.0 中的主题](http://www.odetocode.com/Articles/423.aspx)
- [使用主题的服务器端样式](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [如何： 以编程方式应用 ASP.NET 主题](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Giesenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一篇](declarative-parameters-cs.md)
