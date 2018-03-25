---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: "为 ASP.NET MVC 应用程序 (10 的第 4) 创建更复杂的数据模型 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: accb5ddab8df67dfa29038541dc0cd72eaac173c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>为 ASP.NET MVC 应用程序 (10 的第 4) 创建更复杂的数据模型
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 你可以从头开始教程系列或[下载这一章的初学者项目](building-the-ef5-mvc4-chapter-downloads.md)和从这里开始。
> 
> > [!NOTE] 
> > 
> > 如果你遇到无法解决的问题[下载已完成的章](building-the-ef5-mvc4-chapter-downloads.md)并尝试重现你的问题。 你通常可以通过比较你的代码已完成的代码会发现问题的解决方案。 一些常见的错误和如何解决它们，请参阅[错误和解决方法。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在前面的教程中，你处理简单的数据模型的三个实体组成。 在本教程中你将添加多个实体和关系并将通过指定格式设置、 验证和数据库的映射规则自定义数据模型。 你将看到两种方式自定义数据模型： 通过将特性添加到实体类，通过将代码添加到数据库上下文类。

当完成时，实体类将组成以下图所示的已完成的数据模型：

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>通过使用属性自定义数据模型

在本部分中，你将了解如何通过使用指定格式设置，验证和数据库的映射规则的属性来自定义数据模型。 然后在多个下列各节，你将创建一种完整`School`通过添加数据模型属性的类已在模型中的剩余实体类型的创建和创建新类。

### <a name="the-datatype-attribute"></a>数据类型属性

对于学生注册日期，网页的所有当前显示时间和日期，虽然所有关心为此字段是日期。 通过使用数据 annotation 特性，你可以使一个代码将在每个视图中显示的数据中修复的显示格式的更改。 若要查看如何执行操作，你将添加到属性的示例`EnrollmentDate`中的属性`Student`类。

在*Models\Student.cs*，添加`using`语句`System.ComponentModel.DataAnnotations`命名空间并添加`DataType`和`DisplayFormat`特性以`EnrollmentDate`属性，如下面的示例中所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性用于指定比数据库内部类型更具体的数据类型。 在这种情况下，我们只想跟踪的日期，不的日期和时间。 [DataType 枚举](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)提供多种数据类型，如*日期、 时间、 PhoneNumber、 货币、 电子邮件地址*和的详细信息。 应用程序还可通过 `DataType` 特性自动提供类型特定的功能。 例如，`mailto:`链接可以为创建[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)，和日期选择器可提供用于[DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)中支持的浏览器[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性发出 HTML 5[数据-](http://ejohn.org/blog/html-5-data-attributes/) (发音为*数据 dash*) HTML 5 浏览器可以理解的属性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性不提供任何验证。

`DataType.Date` 不指定显示日期的格式。 默认情况下，数据字段显示根据基于服务器的默认格式[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)。

`DisplayFormat` 特性用于显式指定日期格式：


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode`设置指定，指定的格式设置也应该将应用时的值显示在文本框中以进行编辑。 (你可能不想的某些字段 — 例如，货币值，你可能不希望在文本框中的货币符号以进行编辑。)

你可以使用[DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性通过本身，但它通常是使用一个好办法[数据类型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)还属性。 `DataType`属性传达*语义*的数据作为，而不是如何呈现在屏幕上，然后提供就不会使用了以下好处`DisplayFormat`:

- 浏览器可以启用 HTML5 功能 （例如以显示一个日历控件、 区域设置相对应的货币符号、 电子邮件链接，等等）。
- 默认情况下，浏览器将呈现数据使用基于的正确格式你[区域设置](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性可以启用 MVC 能够选择要呈现的数据的右侧字段模板 ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)本身如果由使用字符串模板)。 有关详细信息，请参阅 Brad Wilson [ASP.NET MVC 2 模板](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。 （尽管为 MVC 2 编写的这篇文章仍适用于 ASP.NET MVC 的当前版本。）

如果你使用`DataType`属性与日期字段中，你必须指定`DisplayFormat`还以确保字段正确呈现在 Chrome 浏览器中的属性。 有关详细信息，请参阅[此 StackOverflow 线程](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)。

再次运行学生索引页，并请注意，注册日期不再显示时间。 相同将适用于使用的任何视图`Student`模型。

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

你还可以指定数据验证规则和使用属性的消息。 假设你想要确保，用户不超过 50 个字符输入名称。 若要添加此限制，添加[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)特性以`LastName`和`FirstMidName`属性，如下面的示例中所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性不会阻止的用户的空白区域输入一个名称。 你可以使用[正则表达式](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)要将限制应用到的输入属性。 例如，下面的代码要求的第一个字符是大写且其余的字符是按字母顺序排列：

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx)属性提供与类似的功能[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性但不提供客户端验证。

运行应用程序，然后单击**学生**选项卡。你收到以下错误：

*数据库创建后，备份 SchoolContext 上下文的模型已更改。请考虑使用 Code First 迁移更新数据库 ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。*

数据库模型已更改需要在数据库架构中，更改的方式，实体框架检测到。 你将使用迁移来更新架构，而不丢失任何通过使用 UI 添加到数据库的数据。 如果更改数据由`Seed`方法，将会更改回其原始状态由于[AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)方法使用在`Seed`方法。 ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)等效于"upsert"操作从数据库术语。)

在包管理器控制台 (PMC) 中，输入以下命令：

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration MaxLengthOnNames`命令创建名为的文件*&lt;时间戳&gt;\_MaxLengthOnNames.cs*。 此文件包含将更新数据库，以匹配当前数据模型的代码。 实体框架使用的迁移文件名称前面预置的时间戳以整理迁移。 如果您删除数据库，创建多个迁移后或在部署项目通过使用迁移，迁移的所有应用于已创建的顺序。

运行**创建**页，然后输入任一名称超过 50 个字符。 一旦超过 50 个字符时，客户端验证将立即显示一条错误消息。

![客户端端 val 错误](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>列属性

特性还可用于控制如何类和属性映射到数据库。 假设你已使用名称`FirstMidName`的第一个名称字段，因为该字段还可能包含中间名。 但你想要将名为的数据库列`FirstName`，因为将编写针对数据库的即席查询的用户习惯于该名称。 若要使此映射，你可以使用`Column`属性。

`Column`属性指定当创建数据库时，列`Student`映射到表`FirstMidName`属性将被命名为`FirstName`。 换而言之，你的代码引用到`Student.FirstMidName`，数据将来自或在中更新`FirstName`列`Student`表。 如果未指定列名称，系统会提供与属性名相同的名称。

添加 using 语句[System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx)和列名称属性与`FirstMidName`属性，如以下突出显示的代码中所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

添加[列属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)更改备份 SchoolContext，使它不会与数据库相匹配的模型。 在创建另一个迁移 PMC 中输入以下命令：

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

在**服务器资源管理器**(**数据库资源管理器**如果你使用的 Express for Web)，双击*学生*表。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

下图显示原始列名，因为其应用前两个迁移之前。 除了从更改的列名称`FirstMidName`到`FirstName`，两个名称列已更改从`MAX`长度为 50 个字符。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

你还可以进行数据库映射更改使用[Fluent API](https://msdn.microsoft.com/data/jj591617)，如你将看到在此教程的后面。

> [!NOTE]
> 如果您尝试编译之前创建的所有这些实体类完毕后，你可能会收到编译器错误。


## <a name="create-the-instructor-entity"></a>创建 Instructor 实体

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

创建*Models\Instructor.cs*，模板代码替换为以下代码：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

请注意的几个属性都在相同`Student`和`Instructor`实体。 在[实现继承](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)本系列教程更高版本，你将重构使用继承来消除此冗余。

### <a name="the-required-and-display-attributes"></a>所需和显示属性

上的特性`LastName`属性指定它是必填的字段，在文本框中的标题，应 （而不是属性名称，它将是"LastName"没有空格） 中,"Last Name"和值不能超过 50 个字符。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

[StringLength 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)数据库中设置的最大长度，并提供客户端和服务器端 ASP.NET MVC 的验证。 你还可以通过以下方式指定最小字符串长度中此特性，但最小值对数据库架构没有任何影响。 [必需的特性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)不需要值类型，如 DateTime、 int、 双精度型、 和 float。 值类型不能被分配 null 值，因此它们都是本质上是必需。 无法删除[必需的特性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)并将其替换的最小长度参数`StringLength`属性：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

你可以在一行上，将多个属性，因此你还可以编写教师类，如下所示：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>FullName 计算属性

`FullName`是返回一个值，通过串联两个其他属性创建一个计算的属性。 因此只有`get`访问器，但没有`FullName`列将生成数据库中。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>课程和 OfficeAssignment 导航属性

`Courses`和`OfficeAssignment`属性是导航属性。 如前所述，它们通常定义为[虚拟](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx)，以便它们可以利用称为实体框架功能[延迟加载](https://msdn.microsoft.com/magazine/hh205756.aspx)。 此外，如果导航属性可以具有多个实体，则其类型必须实现[ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx)接口。 (例如[IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx)但不是限定[IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx)因为`IEnumerable<T>`未实现[添加](https://msdn.microsoft.com/library/63ywd54z.aspx).

一个教师可以教授任意数量的课程，因此`Courses`指一套`Course`实体。 我们业务规则状态教师只能有一个 office 最多，因此`OfficeAssignment`定义为单个`OfficeAssignment`实体 (这可能会`null`如果不分配任何 office)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>创建 OfficeAssignment 实体

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

创建*Models\OfficeAssignment.cs*替换为以下代码：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

生成项目时，这将保存所做的更改，验证尚未创建任何副本，并粘贴编译器可以捕获的错误。

### <a name="the-key-attribute"></a>键属性

没有之间的对零或一一个关系`Instructor`和`OfficeAssignment`实体。 相对于其分配给，教师仅存在一个办公室分配，并且因此其主键也是其外的键与`Instructor`实体。 但实体框架无法自动识别`InstructorID`为主因为其名称不遵循此实体键`ID`或*classname* `ID`命名约定。 因此，`Key`属性用于标识作为键：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

你还可以使用`Key`属性如果实体具有其自己的主键，但你想要 name 属性不同于`classnameID`或`ID`。 默认情况下 EF 将密钥视为非数据库生成因为列是标识关系。

### <a name="the-foreignkey-attribute"></a>ForeignKey 属性

没有两个实体之间的一对一关系或一对零或一一个关系时 (此类之间`OfficeAssignment`和`Instructor`)，EF 无法制定关系的一端是主体，取决于哪一个顶点。 一对一关系具有引用导航属性在转换为其他类的每个类。 [ForeignKey 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)可以应用到依赖的类，以建立关系。 如果省略[ForeignKey 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)，当你尝试创建迁移时出现以下错误：

无法确定类型 ContosoUniversity.Models.OfficeAssignment 和 ContosoUniversity.Models.Instructor 之间的关联的主体端。 必须使用关系 fluent API 或数据注释显式配置此关联的主体端。

在教程后面部分中，我们将演示如何使用 fluent API 中配置此关系。

### <a name="the-instructor-navigation-property"></a>教师导航属性

`Instructor`实体具有一个可以为 null`OfficeAssignment`导航属性 （因为教师可能没有一个办公室分配），和`OfficeAssignment`实体具有非 null`Instructor`导航属性 （因为一个办公室分配无法存在，而一个教师-`InstructorID`是不可为 null)。 当`Instructor`实体具有相关`OfficeAssignment`实体，每个实体将具有对另一个在其导航属性的引用。

您可以将`[Required]`教师导航属性，指定必须有相关的教师，但不需要这样做是因为 InstructorID 外键 （它也是此表的关键） 是不可为 null 的属性。

## <a name="modify-the-course-entity"></a>修改过程实体

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

在*Models\Course.cs*，你添加的代码更早版本替换为以下代码：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

课程实体具有外键属性`DepartmentID`它指向相关`Department`实体，且没有`Department`导航属性。 实体框架不要求你将外键属性添加到你的数据模型，当你具有相关实体的导航属性时。 EF 会自动在数据库中创建外键在需要的位置。 但是，数据模型中具有外键可以使更新更简单、 更高效。 例如，当提取过程实体，若要编辑，`Department`实体为 null 时，如果你不加载它，当你更新课程实体中，你将需要来首次读取`Department`实体。 当外键属性`DepartmentID`包含在数据模型中，你无需提取`Department`之后更新的实体。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 属性

[DatabaseGenerated 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx)与[无](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx)参数`CourseID`属性指定主键值是由用户而不是由数据库生成。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

默认情况下，实体框架将假定主键值都由数据库。 在大多数情况下要执行的操作。 但是，对于`Course`实体，将一个部门，另一个部门，一 2000年系列使用用户指定过程号例如 1000年系列，依次类推。

### <a name="foreign-key-and-navigation-properties"></a>外键和导航属性

外键属性和中的导航属性`Course`实体反映了以下关系：

- 课程都将分配到一个部门，因此没有`DepartmentID`外键和一个`Department`上面提到的原因的导航属性。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- 课程可以有任意数量的学生注册，因此`Enrollments`导航属性是集合： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- 可能由多个教师讲授课程因此`Instructors`导航属性是集合： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>创建部门实体

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

创建*Models\Department.cs*替换为以下代码：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>列属性

前面使用[列属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)若要更改列名称映射。 中的代码`Department`实体，`Column`属性用于更改 SQL 数据类型映射，以便将将列定义使用的 SQL Server [money](https://msdn.microsoft.com/library/ms179882.aspx)数据库中的类型：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

列映射通常不是必需的因为实体框架通常选择基于你定义的属性的 CLR 类型的相应 SQL Server 数据类型。 CLR`decimal`类型映射到 SQL Server`decimal`类型。 但在这种情况下你知道列将包含货币金额和[money](https://msdn.microsoft.com/library/ms179882.aspx)数据类型是更适合。

### <a name="foreign-key-and-navigation-properties"></a>外键和导航属性

外键和导航属性反映了以下关系：

- 部门可能或可能没有管理员，并且管理员始终是一个教师。 因此`InstructorID`属性是作为外键与包含`Instructor`后添加了实体，以及一个问号`int`键入标记用于将标记为可为 null 的属性。导航属性名为`Administrator`但保留`Instructor`实体： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- 部门可能有许多课程，因此`Courses`导航属性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

 > [!NOTE]
 > 按照约定，实体框架使级联删除对于不可为 null 的外键以及多对多关系。 这可能导致循环的级联删除规则，初始值设定项代码运行时将引发异常。 例如，如果你未定义`Department.InstructorID`为可为 null 的属性，你会收到以下异常消息的初始值设定项运行时:"的引用关系会导致不允许循环引用。" 如果您的业务规则需要`InstructorID`作为不可为 null 的属性，你将需要使用以下 fluent API 若要禁用的关系上的级联删除： 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>修改学生实体

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

在*Models\Student.cs*，更早版本将你添加的代码替换为下面的代码。 突出显示所做的更改。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>注册实体

 在*Models\Enrollment.cs*，更早版本将你添加的代码替换为下面的代码

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>外键和导航属性

外键属性和导航属性反映了以下关系：

- 注册记录已经是单个的课程，因此`CourseID`外键属性和`Course`导航属性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- 因此，注册记录将适用于单个学生，`StudentID`外键属性和`Student`导航属性： 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>多对多关系

没有之间的多对多关系`Student`和`Course`实体，与`Enrollment`实体充当多对多联接表*具有负载*数据库中。 这意味着，`Enrollment`表包含用于联接的表的外键除了其他数据 (在这种情况下，主键和`Grade`属性)。

下图显示这些关系中的实体关系图的外观。 (此关系图生成使用[实体框架 Power 工具](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); 创建关系图不属于本教程，只需使用此处为说明。)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

每个关系行已在一台端和星号 1 (\*) 在另一个，，该值指示一个对多关系。

如果`Enrollment`表没有包括年级信息，它只需包含两个外键`CourseID`和`StudentID`。 在这种情况下，它将对应的多对多联接表*而无需负载*(或*纯联接表*) 在数据库中，并且您不需要在所有为它创建一个模型类。 `Instructor`和`Course`实体具有这种多对多关系，并且如你所见，它们之间没有任何实体类：

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

联接表需要在数据库中，但是，以下数据库关系图中所示：

![Instructor-Course_many-to-many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

实体框架会自动创建`CourseInstructor`表和你读取和更新到通过读取和更新的间接`Instructor.Courses`和`Course.Instructors`导航属性。

## <a name="entity-diagram-showing-relationships"></a>关系图显示关系的实体

下图显示为已完成的 School 模型的实体框架 Power 工具创建的关系图。

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

除了多对多关系线 (\*到\*) 和一个对多关系行 (1 到\*)，你可以在此处看到对零或一一个关系行 (1 对 0.1) 之间`Instructor`和`OfficeAssignment`实体和零-或--一对多关系行 (0.1 对\*) 教师和部门实体之间。

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>通过将代码添加到数据库上下文中自定义数据模型

接下来你将添加到新的实体`SchoolContext`类和自定义的映射使用某些[fluent API](https://msdn.microsoft.com/data/jj591617)调用。 （API 是"fluent"因为它通常用于通过连接一系列方法调用一起成一条语句。）

在本教程中将仅为不能具有属性的数据库映射中使用 fluent API。 但是，你可以使用 fluent API 指定的格式、 验证和映射规则，你可以通过使用特性执行大多数。 某些属性，如`MinimumLength`不能使用 fluent API 应用。 如前所述，`MinimumLength`不会更改架构，它仅适用于客户端和服务器端验证规则

一些开发人员更愿意使用 fluent API 以独占方式，以便它们能够获得它们的实体类"清理"。 如果你想，并且有几个仅可由使用 fluent API 中完成的自定义项，可以混合属性和 fluent API，但建议的做法通常是选择这两种方法之一，并使用该一致地尽可能多地。

若要添加新的实体数据模型和执行未通过使用属性执行操作的数据库映射中的代码替换*DAL\SchoolContext.cs*替换为以下代码：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

中的新语句[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法配置多对多联接表：

- 之间的多对多关系`Instructor`和`Course`实体，该代码指定联接表的表和列名称。 代码首先可以配置多对多关系为你不使用此代码，但如果你不调用它，你将获取默认名称类似于`InstructorInstructorID`为`InstructorID`列。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

以下代码举例说明如何你本来也可以使用 fluent API 而不是特性来指定之间的关系`Instructor`和`OfficeAssignment`实体：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

有关在幕后做什么"fluent API"语句的信息，请参阅[Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx)博客文章。

## <a name="seed-the-database-with-test-data"></a>种子使用测试数据的数据库

中的代码替换*Migrations\Configuration.cs*文件替换为以下代码，以便为你已创建的新实体提供种子数据。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

正如你看到的第一个教程中，大多数的此代码只需更新或创建新的实体对象并将示例数据加载到所需的测试的属性。 但请注意如何`Course`实体，具有多对多关系与`Instructor`实体，进行处理：

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

当你创建`Course`对象，初始化`Instructors`导航属性为空集合使用的代码`Instructors = new List<Instructor>()`。 这样便能添加`Instructor`与此相关的实体`Course`使用`Instructors.Add`方法。 如果你尚未创建一个空列表，你将无法添加这些关系，因为`Instructors`属性将为 null，并且将不会有`Add`方法。 你还可以向构造函数添加的列表初始化。

## <a name="add-a-migration-and-update-the-database"></a>添加迁移并更新数据库

从 PMC，输入`add-migration`命令：

`PM> add-Migration Chap4`

如果你尝试在此时更新数据库，你将收到以下错误：

*ALTER TABLE 语句与外键约束冲突"FK\_dbo。课程\_dbo。部门\_DepartmentID"。冲突发生于数据库"ContosoUniversity"表"dbo。部门"，列 DepartmentID。*

编辑&lt;*时间戳&gt;\_Chap4.cs*文件中，并进行下面的代码更改 (将添加 SQL 语句和修改`AddColumn`语句):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(请确保注释掉或删除现有`AddColumn`行添加新库项，或当你输入时，将收到错误时`update-database`命令。)

有时时执行现有数据的迁移，你需要存根 （stub） 数据插入到数据库以满足外键约束，并且现在你要做。 生成的代码将添加不可为 null`DepartmentID`外的键与`Course`表。 如果已有中行`Course`表当代码运行，`AddColumn`操作将失败，因为 SQL Server 并不知道要放入中不能为 null 的列的值。 因此，你已更改的代码，为新列提供默认值，并创建了名为"Temp"使其作为默认部门的存根 （stub） 部门。 因此，如果存在`Course`行时此代码运行时，它们将所有相关到"Temp"部门。

当`Seed`方法运行时，它将插入中行`Department`表和它将与现有`Course`到这些新行`Department`行。 如果你尚未添加任何课程，在 UI 中，则不再需要"Temp"部门或默认值上`Course.DepartmentID`列。 若要允许是否可能会出现，有人可能添加课程使用应用程序，你还想要更新`Seed`方法代码，以确保所有`Course`行 (而不仅仅是由早期的运行插入`Seed`方法) 具有有效`DepartmentID`值之前，请删除默认值列中的值并删除"Temp"部门。

完成编辑后&lt;*时间戳&gt;\_Chap4.cs*文件中，输入`update-database`命令 PMC 执行迁移。

> [!NOTE]
> 很可能会迁移数据和进行架构更改时获得其他错误。 如果您无法解析的迁移错误，您可以更改中的连接字符串*Web.config*文件或删除数据库。 最简单方法是在数据库重命名*Web.config*文件。 例如，将数据库名称更改为 CU\_测试如在下面的示例所示：
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  与新数据库，没有数据迁移，和`update-database`命令是更可能完成且未发生错误。 有关如何删除数据库的说明，请参阅[如何从 Visual Studio 2012 中删除数据库](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。


打开中的数据库**服务器资源管理器**未更早版本，以及展开**表**节点以查看是否已创建的所有表。 (如果仍有**服务器资源管理器**打开从较早的时间，请单击**刷新**按钮。)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

你未创建的模型类`CourseInstructor`表。 如前面所述，这是之间的多对多关系的联接表`Instructor`和`Course`实体。

右键单击`CourseInstructor`表，然后选择**显示表数据**以验证它的结果中有数据`Instructor`添加到实体`Course.Instructors`导航属性。

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>摘要

现在将具有更复杂的数据模型和相应的数据库。 在以下教程中你将了解有关访问相关的数据的不同方式的详细信息。

在找不到其他实体框架资源的链接[ASP.NET 数据访问内容映射](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一页](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
