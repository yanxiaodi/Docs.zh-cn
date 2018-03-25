---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
title: "声明性参数 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将演示如何使用参数设置为硬编码的值来选择说明控件中显示的数据。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 603c9bd3-b895-4ec6-853b-0c81ff36d580
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
msc.type: authoredcontent
ms.openlocfilehash: 4bbe32ae65f71196fc1f939671b9d1a24bee8c34
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="declarative-parameters-c"></a>声明性参数 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_5_CS.exe)或[下载 PDF](declarative-parameters-cs/_static/datatutorial05cs1.pdf)

> 在本教程中我们将演示如何使用参数设置为硬编码的值来选择说明控件中显示的数据。


## <a name="introduction"></a>介绍

在[最后一个教程](displaying-data-with-the-objectdatasource-cs.md)我们看使用 GridView、 说明如何和 FormView 控件绑定到调用 ObjectDataSource 控件中显示数据`GetProducts()`方法从`ProductsBLL`类。 `GetProducts()`方法返回填充从 Northwind 数据库的记录的所有强类型 DataTable`Products`表。 `ProductsBLL`类包含用于返回只是子集的产品的其他方法`GetProductByProductID(productID)`， `GetProductsByCategoryID(categoryID)`，和`GetProductsBySupplierID(supplierID)`。 这些三个方法需要输入的参数，该值指示如何筛选返回的产品信息。

调用方法，应输入的参数，但为了实现这一点我们必须指定这些参数的值的来源，可以使用对象数据源。 参数值可以是硬编码，也可以来自各种动态源，包括： 查询字符串值，会话变量上页上，或其他人的 Web 控件的属性值。

对于本教程让我们先来说明如何使用参数设置为硬编码的值。 具体而言，我们将考察将说明如何添加到显示有关特定产品，即 Chef 秋葵汤，它具有的信息的页`ProductID`为 5。 接下来，我们将了解如何设置基于 Web 控件的参数值。 具体而言，我们将使用文本框中以使用户能够在一个国家/地区，此后，它们可以单击一个按钮以查看位于该国家/地区的供应商提供的列表中键入。

## <a name="using-a-hard-coded-parameter-value"></a>使用硬编码参数值

对于第一个示例中，首先，通过添加说明控件与`DeclarativeParams.aspx`页面`BasicReporting`文件夹。 从说明的智能标记，选择&lt;新数据源&gt;从下拉列表并选择要添加对象数据源。


[![将对象数据源添加到页](declarative-parameters-cs/_static/image2.png)](declarative-parameters-cs/_static/image1.png)

**图 1**： 将对象数据源添加到页 ([单击以查看实际尺寸的图像](declarative-parameters-cs/_static/image3.png))


这将自动启动 ObjectDataSource 控件选择数据源向导。 选择`ProductsBLL`向导的第一个屏幕中的类。


[![选择 ProductsBLL 类](declarative-parameters-cs/_static/image5.png)](declarative-parameters-cs/_static/image4.png)

**图 2**： 选择`ProductsBLL`类 ([单击以查看实际尺寸的图像](declarative-parameters-cs/_static/image6.png))


由于我们想要显示有关特定产品的信息，我们想要使用`GetProductByProductID(productID)`方法。


[![选择 GetProductByProductID(productID) 方法](declarative-parameters-cs/_static/image8.png)](declarative-parameters-cs/_static/image7.png)

**图 3**： 选择`GetProductByProductID(productID)`方法 ([单击以查看实际尺寸的图像](declarative-parameters-cs/_static/image9.png))


由于我们选择了此方法包含一个参数，因此不为向导中，我们询问来定义要用于参数的值的一个多个屏幕。 在左侧的列表将显示所有所选方法的参数。 有关`GetProductByProductID(productID)`只有一个`productID`。 在右侧，我们可以指定所选的参数的值。 参数源下拉列表枚举参数值的各种可能来源。 由于我们想要指定硬编码值为 5`productID`参数，将为无参数源和默认值文本框中输入 5。


[![Hard-Coded 参数值的 5 将用于 productID 参数](declarative-parameters-cs/_static/image11.png)](declarative-parameters-cs/_static/image10.png)

**图 4**: A Hard-Coded 参数值的 5 将用于`productID`参数 ([单击以查看实际尺寸的图像](declarative-parameters-cs/_static/image12.png))


完成配置数据源向导后，ObjectDataSource 控件的声明性标记包括`Parameter`对象在`SelectParameters`集合中定义的方法所需的输入参数的每个`SelectMethod`属性。 由于我们将在此示例中使用的方法要求只是单个输入的参数`parameterID`，此处只有一个条目。 `SelectParameters`集合可以包含任何派生自的类`Parameter`类`System.Web.UI.WebControls`命名空间。 为硬编码的参数值基`Parameter`使用类时，但其他参数源选项派生`Parameter`类; 你也可以创建你自己[自定义参数类型](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11)在需要时。

[!code-aspx[Main](declarative-parameters-cs/samples/sample1.aspx)]

> [!NOTE]
> 如果您一直在你自己的计算机上的声明性标记你此时看到可能包括值`InsertMethod`， `UpdateMethod`，和`DeleteMethod`属性，以及`DeleteParameters`。 ObjectDataSource 选择数据源向导自动指定的方法从`ProductBLL`要用于插入、 更新和删除，因此除非你显式清除这些情况，它们将包含在上面的标记。


Web 控件的数据时访问此页，将调用 ObjectDataSource`Select`方法，将调用该方法`ProductsBLL`类的`GetProductByProductID(productID)`方法使用硬编码值为 5`productID`输入的参数。 该方法将返回一个强类型`ProductDataTable`包含一行，Chef 秋葵汤有关的信息的对象 (与产品`ProductID`5)。


[![显示信息有关 Chef 秋葵汤](declarative-parameters-cs/_static/image14.png)](declarative-parameters-cs/_static/image13.png)

**图 5**： 显示信息有关 Chef 秋葵汤 ([单击以查看实际尺寸的图像](declarative-parameters-cs/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>将参数值设置为 Web 控件的属性值

此外可以设置值的对象数据源的参数基于页上的 Web 控件的值。 若要说明这一点，让我们具有一个 GridView，列出所有供应商在位于用户指定一个国家/地区。 若要通过将一个文本框添加到用户可以在其中输入国家/地区名称页来完成本入门。 设置此文本框中控件的`ID`属性`CountryName`。 此外添加按钮 Web 控件。


[![将一个文本框添加到具有 ID 国家/地区名称的页面](declarative-parameters-cs/_static/image17.png)](declarative-parameters-cs/_static/image16.png)

**图 6**： 将一个文本框添加到与页`ID` `CountryName` ([单击以查看实际尺寸的图像](declarative-parameters-cs/_static/image18.png))


接下来，向页上，以及从智能标记，添加一个 GridView 选择添加新对象数据源。 由于我们想要显示供应商信息选择`SuppliersBLL`从向导的第一个屏幕的类。 从第二个屏幕中，选取`GetSuppliersByCountry(country)`方法。


[![选择 GetSuppliersByCountry(country) 方法](declarative-parameters-cs/_static/image20.png)](declarative-parameters-cs/_static/image19.png)

**图 7**： 选择`GetSuppliersByCountry(country)`方法 ([单击以查看实际尺寸的图像](declarative-parameters-cs/_static/image21.png))


由于`GetSuppliersByCountry(country)`方法具有一个输入的参数，该向导再一次包括选择参数值的最后一个屏幕。 此时，将参数源设置控制。 这将填充页; 上的控件的名称与 ControlID 下拉列表选择`CountryName`从列表的控件。 当首次访问页`CountryName`文本框中将为空白，因此未返回任何结果，并且不显示任何内容。 如果你想要显示默认情况下的某些结果，请相应地设置默认值文本框。


[![将参数值设置为国家/地区名称控件值](declarative-parameters-cs/_static/image23.png)](declarative-parameters-cs/_static/image22.png)

**图 8**： 将参数值设置为`CountryName`控件值 ([单击以查看实际尺寸的图像](declarative-parameters-cs/_static/image24.png))


ObjectDataSource 的声明性标记会稍有不同我们的第一个示例，使用[ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx)而不是标准`Parameter`对象。 A`ControlParameter`具有附加属性以指定`ID`的 Web 控件与要将用于该参数的属性值 (`PropertyName`)。 配置数据源向导的时间足够智能，可确定，对于文本框中，我们将可能希望使用`Text`参数值的属性。 如果你想要使用不同的属性值从 Web 控件的但是，你可以更改`PropertyName`此处或通过单击向导中的"显示高级属性"链接的值。

[!code-aspx[Main](declarative-parameters-cs/samples/sample2.aspx)]

在首次访问页时`CountryName`文本框中为空。 ObjectDataSource `Select` GridView，而的值仍调用方法`null`传递到`GetSuppliersByCountry(country)`方法。 TableAdapter 将转换`null`到数据库`NULL`值 (`DBNull.Value`)，但使用的查询`GetSuppliersByCountry(country)`方法编写，因此它不返回任何值时`NULL`为指定值`@CategoryID`参数。 简单地说，会不返回任何供应商。

在距访客但是，输入在一个国家/地区，并单击显示供应商按钮会导致回发，ObjectDataSource 的后`Select`方法重新查询时，在文本框控件中传递`Text`值作为`country`参数。


[![显示从加拿大这些供应商](declarative-parameters-cs/_static/image26.png)](declarative-parameters-cs/_static/image25.png)

**图 9**： 显示那些供应商的加拿大 ([单击以查看实际尺寸的图像](declarative-parameters-cs/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>显示默认情况下的所有供应商

而是不是第一次查看网页时将供应商的全不显示我们可能想要显示*所有*供应商首先，允许用户削减通过在文本框中输入国家/地区名称的列表。 当文本框中为空，`SuppliersBLL`类的`GetSuppliersByCountry(country)`方法传递中`null`值，则为其 *`country`* 输入的参数。 这`null`然后容器值传到 DAL 的`GetSupplierByCountry(country)`方法，其中对其进行转换到数据库`NULL`值`@Country`在下面的查询的参数：

[!code-sql[Main](declarative-parameters-cs/samples/sample3.sql)]

表达式`Country = NULL`始终返回 False，即使对于记录其`Country`列具有`NULL`值; 因此，返回任何记录。

若要返回*所有*供应商国家/地区文本框中为空时，我们可以增加`GetSuppliersByCountry(country)`中 BLL 来调用方法`GetSuppliers()`方法时的国家/地区参数是`null`并调用 DAL 的`GetSuppliersByCountry(country)`否则为的方法。 这将产生返回时不指定任何国家/地区的所有供应商和供应商提供适当的子集，包括国家/地区参数时的影响。

更改`GetSuppliersByCountry(country)`中的方法`SuppliersBLL`以下类：

[!code-csharp[Main](declarative-parameters-cs/samples/sample4.cs)]

进行此更改`DeclarativeParams.aspx`页显示所有供应商在首次访问时 (或每当`CountryName`文本框中为空)。


[![所有供应商是默认情况下现在显示](declarative-parameters-cs/_static/image29.png)](declarative-parameters-cs/_static/image28.png)

**图 10**： 所有供应商是默认情况下现在显示 ([单击以查看实际尺寸的图像](declarative-parameters-cs/_static/image30.png))


## <a name="summary"></a>摘要

为了使用输入参数的方法，我们需要在对象数据源的指定参数的值`SelectParameters`集合。 要从不同数据源获取的参数值允许不同类型的参数。 默认参数类型使用硬编码的值，但一样轻松地 （以及不带的一行代码） 可以从查询字符串、 会话变量、 cookie 和 Web 页上的控件中甚至用户输入的值获取参数值。

我们在本教程中查看的示例说明如何使用声明性的参数值。 但是，那里可能是当我们需要使用不可用，当前日期和时间，例如参数源的时间或者，如果我们的站点使用的成员资格用户的访问者用户 ID。 对于这种情况下我们可以设置 ObjectDataSource 之前以编程方式的参数值调用其基础对象的方法。 我们将了解如何完成此中[下一教程](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Giesenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](displaying-data-with-the-objectdatasource-cs.md)
[下一页](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
