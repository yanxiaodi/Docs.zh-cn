---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: "以编程方式设置对象数据源的参数值 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将查看将方法添加到我们的 DAL 和 BLL 接受一个输入的参数并返回数据。 该示例会将此参数设置..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 7694c56fa5c50ff75db931e88c2334f560631d74
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a>以编程方式设置对象数据源的参数值 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe)或[下载 PDF](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)

> 在本教程中我们将查看将方法添加到我们的 DAL 和 BLL 接受一个输入的参数并返回数据。 该示例将以编程方式设置此参数。


## <a name="introduction"></a>介绍

正如我们看到在[以前一教程](declarative-parameters-cs.md)，有多种选项都是可用于以声明方式将参数值传递给对象数据源的方法。 如果参数值为硬编码，来自 Web 控件在页上，或在数据源是可读的任何其他源`Parameter`对象，例如，值将绑定到输入参数上，而无需编写一行代码。

可能有些时候，但是，当参数值来源某些不已计由内置数据源之一`Parameter`对象。 如果我们网站支持的用户帐户我们可能想要将基于当前登录的访问者的用户 id。 该参数设置 或者，我们可能需要先将它传递发送到对象数据源的基础对象的方法自定义的参数值。

每当 ObjectDataSource`Select`调用方法 ObjectDataSource 首先引发其[选择事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx)。 然后调用对象数据源的基础对象的方法。 一旦完成 ObjectDataSource[所选事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx)激发 （图 1 展示了这一序列的事件）。 传递给对象数据源的基础对象的方法的参数值可设置或自定义的事件处理程序中`Selecting`事件。


[![调用 ObjectDataSource 选中和选择事件激发之前和之后及其基础对象的方法](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)

**图 1**: ObjectDataSource`Selected`和`Selecting`均会调用事件的激发之前和之后及其基础对象的方法 ([单击以查看实际尺寸的图像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))


在本教程中我们将考察方法添加到我们的 DAL 和接受一个输入的参数的 BLL `Month`，类型的`int`并返回`EmployeesDataTable`填入这些其招聘周年中指定这些员工的对象`Month`. 我们的示例将设置此参数以编程方式根据当前月份，显示"员工周年纪念日的本月。"列表

让我们进入正题！

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>步骤 1： 添加到方法`EmployeesTableAdapter`

对于我们我们需要添加一种方式来检索这些员工的第一个示例其`HireDate`在指定的月份中发生。 若要提供此功能根据我们体系结构，我们需要先创建中的方法`EmployeesTableAdapter`映射到正确的 SQL 语句。 若要完成此操作，首先打开 Northwind 类型化数据集。 右键单击`EmployeesTableAdapter`标签，然后选择添加查询。


[![将新查询添加到 EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)

**图 2**： 添加到一个新查询`EmployeesTableAdapter`([单击以查看实际尺寸的图像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))


选择添加返回行的 SQL 语句。 在访问指定`SELECT`语句屏幕默认`SELECT`语句`EmployeesTableAdapter`已将加载。 在中只需添加`WHERE`子句： `WHERE DATEPART(m, HireDate) = @Month`。 [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx)是 T-SQL 的函数，返回的特定日期部分`datetime`类型; 在这种情况下，我们将使用`DATEPART`返回的月份`HireDate`列。


[![返回仅的那些行其中 hiredate 列是否小于或等于@HiredBeforeDate参数](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)

**图 3**： 返回仅的那些行其中`HireDate`列是否小于或等于`@HiredBeforeDate`参数 ([单击以查看实际尺寸的图像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))


最后，更改`FillBy`和`GetDataBy`方法名称为`FillByHiredDateMonth`和`GetEmployeesByHiredDateMonth`分别。


[![选择比 FillBy 和 GetDataBy 更合适的方法名称](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)

**图 4**： 选择更适当方法名称比`FillBy`和`GetDataBy`([单击以查看实际尺寸的图像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))


单击完成以完成向导并返回到数据集的设计图面。 `EmployeesTableAdapter`现在应包含一组新的用于访问指定的月份内雇用的员工的方法。


[![新的方法显示在数据集的设计图面](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)

**图 5**: 新方法显示在数据集的设计图面 ([单击以查看实际尺寸的图像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>步骤 2： 添加`GetEmployeesByHiredDateMonth(month)`业务逻辑层的方法

由于我们应用程序体系结构用于单独的层的业务逻辑和数据访问逻辑，我们需要将方法添加到指定日期前的租期下 DAL 的调用以检索员工我们 BLL。 打开`EmployeesBLL.cs`文件并添加以下方法：


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

与我们在此类中的其他方法一样`GetEmployeesByHiredDateMonth(month)`只需调入 DAL 并返回结果。

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>步骤 3： 显示其招聘周年日是这个月的员工

此示例中我们最后一个步骤是显示其招聘周年日是本月这些员工。 首先，通过添加到 GridView`ProgrammaticParams.aspx`页面`BasicReporting`文件夹并将新对象数据源添加为其数据源。 配置对象数据源以使用`EmployeesBLL`类，该类具有`SelectMethod`设置为`GetEmployeesByHiredDateMonth(month)`。


[![使用 EmployeesBLL 类](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)

**图 6**： 使用`EmployeesBLL`类 ([单击以查看实际尺寸的图像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))


[![选择从 GetEmployeesByHiredDateMonth(month) 方法](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)

**图 7**: Select From`GetEmployeesByHiredDateMonth(month)`方法 ([单击以查看实际尺寸的图像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))


最后一个屏幕可让我们提供`month`参数值的源。 由于我们将以编程方式设置此值，保留参数源设置默认值为 None 选项，然后单击完成。


[![将参数源设置保留为无](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)

**图 8**： 将保留为无设置参数源 ([单击以查看实际尺寸的图像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))


这将创建`Parameter`中对象数据源的对象`SelectParameters`不具有指定的值的集合。


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

若要以编程方式设置此值，我们需要创建对象数据源的事件处理程序`Selecting`事件。 若要完成此操作，请转到设计视图，并双击 ObjectDataSource。 或者，选中 ObjectDataSource、 转到属性窗口中，然后单击闪电形图标。 接下来，请双击在文本框中旁边`Selecting`事件或输入你想要使用的事件处理程序的名称。


![单击属性窗口列出 Web 控件的事件中的闪电形图标](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

**图 9**： 单击属性窗口列出 Web 控件的事件中的闪电形图标


这两种方法添加的新的事件处理程序 ObjectDataSource`Selecting`到页面的代码隐藏类的事件。 在此事件处理程序中，我们可以读取和写入使用的参数值`e.InputParameters[parameterName]`，其中 *`parameterName`* 是值`Name`属性中`<asp:Parameter>`标记 (`InputParameters`集合也可以是按序号对它们，在索引`e.InputParameters[index]`)。 若要设置`month`到当前月份，参数添加到以下`Selecting`事件处理程序：


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

在访问此页通过浏览器时，我们可以看到该只有一个员工受雇本月 （年 3 月） 刘娜王五，1994年起已在公司人员。


[![其纪念日显示本月这些员工](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)

**图 10**： 那些员工其纪念日此月显示 ([单击以查看实际尺寸的图像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))


## <a name="summary"></a>摘要

尽管对象数据源的参数值通常可设置以声明方式，而无需代码行，很容易以编程方式设置参数值。 我们需要做的就是创建为对象数据源的事件处理程序`Selecting`触发的事件，基础对象的方法未调用，且手动设置通过一个或多个参数的值之前`InputParameters`集合。

本教程到结束基本报告部分。 [下一教程](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)启动关闭筛选和主-详细信息方案部分，我们将在其中查看允许访问者来筛选数据的技术，并从主报表深化到详细信息报告。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Giesenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](declarative-parameters-cs.md)
[下一页](displaying-data-with-the-objectdatasource-vb.md)
