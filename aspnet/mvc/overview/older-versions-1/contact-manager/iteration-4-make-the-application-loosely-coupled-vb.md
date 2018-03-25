---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: "迭代 #4 – 使松耦合的应用程序 (VB) |Microsoft 文档"
author: microsoft
description: "在此第三个迭代中，我们利用多个软件设计模式以使其更轻松地监视和修改联系人管理器应用程序。 预测..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c11c89710723c133a306aaf56cc8797cc036475
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="iteration-4--make-the-application-loosely-coupled-vb"></a>迭代 #4 – 使松耦合的应用程序 (VB)
====================
通过[Microsoft](https://github.com/microsoft)

[下载代码](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> 在此第三个迭代中，我们利用多个软件设计模式以使其更轻松地监视和修改联系人管理器应用程序。 例如，我们将重构应用程序以使用存储库模式和依赖关系注入模式。


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

此第四个迭代中的联系人管理器应用程序，我们将重构要使应用程序更为松散耦合的应用程序。 时松散耦合应用程序，你可以修改应用程序的一部分中的代码，而无需修改应用程序的其他部分中的代码。 松耦合应用程序是更具弹性，若要更改。

目前，所有的联系人管理器应用程序使用的数据访问和验证逻辑都包含在控制器类。 这是个好主意。 无论何时需要修改你的应用程序的一部分，则可能你的应用程序的另一部分中引入 bug。 例如，如果你修改你的验证逻辑，则可能你的数据访问或控制器逻辑引入新 bug。

> [!NOTE] 
> 
> (SRP) 不同，一个类应永远不会有多个原因更改。 混合控制器、 验证和数据库逻辑是大规模违反了单一的责任原则。


有可能需要修改你的应用程序的几个原因。 你可能需要将一项新功能添加到你的应用程序，你可能需要在你的应用程序，修复 bug 或可能需要修改你的应用程序的一项功能的实现方式。 应用程序是很少静态的。 他们往往会增长且随着时间的推移发生变化。

例如，假设你决定要更改如何实现你的数据访问层。 右现在，联系人管理器应用程序使用 Microsoft 实体框架访问数据库。 但是，你可能决定迁移到新的或备用数据访问技术，例如 ADO.NET Data Services 或 nhibernate 进行。 但是，由于数据访问代码是独立于验证和控制器代码的没有方法来修改你的应用程序中的数据访问代码，而不修改其他与数据访问直接相关的代码。

当松散耦合应用程序时，另一方面，你可以更改应用程序的一部分而无需触摸应用程序的其他部分。 例如，你可以切换数据访问技术，而不修改你验证或控制器的逻辑。

在此迭代中，我们利用多个软件设计模式使我们能够重构到更松散耦合的应用程序我们联系人管理器的应用程序。 当我们完后时，获胜 t 联系人管理器执行任何操作，它不是 t 以前执行的操作。 但是，我们将能够更改应用程序更轻松地在将来。

> [!NOTE] 
> 
> 重构是重写的方式，它不会丢失任何现有功能的应用程序的过程。


## <a name="using-the-repository-software-design-pattern"></a>使用存储库软件设计模式

我们第一项更改是充分利用称为存储库模式的软件设计模式。 我们将使用的存储库模式来隔离我们从我们的应用程序的其余部分的数据访问代码。

实现存储库模式要求我们完成以下两个步骤：

1. 要创建用户界面
2. 创建实现接口的具体类

首先，我们需要创建一个描述所有我们需要执行的数据访问方法的接口。 IContactManagerRepository 接口包含在清单 1。 此接口描述五个方法： CreateContact()、 DeleteContact()、 EditContact()、 GetContact 和 ListContacts()。

**列表 1-Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

接下来，我们需要创建一个实现 IContactManagerRepository 接口的具体类。 由于我们使用 Microsoft 实体框架来访问数据库，我们将创建一个名为 EntityContactManagerRepository 的新类。 此类包含在清单 2。

**列出 2-Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

请注意，EntityContactManagerRepository 类可实现 IContactManagerRepository 接口。 类实现该接口所描述的方法的所有五个。

你可能想知道为什么我们需要向两个接口。 我们为何需要创建一个接口和类来实现它？

有一个例外，我们的应用程序的其余部分将与该接口而不是具体的类进行交互。 而不是调用由 EntityContactManagerRepository 类公开的方法，我们将调用由 IContactManagerRepository 接口公开的方法。

这样一来，我们可以实现新类的接口，而无需修改应用程序的其余部分。 例如，在将来的某个日期，我们可能想要实现 DataServicesContactManagerRepository 类实现 IContactManagerRepository 接口。 DataServicesContactManagerRepository 类可能使用 ADO.NET 数据服务来访问数据库而不是 Microsoft 的实体框架。

如果应用程序代码进行编程针对 IContactManagerRepository 接口而不是具体的 EntityContactManagerRepository 类这样我们就可以切换具体类而不修改任何代码的其余部分。 例如，我们可以从切换 EntityContactManagerRepository 类到 DataServicesContactManagerRepository 类而不修改我们的数据访问或验证逻辑。

对接口 （抽象） 而不是具体类编程使我们的应用程序更具弹性，若要更改。

> [!NOTE] 
> 
> 通过选择菜单选项重构，提取接口，可以快速从 Visual Studio 中的具体类创建一个接口。 例如，你可以首先创建 EntityContactManagerRepository 类，然后使用提取接口自动生成 IContactManagerRepository 接口。


## <a name="using-the-dependency-injection-software-design-pattern"></a>使用依赖关系注入软件设计模式

现在，我们已有迁移我们数据访问代码到单独的存储库类，我们需要修改联系人控制器使用此类。 我们将利用调用依赖关系注入要在控制器中使用的存储库类软件设计模式。

修改后的联系人控制器包含在清单 3。

**列出 3-Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

请注意，列出 3 中的联系人控制器有两个构造函数。 第一个构造函数将 IContactManagerRepository 接口的具体实例传递给第二个构造函数。 联系人控制器类使用*构造函数依赖关系注入*。

在第一个构造函数和仅使用 EntityContactManagerRepository 类的位置。 类的其余部分以 IContactManagerRepository 接口而不是具体的 EntityContactManagerRepository 类。

这很容易地在将来切换 IContactManagerRepository 类的实现。 如果你想要使用 DataServicesContactRepository 类而不是 EntityContactManagerRepository 类，只需修改第一个构造函数。

构造函数依赖关系注入使得联系人控制器类非常可测试。 在单元测试中，您可以通过传递 IContactManagerRepository 类的一个模拟实现实例化联系人控制器。 我们生成联系人管理器应用程序的单元测试时，此功能的依赖关系注入将与我们在下一个迭代中非常重要。

> [!NOTE] 
> 
> 如果你想要完全分离 IContactManagerRepository 接口的特定实现中的联系人控制器类然后你可以利用框架，支持如 StructureMap 或 Microsoft 的依赖关系注入实体框架 (MEF)。 通过利用的依赖关系注入框架，永远不会需要引用在代码中的具体类。


## <a name="creating-a-service-layer"></a>创建服务层

你可能已经注意到我们验证逻辑仍混合起来与我们控制器逻辑中列出的 3 修改的控制器类。 对于同样的原因，它是一个好办法隔离我们数据访问逻辑，它是一个好办法隔离我们验证逻辑。

若要解决此问题，我们可以创建一个单独[服务层](http://martinfowler.com/eaaCatalog/serviceLayer.html)。 服务层是我们可以我们控制器和存储库类之间插入一个单独的层。 服务层包含我们业务逻辑，包括所有我们验证逻辑。

ContactManagerService 包含在清单 4。 它包含验证逻辑，从联系人控制器类。

**列出 4-Models\ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

请注意，ContactManagerService 的构造函数 ValidationDictionary。 服务层与通过此 ValidationDictionary 的控制器层通信。 在我们讨论修饰器模式时，我们将讨论以下节中详细介绍 ValidationDictionary。

此外，请注意，ContactManagerService 实现 IContactManagerService 接口。 您始终应尽量针对接口而不是具体类进行编程。 联系人管理器应用程序中的其他类并不直接交互与 ContactManagerService 类中。 相反，有一个例外，则针对 IContactManagerService 接口编程联系人管理器应用程序的其余部分。

IContactManagerService 接口包含在列出 5。

**列出 5-Models\IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

修改的联系人控制器类包含在列出 6。 请注意联系人控制器不再与 ContactManager 存储库交互。 相反，请与控制器与 ContactManager 服务交互。 每一层隔离尽可能多地从其他层。

**列出 6-Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

运行与我们的应用程序不再的单个责任原则 (SRP)。 列出 6 中的联系人控制器已去除以外控制流的应用程序执行的每个责任。 已联系控制器中删除所有的验证逻辑和推送到服务层。 所有数据库逻辑已推送到存储库层。

## <a name="using-the-decorator-pattern"></a>使用修饰器模式

我们想要能够完全分离我们从我们控制器层的服务层。 原则上，我们应能够在单独的程序集从我们控制器层我们服务层编译而无需添加对我们的 MVC 应用程序的引用。

但是，我们的服务层需要能够将验证错误消息传递回控制器层。 我们可以如何启用服务层进行通信而无需耦合的控制器和服务层验证错误消息？ 我们可以利用的名为一种软件设计模式[修饰器模式](http://en.wikipedia.org/wiki/Decorator_pattern)。

控制器使用名为 ModelState ModelStateDictionary 来表示验证错误。 因此，你可能想要将 ModelState 从控制器层传递到服务层。 但是，在服务层中使用 ModelState 会使你的服务层依赖于 ASP.NET MVC framework 的功能。 这将是错误，因为以后，你可能想要使用而不是 ASP.NET MVC 应用程序的 WPF 应用中使用的服务层。 在这种情况下，你对 t 要引用要使用 ModelStateDictionary 类的 ASP.NET MVC framework。

修饰器模式，可将现有类包装在新类，以便实现接口。 我们的联系人管理器项目包括列出 7 中包含的 ModelStateWrapper 类。 在列出 8，ModelStateWrapper 类实现的接口。

**列出 7-Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**列出 8-Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

如果需要仔细查看一下列出 5 你将看到 ContactManager 服务层以独占方式使用 IValidationDictionary 接口。 ContactManager 服务不依赖于 ModelStateDictionary 类。 当联系人控制器创建 ContactManager 服务时，控制器将包装其 ModelState 如下：

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>摘要

在此迭代中，我们未向联系人管理器应用程序添加任何新功能。 此迭代的目标是重构以便更轻松地监视和修改联系人管理器应用程序。

首先，我们实现的存储库软件设计模式。 我们将迁移所有数据访问代码到单独的 ContactManager 存储库类。

我们还与我们控制器逻辑隔离我们验证逻辑。 我们创建一个单独的服务层包含所有我们验证代码。 与服务层中，控制器层交互并与存储库层交互的服务层。

当我们创建的服务层时，我们利用修饰器模式来隔离 ModelState 从我们的服务层。 在我们的服务层，我们将编程针对而不是 ModelState IValidationDictionary 接口。

最后，我们所花费的名为依赖关系注入模式软件设计模式的优点。 此模式可让我们来针对而不是具体的类接口 （抽象） 进行编程。 实现依赖关系注入设计模式还使我们的代码更可测试。 在下一步的迭代中，我们将单元测试添加到我们的项目。

>[!div class="step-by-step"]
[上一页](iteration-3-add-form-validation-vb.md)
[下一页](iteration-5-create-unit-tests-vb.md)
