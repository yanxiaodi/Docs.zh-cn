---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: "显示数据的项，并详细介绍 |Microsoft 文档"
author: Erikre
description: "本系列教程将教您生成有关我们使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 的 ASP.NET Web 窗体应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 809d7a9c21a3ddf5dfd07d079eb8fe0d1d81712d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="display-data-items-and-details"></a>显示数据项和详细信息
====================
通过[艾力克 Reitan](https://github.com/Erikre)

[下载 Wingtip Toys 示例项目 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下载电子书 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教程将教您构建使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web 窗体应用程序的基础知识。 Visual Studio 2013[包含 C# 源代码项目](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)是可以附带本系列教程。


本教程介绍如何显示数据项和使用 ASP.NET Web 窗体和 Entity Framework Code First 数据项详细信息。 本教程以前一教程"UI 和导航"上构建，并为 Wingtip 无实用价值应用商店教程系列的一部分。 当你已完成本教程时，你将能够查看产品上*ProductsList.aspx*页和有关在上一单个产品的详细信息*ProductDetails.aspx*页。

## <a name="what-youll-learn"></a>你将学习：

- 如何添加数据控件用于显示从数据库的产品。
- 如何连接到所选数据的数据控件。
- 如何添加数据控件以显示从数据库的产品详细信息。
- 如何检索查询字符串的值并使用该值来限制从数据库中检索的数据。

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>以下是本教程中引入的功能：

- 模型绑定
- 值在提供程序

## <a name="adding-a-data-control-to-display-products"></a>添加一个数据控件来显示产品

当将数据绑定到服务器控件，有几个不同选项，你可以使用。 最常用的选项包括添加数据源控件、 进行手动添加代码或使用模型绑定。

### <a name="using-a-data-source-control-to-bind-data"></a>使用数据源控件将数据绑定

添加数据源控件，可将数据源控件链接到显示的数据的控件。 此方法允许你以声明方式服务器端控件直接连接到数据源，而不是使用编程方法。

### <a name="coding-by-hand-to-bind-data"></a>编码手动将数据绑定

添加手动的代码涉及读取的值、 检查有 null 值，尝试将其转换为适当的类型，检查指示转换是否成功，以及最后，在查询中使用的值。 当你需要保留对你的数据访问逻辑的完全控制时，将使用此方法。

### <a name="using-model-binding-to-bind-data"></a>使用模型绑定，使其将数据绑定

使用模型绑定允许你将使用更少代码的结果绑定，而使你能够重复使用在整个应用程序的功能。 模型绑定旨在简化代码为中心的数据访问逻辑的处理，同时仍然保留丰富、 数据绑定的框架的优点。

## <a name="displaying-products"></a>显示产品

在本教程中，你将使用模型绑定将数据绑定。 若要配置一个数据控件要使用模型绑定来选择数据，你设置的控件的`SelectMethod`属性页的代码中的方法的名称。 数据控件在页生命周期中适当的时间调用的方法，并自动将绑定返回的数据。 没有无需显式调用`DataBind`方法。

使用以下步骤，您将修改中的标记*ProductList.aspx*页，以便所页可以显示产品。

1. 在**解决方案资源管理器**，打开*ProductList.aspx*页。
2. 将现有的标记替换为以下标记：   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

此代码使用**ListView**控件名为"productList"若要显示的产品。

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView**控件使用的模板和样式定义的格式显示数据。 它是适用于任何重复结构中的数据。 这**ListView**示例只是显示数据库，但是你可以启用用户能够编辑、 插入和删除数据，并能够排序和页数据，所有操作代码中的数据。

通过设置`ItemType`中的属性**ListView**控制，数据绑定表达式`Item`可用和控件将成为强类型化。 根据前面的教程中所述，你可以选择使用 IntelliSense，例如，指定项对象的详细信息`ProductName`:

![显示数据项数和详细信息的 IntelliSense](display_data_items_and_details/_static/image1.png)

此外，所使用模型绑定指定`SelectMethod`值。 此值 (`GetProducts`) 将对应于你将添加到背后的代码在下一步中显示产品的方法。

### <a name="adding-code-to-display-products"></a>添加代码以显示产品

在此步骤中，你将添加代码以填充**ListView**与数据库中的产品数据的控件。 代码将支持单个类别中，以及显示的所有产品都显示产品。

1. 在**解决方案资源管理器**，右键单击*ProductList.aspx* ，然后单击**查看代码**。
2. 中的现有代码替换*ProductList.aspx.cs*文件替换为以下代码：   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

此代码演示`GetProducts`被引用的方法`ItemType`属性**ListView**中控制*ProductList.aspx*页。 若要将结果限制为在数据库中的特定类别，该代码将设置`categoryId`传递给查询字符串值的值*ProductList.aspx*时页*ProductList.aspx*页导航到。 `QueryStringAttribute`类`System.Web.ModelBinding`命名空间用于检索查询字符串变量 id 的值。这会指示要尝试将值绑定到查询字符串中的模型绑定`categoryId`在运行时的参数。

有效的类别作为查询字符串传递给页上，当查询的结果被限制为这些数据库中的产品的匹配`categoryId`值。 例如，如果的 URL *ProductsList.aspx*页是以下：

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

页仅显示产品其中`category`等于`1`。

如果没有查询字符串包括，则导航到时*ProductList.aspx*页上，所有产品都将都显示。

这些方法的值的源被称为*值提供程序*(如*QueryString*)，并指示要使用的值提供程序的参数属性统称为值提供程序属性 (如"`id`")。 ASP.NET Web 窗体应用程序，例如查询字符串、 cookie、 窗体值、 控件、 视图状态、 会话状态和配置文件属性中包括值在提供程序和所有用户输入的典型源的相应属性。 你还可以编写自定义值在提供程序。

### <a name="running-the-application"></a>运行应用程序

运行应用程序现在以查看如何查看所有产品或只是一套产品类别受限制。

1. 在**解决方案资源管理器**，右键单击*Default.aspx*页，选择**用浏览器查看**。  
 浏览器将打开并显示*Default.aspx*页。
2. 选择**汽车**从产品类别导航菜单。  
 *ProductList.aspx*显示仅"汽车"类别中包含的产品显示页。 稍后在本教程中，将显示产品详细信息。  

    ![显示数据项数和详细信息-汽车](display_data_items_and_details/_static/image2.png)
3. 选择**产品**从顶部导航菜单。  
 同样， *ProductList.aspx*页将显示，但这次它显示的产品的整个列表。   

    ![显示数据项数和详细信息-产品](display_data_items_and_details/_static/image3.png)
4. 关闭浏览器并返回到 Visual Studio。

### <a name="adding-a-data-control-to-display-product-details"></a>添加一个数据控件来显示产品详细信息

接下来，将修改中的标记*ProductDetails.aspx*以便页可以显示有关各个产品的信息在前面的教程中添加的页。

1. 在**解决方案资源管理器**，打开*ProductDetails.aspx*页。
2. 将现有的标记替换为以下标记：   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

此代码使用**FormView**控件来显示有关各个产品的详细信息。 此标记使用的一样，用于显示中的数据的方法*ProductList.aspx*页。 **FormView**控件用于从数据源一次显示一条记录。 当你使用**FormView**控件，您创建模板，用于显示和编辑数据绑定值。 这些模板包含，绑定表达式和格式设置用于定义控件的外观和表单的功能。

若要连接到数据库的上面的标记，必须添加到的其他代码*ProductDetails.aspx*代码。

1. 在**解决方案资源管理器**，右键单击*ProductDetails.aspx* ，然后单击**查看代码**。  
 *ProductDetails.aspx.cs*文件将会显示。
2. 用下面的代码替换现有代码：   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

此代码检查"`productID`"查询字符串值。 如果找到一个有效的查询字符串值，则将显示匹配的产品。 如果不找到任何查询字符串，或查询字符串值无效，不到产品将显示在*ProductDetails.aspx*页。

### <a name="running-the-application"></a>运行应用程序

现在你可以运行应用程序以查看单个产品显示基于产品的 id。

1. 按**F5** Visual Studio 运行应用程序中时，在。  
 浏览器将打开并显示*Default.aspx*页。
2. 从类别导航菜单中选择"船"。  
 *ProductList.aspx*显示页。
3. 从产品列表中选择"纸张船"产品。  
 *ProductDetails.aspx*显示页。   

    ![显示数据项数和详细信息-产品](display_data_items_and_details/_static/image4.png)
4. 关闭浏览器。

## <a name="summary"></a>摘要

在本教程中的序列中，你已添加标记和代码以显示产品列表并显示产品详细信息。 在此过程中，你已了解有关强类型化的数据控件、 模型绑定和值在提供程序。 在下一步的教程中，你将添加购物车 Wingtip Toys 示例应用程序。

## <a name="additional-resources"></a>其他资源

[检索和显示使用模型的绑定和 web 窗体的数据](../../presenting-and-managing-data/model-binding/retrieving-data.md)

>[!div class="step-by-step"]
[上一页](ui_and_navigation.md)
[下一页](shopping-cart.md)
