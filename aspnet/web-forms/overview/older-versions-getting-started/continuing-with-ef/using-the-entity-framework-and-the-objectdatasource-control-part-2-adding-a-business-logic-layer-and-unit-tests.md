---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: "使用 Entity Framework 4.0 和 ObjectDataSource 控件，第 2 部分： 添加业务逻辑层和单元测试 |Microsoft 文档"
author: tdykstra
description: "本教程系列上的 Contoso 大学 web 应用程序创建的 Getting Started with 实体 Framework 4.0 教程系列生成。 I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: df37acd8901b457f7887afe767d42d53e45e4815
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>使用 Entity Framework 4.0 和 ObjectDataSource 控件，第 2 部分： 添加业务逻辑层和单元测试
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> 本教程系列上的 Contoso 大学 web 应用程序创建的生成[Getting Started with 实体 Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教程系列。 如果未完成前面的教程，作为一个起始点本教程，你可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)，你应已创建。 你还可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整教程系列创建。 如果你有疑问的教程，你可以发布到[ASP.NET 实体框架论坛](https://forums.asp.net/1227.aspx)。


在前面的教程中创建使用实体框架的 n 层 web 应用程序和`ObjectDataSource`控件。 本教程演示如何添加业务逻辑，同时保持业务逻辑层 (BLL) 和数据访问层 (DAL) 是独立的并显示如何为 BLL 创建自动的单元测试。

在本教程中，你将完成以下任务：

- 创建存储库声明接口，你需要的数据访问方法。
- 存储库类中实现存储库的接口。
- 创建调用存储库类以执行数据访问功能的业务逻辑类。
- 连接`ObjectDataSource`控件添加到存储库类而不是业务逻辑类。
- 创建单元测试项目和为其数据存储区使用内存中集合的存储库类。
- 创建你想要将添加到业务逻辑类，则运行测试并查看其失败的业务逻辑的单元测试。
- 实现业务逻辑在业务逻辑类中，然后重新运行单元测试并查看之通过。

您将使用*Departments.aspx*和*DepartmentsAdd.aspx*你在前面的教程中创建的页。

## <a name="creating-a-repository-interface"></a>创建存储库接口

你将首先创建存储库接口。

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

在*DAL*文件夹中，创建一个新的类文件，将其命名为*ISchoolRepository.cs*，并将现有代码替换为以下代码：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

该接口定义一种方法为每个 CRUD （创建、 读取、 更新、 删除） 存储库类中创建的方法。

在`SchoolRepository`类*SchoolRepository.cs*，指示此类实现`ISchoolRepository`接口：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>创建业务逻辑类

接下来，你将创建业务逻辑类。 执行此操作，以便您可以添加将由执行的业务逻辑`ObjectDataSource`控件，但不会尚未执行的。 现在，新的业务逻辑类将仅执行相同的 CRUD 操作执行存储库。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

创建一个新文件夹并将其命名*BLL*。 (在实际应用中，业务逻辑层通常的实现为类库 — 一个单独的项目，但为了简化本教程，BLL 类将保留在项目文件夹。)

在*BLL*文件夹中，创建一个新的类文件，将其命名为*SchoolBL.cs*，并将现有代码替换为以下代码：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

此代码将创建之前在存储库类中，你看到的相同 CRUD 方法但而不是直接访问的实体框架方法，它调用的存储库类方法。

保存对存储库类的引用类变量定义为接口类型，并且实例化的存储库类的代码包含在两个构造函数。 无参数构造函数将由`ObjectDataSource`控件。 它创建的实例`SchoolRepository`前面创建的类。 另一个构造函数允许实例化的业务逻辑类以实现存储库接口的任何对象中传递的任何代码。

调用存储库类和两个构造函数的 CRUD 方法，使其可以与你选择任何后端数据存储区中使用业务逻辑类。 业务逻辑类不必注意它所调用类如何保持数据。 (此情况通常称作*持久性无感知*。)因为你可以连接到使用的内容作为一种简单的存储库实现的业务逻辑类来帮助进行单元测试，内存中作为`List`来存储数据的集合。

> [!NOTE]
> 从技术上讲，实体对象不仍不持久性未知，因为它们正在从实体框架的继承的类实例化`EntityObject`类。 对于完整持久性无感知，你可以使用*纯旧 CLR 对象*，或*POCOs*，来从继承的对象代替`EntityObject`类。 使用 POCOs 不在本教程的范围。 有关详细信息，请参阅[可测试性和 Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) MSDN 网站上。)


现在你可以连接`ObjectDataSource`业务逻辑的控件类而不是到存储库并验证一切就绪像以前一样。

在*Departments.aspx*和*DepartmentsAdd.aspx*，更改每个匹配项`TypeName="ContosoUniversity.DAL.SchoolRepository"`到`TypeName="ContosoUniversity.BLL.SchoolBL`"。 有四个实例中所有。）

运行*Departments.aspx*和*DepartmentsAdd.aspx*页以验证它们仍工作以前一样。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>创建单元测试项目和存储库实现

将新项目添加到解决方案使用**测试项目**模板，并将其命名`ContosoUniversity.Tests`。

在测试项目中，添加对的引用`System.Data.Entity`并添加到项目引用`ContosoUniversity`项目。

你现在可以创建将用于单元测试的存储库类。 此存储库的数据存储将在类中。

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

在测试项目中，创建一个新的类文件，将其*MockSchoolRepository.cs*，并将现有代码替换为以下代码：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

此存储库类具有相同的 CRUD 方法与直接访问实体框架，但它们适用于的`List`而不是与数据库的内存中集合。 这样会对测试类来设置和验证业务逻辑类的单元测试更容易。

## <a name="creating-unit-tests"></a>创建单元测试

**测试**项目模板创建一个存根 （stub） 单元测试类，并且下一个任务是通过向其添加单元测试方法，你想要将添加到业务逻辑类的业务逻辑中修改此类。

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Contoso 大学，任何单个教师只能是单个部门的管理员，你需要添加业务逻辑来强制执行此规则。 首先，将通过添加测试和运行测试后，若要查看这些失败。 然后，你将添加代码并重新运行测试，预期会看到它们通过。

打开*UnitTest1.cs*文件并添加`using`ContosoUniversity 项目中创建业务逻辑和数据访问层的语句：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

替换`TestMethod1`方法使用以下方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL`方法创建的存储库类创建单元测试项目，然后将其传递到业务逻辑类的新实例的一个实例。 然后，该方法使用业务逻辑类来在测试方法中插入三个您可以使用的部门。

测试方法可以验证业务逻辑类引发异常，如果有人试图插入一个新部门有相同的管理员为现有的部门，或如果有人试图通过将其设置为个人的 ID 更新部门的管理员谁已经是另一个部门的管理员。

你尚未创建异常类，因此将不会编译此代码。 若要获取编译它，右键单击`DuplicateAdministratorException`和选择**生成**，，然后**类**。

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

这可以删除其中的测试项目中创建一个类主项目中创建的异常类后。 实现业务逻辑。

运行该测试项目。 按预期方式运行测试将失败。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>添加业务逻辑，以使测试通过

接下来，你将实施使得无法设置为部门的管理员已经是另一个部门的管理员的人员的业务逻辑。 你将从业务逻辑层中，引发异常并将然后将其表示层中捕获如果用户编辑一个部门，并单击**更新**选择已经是管理员的任何人后。 （你还可以从下拉列表中的用户已是管理员之前呈现页上，, 删除教师，但此处的目的是要使用业务逻辑层）。

首先，创建在用户尝试使多个部门的管理员的一个教师时将引发的异常类。 在主项目中，在其中创建新类文件*BLL*文件夹中，将其命名为*DuplicateAdministratorException.cs*，并将现有代码替换为以下代码：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

现在删除该临时*DuplicateAdministratorException.cs*你测试项目中创建更早版本以便能够编译的文件。

在主项目中，打开*SchoolBL.cs*文件并添加以下包含验证逻辑的方法。 （代码是指将更高版本创建的方法。）

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

要插入或更新时，将调用此方法`Department`以便检查是否另一个部门已有相同的管理员的实体。

该代码调用方法要搜索的数据库`Department`具有相同的实体`Administrator`作为实体的属性值正在被插入或更新。 如果找到一个对象，该代码将引发异常。 不进行验证检查是必需的如果正在插入或更新的实体未包含任何`Administrator`如果在更新过程中调用该方法引发值和任何异常和`Department`找到匹配项的实体`Department`正在更新的实体。

调用中的新方法`Insert`和`Update`方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

在*ISchoolRepository.cs*，添加新的数据访问方法的以下声明：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

在*SchoolRepository.cs*，添加以下`using`语句：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

在*SchoolRepository.cs*，添加以下新的数据访问方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

此代码检索`Department`具有指定的管理员的实体。 （如果有），则应找到只有一个部门。 但是，由于没有约束构建在数据库中，返回类型是一个集合，如果找到多个部门。

默认情况下，当对象上下文中从数据库中检索实体时它将跟踪的它们在其对象状态管理器中。 `MergeOption.NoTracking`参数指定此查询将不完成此跟踪。 这是必需的因为查询可能返回您正在尝试进行更新，确切实体，然后你将无法附加该实体。 例如，如果编辑中的历史记录部门*Departments.aspx*页和将管理员保留不变，此查询将返回历史系。 如果`NoTracking`未设置，在其对象状态管理器中的对象上下文将已有历史记录部门实体。 然后对象上下文附加重新创建从视图状态的历史记录部门实体时，会引发异常，指出`"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`。

(作为指定的替代方法`MergeOption.NoTracking`，你可以创建新的对象上下文只需为此查询。 因为新的对象上下文应具有其自己的对象状态管理器，有可能会不会发生冲突时调用`Attach`方法。 新的对象上下文将与原始对象上下文中，共享元数据和数据库连接，因此此另一种方法对性能的影响将最小。 在这里，显示的方法，但是，引入了`NoTracking`选项，你将找到在其他上下文中有用。 `NoTracking`讨论选项进一步在本系列后面的教程。)

在测试项目中，添加新数据访问方法*MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

此代码使用 LINQ 来执行相同的数据选择，`ContosoUniversity`项目存储库使用 LINQ to Entities 有关。

再次运行测试项目。 这次测试通过了。

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>处理 ObjectDataSource 异常

在`ContosoUniversity`项目中，运行*Departments.aspx*页上，尝试将某个部门管理员更改为人已经是另一个部门的管理员。 （记住，你只能编辑在本教程中，过程中添加的部门因为数据库将具有无效数据预加载）。获取以下服务器错误页面：

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

你不希望用户看到的错误页面，此类型，因此，你需要添加错误处理代码。 打开*Departments.aspx*和指定的处理程序`OnUpdated`事件`DepartmentsObjectDataSource`。 `ObjectDataSource`现在开始标记类似于下面的示例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

在*Departments.aspx.cs*，添加以下`using`语句：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

添加以下处理程序`Updated`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

如果`ObjectDataSource`控件捕获异常，它尝试执行更新时，它将异常传递中的事件自变量 (`e`) 到此处理程序。 处理程序中的代码检查该异常是否是重复管理员异常。 如果是，代码将创建包含的错误消息的验证程序控件`ValidationSummary`控件来显示。

运行页面，并尝试再次使人成为两个部门的管理员。 这一次`ValidationSummary`控件将显示一条错误消息。

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

进行类似更改到*DepartmentsAdd.aspx*页。 在*DepartmentsAdd.aspx*，指定的处理程序`OnInserted`事件`DepartmentsObjectDataSource`。 生成的标记将类似于下面的示例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

在*DepartmentsAdd.aspx.cs*，将同一`using`语句：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

添加以下事件处理程序：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

你现在可以测试*DepartmentsAdd.aspx.cs*页以验证它还能正确地处理尝试使多个部门的管理员的人。

这将完成实现使用的存储库模式简介`ObjectDataSource`与实体框架的控件。 有关存储库模式和可测试性的详细信息，请参阅 MSDN 白皮书[可测试性和 Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx)。

在以下教程中，你将看到如何添加排序和筛选到应用程序的功能。

>[!div class="step-by-step"]
[上一页](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
[下一页](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
