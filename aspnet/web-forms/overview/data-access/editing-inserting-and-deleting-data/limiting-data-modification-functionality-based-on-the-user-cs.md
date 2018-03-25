---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: "限制数据修改功能根据用户 (C#) |Microsoft 文档"
author: rick-anderson
description: "中的 web 应用程序允许用户编辑数据，另一个用户帐户也可能具有不同数据编辑权限。 在本教程中我们将研究如何 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: 44d0c192e082a7ad123096acb57fd053f6dcaeb1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="limiting-data-modification-functionality-based-on-the-user-c"></a>限制基于用户 (C#) 的数据修改功能
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe)或[下载 PDF](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> 中的 web 应用程序允许用户编辑数据，另一个用户帐户也可能具有不同数据编辑权限。 在本教程中我们将研究如何动态调整基于正在访问的用户的数据修改功能。


## <a name="introduction"></a>介绍

大量的 web 应用程序支持用户帐户，并提供不同的选项、 报表和基于登录用户的功能。 例如，使用我们的教程我们可能想要允许用户从供应商公司可能-以及供应商信息，例如其公司名称、 在登录到他们的产品的其名称和每个单元，数量有关的站点和更新的常规信息地址、 联系人的信息和等等。 此外，我们可能想要从我们公司人士包括一些用户帐户，以便他们可以登录和更新产品信息，例如量股价图、 重新排序级别，和等等。 我们的 web 应用程序还可能允许匿名用户访问 （用户未登录），但会将它们限制为仅查看数据。 与此类用户帐户系统中的位置，我们将在 ASP.NET 页提供插入、 编辑和删除功能适用于当前登录的用户希望 Web 控件的数据。

在本教程中我们将研究如何动态调整基于正在访问的用户的数据修改功能。 具体而言，我们将创建显示中可编辑的说明，以及一个 GridView，列出由供应商提供的产品供应商信息页。 如果从我们公司用户访问的页面，他们可以： 查看任何供应商的信息;编辑其地址;和编辑对供应商提供的任何产品的信息。 如果，但是，用户是从特定的公司，它们可以仅查看和编辑自己的地址信息和只能编辑未标记为已停止使用其产品。


[![我们公司中的用户可以编辑任何供应商的信息](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**图 1**： 用户从我们公司可以编辑任何供应商的信息 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))


[![中的特定供应商只能查看和编辑其信息的用户](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**图 2**： 从特定供应商可以仅查看和编辑其信息的用户 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))


让我们来开始 ！

> [!NOTE]
> ASP.NET 2.0 s 成员资格系统提供标准化、 可扩展的平台，用于创建、 管理和验证用户帐户。 由于超出范围的这些教程是的成员资格系统检查，本教程改用"fakes"成员身份通过允许匿名访问者选择它们是否从特定的供应商或我们的公司。 有关成员身份的详细信息，请参阅我[检查 ASP.NET 2.0 s 成员资格、 角色和配置文件](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)文章系列。


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>步骤 1： 允许用户指定其访问权限

在实际的 web 应用程序中，用户的帐户信息将包括是否为我们的公司或特定供应商，它们的工作和用户登录到站点后会将来自我们 ASP.NET 页以编程方式访问此信息。 作为用户级帐户信息通过配置文件系统中，或某些自定义方法，无法通过 ASP.NET 2.0 的角色系统，捕获此信息。

由于本教程的目的是为了演示调整登录的用户，所基于的数据修改功能而并不意味着展示 ASP.NET 2.0 s 成员资格、 角色和配置文件系统，我们将使用的非常简单机制来确定用户访问页-从中用户可以指示应能够查看和编辑任何供应商或信息，或者，什么是否 DropDownList 的功能特定的供应商的信息才能查看和编辑。 如果用户指示，她可以查看和编辑所有供应商信息 （默认值），她可以通过所有供应商页上，编辑任何供应商的地址信息，和编辑的名称和每个单位的所选供应商提供的任何产品的数量。 如果用户指示她只能查看和编辑的特定供应商提供，但是，则她可以仅查看该一个供应商的详细信息和产品，并且只能更新名称和每个单元信息这些产品的数量*不*停止使用。

然后，在本教程中，我们第一步是创建此 DropDownList 并填充它与供应商系统中。 打开`UserLevelAccess.aspx`页面`EditInsertDelete`文件夹中，添加 DropDownList 其`ID`属性设置为`Suppliers`，并将此 DropDownList 绑定到名为新 ObjectDataSource `AllSuppliersDataSource`。


[![创建名为 AllSuppliersDataSource 新对象数据源](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**图 3**： 创建新对象数据源命名`AllSuppliersDataSource`([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))


由于我们希望此 DropDownList 以包括所有供应商，配置对象数据源来调用`SuppliersBLL`类的`GetSuppliers()`方法。 此外确保 ObjectDataSource s`Update()`方法映射到`SuppliersBLL`类的`UpdateSupplierAddress`方法，为此对象数据源将还使用我们将在步骤 2 中添加说明。

完成对象数据源向导后，通过配置完成步骤`Suppliers`DropDownList，因为它显示`CompanyName`数据字段并使用`SupplierID`作为每个值的数据字段`ListItem`。


[![配置供应商 DropDownList 若要使用的公司名称和供应商数据字段](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**图 4**： 配置`Suppliers`使用 DropDownList`CompanyName`和`SupplierID`数据字段 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))


此时，下拉列表列出了在数据库中供应商提供的公司名称。 但是，我们还需要包括 DropDownList"显示/编辑所有供应商"选项。 若要完成此操作，将设置`Suppliers`DropDownList s`AppendDataBoundItems`属性`true`，然后添加`ListItem`其`Text`属性是"显示/编辑所有供应商"，并且其值是`-1`。 这可以直接通过声明性的标记或添加通过设计器通过转到属性窗口并单击省略号以 DropDownList 的`Items`属性。

> [!NOTE]
> 将回指[*主/详细信息筛选与 DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教程，以将所有选择的项添加到数据绑定的 DropDownList 的更多详细讨论。


后`AppendDataBoundItems`设置属性和`ListItem`添加，DropDownList s 声明性标记应如下所示：


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

通过浏览器查看时，图 5 显示我们当前进度的屏幕截图。


[![供应商 DropDownList 包含显示所有 ListItem，以及一个用于每个供应商](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**图 5**: `Suppliers` DropDownList 包含显示所有`ListItem`，加上一个为每个供应商 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))


由于我们想要更新的用户界面，立即在用户更改它们的选择后，设置`Suppliers`DropDownList s`AutoPostBack`属性`true`。 在步骤 2 中，我们将创建一个说明如何控件将显示供应商必须基于 DropDownList 选择的信息。 然后，在步骤 3 中，我们将创建的事件处理程序此 DropDownList 的`SelectedIndexChanged`事件，我们将添加代码，将相应的供应商信息绑定到说明如何基于所选供应商。

## <a name="step-2-adding-a-detailsview-control"></a>步骤 2： 添加说明如何控制

让我们来使用说明如何显示供应商信息。 为可以查看和编辑所有供应商的用户，说明如何就将支持分页，允许用户一次单步执行的供应商信息一条记录。 如果用户适用于特定的供应商，但是，说明如何将显示仅该特定的供应商的信息，并且不会包括分页接口。 在任一情况下，需要允许用户编辑供应商的地址、 市和国家/地区字段说明。

将说明如何添加到下页`Suppliers`DropDownList，设置其`ID`属性`SupplierDetails`，并将其绑定到`AllSuppliersDataSource`在上一步中创建对象数据源。 接下来，检查从说明 s 智能标记启用分页和启用编辑复选框。

> [!NOTE]
> 如果 don t 看到智能说明 s 中的启用编辑选项标记它 s 因为未映射 ObjectDataSource s`Update()`方法`SuppliersBLL`类的`UpdateSupplierAddress`方法。 花些时间回过头来进行此配置更改，此后启用编辑选项应出现在说明 s 智能标记。


由于`SuppliersBLL`类 s`UpdateSupplierAddress`方法仅接受四个参数- `supplierID`， `address`， `city`，和`country`-修改说明的 BoundFields 以便`CompanyName`和`Phone`BoundFields 是只读的。 此外，如果删除`SupplierID`BoundField 一起删除。 最后， `AllSuppliersDataSource` ObjectDataSource 当前具有其`OldValuesParameterFormatString`属性设置为`original_{0}`。 花些时间从声明性语法，完全，删除此属性设置，或将其设置为默认值， `{0}`。

配置后`SupplierDetails`说明和`AllSuppliersDataSource`ObjectDataSource，我们将具有以下声明性标记：


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

此时可以通过分页说明和所选供应商的地址信息可以更新，而不考虑中所做的选择`Suppliers`DropDownList （请参阅图 6）。


[![可以查看任何供应商信息，并更新其地址](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**图 6**： 任何供应商可以查看信息，并更新其地址 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>步骤 3： 显示所选供应商的 s 信息

我们的页面当前显示的信息而不考虑是否已从选择的特定供应商提供的所有供应商`Suppliers`DropDownList。 为了显示仅将所选供应商的供应商信息，我们需要将另一个对象数据源添加到我们的页面，一个用于检索有关的特定供应商提供的信息。

将新对象数据源添加到页上，其命名为`SingleSupplierDataSource`。 从其智能标记上，单击配置数据源链接和让它使用`SuppliersBLL`类的`GetSupplierBySupplierID(supplierID)`方法。 与`AllSuppliersDataSource`ObjectDataSource，具有`SingleSupplierDataSource`ObjectDataSource s`Update()`方法映射到`SuppliersBLL`类的`UpdateSupplierAddress`方法。


[![配置为使用 GetSupplierBySupplierID(supplierID) 方法 SingleSupplierDataSource ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**图 7**： 配置`SingleSupplierDataSource`使用 ObjectDataSource`GetSupplierBySupplierID(supplierID)`方法 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))


接下来，我们重新提示您指定的参数源`GetSupplierBySupplierID(supplierID)`方法的`supplierID`输入的参数。 由于我们想要显示的信息从下拉列表，使用选择的供应商`Suppliers`DropDownList 的`SelectedValue`作为参数源的属性。


[![供应商 DropDownList 用作供应商 Id 参数源](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**图 8**： 使用`Suppliers`作为 DropDownList`supplierID`参数源 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))


即使使用添加此第二个 ObjectDataSource，说明当前配置为始终使用`AllSuppliersDataSource`对象数据源。 我们需要添加逻辑以调整数据源，具体取决于说明如何使用`Suppliers`DropDownList 项选择。 若要实现此目的，创建`SelectedIndexChanged`供应商 DropDownList 的事件处理程序。 这可以最轻松地创建通过双击设计器中的 DropDownList 中。 此事件处理程序需要确定要使用的数据源，并且必须重新绑定数据的说明。 替换为以下代码完成此操作：


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

通过确定是否选择了"显示/编辑所有供应商"选项开始事件处理程序。 如果是，它将设置`SupplierDetails`说明 s`DataSourceID`到`AllSuppliersDataSource`并将用户返回到供应商提供集中的第一个记录，通过设置`PageIndex`属性设为 0。 如果，但是，用户已选择特定供应商，从下拉列表，说明如何 s`DataSourceID`分配给`SingleSuppliersDataSource`。 无论何种数据源使用，`SuppliersDetails`模式恢复为只读模式和数据重新绑定到说明如何通过调用`SuppliersDetails`控件的`DataBind()`方法。

与就地此事件处理程序，说明如何控制现在将显示所选供应商，除非选择了"显示/编辑所有供应商"选项，所有供应商可以通过分页界面查看的这种情况下。 图 9 显示了页面并选择; 的"显示/编辑所有供应商"选项请注意分页接口存在，允许用户访问和更新任何供应商。 图 10 显示与选择佳佳供应商的页。 仅佳佳的信息在此情况下是可查看和编辑。


[![您可以查看和编辑所有供应商信息](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**图 9**： 的所有供应商的情况下可以查看信息和编辑 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))


[![仅将所选供应商的信息都可以查看和编辑](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**图 10**： 仅所选供应商的信息可以查看和编辑 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))


> [!NOTE]
> 对于本教程中，DropDownList 和说明如何控制 s`EnableViewState`必须设置为`true`（默认值） 因为 DropDownList s`SelectedIndex`和说明如何的`DataSourceID`属性的更改必须记住跨回发。


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>步骤 4： 列出中可编辑的 GridView 的供应商产品

说明如何完成，我们的下一步是包括列出所选供应商提供的这些产品可编辑 GridView。 此 GridView 应允许编辑唯一`ProductName`和`QuantityPerUnit`字段。 此外，如果用户访问的页面来自特定供应商，它只应允许对这些产品的更新*不*停止使用。 若要完成此我们将需要首先添加的重载`ProductsBLL`类 s`UpdateProducts`中采用的方法仅`ProductID`， `ProductName`，和`QuantityPerUnit`作为输入的字段。 我们遇到单步执行完成此过程事先在许多教程，因此让我们来在这里，应添加到的代码只需查看`ProductsBLL`:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

使用此我们已准备好添加 GridView 控件和其关联的对象数据源重新重载创建， 向页面添加一个新的 GridView，设置其`ID`属性`ProductsBySupplier`，并将其配置为使用名为新 ObjectDataSource `ProductsBySupplierDataSource`。 由于我们想要列出那些通过所选供应商此 GridView，使用`ProductsBLL`类的`GetProductsBySupplierID(supplierID)`方法。 此外将映射`Update()`方法对新`UpdateProduct`我们刚刚创建的重载。


[![配置对象数据源以使用刚刚创建的 UpdateProduct 重载](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**图 11**： 配置使用 ObjectDataSource`UpdateProduct`重载刚刚创建 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))


我们 re 系统提示您选择的参数源`GetProductsBySupplierID(supplierID)`方法的`supplierID`输入的参数。 由于我们想要显示的产品中的说明，使用所选供应商`SuppliersDetails`说明如何控制的`SelectedValue`作为参数源的属性。


[![使用作为参数源 SuppliersDetails 说明的 SelectedValue 属性](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**图 12**： 使用`SuppliersDetails`说明 s`SelectedValue`属性作为参数源 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))


返回到 GridView，删除所有除 GridView 字段`ProductName`， `QuantityPerUnit`，和`Discontinued`、 标记`Discontinued`CheckBoxField 以只读方式。 另外，请检查从 GridView s 智能标记启用编辑选项。 做出这些更改后，GridView 和对象数据源的声明性标记应类似于以下：


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

与我们以前 ObjectDataSources，此一个 s`OldValuesParameterFormatString`属性设置为`original_{0}`，尝试更新的产品的名称或每个单位的数量时，这将导致问题。 完全声明性语法中移除此属性或将其设置为其默认值， `{0}`。

完成此配置中，我们的页面现在列出在 GridView 中选择供应商提供的产品 （请参阅图 13）。 当前*任何*可以更新产品的名称或每个单位的数量。 但是，我们需要更新我们页逻辑，以便对与特定的供应商相关联的用户停产的产品禁止的此类功能。 例如，我们将介绍在步骤 5 中的此最后一段。


[![显示所选供应商提供的产品](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**图 13**： 选择供应商提供产品显示 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))


> [!NOTE]
> 借助新增的此可编辑的 GridView `Suppliers` DropDownList 的`SelectedIndexChanged`应更新事件处理程序以返回到只读状态的 GridView。 否则，如果在编辑产品信息选择不同的供应商，则在新的供应商的 GridView 相应的索引也将可编辑。 若要防止此情况，只需设置 GridView s`EditIndex`属性`-1`中`SelectedIndexChanged`事件处理程序。


此外，还记得很重要 GridView 的视图状态是启用 （默认行为）。 如果设置 GridView s`EnableViewState`属性`false`，运行出现无意中删除或编辑记录的并发用户的风险。 请参阅[警告： 禁用并发问题与 ASP.NET 2.0 GridViews/说明/FormViews 该支持编辑和/或删除和其视图状态](http://scottonwriting.net/sowblog/posts/10054.aspx)有关详细信息。

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>步骤 5： 不允许编辑，未选择停止使用产品时显示/编辑所有供应商为

虽然`ProductsBySupplier`GridView 是完全正常运行，它当前授予过多访问来自特定供应商的用户。 每个我们业务规则，这类用户不应能够更新停产的产品。 若要强制此，我们可以隐藏 （或禁用） 与停产的产品时页由用户的供应商提供访问那些 GridView 行中的编辑按钮。

创建的事件处理程序 GridView 的`RowDataBound`事件。 我们需要此事件处理程序中确定用户是否不是与特定的供应商，这对于本教程，可以确定通过检查供应商的 DropDownList s 关联`SelectedValue`属性-如果它是 s 以外的值比-1，则用户与特定的供应商相关联。 对于此类用户，我们然后需要确定产品已停止使用。 在我们可以获取实际的引用`ProductRow`实例绑定到 GridView 该行通过`e.Row.DataItem`属性，如下所述[ *GridView 的页脚中显示摘要信息*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md)本教程。 如果产品已停止使用，在我们可以获取 GridView s CommandField 使用在前面的教程，讨论的技术中的编辑按钮的编程参考[*添加客户端确认时删除*](adding-client-side-confirmation-when-deleting-cs.md). 一旦我们有我们然后可以隐藏或禁用按钮的引用。


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

与此事件处理程序就地情况下，当访问此页以用户身份的特定供应商提供这些已停产的产品是不可编辑，作为编辑按钮隐藏对这些产品。 例如，Chef Anton 的秋葵组合是新奥尔良 Cajun 带来快乐供应商断货的产品。 当此特定的供应商访问的页面，此产品的编辑按钮隐藏从使其不可见 （请参阅图 14）。 但是，当访问使用"显示/编辑所有供应商"，编辑按钮是可用 （请参阅图 15）。


[![为特定于供应商的用户隐藏 Chef Anton s 秋葵组合编辑按钮](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**图 14**: Chef Anton s 秋葵组合编辑按钮处于隐藏状态的特定于供应商的用户 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))


[![显示/编辑所有供应商用户，显示 Chef Anton s 秋葵组合编辑按钮](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**图 15**： 用于显示/编辑所有供应商用户、 Chef Anton s 秋葵组合将显示编辑按钮 ([单击以查看实际尺寸的图像](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>检查业务逻辑层中的访问权限

在本教程中 ASP.NET 页处理用户可以看到哪些信息方面的所有逻辑，他可以更新哪些产品。 理想情况下，此逻辑还也会出现在业务逻辑层。 例如，`SuppliersBLL`类 s`GetSuppliers()`方法 （它返回所有供应商） 可能会进行检查，以确保当前登录的用户*不*与特定的供应商相关联。 同样，`UpdateSupplierAddress`方法无法进行检查，以确保当前登录的用户是我们公司工作的 （并因此可以更新所有供应商地址信息） 或与正在更新其数据的供应商相关联。

我未包括此类的 BLL 层检查，因为在我们的教程用户的权限由在页上，BLL 类不能访问 DropDownList。 使用成员资格系统或由 ASP.NET 提供 （例如 Windows 身份验证），扩展的框身份验证方案之一时当前记录可以从 BLL，从而使此类访问用户 s 访问信息和角色信息权限检查可能在演示文稿和 BLL 层。

## <a name="summary"></a>摘要

提供用户帐户的大多数站点需要自定义基于在已登录用户的数据修改接口。 管理用户可以删除和编辑任何记录，而非管理用户可能会限制为仅更新或删除他们自己创建的记录。 任何方案可能是，数据 Web 控件，ObjectDataSource，并且可以扩展业务逻辑层类来添加或拒绝基于登录用户的特定功能。 在本教程中我们已了解如何限制可查看和编辑具体取决于用户是否与特定的供应商关联或如果它们的工作为我们的公司数据。

本教程到结束插入、 更新和删除数据使用 GridView、 说明如何和 FormView 控件我们的检查。 从开始下一个教程，我们将着重于添加分页和排序支持。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](adding-client-side-confirmation-when-deleting-cs.md)
[下一页](an-overview-of-inserting-updating-and-deleting-data-vb.md)
