---
title: "ASP.NET Core MVC 和 EF Core - 数据模型 - 第 5 个教程（共 10 个）"
author: tdykstra
description: "本教程将添加更多实体和关系，并通过指定格式设置、验证和数据库映射规则来自定义数据模型。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: ac30d9ae5531934ba5163a8d9114b11ac54af8d2
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a>创建复杂数据模型 - EF Core 和 ASP.NET Core MVC 教程（第 5 个教程，共 10 个）

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University 示例 Web 应用程序演示如何使用 Entity Framework Core 和 Visual Studio 创建 ASP.NET Core MVC Web 应用程序。 若要了解教程系列，请参阅[本系列中的第一个教程](intro.md)。

之前的教程介绍了由三个实体组成的简单数据模型。 本教程将添加更多实体和关系，并通过指定格式化、验证和数据库映射规则来自定义数据模型。

完成本教程后，实体类将构成下图所示的完整数据模型：

![实体关系图](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>使用特性自定义数据模型

本节介绍如何使用指定格式化、验证和数据库映射规则的特性来自定义数据模型。 随后在接下来的几节中创建完整的学校数据模型，创建方法：向已创建的类添加特性，并为模型中剩余的实体类型创建新类。

### <a name="the-datatype-attribute"></a>DataType 特性

对于学生注册日期，目前所有网页都显示有时间和日期，尽管对此字段而言重要的只是日期。 使用数据注释特性，可更改一次代码，修复每个视图中数据的显示格式。 若要查看如何执行此操作，请向 `Student` 类的 `EnrollmentDate` 属性添加一个特性。

在 Models/Student.cs 中，为 `System.ComponentModel.DataAnnotations` 命名空间添加一个 `using` 语句，将 `DataType` 和 `DisplayFormat` 特性添加到 `EnrollmentDate` 属性，如以下示例所示：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

`DataType` 属性用于指定比数据库内部类型更具体的数据类型。 在此示例中，我们只想跟踪日期，而不是日期和时间。 `DataType` 枚举提供了多种数据类型，例如日期、时间、电话号码、货币、电子邮件地址等。 应用程序还可通过 `DataType` 特性自动提供类型特定的功能。 例如，可以为 `DataType.EmailAddress` 创建 `mailto:` 链接，并且可以在支持 HTML5 的浏览器中为 `DataType.Date` 提供日期选择器。 `DataType` 特性发出 HTML 5 `data-`（读作 data dash）特性供 HTML 5 浏览器理解。 `DataType` 特性不提供任何验证。

`DataType.Date` 不指定显示日期的格式。 默认情况下，数据字段根据默认格式显示，后者以服务器的 CultureInfo 为基础。

`DisplayFormat` 特性用于显式指定日期格式：

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` 设置指定在文本框中显示值以进行编辑时也应用格式。 （你可能不想为某些字段执行此操作 — 例如，对于货币值，你可能不希望文本框中的货币符号可编辑。）

可单独使用 `DisplayFormat` 特性，但通常建议同时使用 `DataType` 特性。 `DataType` 特性传达数据的语义而不是传达数据在屏幕上的呈现方式，并提供 `DisplayFormat` 不具备的以下优势：

* 浏览器可启用 HTML5 功能（例如，显示日历控件、区域设置适用的货币符号、电子邮件链接、某种客户端输入验证等）。

* 默认情况下，浏览器将根据区域设置采用正确的格式呈现数据。

有关详细信息，请参阅 [\<input> 标记帮助器文档](../../mvc/views/working-with-forms.md#the-input-tag-helper)。

运行应用，进入“学生”索引页，注意注册日期中不再显示时间。 任何使用“学生”模型的视图都是如此。

![“学生”索引页显示不带时间的日期](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength 特性

还可使用特性指定数据验证规则和验证错误消息。 `StringLength` 特性设置数据库中的最大长度，并为 ASP.NET MVC 提供客户端和服务器端验证。 还可在此属性中指定最小字符串长度，但最小值对数据库架构没有影响。

假设要确保用户输入的名称不超过 50 个字符。 若要添加此限制，请将 `StringLength` 特性添加到 `LastName` 和 `FirstMidName` 属性，如下例所示：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

`StringLength` 特性不会阻止用户在名称中输入空格。 可使用 `RegularExpression` 特性应用输入限制。 例如，以下代码要求第一个字符为大写，其余字符按字母顺序排列：

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

`MaxLength` 特性的作用与 `StringLength` 特性类似，但不提供客户端验证。

现在数据库模型发生了变化，需要更改数据库架构。 请使用迁移来更新架构，迁移时不会丢失通过应用程序 UI 添加到数据库的任何数据。

保存更改并生成项目。 随后打开项目文件夹中的命令窗口并输入以下命令：

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

`migrations add` 命令发出警告：可能出现数据丢失，因为更改后，有两列的最大长度缩短。  迁移时会创建一个名为 \<timeStamp>_MaxLengthOnNames.cs 的文件。 此文件包含 `Up` 方法中的代码，该代码将更新数据库以匹配当前数据模型。 `database update` 命令运行该代码。

Entity Framework 使用迁移文件名的前缀时间戳发出迁移命令。 可在运行 update-database 命令前创建多个迁移，然后按照创建顺序应用所有迁移。

运行该应用，选择“学生”选项卡，单击“新建”，然后输入名称（超过 50 个字符）。 单击“创建”时，客户端验证会显示一条错误消息。

![显示字符串长度错误的“学生索引”页](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>Column 特性

还可使用特性来控制类和属性映射到数据库的方式。 假设在名字字段使用了 `FirstMidName`，这是因为该字段也可能包含中间名。 但却希望将数据库列命名为 `FirstName`，因为要针对数据库编写即席查询的用户习惯使用该姓名。 若要进行此映射，可使用 `Column` 特性。

`Column` 特性指定，创建数据库时，映射到 `FirstMidName` 属性的 `Student` 表的列将被命名为 `FirstName`。 换言之，在代码引用 `Student.FirstMidName` 时，数据来源将是 `Student` 表的 `FirstName` 列或在其中进行更新。 如果不指定列名称，则其名称与属性名称相同。

在 Student.cs 文件中，为 `System.ComponentModel.DataAnnotations.Schema` 添加一个 `using` 语句，并将列名称特性添加到 `FirstMidName` 属性，如以下突出显示的代码所示：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

添加 `Column` 特性后，`SchoolContext` 的支持模型会发生改变，与数据库不再匹配。

保存更改并生成项目。 随后打开项目文件夹中的命令窗口，输入以下命令，创建另一个迁移：

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

在 SQL Server 对象资源管理器中，双击 Student 表，打开 Student 表设计器。

![迁移后 SSOX 中的 Students 表](complex-data-model/_static/ssox-after-migration.png)

在进行前两次迁移前，名称列的类型为 nvarchar (MAX)。 而现在则是 nvarchar (50)，列名从 FirstMidName 变为 FirstName。

> [!Note]
> 如果尚未按以下各节所述创建所有实体类就尝试进行编译，则可能会出现编译器错误。

## <a name="final-changes-to-the-student-entity"></a>Student 实体的最终更改
![Student 实体](complex-data-model/_static/student-entity.png)

在 Models/Student.cs 中，将之前添加的代码替换为以下代码。 突出显示所作更改。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Required 特性

`Required` 特性使名称属性成为必填字段。 值类型（DateTime、int、double、float 等）等不可为 null 的类型不需要 `Required` 特性。 系统会将不可为 null 的类型自动视为必填字段。

可删除 `Required` 特性，并用 `StringLength` 特性的最小长度参数来替换：

可删除 `Required` 特性，并用 `StringLength` 特性的最小长度参数来替换：

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Display 特性

`Display` 特性指定文本框的标题应是“名”、“姓”、“全名”和“注册日期”，而不是每个实例中的属性名称（其中没有分隔单词的空格）。

### <a name="the-fullname-calculated-property"></a>FullName 计算属性

`FullName` 是计算属性，可返回通过串联两个其他属性创建的值。 因此它只有一个 get 访问器，且数据库中不会生成 `FullName` 列。

## <a name="create-the-instructor-entity"></a>创建 Instructor 实体

![Instructor 实体](complex-data-model/_static/instructor-entity.png)

创建 Models/Instructor.cs，使用以下代码替换模板代码：

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

请注意，在 Student 和 Instructor 实体中有几个属性是相同的。 本系列后面的[实现继承](inheritance.md)教程将重构此代码以消除冗余。

可在一行中放置多个特性，因此也可以按如下所示编写 `HireDate` 特性：

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments 和 OfficeAssignment 导航属性

`CourseAssignments` 和 `OfficeAssignment` 是导航属性。

一名讲师可以教授任意数量的课程，因此 `CourseAssignments` 定义为集合。

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

如果导航属性可包含多个实体，则其类型必须是可添加、可删除和可更新实体的列表。  可指定 `ICollection<T>` 或诸如 `List<T>` 或 `HashSet<T>` 的类型。 如果指定 `ICollection<T>`，EF 默认会创建一个 `HashSet<T>` 集合。

下面在关于多对多关系的部分中解释了这些作为 `CourseAssignment` 实体的原因。

Contoso University 业务规则规定，讲师最多只能有一个办公室，因此 `OfficeAssignment` 属性拥有一个 OfficeAssignment 实体（如果未分配办公室，则该实体可能为 null）。

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>创建 OfficeAssignment 实体

![OfficeAssignment 实体](complex-data-model/_static/officeassignment-entity.png)

用以下代码创建 Models/OfficeAssignment.cs：

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Key 特性

讲师与 OfficeAssignment 实体间存在一对零或一的关系。 办公室分配仅与分配有办公室的讲师相关，因此其主键也是 Instructor 实体的外键。 但 Entity Framework 无法自动识别 InstructorID 作为此实体的主键，因为其名称不符合 ID 或 classnameID 命名约定。 因此，`Key` 特性用于将其识别为主键：

```csharp
[Key]
public int InstructorID { get; set; }
```

如果实体具有自己的主键，但你希望使用 classnameID 或 ID 以外的其他属性名称，则也可使用 `Key` 特性。

默认情况下，EF 将键视为非数据库生成，因为该列面向的是识别关系。

### <a name="the-instructor-navigation-property"></a>Instructor 导航属性

Instructor 实体具有可为 null `OfficeAssignment` 导航属性（因为可能未向讲师分配办公室），而 OfficeAssignment 实体具有不可为 null `Instructor` 导航属性 （因为在没有讲师的情况下不会分配办公室 - `InstructorID` 不可为 null）。 Instructor 实体具有相关 OfficeAssignment 实体时，每个实体都将在其导航属性中引用另一实体。

可向 Instructor 导航属性添加 `[Required]` 特性，指定必须有相关讲师，但这不是必须的，因为 `InstructorID` 外键（也是此表的键）不可为 null。

## <a name="modify-the-course-entity"></a>修改 Course 实体

![Course 实体](complex-data-model/_static/course-entity.png)

在 Models/Course.cs 中，将之前添加的代码替换为以下代码。 突出显示所作更改。

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Course 实体具有外键属性 `DepartmentID`，该属性指向相关 Department 实体，同时它还具有 `Department` 导航属性。

如果拥有相关实体的导航属性，则 Entity Framework 不会要求为数据模型添加外键属性。  只要有需要，EF 就会自动在数据库中创建外键，并为其创建[阴影属性](https://docs.microsoft.com/ef/core/modeling/shadow-properties)。 但如果数据模型包含外键，则更新会变得更简单、更高效。 例如，提取 Course 实体进行编辑时，如果未加载该实体，那么 Department 实体为 null，因此，更新 Course 实体时，必须先提取 Department 实体。 数据模型中包含外键属性 `DepartmentID` 时，更新前无需提取 Department 实体。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 特性

`CourseID` 属性上具有 `None` 参数的 `DatabaseGenerated` 特性指定主键值由用户提供，而不是由数据库生成。

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

默认情况下，Entity Framework 假定主键值由数据库生成。 大多数情况下，这是理想情况。 但对 Course 实体而言，需使用用户指定的课程编号，例如一个系为 1000 系列，另一个系为 2000 系列等。

`DatabaseGenerated` 特性也可用于生成默认值，如在用于记录行创建或更新日期的数据库列中。  有关详细信息，请参阅[生成的属性](https://docs.microsoft.com/ef/core/modeling/generated-properties)。

### <a name="foreign-key-and-navigation-properties"></a>外键和导航属性

Course 实体中的外键属性和导航属性可反映以下关系：

向一个系分配课程后，出于上述原因，会出现 `DepartmentID` 外键和 `Department` 导航属性。

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

参与一门课程的学生数量不定，因此 `Enrollments` 导航属性是一个集合：

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

一门课程可能有多位授课讲师，因此 `CourseAssignments` 导航属性是一个集合（[稍后](#many-to-many-relationships)会解释 `CourseAssignment` 类型）：

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>创建 Department 实体

![Department 实体](complex-data-model/_static/department-entity.png)


用以下代码创建 Models/Department.cs：

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Column 特性

`Column` 特性之前用于更改列名称映射。 在 Department 实体的代码中，`Column` 特性用于更改 SQL 数据类型映射，以便使用数据库中的 SQL Server 货币类型来定义该列：

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

通常不需要列映射，因为 Entity Framework 会根据你为属性定义的 CLR 类型选择适当的 SQL Server 数据类型。 CLR `decimal` 类型会映射到 SQL Server `decimal` 类型。 但假如你知道该列要表示货币金额，那么货币数据类型会更加合适。

### <a name="foreign-key-and-navigation-properties"></a>外键和导航属性

外键和导航属性可反映以下关系：

一个系可能有也可能没有管理员，而管理员始终是讲师。 因此 `InstructorID` 属性作为 Instructor 实体内外键，且 `int` 类型指定后跟有一个问号，将该属性标记为可为 null。 导航属性名为 `Administrator`，但其中包含 Instructor 实体：

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

一个系可以有多门课程，因此存在 Course 导航属性：

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> 按照约定，Entity Framework 能针对不可为 null 的外键和多对多关系启用级联删除。 这可能导致循环级联删除规则，尝试添加迁移时该规则会造成异常。 例如，如果未将 Department.InstructorID 属性定义为可为 null，那么在删除系时，EF 会配置级联删除规则来删除讲师，这是预期外的情况。 如果业务规则要求 `InstructorID` 属性不可为 null，则必须使用以下 Fluent API 语句禁用关系中的级联删除：
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>修改 Enrollment 实体

![Enrollment 实体](complex-data-model/_static/enrollment-entity.png)

在 Models/Enrollment.cs 中，将之前添加的代码替换为以下代码：

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>外键和导航属性

外键属性和导航属性可反映以下关系：

注册记录面向一门课程，因此存在 `CourseID` 外键属性和 `Course` 导航属性：

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

注册记录面向一名学生，因此存在 `StudentID` 外键属性和 `Student` 导航属性：

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>多对多关系

Student 和 Course 实体间存在多对多关系，Enrollment 实体在数据库中充当带有效负载的多对多联接表。 “带有效负载”是指 Enrollment 表包含除联接表外键之外的其他数据（在此示例中为主键和 Grade 属性）。

下图显示这些关系在实体关系图中的外观。 （该图是使用 EF 6.x 的 Entity Framework Power Tools 生成的，教程未介绍如何创建该图，此处仅作为示例使用。）

![学生-课程之间的多对多关系](complex-data-model/_static/student-course.png)

每条关系线的一端显示 1，另一端显示星号 (*)，这表示一对多关系。

如果 Enrollment 表不包含年级信息，则它只需包含两个外键（CourseID 和 StudentID）。 在这种情况下，该表是数据库中不带有效负载的多对多联接表（或纯联接表）。 Instructor 和 Course 实体具有这种多对多关系，下一步是创建实体类，将其作为不带有效负载的联接表。

（EF 6.x 支持多对多关系的隐式联接表，但 EF Core 不支持。 有关详细信息，请参阅 [EF Core GitHub 存储库中的讨论](https://github.com/aspnet/EntityFramework/issues/1368)。） 

## <a name="the-courseassignment-entity"></a>CourseAssignment 实体

![CourseAssignment 实体](complex-data-model/_static/courseassignment-entity.png)

用以下代码创建 Models/CourseAssignment.cs：

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>联接实体名称

数据库中的 Instructor 与 Course 多对多关系需要联接表，并且必须由实体集表示。 将联接实体命名为 `EntityName1EntityName2` 很常见，在此示例中实体名为 `CourseInstructor`。 但是，建议选择一个可描述关系的名称。 数据模型开始很简单，且会不断增长，随后无有效负载联接会频繁获取有效负载。 如果一开始实体名称为描述性名称，那么之后就不必更改名称。 理想情况下，联接实体在业务域中可能具有自己的自带名称（可能是单个字）。 例如，可通过 Rating 链接 Book 和 Customer。 对于此关系，相比 `CourseInstructor`，`CourseAssignment` 是更好的选择。

### <a name="composite-key"></a>组合键

由于外键不可为 null，且它们共同唯一标识表的每一行，因此不需要单独的主键。 InstructorID 和 CourseID 属性应充当组合主键。 标识 EF 组合主键的唯一方法是使用 Fluent API（无法借助特性来完成）。 下一节将介绍如何配置组合主键。

在一个课程可以有多个行，一个讲师可以有多个行的情况下，组合键可确保同一讲师和课程不会有多个行。 `Enrollment` 联接实体定义其自己的主键，因此可能会出现此类重复。 若要防止出现此类重复，可在外键字段上添加唯一索引，或使用类似于 `CourseAssignment` 的主组合键配置 `Enrollment`。 有关详细信息，请参阅[索引](https://docs.microsoft.com/ef/core/modeling/indexes)。

## <a name="update-the-database-context"></a>更新数据库上下文

将以下突出显示的代码添加到 Data/SchoolContext.cs 文件：

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

此代码添加新实体并配置 CourseAssignment 实体的组合主键。

## <a name="fluent-api-alternative-to-attributes"></a>用 Fluent API 替代特性

`DbContext` 类的 `OnModelCreating` 方法中的代码使用 Fluent API 来配置 EF 行为。 API 称为“Fluent”，因为它通常在将一系列方法调用连接成单个语句后才能使用，如 [EF Core 文档](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) 中的此示例所示：

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

本教程仅将 Fluent API 用于数据库映射，这是无法使用特性实现的。 但 Fluent API 还可用于指定大多数格式化、验证和映射规则，这可通过特性完成。 `MinimumLength` 等特性不能通过 Fluent API 应用。 如前所述，`MinimumLength` 不会更改架构，它只应用客户端和服务器端验证规则。

某些开发者倾向于仅使用 Fluent API 以保持实体类的“纯净”。 如有需要，可混合使用特性和 Fluent API，且有些自定义只能通过 Fluent API 实现，但通常建议选择一种方法并尽可能坚持使用这一种。 如果确实要使用两种，请注意，只要出现冲突，Fluent API 就会覆盖特性。

有关特性和 Fluent API 的详细信息，请参阅[配置方法](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)。

## <a name="entity-diagram-showing-relationships"></a>显示关系的实体关系图

下图显示 Entity Framework Power Tools 针对已完成的学校模型创建的关系图。

![实体关系图](complex-data-model/_static/diagram.png)

除一对多关系线（1到 \*）外，此处还显示了 Instructor 和 OfficeAssignment 实体间的一对零或一关系线（1 到 0..1），以及 Instructor 和 Department 实体间的零对多或一对多关系线（0..1 到 *）。

## <a name="seed-the-database-with-test-data"></a>使用测试数据设定数据库种子

使用以下代码替换 Data/DbInitializer.cs 文件中的代码，从而为创建的新实体提供种子数据。

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

如第一个教程所述，大部分此类代码仅创建新实体对象，并按测试要求将示例数据加载到属性中。 注意多对多关系的处理方法：代码在 `Enrollments` 和 `CourseAssignment` 联接实体集中创建实体，以此来创建关系。

## <a name="add-a-migration"></a>添加迁移

保存更改并生成项目。 然后打开项目文件夹中的命令窗口，输入 `migrations add` 命令（先不要执行 update-database 命令）：

```console
dotnet ef migrations add ComplexDataModel
```

会出现数据可能丢失的警告。

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

如果此时尝试运行 `database update` 命令（先不要执行此操作），则会出现以下错误：

> ALTER TABLE 语句与 FOREIGN KEY 约束“FK_dbo.Course_dbo.Department_DepartmentID”冲突。 冲突发生位置：数据库“ContosoUniversity”、表“dbo.Department”和列“DepartmentID”。

有时使用现有数据执行迁移时，需将存根数据插入数据库，满足外键约束。 `Up` 方法中生成的代码将不可为 null 的 DepartmentID 外键添加到 Course 表中。 如果代码运行时，Course 表中已经有了行，则 `AddColumn` 操作失败，因为 SQL Server 不知道要向不可为 null 的列中放入什么值。 本教程将在新数据库上运行迁移，但在生产应用程序中，必须使迁移处理现有数据，因此下方通过示例介绍如何执行此操作。

为使此迁移处理现有数据，必须更改代码，赋予新列默认值，并创建一个名为“Temp”的存根系，作为默认系。 之后，Course 行将在 `Up` 方法运行后与“Temp”系建立联系。

* 打开 {timestamp}_ComplexDataModel.cs 文件。 

* 对将 DepartmentID 列添加到 Course 表的代码行添加注释。

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* 在创建 Department 表的代码后添加以下突出显示的代码：

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

在生产应用程序中，可编写代码或脚本来添加 Department 行并将 Course 行与新 Department 行相关联。 随后将不在需要“Temp”系或 Course.DepartmentID 列中的默认值。

保存更改并生成项目。

## <a name="change-the-connection-string-and-update-the-database"></a>更改连接字符串并更新数据库

现在 `DbInitializer` 类中就有了新代码，可将新实体的种子数据添加到空数据库。 若要让 EF 创建新的空数据库，请将 appsettings.json 中连接字符串内的数据库名称更改为 ContosoUniversity3 或正在使用的计算机上未使用过的其他名称。

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

将更改保存到 appsettings.json。

> [!NOTE]
> 除更改数据库名称外，删除数据库同样可行。 使用 SQL Server 对象资源管理器 (SSOX) 或 `database drop` CLI 命令：
> ```console
> dotnet ef database drop
> ```

更改数据库名称或删除数据库后，在命令窗口运行 `database update` 命令，执行迁移。

```console
dotnet ef database update
```

运行应用，使 `DbInitializer.Initialize` 方法运行并填充新数据库。

像之前一样在 SSOX 中打开数据库，然后展开 Tables 节点，查看是否已创建所有表。 （如果之前打开的 SSOX 尚未关闭，请单击“刷新”按钮。）

![SSOX 中的表](complex-data-model/_static/ssox-tables.png)

运行应用，触发设定数据库种子的初始化代码。

右键单击“CourseAssignment”表，然后选择“查看数据”，验证其中是否存在数据。

![SSOX 中的 CourseAssignment 数据](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>摘要

现在你就得到了更复杂的数据模型和相应的数据库。 后面教程将更多详细的介绍如何访问相关数据。

>[!div class="step-by-step"]
[上一页](migrations.md)
[下一页](read-related-data.md)  
