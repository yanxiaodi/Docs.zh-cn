---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: "迭代 #5 – 创建单元测试 (VB) |Microsoft 文档"
author: microsoft
description: "在第五个迭代中，我们使我们的应用程序更轻松地监视和修改通过添加单元测试。 我们模拟我们数据模型类，并生成单元测试 o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: ab9ff5629cb468b785f5b82178f9f6247a55cacb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="iteration-5--create-unit-tests-vb"></a>迭代 #5 – 创建单元测试 (VB)
====================
通过[Microsoft](https://github.com/microsoft)

[下载代码](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> 在第五个迭代中，我们使我们的应用程序更轻松地监视和修改通过添加单元测试。 我们模拟我们数据模型类，并生成单元测试控制器和验证逻辑。


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>生成联系人管理 ASP.NET MVC 应用程序 (VB)

在这一系列的教程，我们生成整个联系人管理应用程序从头到尾完成。 联系人管理器应用程序，可存储的名称，电话号码和电子邮件地址的联系人信息有关的人员列表。

我们在多次迭代中生成应用程序。 每次迭代时，我们逐渐提高应用程序。 此多个迭代方法旨在使您能够了解每个更改的原因。

- 迭代 #1-创建应用程序。 在第一次迭代中，我们创建联系人管理器中的最简单方法可能。 我们将添加对基本数据库操作的支持： 创建、 读取、 更新和删除 (CRUD)。

- 迭代 #2-使应用程序，看上去很好。 在此迭代中，我们通过修改默认 ASP.NET MVC 视图母版页和级联样式表改进应用程序的外观。

- 迭代 #3-添加窗体验证。 在第三个迭代中，我们将添加基本窗体验证。 我们可以防止人员提交窗体，但不完成需要的表单域。 我们还验证电子邮件地址和电话号码。

- 迭代 #4-请松散耦合的应用程序。 在此第三个迭代中，我们利用多个软件设计模式以使其更轻松地监视和修改联系人管理器应用程序。 例如，我们将重构应用程序以使用存储库模式和依赖关系注入模式。

- 迭代 #5-创建单元测试。 在第五个迭代中，我们使我们的应用程序更轻松地监视和修改通过添加单元测试。 我们模拟我们数据模型类，并生成单元测试控制器和验证逻辑。

- 迭代 #6-使用测试驱动开发。 在此第六个迭代中，我们将添加新功能到我们的应用程序通过首先编写单元测试和针对单元测试编写代码。 在此迭代中，我们添加联系人的组。

- 迭代 #7-添加 Ajax 功能。 在第七个迭代中，我们通过添加对 Ajax 的支持提高响应能力和我们的应用程序的性能。


## <a name="this-iteration"></a>此迭代

在联系人管理器应用程序前一次迭代，我们将重构要更为松散耦合的应用程序。 我们分隔到不同的控制器、 服务和存储库层应用程序。 与通过接口它下面的层交互，每个图层。

我们将重构应用程序，以便更轻松地监视和修改应用程序。 例如，如果我们需要使用新的数据访问技术，我们只需可以直接更改存储库层，而无需触摸控制器或服务层。 通过进行松散耦合，Contact Manager 我们进行了应用程序更具弹性，若要更改。

但是，当我们需要将一项新功能添加到联系人管理器应用程序时，会发生什么情况？ 或者，当我们修复 bug 时，会发生什么情况？ 编写代码的很遗憾，但也经过验证，事实是，每当你触摸创建引入新 bug 的风险的代码。

例如，一个好的一天，你的经理可能会要求你添加到联系人管理器中的一项新功能。 她希望添加联系人组的支持。 她希望你以使用户能够将他们的联系人组织到组，例如友元，业务，依次类推。

为了实现这一新功能，你将需要修改的联系人管理器应用程序的所有三个层。 你将需要将新功能添加到服务层，并存储库中的所有控制器。 只要你开始修改代码，则可能中断之前处理的功能。

重构到单独的层，我们的应用程序，正如我们在前一次迭代，做了一件好事。 它是一件好事，因为它使我们能够对整个层进行更改，而无需触摸应用程序的其余部分。 但是，如果你想要轻松地监视和修改在层内的代码，你需要创建代码的单元测试。

你使用单元测试来测试代码的单个单元。 代码的这些单位为小于整个应用程序层。 通常，你可以使用单元测试以验证你的代码中的特定方法的行为是否你预期的方式。 例如，你将创建由 ContactManagerService 类公开的 CreateContact() 方法的单元测试。

只是应用程序工作的单元测试，如网络安全。 每当你修改应用程序中的代码，你可以运行单元测试以检查是否修改将断开现有功能的一组。 单元测试，你的代码可以更安全地修改。 单元测试进行的所有代码中你的应用程序更具弹性，若要更改。

在此迭代中，我们将单元测试添加到我们的联系人管理器应用程序。 这样一来，在下一个迭代中，我们可以添加联系人组到我们的应用程序而无需担心破坏现有功能。

> [!NOTE] 
> 
> 有大量的单元测试框架，包括 NUnit、 xUnit.net 和 MbUnit。 在本教程中，我们使用的单元测试框架随 Visual Studio。 但是，你可以轻松地使用这些替代框架之一。


## <a name="what-gets-tested"></a>什么获取测试

在理想情况下，所有代码将被覆盖的单元测试。 在理想情况下，你将必须完美的网络安全。 你将能够修改任意应用程序中的代码行，并且知道此时，通过执行单元测试，更改是否中断现有功能。

但是，我们并不理想的环境中的实时 t。 在实践中，编写单元测试时，你专注于编写你的业务逻辑 （例如，验证逻辑） 的测试。 具体而言，你*不这样做*编写单元测试为你的数据访问逻辑或你的视图逻辑。

若要有一定用处，单元测试必须执行得非常快。 轻松，你可以累积数百 （或甚至数千） 的应用程序的单元测试。 如果单元测试需要很长时间才能运行你可以避免执行它们。 换而言之，长时间运行的单元测试是对于每天编码用途没有用处。

为此，你通常不要编写的数据库交互的代码的单元测试。 实时数据库运行数百个单元测试将太慢。 相反，模拟你的数据库，并编写与模拟 （我们讨论模拟以下数据库） 的数据库交互的代码。

类似的原因，你通常不要编写视图的单元测试。 若要测试的视图，你必须启动 web 服务器。 由于运转的 web 服务器是一个相对较慢的过程，不建议为你的视图创建单元测试。

如果你的视图包含复杂的逻辑则应考虑将逻辑移到帮助器方法。 你可以编写执行而不在运转的 web 服务器的帮助器方法的单元测试。

> [!NOTE] 
> 
> 虽然编写测试数据访问逻辑或视图逻辑不是一个不错的主意编写单元测试时，这些测试可在构建功能或集成测试时非常有价值。


> [!NOTE] 
> 
> ASP.NET MVC 是 Web 窗体视图引擎。 Web 窗体视图引擎依赖于 web 服务器时，可能不是其他视图引擎。


## <a name="using-a-mock-object-framework"></a>使用模拟对象框架

在生成单元测试时，你几乎总是需要利用模拟对象框架。 模拟对象框架，可在你的应用程序中创建 mock 和类的存根。

例如，可以使用模拟对象框架来生成你的存储库类的模型版本。 这样一来，你可以使用模拟的存储库类而不是实际的存储库类在单元测试。 使用模拟的存储库，可避免执行单元测试时执行数据库的代码。

Visual Studio 不包括一个模拟对象框架。 但是，有可用于.NET framework 的多个商业和开放源代码模拟对象框架：

1. Moq-此框架，请参阅的开放源代码 BSD 许可。 你可以下载从 Moq [https://code.google.com/p/moq/](https://code.google.com/p/moq/)。
2. Rhino Mocks-此框架是开放源代码 BSD 许可下可用。 你可以下载 Rhino Mocks 从[http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx)。
3. Typemock 隔离器-这是一个商业框架。 你可以下载从试用版[http://www.typemock.com/](http://www.typemock.com/)。

在本教程中，我决定使用 Moq。 但是，你可以轻松地使用 Rhino Mocks Typemock 隔离器创建模型的对象或联系人管理器应用程序。

你可以使用 Moq 之前，你需要完成以下步骤：

1. .
2. 解压缩下载之前，请确保你右键单击该文件，然后单击标记按钮**解除阻止**（请参见图 1）。
3. 解压缩下载。
4. 将 Moq 程序集的引用添加到你的测试项目中，通过选择菜单选项**项目中，添加引用**以打开**添加引用**对话框。 在浏览选项卡，浏览到您在其中解压缩 Moq 的文件夹并选择 Moq.dll 程序集。 单击**确定**按钮 （请参见图 2）。


[![取消阻塞 Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**图 01**： 取消阻塞 Moq ([单击以查看实际尺寸的图像](iteration-5-create-unit-tests-vb/_static/image2.png))


[![在添加 Moq 后的引用](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**图 02**： 添加 Moq 后的引用 ([单击以查看实际尺寸的图像](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>为服务层创建单元测试

让我们来首先创建一组我们联系人管理器应用程序 s 服务层的单元测试。 我们将使用这些测试来验证我们验证逻辑。

创建 ContactManager.Tests 项目中名为 Models 的新文件夹。 接下来，右键单击 Models 文件夹，然后选择**添加、 新的测试**。 **添加新测试**此时将显示图 3 所示的对话框。 选择**单元测试**模板并命名为新的测试 ContactManagerServiceTest.vb。 单击**确定**按钮以向测试项目中添加新的测试。

> [!NOTE] 
> 
> 一般情况下，你希望测试项目中以匹配你的 ASP.NET MVC 项目的文件夹结构的文件夹结构。 例如，你将控制器测试放在控制器文件夹中，模型测试中的模型文件夹中，依次类推。


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**图 03**: Models\ContactManagerServiceTest.cs ([单击以查看实际尺寸的图像](iteration-5-create-unit-tests-vb/_static/image6.png))


最初，我们想要测试由 ContactManagerService 类公开的 CreateContact() 方法。 我们将创建下列五项测试：

- CreateContact()-向方法传递有效联系人时，测试该 CreateContact() 返回值 true。
- CreateContactRequiredFirstName() 的测试，将一条错误消息添加到模型状态时与缺少的第一个名称传递给 CreateContact() 方法。
- CreateContactRequredLastName() 的测试，将一条错误消息添加到模型时缺少姓氏的联系人的状态被传递给 CreateContact() 方法。
- CreateContactInvalidPhone() 的测试，将一条错误消息添加到模型状态时具有电话号码无效的协定传递给 CreateContact() 方法。
- CreateContactInvalidEmail() 的测试，将一条错误消息添加到模型时使用无效的电子邮件地址的联系人的状态被传递给 CreateContact() 方法...

第一个测试验证有效联系人不会生成验证错误。 其余测试，检查每个验证规则。

列表 1 中包含这些测试的代码。

**列表 1-Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


由于我们在列出 1 中使用联系人类，因此我们需要我们的测试项目添加到 Microsoft 的实体框架的引用。 添加对 System.Data.Entity 程序集的引用。


列表 1 包含一个名为使用 [TestInitialize] 特性修饰的 initialize （） 方法。 每个单元测试运行之前自动调用此方法 （它每个单元测试前称为 5 次）。 Initialize （） 方法创建具有以下代码行的模拟的存储库：

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

此代码行使用 Moq 框架从 IContactManagerRepository 接口生成的模拟的存储库。 模拟的存储库而不是实际 EntityContactManagerRepository 用于避免每个单元测试运行时访问数据库。 模拟的存储库实现的方法 IContactManagerRepository 接口，但方法 don t 实际执行任何操作。

> [!NOTE] 
> 
> 当使用 Moq 框架，没有区分\_mockRepository 和\_mockRepository.Object。 前者是指包含方法来指定模拟的存储库的行为方式的模型 (的 IContactManagerRepository) 类。 后者是指实现 IContactManagerRepository 接口的实际模拟存储库。


创建 ContactManagerService 类的实例时，将在 initialize （） 方法中使用模拟的存储库。 单个单元测试的所有使用 ContactManagerService 类的此实例。

列表 1 包含五个方法对应于每个单元测试。 上述每种方法是使用 [TestMethod] 特性修饰。 运行单元测试时，调用任何具有此属性的方法。 换而言之，任何使用 [TestMethod] 特性修饰的方法是单元测试。

名为 CreateContact()，第一个单元测试验证，请与类的有效实例传递给方法时，调用 CreateContact() 返回值 true。 测试创建联系人类的实例、 调用 CreateContact() 方法，并验证 CreateContact() 返回值 true。

剩余的测试验证当 CreateContact() 方法被调用无效的联系人时然后该方法返回 false 并预期的验证错误消息添加到模型状态。 例如，CreateContactRequiredFirstName() 测试使用空字符串作为其 FirstName 属性创建联系人类的实例。 接下来，CreateContact() 方法已调用无效的联系人。 最后，此项测试确认 CreateContact() 返回 false，模型状态包含预期的验证错误消息"名字是必需的。"

你可以列出 1 中运行单元测试，通过选择菜单选项**测试，运行，解决方案 （CTRL + R、 A） 中的所有测试**。 测试的结果将显示在测试结果窗口 （请参见图 4）。


[![测试结果](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**图 04**： 测试结果 ([单击以查看实际尺寸的图像](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>为控制器创建单元测试

ASP.NET MVC 应用程序控制的用户交互的流。 当测试控制器时，你想要测试是否则控制器将返回正确的操作结果和查看数据。 你还可能想要测试是否与预期的方式的模型类的控制器交互。

例如，清单 2 包含联系人控制器 create （） 方法的两个单元测试。 第一个单元测试验证有效联系人传递给 create （） 方法，则 create （） 方法将重定向到的索引操作时。 换而言之，当传递有效的联系人，create （） 方法应返回表示索引操作 RedirectToRouteResult。

我们不想以测试我们正在测试的控制器层 ContactManager 服务层。 因此，我们模拟替换为以下代码的 Initialize 方法中的服务层：

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

在 CreateValidContact() 单元测试中，我们可以模拟调用服务层具有以下代码行的 CreateContact() 方法的行为：

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

以下代码行将会导致模拟的 ContactManager 服务，以调用其 CreateContact() 方法时返回值 true。 通过模拟的服务层，我们可以测试控制器的行为而无需在服务层中执行的任何代码。

第二个单元测试验证向方法传递无效的联系人时，create （） 操作，返回创建视图。 我们会导致服务层 CreateContact() 方法来返回值 false 与以下代码行：

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

如果 create （） 方法的行为与我们的期望则应在服务层，则返回值 false 时返回创建视图。 这样一来，控制器可以显示验证错误消息，在创建视图中，用户有可能更正该了无效的联系人属性。


如果你打算生成你的控制器的单元测试然后你需要的控制器操作返回显式的视图名称。 例如，不返回如下的视图：

返回 View()

相反，返回如下视图：

返回 View("Create")

如果不能显式返回视图时 ViewResult.ViewName 属性将返回空字符串。


**列出 2-Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>摘要

在此迭代中，我们为我们的联系人管理器应用程序创建单元测试。 我们可以在任何时间，以验证，我们的应用程序仍行为与我们的预期方式运行这些单元测试。 单元测试充当一个安全网络，使应用程序，使我们能够安全地修改在将来，我们的应用程序。

我们将创建两个集的单元测试。 首先，为我们的服务层创建单元测试测试我们的验证逻辑。 接下来，我们测试我们的流控制逻辑为我们控制器层创建单元测试。 当测试我们的服务层，我们隔离的我们的测试我们的服务层从我们的存储库层通过模拟我们存储库层。 当测试控制器层，我们隔离的我们的测试为我们控制器层通过模拟的服务层。

在下一步的迭代中，我们将修改联系人管理器应用程序，以便它支持联系人组。 我们将此新功能添加到我们的应用程序使用称为测试驱动开发的软件设计过程。

>[!div class="step-by-step"]
[上一页](iteration-4-make-the-application-loosely-coupled-vb.md)
[下一页](iteration-6-use-test-driven-development-vb.md)
