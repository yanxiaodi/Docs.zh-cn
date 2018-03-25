---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: "迭代 6 – 使用测试驱动开发 (VB) |Microsoft 文档"
author: microsoft
description: "在此第六个迭代中，我们将添加新功能到我们的应用程序通过首先编写单元测试和针对单元测试编写代码。 此迭代中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b558df9c0b44f5f76115270d361b6022658f9f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="iteration-6--use-test-driven-development-vb"></a>迭代 6 – 使用测试驱动开发 (VB)
====================
通过[Microsoft](https://github.com/microsoft)

[下载代码](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> 在此第六个迭代中，我们将添加新功能到我们的应用程序通过首先编写单元测试和针对单元测试编写代码。 在此迭代中，我们添加联系人的组。


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

在联系人管理器应用程序前一次迭代，我们创建单元测试以提供网络安全代码。 用于创建单元测试的动机是使我们的代码更具弹性，若要更改。 使用就地的单元测试，我们可以不假思索地对代码进行任何更改，并立即知道是否但我们破坏了现有功能。

在此迭代中，我们使用单元测试完全不同目的。 在此迭代中，我们使用单元测试的调用应用程序设计理念一部分*测试驱动开发*。 当您练习测试驱动开发，首先编写测试，然后编写针对测试的代码。

更确切地说，当练习测试驱动开发，有三个步骤，在完成后创建代码 (红色 / 绿色/重构):

1. 编写失败 （红色） 的单元测试
2. 编写代码中通过单元测试 （绿色）
3. 重构代码 （重构）

首先，你编写单元测试。 单元测试应表达你打算为您的代码的预期行为。 当你首次创建单元测试时，单元测试应失败。 测试应失败，因为尚未编写满足测试任何应用程序代码。

接下来，按顺序传递使单元测试编写刚好足够的代码。 目标是 laziest、 sloppiest 和速度最快可能方式编写的代码。 不应浪费时间思考一下你的应用程序的体系结构。 相反，你应专注于编写代码满足表示由单元测试的目的所需的最少工作量。

最后，在编写完足够的代码后，你可以一步并考虑你的应用程序的总体体系结构。 在此步骤中，重写 （重构） 你的代码通过利用软件设计模式-例如的存储库模式-以使代码更易于维护。 因为你的代码涵盖的单元测试，你便可以 fearlessly 重写此步骤中的代码。

有很多好处由测试驱动开发在其中完成练习导致的。 首先，测试驱动开发强制您可以专注于实际上需要以编写的代码。 因为你不断侧重于只编写足够的代码来传递特定测试，将阻止到杂草发散型和写入大量您不应使用的代码。

其次，"第一次测试"的设计方法将强制你编写代码，从如何将使用你代码的角度来看。 换而言之，如果测试驱动开发在其中完成练习，您不断地编写测试从用户角度来看。 因此，测试驱动开发会导致更简洁且更易于理解的 Api。

最后，测试驱动开发将强制你编写的应用程序的正常过程的一部分编写单元测试。 随着项目截止时间的临近，通常多测试是窗口出现故障的第一个操作。 当测试驱动开发，在其中完成练习另一方面，你很有可能是良性有关编写单元测试，因为测试驱动开发使单元测试中央为构建一个应用程序的过程。

> [!NOTE] 
> 
> 若要了解有关测试驱动开发的详细信息，建议你阅读 Michael 羽化簿**工作有效地使用旧代码**。


在此迭代中，我们将一项新功能添加到我们的联系人管理器应用程序。 我们将添加对联系人组的支持。 你可以使用联系人组将你的联系人组织为类别，如业务和友元组。

我们将此新功能添加到我们的应用程序，按照测试驱动开发的进程。 我们将首先编写单元测试，并且我们将编写所有我们针对这些测试的代码。

## <a name="what-gets-tested"></a>什么获取测试

如我们在前一次迭代中所述，您通常不要的数据访问逻辑编写单元测试或查看逻辑。 您不 t 写入单元测试的数据访问逻辑，因为访问数据库是一个相对较慢的操作。 您不 t 写入单元测试的视图逻辑，因为访问视图需要 web 服务器是一个相对较慢的操作的运转。 你不应 t 编写单元测试，除非测试可执行反复非常快

因为测试驱动开发驱动的单元测试中，我们重点最初编写控制器和业务逻辑。 我们避免触摸视图的数据库。 我们获胜 t 修改数据库或本教程的最后才创建我们的视图。 我们开始什么可以进行测试。

## <a name="creating-user-stories"></a>创建用户情景

当测试驱动开发在其中完成练习，你始终通过启动编写的测试。 这将立即引发问题： 如何决定哪个测试将第一次？ 若要回答这个问题，应编写一套[*用户情景*](http://en.wikipedia.org/wiki/User_stories)。

用户情景是非常短暂 （通常一个句子） 描述的软件要求。 它应从用户角度来看写入的要求的非技术说明。

此处 s 描述新的联系人组功能所需的功能的用户情景的集：

1. 用户可以查看联系人的组的列表。
2. 用户可以创建新联系人的组。
3. 用户可以删除现有联系人组。
4. 创建新的联系人时，用户可以选择联系人的组。
5. 编辑现有联系人时，用户可以选择一个联系人的组。
6. 在索引视图中显示的联系人的组列表。
7. 当用户单击联系人的组时，显示匹配联系人的列表。

请注意，此列表的用户情景客户完全理解。 并没有提及的技术实现详细信息。

虽然过程中生成应用程序，组的用户情景可能会变得更完善。 为多个情景 （要求），可能会中断用户情景。 例如，你可能会决定创建一个新的联系人组应涉及验证。 提交联系人组没有名称应返回验证错误。

创建的用户情景的列表后，你就可以编写第一个单元测试。 我们首先创建一个单元测试用于查看的联系人的组的列表。

## <a name="listing-contact-groups"></a>列出联系人的组

我们第一个用户情景，用户应该是能够查看联系人的组的列表。 我们需要使用测试 express 这篇文章。

创建新的单元测试，请右键单击 Controllers 文件夹在 ContactManager.Tests 项目中，选择**添加、 新建测试**，并选择**单元测试**模板 （请参见图 1）。 在新的单元测试 GroupControllerTest.vb 并单击**确定**按钮。


[![添加 GroupControllerTest 单元测试](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**图 01**： 添加 GroupControllerTest 单元测试 ([单击以查看实际尺寸的图像](iteration-6-use-test-driven-development-vb/_static/image2.png))


我们第一个单元测试包含在清单 1。 此测试验证组控制器的 index （） 方法返回一组的组。 测试将验证的组的集合视图中返回数据。

**列表 1-Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

当你第一次键入列出 Visual Studio 中的 1 中的代码时，你将获得大量的红色波浪线。 我们尚未创建 GroupController 或组类。

此时，我们可以 t 甚至生成我们的应用程序，以便我们可以 t 执行我们的第一个单元测试。 S 良好。 将计为未通过的测试。 因此，我们现在有权开始编写应用程序代码。 我们需要编写足够的代码来执行我们的测试。

列出 2 中的组控制器类包含需通过单元测试的代码的最低要求。 Index （） 操作返回组 （组类定义中列出的 3） 的静态编码的的列表。

**列出 2-Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**列出 3-Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

我们将 GroupController 和组类添加到我们的项目之后，我们第一个单元测试成功完成 （请参见图 2）。 我们已完成通过的测试所需的最小工作量。 是时候纸婚。


[![成功 ！](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**图 02**： 成功 ！ ([单击以查看实际尺寸的图像](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>创建联系人的组

现在我们可以转到第二个用户情景。 我们需要能够创建新联系人的组。 我们需要使用测试 express 此目的。

列出 4 中的测试验证调用 create （） 方法与新的组将组添加到组 index （） 方法返回的列表。 换而言之，如果我创建一个新组然后我应能够新组取回从 index （） 方法返回的组的列表。

**列出 4-Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

在测试中列出 4 调用组控制器与新联系人组的 create （） 方法。 接下来，测试验证，调用组控制器 index （） 方法返回新的组中查看数据。

列出 5 中的已修改的组控制器包含更改通过新的测试所需的最低。

**列出 5-Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>列出 5 中的组控制器具有新的 create （） 操作。 此操作将组添加到组的集合。 请注意 index （） 操作修改为返回的组的集合的内容。

同样，我们进行裸机最少量的工作来进行传递的单元测试。 我们对组控制器进行这些更改后，我们单元的所有测试都通过。

## <a name="adding-validation"></a>添加验证

显式用户情景中，未指定此要求。 但是，它是合理要求一组有一个名称。 否则，组织联系人为组，将不会非常有用。

列出 6 包含新的测试表明此目的。 此测试验证尝试未提供的验证错误消息模型状态中的名称结果情况下创建组。

**列出 6-Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

为了满足此测试，我们需要添加到组类 （请参阅列出 7） 的名称属性。 此外，我们需要将一小段验证逻辑添加到我们组控制器 s create （） 操作 （请参阅列出 8）。

**列出 7-Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**列出 8-Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

请注意，组控制器 create （） 操作现在包含同时验证和数据库的逻辑。 当前，使用组控制器的数据库所包含的只是内存中的集合。

## <a name="time-to-refactor"></a>重构的时间

红色/绿色/重构的第三步是重构一部分。 此时，我们需要从我们的代码返回步骤并考虑如何我们可以重构应用程序以提高其设计。 重构阶段是从该处我们认真考虑实施软件设计原则和模式的最佳方式的阶段。

我们可以随意修改我们代码中我们选择以改进代码的设计以任何方式。 我们有安全网我们阻止在出现中断现有功能的单元测试。

现在，我们组控制器是优秀的软件设计角度混乱。 组控制器包含验证和数据访问代码 tangled 的的混乱。 若要避免违反单个责任原则，我们需要将这些问题分离到不同的类。

我们重构的组控制器类包含在列出 9。 控制器已修改为使用 ContactManager 服务层。 这是我们的联系人控制器使用的相同服务层。

列出 10 包含添加到 ContactManager 服务层，以支持验证、 列出和创建组的新方法。 IContactManagerService 接口已更新为包括新方法。

列出 11 包含一个新的 FakeContactManagerRepository 类实现 IContactManagerRepository 接口。 与不同的是还实现 IContactManagerRepository 接口 EntityContactManagerRepository 类，我们新 FakeContactManagerRepository 类不与数据库通信。 FakeContactManagerRepository 类使用的内存中集合作为代理的数据库。 作为假存储库层，我们将在我们的单元测试中使用此类。

**列出 9-Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**列出 10-Controllers\ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**列出 11-Controllers\FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


修改接口需要 IContactManagerRepository 用于 EntityContactManagerRepository 类中实现的 CreateGroup() 和 ListGroups() 方法。 执行此操作的 laziest 和最快方法是添加存根 （stub） 方法，如下所示：

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


最后，我们的应用程序的设计这些更改要求我们使到我们的单元测试中的一些修改。 现在，我们需要执行单元测试时使用 FakeContactManagerRepository。 更新后的 GroupControllerTest 类包含在列出 12。

**列出 12-Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

我们进行所有这些后更改，同样，所有我们通过单元测试。 我们已经完成红色/绿色/重构的整个周期。 我们已实施的第一个的两个用户情景。 我们现在有支持要求表示在用户情景中的单元测试。 实现用户情景的其余部分包括重复的红色/绿色/重构此相同循环。

## <a name="modifying-our-database"></a>修改我们的数据库

遗憾的是，即使我们已经满足所有要求表示我们单元测试，我们的工作不会执行。 我们仍需要修改我们的数据库。

我们需要创建一个新的组数据库表。 请执行这些步骤：

1. 在服务器资源管理器窗口中，右键单击表文件夹，然后选择菜单选项**添加新表**。
2. 输入在表设计器中，如下所述的两个列。
3. 将标记作为 primary key 和标识列的 Id 列。
4. 通过单击软盘图标保存命名组的新表。

<a id="0.12_table01"></a>


| **列名称** | **数据类型** | **允许 null 值** |
| --- | --- | --- |
| Id | int | False |
| 名称 | nvarchar(50) | False |


接下来，我们需要从联系人表中删除的所有数据 (否则，我们获胜 t 能够创建联系人和组表之间的关系)。 请执行这些步骤：

1. 右键单击联系人表，然后选择菜单选项**显示表数据**。
2. 删除的所有行。

接下来，我们需要定义组数据库表和现有的联系人数据库表之间的关系。 请执行这些步骤：

1. 双击服务器资源管理器窗口以打开表设计器中的联系人表。
2. 将新的整数列添加到 Contacts 表名为 GroupId。
3. 单击关系按钮以打开外键关系对话框 （请参见图 3）。
4. 单击添加按钮。
5. 单击表和列规范按钮旁边显示的省略号按钮。
6. 在表和列对话框中，选择作为主键表和作为主键列的 Id 的组。 选择联系人作为外键表和作为外键列的 GroupId （请参见图 4）。 单击确定按钮。
7. 下**INSERT 和 UPDATE 规范**，选择值**Cascade**为**删除规则**。
8. 单击关闭按钮以关闭外键关系对话框。
9. 单击保存按钮以保存对联系人表的更改。


[![创建数据库表关系](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**图 03**： 创建数据库表关系 ([单击以查看实际尺寸的图像](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![指定表关系](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**图 04**： 指定表关系 ([单击以查看实际尺寸的图像](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>更新我们的数据模型

接下来，我们需要更新我们的数据模型来表示新的数据库表。 请执行这些步骤：

1. 双击模型文件夹，打开实体设计器中的 ContactManagerModel.edmx 文件。
2. 右键单击设计器图面，然后选择菜单选项**从数据库更新模型**。
3. 在更新向导中，选择组表，然后单击完成按钮 （请参见图 5）。
4. 右键单击组实体，然后选择菜单选项**重命名**。 更改的名称*组*实体到*组*（采用单数形式）。
5. 右键单击联系人实体的底部将显示的组导航属性。 更改的名称*组*导航属性*组*（采用单数形式）。


[![更新数据库中的实体框架模型](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**图 05**： 更新从数据库的实体框架模型 ([单击以查看实际尺寸的图像](iteration-6-use-test-driven-development-vb/_static/image10.png))


完成这些步骤后，你的数据模型将表示的联系人和组的表。 实体设计器应显示这两个实体 （请参阅图 6）。


[![实体设计器中显示组和联系人](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**图 06**： 实体设计器中显示组和联系人 ([单击以查看实际尺寸的图像](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>创建我们存储库类

接下来，我们需要实现我们存储库类。 在此迭代过程中，我们添加几个新方法到 IContactManagerRepository 界面在编写代码以满足我们的单元测试时。 在列出 14 包含 IContactManagerRepository 接口的最终版本。

**列出 14-Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

我们还没有实际实现任何与使用我们的实际 EntityContactManagerRepository 类中的联系人组相关的方法。 目前，EntityContactManagerRepository 类具有存根 （stub） 方法的每个 IContactManagerRepository 界面中列出联系人组方法。 例如，ListGroups() 方法当前类似如下所示：

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

存根 （stub） 方法使我们能够编译我们的应用程序和通过的单元测试。 但是，现在它是时间实际实现这些方法。 EntityContactManagerRepository 类的最终版本包含在列出 13。

**列出 13-Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>创建视图

ASP.NET MVC 应用程序时使用的默认 ASP.NET 视图引擎。 因此，don t 创建视图，以响应特定的单元测试。 但是，因为应用程序将毫无用处，除非视图，我们可以 t 完成此迭代，而创建和修改联系人管理器应用程序中包含的视图。

我们需要创建以下新视图用于管理联系人的组 （请参阅图 7）：

- Views\Group\Index.aspx 的联系人的组的显示列表
- Views\Group\Delete.aspx-用于删除联系人的组的显示确认窗体


[![组索引视图](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**图 07**: 组索引视图 ([单击以查看实际尺寸的图像](iteration-6-use-test-driven-development-vb/_static/image14.png))


我们需要修改以下现有视图，使其包含联系人的组：

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

你可以通过查看此教程中随附的 Visual Studio 应用程序看到修改后的视图。 例如，图 8 显示了联系人索引视图。


[![请与索引视图](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**图 08**: 联系人索引视图 ([单击以查看实际尺寸的图像](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>摘要

在此迭代中，我们添加新功能到我们的联系人管理器应用程序按照测试驱动开发应用程序设计方法。 通过创建一组用户情景，我们已开始。 我们创建一组单元测试，对应于表达用户情景的需求。 最后，我们已写入刚好足够的代码，以满足要求的单元测试来表示。

我们完成编写足够的代码来满足表示单元测试的要求后，我们更新我们的数据库和视图。 我们向我们的数据库添加新的组表，并更新我们实体框架数据模型。 我们还创建和修改一组视图。

在下一次迭代-最后一个迭代-我们重写应用程序以充分利用 Ajax。 通过利用 Ajax，我们将提高响应能力和联系人管理器应用程序的性能。

>[!div class="step-by-step"]
[上一页](iteration-5-create-unit-tests-vb.md)
[下一页](iteration-7-add-ajax-functionality-vb.md)
