---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: "启用自动单元测试 |Microsoft 文档"
author: microsoft
description: "步骤 12 演示如何开发一套自动的单元测试用于验证 NerdDinner 功能，而且这将向我们提供置信度来更改..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 1a4258054d90b2d5bcc06a63fb6f3b4673a4837d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="enable-automated-unit-testing"></a>启用自动的单元测试
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是一个免费的步骤 12 ["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 步骤 12 演示如何开发一套自动的单元测试用于验证 NerdDinner 功能，而且这将向我们提供置信度进行更改和对应用程序在将来的改进。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner 步骤 12： 单元测试

让我们开发一套自动的单元测试用于验证 NerdDinner 功能，而且这将向我们提供置信度进行更改和对应用程序在将来的改进。

### <a name="why-unit-test"></a>为什么单元测试？

在将驱动器分割为工作一个早上，有有关应用程序正在处理的灵感突然的 flash。 你实现将使该应用程序显著更好的更改可以实现。 它可能是重构清理代码、 添加一个新的功能，或修复 bug。

当你亲自前往你的计算机 confronts 你的问题是 –"如何安全是以使这一改进？" 如果在更改还具有负面影响或中断的内容？ 更改可能非常简单，只需几分钟才能实现，但如果需要手动测试的应用程序方案的所有小时？ 如果你忘记了以涵盖方案和损坏的应用程序投入生产？ 正在此改善真的值得所有工作量？

自动的单元测试可以提供一个安全网络，使您能够不断改进你的应用程序，并避免正在担心您正在使用的代码。 具有自动快速验证功能使你能够置信度 – 代码，并使你能够进行改进，你可能否则不具有认为合适的测试执行。 它们还可帮助创建更易于维护的解决方案，并让较长的生存期的哪些导致较高的投资回报。

ASP.NET MVC 框架，可以简单而自然到单元测试应用程序功能。 它还使，可以测试第一个基于的开发的测试驱动开发 (TDD) 工作流。

### <a name="nerddinnertests-project"></a>NerdDinner.Tests 项目

当我们在本教程开头创建我们 NerdDinner 的应用程序时，我们系统已提示用户与一个对话框，询问是否我们想要创建要会随之提供应用程序项目的单元测试项目：

![](enable-automated-unit-testing/_static/image1.png)

我们保持"是，创建单元测试项目"单选按钮处于选中状态 – 这将导致"NerdDinner.Tests"项目添加到我们的解决方案中：

![](enable-automated-unit-testing/_static/image2.png)

NerdDinner.Tests 项目引用 NerdDinner 应用程序中的项目程序集，并使我们能够轻松地将自动化的测试添加到该验证的应用程序功能。

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>为我们 Dinner 模型类创建单元测试

让我们在验证我们创建我们生成我们模型层时的 Dinner 类我们 NerdDinner.Tests 项目中添加一些测试。

我们将开始创建新文件夹，我们称为"模型"的测试项目中我们将在此处我们模型相关的测试。 我们将然后右键单击文件夹并选择**添加-&gt;新测试**菜单命令。 这将显示"添加新的测试"对话框。

我们将选择要创建"单元测试"并将其命名为"DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

当我们单击"确定"按钮时 Visual Studio 将添加 （和打开） DinnerTest.cs 文件到项目：

![](enable-automated-unit-testing/_static/image4.png)

默认的 Visual Studio 单元测试模板还包含一组在其中找到有点混乱的样板代码。 让我们清理它以仅包含下面的代码：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

上面的 DinnerTest 类上的 [TestClass] 属性将其识别为测试，以及可选的测试初始化和拆卸代码将包含的类。 我们可以通过添加在其具有 [TestMethod] 属性的公共方法定义中的测试。

下面是我们将添加的两个演练我们 Dinner 类测试的第一个。 第一个测试验证我们 Dinner 无效，是否新 Dinner 创建没有正确设置的所有属性。 第二个测试来验证我们 Dinner 有效时 Dinner 已使用有效的值进行设置的所有属性：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

你会注意到上面我们测试名称是很明确 （和某种程度上详细）。 因为我们可能会得到创建数百或数千个小规模测试中，并且我们想要方便地快速确定的意向和每个行为 （尤其是在我们正在通过测试运行程序中的失败的列表），我们就这样做。 测试名称应以命名他们正在测试的功能。 更高版本，我们将使用"名词\_应\_谓词"的命名模式。

我们构建测试使用测试模式 –"排列、 Act、 Assert"代表"AAA":

- 排列： 设置要测试的单元
- Act： 执行测试的单元，并捕获结果
- 断言： 验证行为

在我们编写我们想要避免的各个测试的测试执行过多。 而是每个测试应该验证仅在一个概念 （这将使可以更轻松地找出失败的原因）。 比较好的原则是尝试并只有一个断言语句为每个测试。 如果你有多个断言测试方法中的语句，请确保它们所有用于测试相同的概念。 如有疑问，请另一个测试。

### <a name="running-tests"></a>正在运行测试

Visual Studio 2008 专业版 （和更高版本） 包括可以用于运行 Visual Studio 单元测试项目在 IDE 中的内置测试运行程序。 我们可以选择**测试-&gt;运行-&gt;解决方案中的所有测试**菜单命令 （或类型 Ctrl R、 A） 若要运行所有我们的单元测试。 或者我们可以在特定的测试类或测试方法中我们光标的位置，并使用**测试-&gt;运行-&gt;当前上下文中的测试**菜单命令 （或类型 Ctrl R、 T） 运行的单元测试的子集。

让我们 DinnerTest 类中的我们光标的位置，请运行这两个测试我们刚才定义的键入"Ctrl R、 T"。 当我们执行此操作的"测试结果"窗口将显示在 Visual Studio 内，我们将看到我们测试运行中列出的结果：

![](enable-automated-unit-testing/_static/image5.png)

*注意: VS 测试结果窗口不默认情况下显示的类名称列。可以通过在测试结果窗口内右键单击并使用添加/删除列菜单命令来添加这种情况。*

我们的两个测试中花费了仅运行，并作为你可以一秒的小数部分它们都传递，请参阅。 我们现在可以继续，通过创建其他的测试，验证特定规则验证，以及涉及两个 helper 方法的 IsUserHost() 和 IsUserRegisterd() – 我们添加到 Dinner 类对其进行扩大。 所有这些测试时采用的 Dinner 类准备好将使其更轻松、 更安全，以在将来向其中添加新的业务规则和验证。 我们可以将我们新的规则逻辑添加到 Dinner，，然后在秒内验证它未中断任何我们以前的逻辑功能。

请注意如何使用描述性的测试名称可以轻松地快速了解每个测试验证。 建议使用**Tools-&gt;选项**菜单命令，打开的测试工具-&gt;测试执行配置屏幕中，并检查"双击已失败的或无结论单元测试结果显示在测试中的故障点"复选框。 这样，你可以双击测试结果窗口中的失败，直接跳到断言失败。

### <a name="creating-dinnerscontroller-unit-tests"></a>创建 DinnersController 单元测试

让我们现在创建一些单元测试，以确认我们 DinnersController 功能。 我们将开始通过右键单击在我们的测试项目中的"控制器"文件夹，然后选择**添加-&gt;新测试**菜单命令。 我们将创建一个"单元测试"并将其命名为"DinnersControllerTest.cs"。

我们将创建两个验证 DinnersController Details() 操作方法的测试方法。 第一个将验证现有 Dinner 请求时，返回一个视图。 第二个将验证不存在 Dinner 请求时，返回"NotFound"视图：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

上面的代码将编译成干净。 当我们运行测试时，不过，它们都失败：

![](enable-automated-unit-testing/_static/image6.png)

如果我们看一下的错误消息，我们将看到测试失败的原因是因为我们 DinnersRepository 类无法连接到数据库。 我们 NerdDinner 应用程序到一个 SQL Server Express 的本地文件位于 \App 下使用连接字符串\_NerdDinner 应用程序项目的数据目录。 因为我们 NerdDinner.Tests 项目编译和运行在不同的目录然后应用程序项目，我们连接字符串的相对路径位置不正确。

我们*无法*解决此问题通过将 SQL Express 数据库文件复制到我们的测试项目，然后向其中添加相应的测试连接字符串，我们的测试项目的 App.config 中。 此参数将被解除阻止，并运行上述测试。

单元测试代码中使用实际的数据库，不过，会带来了许多挑战。 尤其是在下列情况下：

- 这显著会减慢执行时间的单元测试。 它所花费的时间来运行测试，要经常执行它们可能性就越小。 理想情况下你希望你能够在秒数 – 中运行，并让它是你所做的事情作为自然作为编译该项目的单元测试。
- 它将增加复杂性中测试的设置和清理逻辑。 你想要隔离，并独立于其他 （没有负面影响或依赖项） 的每个单元测试。 实际的数据库上工作时需要注意的状态并重置测试之间。

让我们看一下调用"依赖关系注入"，可帮助我们解决这些问题并避免需要与我们的测试中使用实际的数据库的设计模式。

### <a name="dependency-injection"></a>依赖关系注入

稍后再试 DinnersController 是"与紧密耦合"DinnerRepository 类。 "耦合"引用的情况下，其中一个类显式依赖于另一个类才能工作：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

由于 DinnerRepository 类需要对数据库的访问，紧密耦合的依赖项 DinnersController 类具有 DinnerRepository 最后剩下要求我们要测试的 DinnersController 操作方法的顺序中有一个数据库上。

我们可以通过采用一种设计模式，称为"依赖关系注入"– 这是一种方法 （如提供数据访问的存储库类） 的依赖关系中使用它们的类的不再隐式创建位置获取解决此。 相反，依赖项可以显式传递给使用它们的类使用构造函数自变量。 如果使用接口定义依赖关系，必须能够灵活地在单元测试方案的"假"依赖项实现中传递。 这使我们能够创建实际上不需要对数据库的访问的特定测试的依赖项实现。

若要在操作中看到此内容，让我们实现与我们 DinnersController 的依赖关系注入。

#### <a name="extracting-an-idinnerrepository-interface"></a>提取 IDinnerRepository 接口

我们第一步将创建一个新的 IDinnerRepository 接口，封装我们控制器要求以检索和更新晚餐的存储库协定。

我们可以手动定义此接口协定上的 \Models 文件夹中，右键单击，然后选择**添加-&gt;新项**菜单命令，并创建名为 IDinnerRepository.cs 的新接口。

或者我们可以使用到中重构工具内置于 Visual Studio 专业版 （和更高版本） 将自动提取，并为我们创建一个接口，我们现有 DinnerRepository 类中。 若要提取使用 VS 此接口，只需将光标置于 DinnerRepository 类中，在文本编辑器，然后右键单击并选择**重构-&gt;提取接口**菜单命令：

![](enable-automated-unit-testing/_static/image7.png)

这将启动"提取接口"对话框并提示我们要创建的接口名称。 它将默认为 IDinnerRepository，自动选择现有的 DinnerRepository 类，将添加到接口上的所有公共方法：

![](enable-automated-unit-testing/_static/image8.png)

当我们单击"确定"按钮时，Visual Studio 将添加到我们的应用程序的新 IDinnerRepository 接口：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

并将更新我们现有 DinnerRepository 类，以便它实现该接口：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>更新 DinnersController 以支持构造函数注入

现在，我们将更新要使用的新接口的 DinnersController 类。

当前 DinnersController 是硬编码，这样，其"dinnerRepository"字段始终 DinnerRepository 类：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

我们将更改它，以便"dinnerRepository"字段的类型而不是 DinnerRepository IDinnerRepository 是。 然后，我们将添加两个公共 DinnersController 构造函数。 允许 IDinnerRepository 作为自变量传递的构造函数之一。 另一种是使用我们现有 DinnerRepository 实现的默认构造函数：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

默认情况下的 ASP.NET MVC 创建控制器类使用默认的构造函数，因为我们 DinnersController 在运行时将继续使用 DinnerRepository 类来执行数据访问。

我们现在可以更新我们单元测试，不过，在"假"dinner 存储库实现中使用的参数构造函数传递。 此"假"dinner 存储库不需要访问的实际数据库，并改为将使用内存中示例数据。

#### <a name="creating-the-fakedinnerrepository-class"></a>创建 FakeDinnerRepository 类

让我们创建 FakeDinnerRepository 类。

我们首先创建我们 NerdDinner.Tests 项目中的"Fakes"目录，然后向其添加新的 FakeDinnerRepository 类 (右键单击文件夹，然后选择**添加-&gt;新类**):

![](enable-automated-unit-testing/_static/image9.png)

我们将更新代码，以便 FakeDinnerRepository 类实现 IDinnerRepository 接口。 然后，我们可以右键单击它并选择"实现接口 IDinnerRepository"上下文菜单命令：

![](enable-automated-unit-testing/_static/image10.png)

这将导致 Visual Studio 会自动将所有 IDinnerRepository 接口成员添加到默认"派生出"实现我们 FakeDinnerRepository 类：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

然后，我们可以更新 FakeDinnerRepository 实现，若要从内存中列表中移出&lt;Dinner&gt;集合作为构造函数自变量传递给它：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

我们现在有一个假 IDinnerRepository 实现，不需要数据库，并且可以处理关闭 Dinner 对象的内存中列表。

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>使用 FakeDinnerRepository 与单元测试

让我们回到前面失败，因为数据库不可用的 DinnersController 单元测试。 我们可以更新测试方法，用于填充示例内存中 Dinner 数据到使用下面的代码 DinnersController FakeDinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

现在我们运行这些测试时它们都通过：

![](enable-automated-unit-testing/_static/image11.png)

最重要的是，它们采用只有一小部分第二个若要运行，并不需要的任何复杂的安装程序清理逻辑。 我们可以现在不需要连接到真实的数据库的单元测试所有我们 DinnersController 操作方法的代码 （包括列表中，分页，详细信息，创建、 更新和删除）。

| **端主题内容： 依赖关系注入框架** |
| --- |
| 执行手动依赖关系注入 （例如，我们将位于之上） 正常工作，但实际上已成为哪一维护作为依赖关系的数量，并提高应用程序中的组件。 For.NET，可以帮助提供更多的依赖项管理灵活性存在多个依赖关系注入框架。 这些框架，有时也称为"控制反向"(IoC) 容器提供启用的额外级别的配置对指定并将依赖项传递给在运行时 （最常使用构造函数注入的对象的支持的机制). 一些更受欢迎的 OSS 依赖关系注入 / IOC 框架在.NET 中的包括： AutoFac、 Ninject、 Spring.NET、 StructureMap，和 Windsor。 ASP.NET MVC 公开扩展性 Api 支持开发人员参与解析和的控制器，实例化并这样依赖关系注入 / IoC 框架，以在此过程内完全集成。 使用 DI/IOC 框架还将使我们能够从我们 DinnersController – 将完全删除它和 DinnerRepositorys 之间的耦合删除默认构造函数。 我们将不使用依赖关系注入 / IOC framework 与我们 NerdDinner 应用程序。 但是，这是一个我们可以考虑在将来，如果 NerdDinner 基本代码和功能是增长。 |

### <a name="creating-edit-action-unit-tests"></a>创建编辑操作单元测试

让我们现在创建一些验证 DinnersController 的编辑功能的单元测试。 我们首先测试我们的编辑操作的 HTTP GET 版本：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

我们将创建测试来验证请求有效 dinner 时重新呈现 DinnerFormViewModel 对象作为后盾的视图：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

当我们运行测试时，不过，我们将找到失败，因为编辑方法访问 User.Identity.Name 属性执行 Dinner.IsHostedBy() 检查时，会引发一个空引用异常。

控制器基类上的用户对象封装有关登录的用户的详细信息，并在运行时创建控制器时，由 ASP.NET MVC 填充。 因为我们要测试 web 服务器环境外部 DinnersController，未设置用户对象 （因此 null 引用异常）。

### <a name="mocking-the-useridentityname-property"></a>模拟 User.Identity.Name 属性

模拟框架简化测试过程通过使我们能够动态创建支持我们的测试中的依赖对象的假版本。 例如，我们可以在我们的编辑操作测试使用模拟框架，动态创建我们 DinnersController 可用于查找模拟的用户名的用户对象。 这将避免当我们运行我们的测试引发空引用。

有许多.NET 模拟框架可与 ASP.NET MVC 使用 (你可以看到此处这些设置的列表： [http://www.mockframeworks.com/](http://www.mockframeworks.com/))。 用于测试，我们将使用一种开放源模拟称为"Moq"framework 我们 NerdDinner 应用程序，其中可免费从下载[http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq)。

下载完成后，我们将对 Moq.dll 程序集 NerdDinner.Tests 项目中添加的引用：

![](enable-automated-unit-testing/_static/image12.png)

然后，我们将添加一个"CreateDinnersControllerAs(username)"帮助器方法到我们的测试类，它将作为一个参数，和的用户名，然后"mocks"DinnersController 实例上的 User.Identity.Name 属性：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

更高版本，我们将使用 Moq 来创建 fakes 的 ControllerContext 对象 （这是什么 ASP.NET MVC 将传递到控制器类来公开运行时对象，如用户、 请求、 响应和会话） 的模型对象。 我们在 Mock 指示 ControllerContext HttpContext.User.Identity.Name 属性应返回我们传递给帮助器方法的用户名字符串上调用"SetupGet"方法。

我们可以模拟任意数量的 ControllerContext 属性和方法。 为了说明这一点我还添加了 Request.IsAuthenticated 属性 （这不实际所需的下面 – 的测试，但这将帮助说明如何可以模拟请求属性） 的 SetupGet() 调用。 我们完成时我们将分配到我们的帮助程序方法中返回 DinnersController ControllerContext 模型的实例。

现在，我们可以编写使用此帮助器方法来测试编辑方案涉及不同用户的单元测试：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

现在我们运行测试时它们通过：

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>测试 UpdateModel() 方案

我们创建了测试以涵盖的编辑操作的 HTTP GET 版本。 让我们现在创建一些测试的验证的编辑操作的 HTTP POST 版本：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

有趣的新测试方案，为我们以支持与此操作方法是控制器基类上的 UpdateModel() 帮助器方法的用法。 我们将使用此帮助器方法来将窗体发布请求值绑定到我们 Dinner 对象实例。

下面是演示如何我们才能够提供窗体发送的要使用的 UpdateModel() 帮助器方法的值的两个测试。 我们执行此操作通过创建和填充 FormCollection 对象，然后将其分配给在控制器上的"ValueProvider"属性。

第一个测试验证，成功保存将浏览器重定向到详细信息的操作。 第二个测试将验证发送无效的输入时操作显示编辑视图再次并显示错误消息。


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>测试总结

我们已介绍所涉及的单元测试控制器类的核心概念。 我们可以使用这些技术可以轻松地创建数百个简单验证我们的应用程序的行为的测试。

由于我们控制器和模型测试不需要实际数据库，它们是极快速且轻松地运行。 我们将能够以秒为单位，执行数百个自动测试并立即获取有关我们所做的更改是否中断的内容的反馈。 这将帮助向我们提供置信度来持续改进，重构，并改进我们的应用程序。

我们介绍测试作为最后一个主题本章 – 但不是测试是因为你应在开发过程的一端执行此 ！ 相反，应在开发过程中尽早编写自动的测试。 这样做因此，您可以获得的即时反馈开发时，可帮助你仔细考虑你的应用程序使用的情况下，并将指导你设计应用程序，干净分层和记住耦合。

书中的更高版本章将讨论测试驱动开发 (TDD) 以及如何使用 ASP.NET MVC。 TDD 是首先在其中编写生成代码将满足的测试迭代的编码做法。 与 TDD 首先，每个功能创建的测试来验证将要实现的功能。 编写单元测试第一次可帮助确保您清楚地了解该功能及其如何它应向其工作。 仅编写测试 （并已验证，它将失败后） 然后实现验证测试的实际功能。 因为你已花费时间来考虑有关的功能是怎样工作的用例，你将具有更好地了解要求以及如何最好地实现它们。 完成此操作与实现可以重新运行的测试中 – 并获取与即时的反馈是否功能能够正常运行。 我们将介绍第 10 章中更 TDD。

### <a name="next-step"></a>下一步

向上注释某些最终换行。

>[!div class="step-by-step"]
[上一页](use-ajax-to-implement-mapping-scenarios.md)
[下一页](nerddinner-wrap-up.md)
