---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: "生成一个具有业务规则验证模型 |Microsoft 文档"
author: microsoft
description: "第 3 步演示如何创建模型，我们可以使用这两个查询，并且我们 NerdDinner 为应用程序更新数据库。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: dbe6370979f218988c168df3e80314ef9b338fbd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="build-a-model-with-business-rule-validations"></a>生成一个具有业务规则验证模型
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是一个免费的第 3 步["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 第 3 步演示如何创建模型，我们可以使用这两个查询，并且我们 NerdDinner 为应用程序更新数据库。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner 步骤 3： 构建模型

模型-视图-控制器框架中的术语"模型"是指表示应用程序，以及与之集成验证和业务规则的相应域逻辑数据的对象。 模型在基于 MVC 的应用程序的许多方面"核心"是和正如我们看到更高版本从根本上驱动器的行为。

ASP.NET MVC framework 支持使用任何数据访问技术，开发人员可以选择使用不同的丰富的.NET 数据选项，来实现其模型，包括： LINQ to 实体、 LINQ to SQL、 NHibernate、 LLBLGen Pro、 SubSonic、 WilsonORM 或只是原始 ADO。NET DataReaders 或数据集。

我们 NerdDinner 为应用程序，我们将以使用 LINQ to SQL 来创建一个简单的模型的方法相当接近于我们的数据库设计，并将一些自定义验证逻辑和业务规则添加。 我们然后将实现存储库类，可帮助消失抽象应用程序，并启用我们到轻松单元测试的其余部分的数据持久性实现。

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL 是作为.NET 3.5 的一部分附带的 ORM （对象关系映射器）。

LINQ to SQL 提供将数据库表映射到我们可以编写代码，针对的.NET 类的简单办法。 我们 NerdDinner 为应用程序我们将使用它来将晚餐和回复表映射到 Dinner 和回复类我们数据库中。 晚餐和回复表的列将对应于 Dinner 和回复类上的属性。 每个 Dinner 和回复对象将表示数据库中的晚餐或的回复表中的单独行。

LINQ to SQL 允许我们避免不得不手动构造 SQL 语句以便检索和更新 Dinner 以及回复处理数据库数据的对象。 相反，我们将定义 Dinner 和回复类中，如何它们映射到/从该数据库，以及它们之间的关系。 然后，LINQ to SQL 将采用负责生成相应的 SQL 执行逻辑，当我们进行交互，并使用它们时，在运行时使用。

我们可以使用 VB 和 C# 中的 LINQ 语言支持可以编写可表达查询来检索 Dinner 和回复从数据库对象。 这将减少量数据我们编写的代码，并允许我们构建真正干净的应用程序。

### <a name="adding-linq-to-sql-classes-to-our-project"></a>将 LINQ to SQL 类添加到我们的项目

我们将开始通过我们的项目中的"模型"文件夹中右键单击并选择**添加-&gt;新项**菜单命令：

![](build-a-model-with-business-rule-validations/_static/image1.png)

这将显示"添加新项"对话框。 我们将按"数据"类别筛选，并选择在其中的"LINQ 到 SQL 类"模板：

![](build-a-model-with-business-rule-validations/_static/image2.png)

我们将命名的项"NerdDinner"，然后单击"添加"按钮。 Visual Studio 将添加一个 NerdDinner.dbml 文件我们 \Models 目录下的，然后打开 LINQ to SQL 对象关系设计器：

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>使用 LINQ to SQL 创建数据模型类

LINQ to SQL，我们可以快速从现有数据库架构创建数据模型类。 待办事项我们想要在其中进行建模的此处我们将在服务器资源管理器中打开 NerdDinner 数据库并选择的表：

![](build-a-model-with-business-rule-validations/_static/image4.png)

然后，我们可以将表拖到 LINQ 到 SQL 设计器图面。 此 LINQ to SQL 执行时将自动创建 Dinner 和使用 （包括映射到数据库表中各列的类属性） 表的架构的回复类：

![](build-a-model-with-business-rule-validations/_static/image5.png)

默认情况下，LINQ to SQL 设计器自动"以复数形式表示"表名和列名时它创建基于数据库架构的类。 例如： 在上述示例中的"晚餐"表导致"Dinner"类。 此类命名可帮助使我们的模型与.NET 命名约定保持一致和通常找到该设计器修复这向上方便 （尤其是时遇到添加大量表）。 如果你不喜欢的类或设计器生成，但始终可以重写此方法，并将其更改为所需的任何名称的属性的名称。 你可以执行此操作通过编辑实体/属性名称中，线设计器中或通过修改通过属性网格。

默认情况下，LINQ to SQL 设计器还检查表，主键/外的键关系，并自动基于它们创建默认"关系关联"的它将创建不同的模型类之间。 例如，当我们拖动晚餐和回复表到 LINQ to SQL 设计器两者之间的一对多关系关联已推断出基于这一事实的回复表具有外键，到晚餐表 (这由中的箭头指示设计器）：

![](build-a-model-with-business-rule-validations/_static/image6.png)

上面的关联将导致 LINQ to SQL 将强类型化"Dinner"属性添加到开发人员可以使用访问 Dinner 与给定的回复关联的回复类。 它将导致 Dinner 该类具有一个"RSVPs"集合属性，使开发人员能够检索和更新与特定 Dinner 关联的回复对象。

下面可以看到举例说明了 Visual Studio 中的 intellisense，当我们创建一个新的回复对象并将其添加到 Dinner RSVPs 集合。 请注意如何 LINQ to SQL 会自动添加"RSVPs"集合上的 Dinner 对象：

![](build-a-model-with-business-rule-validations/_static/image7.png)

将回复对象添加到 Dinner RSVPs 集合我们相当于告诉 LINQ to SQL 将 Dinner 和我们的数据库中的回复行之间的外键关系相关联：

![](build-a-model-with-business-rule-validations/_static/image8.png)

如果你不喜欢如何在设计器具有建模或名为表关联，则可以重写它。 只需单击设计器中的关联箭头，然后访问通过属性网格重命名、 删除或对其进行修改其属性。 我们 NerdDinner 的应用程序，不过，默认关联规则适用于我们要生成的数据模型类和我们只需使用的默认行为。

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext 类

Visual Studio 将自动创建表示的模型和使用 LINQ to SQL 设计器定义的数据库关系的.NET 类。 LINQ to SQL DataContext 类还会为每个 LINQ to SQL 设计器文件添加到解决方案中生成。 由于我们命名我们 LINQ to SQL 类项"NerdDinner"，创建的 DataContext 类将调用"NerdDinnerDataContext"。 此 NerdDinnerDataContext 类是我们将与数据库交互的主要方法。

我们 NerdDinnerDataContext 该类会公开两个属性-"晚餐"和"RSVPs"-表示我们建模数据库中的两个表。 我们可以使用 C# 来编写针对这些属性的 LINQ 查询的查询和检索 Dinner 和回复对象从数据库。

下面的代码演示如何实例化 NerdDinnerDataContext 对象和执行 LINQ 查询来获取晚餐将来发生的序列。 Visual Studio 提供了完整的 intellisense，当编写 LINQ 查询，并从它返回的对象都是强类型，而且还支持 intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

除了使我们能够 Dinner 和回复对象查询，NerdDinnerDataContext 还自动跟踪我们随后对我们通过它检索的 Dinner 和回复对象进行任何更改。 我们可以使用此功能来轻松地将所做的更改保存回数据库-无需编写任何显式的 SQL 更新代码。

例如，下面的代码演示如何使用 LINQ 查询以从数据库中检索单个 Dinner 对象更新两个 Dinner 属性，然后将所做的更改保存回数据库：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

在上面的代码的 NerdDinnerDataContext 对象自动跟踪对我们从它检索的 Dinner 对象的属性更改。 当我们调用"SubmitChanges()"方法时，它将执行到要保存更新后的值返回的数据库的相应 SQL"更新"语句。

### <a name="creating-a-dinnerrepository-class"></a>创建 DinnerRepository 类

对于小型应用程序是有时可以将具有控制器直接针对 LINQ to SQL DataContext 类，并将嵌入在控制器中的 LINQ 查询。 随着应用程序变得更长，不过，这种方法变得难以维护和测试。 此外可能导致我们复制多个位置中的同一个 LINQ 查询导致。

可以使应用程序更易于维护和测试的一种方法是使用"存储库"模式。 存储库类可帮助将数据查询和持久性逻辑和摘要消失封装从应用程序的数据持久性的实现详细信息。 除了让应用程序代码更简洁，使用的存储库模式会导致更轻松地在将来，更改数据存储实现，并且它可以帮助实现单元测试应用程序，而无需实际的数据库。

对于我们 NerdDinner 的应用程序中，我们将定义具有的 DinnerRepository 类以下签名：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*注意： 本章后面我们从此类提取 IDinnerRepository 接口，启用与之我们控制器上的依赖关系注入。开始时，不过，我们将启动简单和直接与 DinnerRepository 类正常工作。*

若要实现此类，我们将在我们"Models"文件夹上右键单击并选择**添加-&gt;新项**菜单命令。 在"添加新项"对话框中，我们将选择"类"模板，并将文件命名为"DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

然后，我们可以实现下面的代码使用我们 DinnerRespository 类：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>检索、 更新、 插入和删除使用 DinnerRepository 类

现在，我们已创建我们 DinnerRepository 类，让我们看一些演示我们可以使用它执行操作的常见任务的代码示例：

#### <a name="querying-examples"></a>查询示例

下面的代码检索单个 Dinner 使用 DinnerID 值：


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

下面的代码检索在其所有即将发布晚餐和循环：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>插入和更新示例

下面的代码演示如何添加两个新晚餐。 添加/修改存储库不提交到数据库，直到对它调用了"save （）"方法。 LINQ to SQL 会自动将所有更改都包装在一项数据库事务 – 因此发生了所有更改或其中任何一个执行我们的存储库保存时：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

下面的代码检索现有 Dinner 对象，并修改它的两个属性。 在我们的存储库上调用"save （）"方法时，在提交回数据库更改：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

下面的代码检索 dinner，，然后向其添加回复。 不过这将为我们创建 LINQ to SQL （因为在数据库中这两者之间没有主键/外键关系） 的 Dinner 对象上使用 RSVPs 集合。 存储库上调用"save （）"方法时，此更改为新的回复表行保存回数据库中：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>删除示例

下面的代码检索现有 Dinner 对象，然后将其标记为要删除。 存储库上调用"save （）"方法时它将提交回数据库的删除：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>将与模型类集成验证和业务规则逻辑

集成验证和业务规则逻辑是任何可使用应用程序数据的重要组成部分。

#### <a name="schema-validation"></a>架构验证

如果使用 LINQ to SQL 设计器定义模型类中的数据模型类的属性的数据类型对应于数据库表的数据类型。 例如： 如果晚餐表中的"EventDate"列是"datetime"，创建 LINQ to SQL 的数据模型类将是类型"DateTime"（这是内置的.NET 数据类型）。 这意味着如果你尝试从代码中，为其指定的整数或布尔值，并且如果你尝试隐式将非有效的字符串类型转换为它在运行时，它将自动引发错误，则将收到编译错误。

使用的字符串-这有助于帮助你抵御 SQL 注入式攻击时使用它时，LINQ to SQL 还会自动将为你处理转义 SQL 值。

#### <a name="validation-and-business-rule-logic"></a>验证和业务规则逻辑

架构验证是作为第一个步骤中，有用，但很少足够。 大多数实际情况下，需要指定更丰富的验证逻辑，可以跨多个属性、 执行代码，并且通常具有感知的模型的状态的功能 (例如： 它正在创建/更新/删除，或在某一特定于域的状态如"存档"）。 有各种不同的模式和可用来定义和将验证规则应用于模型类的框架和几个基于.NET 框架现成可用来帮助实现这一点。 你可以使用 ASP.NET MVC 应用程序中的其中几乎任何。

对于我们 NerdDinner 的应用程序的目的，我们将使用一种相对简单和直接模式其中我们会在我们 Dinner 模型对象上公开 IsValid 属性和 GetRuleViolations() 方法。 IsValid 属性将返回 true 或 false，具体取决于是否验证和业务规则均有效。 GetRuleViolations() 方法将返回任何规则错误的列表。

我们将实现 IsValid 和 GetRuleViolations() 为我们 Dinner 模型通过向我们的项目中添加"分部类"。 分部类可以用来将方法/属性/事件添加到类 （如 linq to SQL 设计器生成的 Dinner 类） 的 VS 设计器由维护，并且有助于避免混乱与我们的代码中的工具。 我们可以右键单击 \Models 文件夹中，将新的分部类添加到我们的项目，然后选择"添加新项"菜单命令。 然后，我们可以选择在"添加新项"对话框的"类"模板，并将其命名 Dinner.cs。

![](build-a-model-with-business-rule-validations/_static/image11.png)

单击"添加"按钮将将 Dinner.cs 文件添加到我们的项目，并在 IDE 中打开它。 我们然后实现基本的规则验证强制 framework 使用下面的代码：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

有关上面的代码的几个注意事项：

- Dinner 类开头为"partial"关键字 – 这意味着将与生成/维护类结合使用 linq to SQL 设计器中包含的代码，并将其编译成一个类。
- RuleViolation 类是我们将添加到项目，我们提供有关某一规则冲突的更多详细信息的帮助程序类。
- Dinner.GetRuleViolations() 方法使我们验证规则和业务规则要进行求值 （我们将实现它们很快）。 它然后返回 RuleViolation 对象，它提供有关任何规则错误的更多详细信息的序列。
- Dinner.IsValid 属性提供了一个便利的帮助器属性，指示 Dinner 对象是否具有任何活动 RuleViolations。 它可以在任何时候使用 Dinner 对象的开发人员通过主动检查 （并不会引发异常）。
- Dinner.OnValidate() 分部方法是提供 LINQ to SQL 允许我们 Dinner 对象将保留在数据库中的任何时候要通知的挂钩。 上述我们 OnValidate() 实现可确保 Dinner 具有任何 RuleViolations，则在保存前。 如果它处于无效状态，因此它会引发异常，这将导致 LINQ to SQL 中止事务。

此方法提供一个简单的框架，我们可以将验证和业务规则到集成。 现在让我们添加以下规则到我们 GetRuleViolations() 方法：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

我们将使用 C# 的"产量返回"的功能来返回任何 RuleViolations 的序列。 上面的第一个六个规则检查只是强制执行我们 Dinner 的字符串属性不能为 null 或为空。 最后一条规则很变得更加有趣，而且调用 PhoneValidator.IsValidNumber() 帮助器方法，我们可以将添加到我们的项目，以便确认 ContactPhone 数字格式匹配 Dinner 国家/地区。

我们可以使用。若要实现此电话验证支持的 NET 的正则表达式支持。 下面是一个简单的 PhoneValidator 实现，我们可以添加到我们使我们能够添加特定于国家/地区的正则表达式模式检查的项目：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>处理验证和业务逻辑冲突

现在，我们已添加上述验证和业务规则代码中，我们尝试创建或更新 Dinner 任何时间，将评估和强制执行我们的验证逻辑规则。

开发人员可以编写代码类似下面主动确定 Dinner 对象是否有效，并检索中它的所有冲突，而不会引发任何异常的列表：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

如果我们尝试保存 Dinner 处于无效状态，当我们调用 DinnerRepository save （） 方法时将引发异常。 这是因为它将保存 Dinner 的更改，然后我们将代码添加到 Dinner.OnValidate() 以引发异常时如果 Dinner 中不存在任何规则冲突后，LINQ to SQL 将自动调用我们 Dinner.OnValidate() 分部方法。 我们可以捕获此异常，并可以被动检索冲突后，若要解决问题的列表：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

因为我们验证规则和业务规则并实现在我们的模型层，以及不中的 UI 层，它们将应用并在我们的应用程序内的所有方案中都使用。 我们可以稍后更改或添加业务规则并具有适用于我们 Dinner 对象遵循它们的所有代码。

具有灵活地更改业务规则在一个位置，而无 ripple 在整个应用程序和用户界面逻辑，这些更改是正确编写的应用程序，和的 MVC 框架可帮助鼓励权益的符号。

### <a name="next-step"></a>下一步

我们现在我们可以使用查询和更新我们的数据库的模型。

让我们现在将添加一些控制器和视图到我们可以使用生成在其周围的 HTML UI 体验的项目。

>[!div class="step-by-step"]
[上一页](create-a-database.md)
[下一页](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
