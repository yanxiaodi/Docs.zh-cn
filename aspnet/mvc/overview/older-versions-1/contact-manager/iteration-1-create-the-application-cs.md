---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: "迭代 #1 – 创建应用程序 (C#) |Microsoft 文档"
author: microsoft
description: "在第一次迭代中，我们创建联系人管理器中的最简单方法可能。 我们将添加对基本数据库操作的支持： 创建、 读取、 更新和 D...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: 12927250595a8f3130328d2fe219280a13349787
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="iteration-1--create-the-application-c"></a>迭代 #1 – 创建应用程序 (C#)
====================
通过[Microsoft](https://github.com/microsoft)

[下载代码](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> 在第一次迭代中，我们创建联系人管理器中的最简单方法可能。 我们将添加对基本数据库操作的支持： 创建、 读取、 更新和删除 (CRUD)。


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

在此第一次迭代中，我们生成的基本应用程序。 目标是中的最快和最简单的方法可能生成联系人管理器。 在更高版本迭代中，我们改进应用程序的设计。

联系人管理器应用程序是一个基本的数据库驱动应用程序。 应用程序可用于创建新的联系人、 编辑现有联系人和删除联系人。

在此迭代中，我们将完成以下步骤：

1. ASP.NET MVC 应用程序
2. 创建一个数据库来存储我们联系人
3. 生成用于我们的 Microsoft 实体框架的数据库的模型类
4. 创建控制器操作和视图，以使我们以列出所有数据库中的联系人
5. 创建控制器操作和使我们能够在数据库中创建新联系人的视图
6. 创建控制器操作和使我们能够编辑现有联系人数据库中的视图
7. 创建控制器操作和使我们能够删除现有联系人数据库中的视图

## <a name="software-prerequisites"></a>软件必备项

在 ASP.NET MVC 应用程序，你必须具有 Visual Studio 2008 或 （Visual Web Developer 是不包括所有 Visual Studio 的高级功能的 Visual Studio 的免费版本） 在计算机上安装的 Visual Web Developer 2008。 你可以从以下地址下载 Visual Studio 2008 试用版，或者 Visual Web Developer 中：

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> 对于使用 Visual Web Developer 的 ASP.NET MVC 应用程序，你必须安装的 Visual Web Developer Service Pack 1。 不带 Service Pack 1，无法创建 Web 应用程序项目。


ASP.NET MVC 框架。 你可以从以下地址下载 ASP.NET MVC framework:

[https://www.asp.net/mvc](../../../index.md)

在本教程中，我们可以使用 Microsoft 实体框架访问数据库。 实体框架将包含在.NET Framework 3.5 Service Pack 1。 你可以从以下位置下载此 service pack:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

作为执行每个这些下载逐个的替代方法，可以利用 Web 平台安装程序 (Web PI)。 你可以从以下地址下载 Web PI:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET MVC 项目

ASP.NET MVC Web 应用程序项目。 启动 Visual Studio，然后选择菜单选项**文件、 新项目**。 **新项目**对话框 （请参见图 1）。 选择**Web**项目类型和**ASP.NET MVC Web 应用程序**模板。 命名新项目*ContactManager* ，然后单击确定按钮。


请确保你具有从顶部的下拉列表中选择的.NET Framework 3.5 角**新项目**对话框。 否则，将出现获胜 t 的 ASP.NET MVC Web 应用程序模板。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**图 01**: 的新项目对话框 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image2.png))


ASP.NET MVC 应用程序，**创建单元测试项目**此时将显示对话框。 可以使用此对话框以指示你想要创建并将单元测试项目添加到你的解决方案创建 ASP.NET MVC 应用程序。 虽然我们获胜 t 会生成此迭代中的单元测试，则应选择选项**是，创建单元测试项目**由于我们计划在后续迭代中添加单元测试。 首次创建新的 ASP.NET MVC 项目中添加的测试项目是比创建 ASP.NET MVC 项目后添加的测试项目容易得多。

> [!NOTE] 
> 
> 因为 Visual Web Developer 不支持测试项目，则无法获得创建单元测试项目对话框，使用 Visual Web Developer 时。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**图 02**: 创建单元测试项目对话框 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image4.png))


Visual Studio 解决方案资源管理器窗口中显示的 ASP.NET MVC 应用程序 （请参见图 3）。 如果 don t 看到解决方案资源管理器窗口，然后通过选择菜单选项可打开此窗口**视图、 解决方案资源管理器**。 请注意，该解决方案包含两个项目： ASP.NET MVC 项目和测试项目。 ASP.NET MVC 项目命名为 ContactManager 和测试项目命名为 ContactManager.Tests。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**图 03**: 解决方案资源管理器窗口 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>删除项目示例文件

ASP.NET MVC 项目模板包括用于控制器和视图的示例文件。 在创建新的 ASP.NET MVC 应用程序之前, 应删除这些文件。 你可以通过右键单击文件或文件夹并选择菜单选项删除文件和文件夹在解决方案资源管理器窗口中的**删除**。

你需要从 ASP.NET MVC 项目中删除以下文件：

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

而且，你需要从测试项目中删除以下文件：

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>创建数据库

联系人管理器应用程序是一个数据库驱动的 web 应用程序。 我们使用数据库来存储的联系信息。

与任何现代的数据库，包括 Microsoft SQL Server、 Oracle、 MySQL 和 IBM DB2 数据库的 ASP.NET MVC 框架。 在本教程中，我们使用 Microsoft SQL Server 数据库。 在安装 Visual Studio 时，将向你提供的安装 Microsoft SQL Server Express，是免费版的 Microsoft SQL Server 数据库的选项。

通过右键单击应用程序创建新数据库\_在解决方案资源管理器窗口并选择菜单选项的数据文件夹**添加、 新项**。 在**添加新项**对话框中，选择**数据**类别和**SQL Server 数据库**模板 （请参见图 4）。 将新数据库 ContactManagerDB.mdf，然后单击确定按钮。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**图 04**： 创建新的 Microsoft SQL Server Express 数据库 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image8.png))


创建新的数据库后，数据库将出现在应用程序\_在解决方案资源管理器窗口中的数据文件夹。 双击 ContactManager.mdf 文件以打开服务器资源管理器窗口，然后连接到数据库。

> [!NOTE] 
> 
> 服务器资源管理器窗口调用在 Microsoft Visual Web Developer 的情况下的数据库资源管理器窗口。


服务器资源管理器窗口可用于创建新的数据库对象，如数据库表、 视图、 触发器和存储的过程。 右键单击表文件夹，然后选择菜单选项**添加新表**。 此数据库表设计器显示 （请参见图 5）。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**图 05**： 数据库表设计器 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image10.png))


我们需要创建包含以下各列的表：

<a id="0.1_table01"></a>


| **列名称** | **数据类型** | **允许 null 值** |
| --- | --- | --- |
| Id | int | false |
| FirstName | nvarchar(50) | false |
| LastName | nvarchar(50) | false |
| 电话 | nvarchar(50) | false |
| 电子邮件 | nvarchar （255) | false |


第一列，Id 列中，是非常特殊。 你需要将 Id 列标记为标识列和主键列。 指示列是标识列，通过展开列属性 （查找底部的图 6） 和滚动到标识规范属性。 设置**（是标识）**属性值**是**。

通过选择列并单击带有密钥的图标的按钮可将列标记为主键列中。 某列被标记为主键列后，列旁边将显示一个图标的密钥 （请参阅图 6）。

完成创建表后，单击保存按钮 （带有软盘图标的按钮） 以保存新的表。 将新表命名*联系人*。

后创建的联系人数据库表的完成，应将某些记录添加到表中。 右键单击服务器资源管理器窗口中的联系人表，然后选择菜单选项**显示表数据**。 在出现的网格中输入一个或多个联系人。

## <a name="creating-the-data-model"></a>创建数据模型

ASP.NET MVC 应用程序包含模型、 视图和控制器。 我们首先创建一个表示联系人表，我们在上一节中创建的模型类。

在本教程中，我们可以使用 Microsoft 实体框架自动从数据库生成模型类。

> [!NOTE] 
> 
> ASP.NET MVC framework 未绑定到以任何方式 Microsoft 实体框架。 与其他数据库访问技术包括 NHibernate，LINQ to SQL 中或 ADO.NET，可以使用 ASP.NET MVC。


请按照下列步骤以创建数据模型类：

1. 右击解决方案资源管理器窗口中的 Models 文件夹并选择**添加、 新项**。 **添加新项**对话框 （请参见图 6）。
2. 选择**数据**类别和**ADO.NET 实体数据模型**模板。 命名你的数据模型*ContactManagerModel.edmx*单击**添加**按钮。 出现实体数据模型向导 （请参阅图 7）。
3. 在**选择模型内容**步骤中，选择**从数据库生成**（请参阅图 7）。
4. 在**选择你的数据连接**步骤，选择 ContactManagerDB.mdf 数据库，然后输入名称*ContactManagerDBEntities* （请参阅图 8） 的实体连接设置。
5. 在**选择数据库对象**步骤中，选中复选框标记为的表 （请参阅图 9）。 数据模型将包括 （仅仅是一个，Contacts 表没有） 查看数据库中包含的所有表。 输入的命名空间*模型*。 单击完成按钮以完成向导。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**图 06**: 添加新项对话框 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image12.png))


[![新项目对话框](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**图 07**： 选择模型内容 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image14.png))


[![新项目对话框](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**图 08**： 选择你的数据连接 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image16.png))


[![新项目对话框](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**图 09**： 选择数据库对象 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image18.png))


完成实体数据模型向导后，将显示实体数据模型设计器。 设计器为每个要建模的表显示相对应的类。 你应看到一个名为联系人的类。

实体数据模型向导生成基于数据库的表名称的类名称。 几乎总是需要更改由向导生成的类的名称。 右键单击设计器中的联系人类，然后选择菜单选项**重命名**。 将类的名称从联系人 （复数形式） 更改为联系人 （单个）。 更改类名称后，类应该如下图 10。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**图 10**: 联系人类 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image20.png))


此时，我们已创建我们的数据库模型。 我们可以使用联系人类来表示我们的数据库中的特定联系人记录。

## <a name="creating-the-home-controller"></a>创建主控制器

下一步是创建我们主控制器。 Home 控制器是在 ASP.NET MVC 应用程序中调用的默认控制器。

通过右键单击解决方案资源管理器窗口中的 Controllers 文件夹并选择菜单选项创建主页控制器类**添加、 控制器**（请参阅图 11）。 请注意该复选框标记为**添加用于创建、 更新和详细信息的方案的操作方法**。 请确保单击前选中此复选框，**添加**按钮。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**图 11**： 添加主控制器 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image22.png))


创建主控制器时，获取类列表 1 中。

**列表 1-Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>列出联系人

为了在联系人数据库表中显示的记录，我们需要创建 index （） 操作和索引视图。

Home 控制器已包含 index （） 操作。 我们需要修改此方法，使其类似列出 2 所示。

**列出 2-Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

请注意，列出 2 中的主页控制器类包含名为的私有字段\_实体。 \_实体字段表示实体数据模型中。 我们使用\_实体字段以与数据库通信。

Index （） 方法返回表示联系人的所有联系人数据库表中的视图。 表达式\_实体。ContactSet.ToList() 泛型列表的形式返回联系人的列表。

现在我们制作了索引控制器中，我们接下来需要创建索引视图。 在创建索引视图之前, 编译你的应用程序通过选择菜单选项**生成，生成解决方案**。 始终应在将视图添加以使模型类的列表中显示之前编译你的项目**添加视图**对话框。

你创建索引视图，请右键单击 index （） 方法并选择菜单选项**添加视图**（请参阅图 12）。 选择此菜单选项将打开**添加视图**对话框 （请参阅图 13）。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**图 12**： 添加索引视图 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image24.png))


在**添加视图**对话框中，选中复选框标记为**创建强类型化视图**。 选择视图数据类 ContactManager.Models.Contact 和查看内容列表。 选择这些选项生成的视图，显示的联系人记录的列表。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**图 13**: 添加视图对话框 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image26.png))


当你单击**添加**生成按钮，列出 3 中的索引视图。 请注意&lt;%@ 页 %&gt;文件的顶部显示的指令。 索引视图继承自 ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt;类。 换而言之，在视图中的模型类表示联系人实体的列表。

索引视图的正文包含的 foreach 循环中循环访问每个模型类表示的联系人。 HTML 表中显示的联系人类的每个属性的值。

**列出 3-Views\Home\Index.aspx （未修改）**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

我们需要一个对进行修改的索引视图。 因为我们不创建详细信息视图，我们可以移除的详细信息链接。 查找并从索引视图中删除以下代码：

{id = 项。Id}) %&gt;

修改索引视图后，你可以运行联系人管理器应用程序。 选择启动调试菜单选项调试，或只需按 F5。 首次运行该应用程序，您会得到对话框图 14 中。 选择选项**修改 Web.config 文件以启用调试**，然后单击确定按钮。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**图 14**： 启用调试 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image28.png))


默认情况下，返回索引视图。 此视图将列出所有联系人数据库表中的数据 （请参阅图 15）。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**图 15**: 索引视图 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image30.png))


请注意索引视图包括带有标签视图底部创建新的链接。 在下一部分中，你将了解如何创建新的联系人。

## <a name="creating-new-contacts"></a>创建新的联系人

若要使用户能够创建新的联系人，我们需要将两个 create （） 操作添加到 Home 控制器。 我们需要创建一个返回用于创建新联系人的 HTML 窗体的 create （） 操作。 我们需要创建执行实际的数据库插入新联系人的第二个 create （） 操作。

我们需要将添加到 Home 控制器的新 create （） 方法包含在清单 4。

**列出 4-Controllers\HomeController.cs （使用创建方法）**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

可以仅由 HTTP POST 调用第二个 create （） 方法时，可以使用 HTTP GET 调用第一个 create （） 方法。 换而言之，仅当发布 HTML 窗体时，可以调用第二个 create （） 方法。 第一种 create （） 方法只返回一个包含用于创建新联系人的 HTML 窗体视图。 第二个 create （） 方法是更有意义： 它向数据库添加新联系人。

请注意，第二个 create （） 方法已被修改以接受联系人类的实例。 从 HTML 窗体发布窗体值绑定到此联系人类由 ASP.NET MVC 框架自动。 从 HTML 创建窗体的每个窗体字段分配给联系人参数的属性。

请注意，使用 [绑定] 特性修饰的联系人参数。 [绑定] 属性用于从绑定中排除的联系人 Id 属性。 由于 Id 属性表示标识属性，我们不 t 想要设置的 Id 属性。

在 create （） 方法的正文中，实体框架用于向数据库中插入新的联系人。 新的联系人添加到联系人的现有设置和 savechanges （） 方法调用以将这些更改推送回基础数据库。

你可以生成一个 HTML 表单，通过右键单击任一两个 create （） 方法并选择菜单选项创建新的联系人的**添加视图**（请参阅图 16）。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**图 16**： 添加 Create 视图 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image32.png))


在**添加视图**对话框中，选择**ContactManager.Models.Contact**类和**创建**视图内容的选项 （请参阅图 17）。 当你单击**添加**按钮，自动生成的视图创建。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**图 17**： 看到分解的页 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image34.png))


创建视图包含联系人类的属性的每个窗体字段。 创建视图的代码包括在列出 5。

**列出 5-Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

修改 create （） 方法并添加创建视图后，你可以运行联系人管理器应用程序并创建新的联系人。 单击**新建**显示在索引视图，以便导航到创建视图的链接。 你应看到图 18 中的视图。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**图 18**: Create View ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image36.png))


## <a name="editing-contacts"></a>编辑联系人

添加编辑联系人记录的功能是添加用于创建新的联系人记录功能非常相似。 首先，我们需要将两个新的编辑方法添加到 Home 控制器类。 这些新 edit （） 方法包含在列出 6。

**列出 6-Controllers\HomeController.cs （与编辑方法）**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

第一种 edit （） 方法调用一个 HTTP GET 操作。 Id 参数传递给此方法，它表示正在编辑的联系人记录的 Id。 实体框架将用于检索联系人相匹配的 id。返回一个包含用于编辑记录 HTML 窗体视图。

第二个 edit （） 方法来执行实际更新到数据库。 此方法作为参数接受联系人类的实例。 ASP.NET MVC framework 窗体字段从编辑窗体与此类将自动绑定。 请注意不要 t [绑定] 属性包括在编辑的联系人 （我们需要 Id 属性的值） 时。

实体框架用于将已修改的联系人保存到数据库。 必须先从数据库检索原始的联系人。 接下来，实体框架 ApplyPropertyChanges() 方法调用可记录对联系人的更改。 最后，调用实体框架 savechanges （） 方法以持久保存到基础数据库更改。

你可以生成包含编辑窗体通过右键单击 edit （） 方法并选择添加视图菜单选项的视图。 在添加视图对话框中，选择**ContactManager.Models.Contact**类和**编辑**查看内容 （请参阅图 19）。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**图 19**： 添加一个编辑视图 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image38.png))


单击添加按钮时，自动生成新的编辑视图。 生成的 HTML 窗体包含字段对应于每个联系人类 （请参阅列出 7） 的属性。

**列出 7-Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>删除联系人

如果你想要删除联系人，则需要将两个 delete （） 操作添加到 Home 控制器类。 第一个 delete （） 操作显示一个删除确认窗体。 第二个 delete （） 操作执行实际的删除。

> [!NOTE] 
> 
> 更高版本，在迭代 #7 中，我们修改联系人管理器以便它支持 Ajax 删除一个步骤。


列出 8 中包含两个新的 delete （） 方法。

**列出 8-Controllers\HomeController.cs （删除方法）**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

第一个 delete （） 方法返回用于从数据库中删除联系人记录的确认窗体 （请参阅 Figure20）。 第二个 delete （） 方法执行对数据库的实际删除操作。 从数据库中检索原始联系人后，在调用的实体框架 DeleteObject() 和 savechanges （） 方法来执行数据库删除。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**图 20**： 删除确认视图 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image40.png))


我们需要修改索引视图，以使其包含一个链接，以便删除 （请参阅图 21） 的联系人记录。 你需要将以下代码添加到同一个表包含的单元格的编辑链接：

次 Html.ActionLink ({id = 项。Id}) %&gt;


[![新项目对话框](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**图 21**： 索引视图编辑链接 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image42.png))


接下来，我们需要创建的删除确认视图。 右键单击主页控制器类中的 delete （） 方法并选择添加视图菜单选项。 （请参阅图 22） 时出现添加视图对话框。

与不同的列表、 创建和编辑视图中，对于添加视图对话框中不包含用于创建删除视图的选项。 相反，选择**ContactManager.Models.Contact**数据类和**空**查看内容。 选择空视图内容的选项将需要我们自行创建视图。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**图 22**： 添加的删除确认视图 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image44.png))


删除视图的内容包含在列出 9。 此视图包含窗体，其中确认是否特定联系人应删除 （请参阅图 21）。

**列出 9-Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>更改默认控制器的名称

它可能会打扰您用于使用联系人我们控制器类的名称名为 HomeController 类。 不应 t 控制器命名为 ContactController？

此问题问题很容易修复。 首先，我们需要重构 Home 控制器的名称。 在 Visual Studio 代码编辑器中打开 HomeController 类、 右键单击类的名称，然后选择菜单选项**重构重命名**。 选择此菜单选项将打开重命名对话框。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**图 23**： 重构的控制器名称 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image46.png))


[![新项目对话框](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**图 24**： 使用重命名对话框 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image48.png))


如果重命名控制器类时，Visual Studio 将更新视图文件夹中的文件夹的名称。 Visual Studio 将到 \Views\Contact 文件夹中重命名 \Views\Home 文件夹。

进行此更改后，你的应用程序将不再有一个主控制器。 运行你的应用程序时，你将获得图 25 的错误页。


[![新项目对话框](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**图 25**： 无默认控制器 ([单击以查看实际尺寸的图像](iteration-1-create-the-application-cs/_static/image50.png))


我们需要更新要使用而不是主控制器的联系控制器的 Global.asax 文件中的默认路由。 打开 Global.asax 文件并修改默认路由 （请参阅列出 10） 使用的默认控制器。

**列出 10-Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

进行这些更改之后，联系人管理器将正确运行。 现在，它将使用作为默认控制器的联系控制器类。

## <a name="summary"></a>摘要

在此第一次迭代中，我们创建一个基本的联系人管理器应用程序中的最快方法可能。 我们利用 Visual Studio 自动生成我们控制器和视图的初始代码。 我们还利用实体框架可以自动生成我们数据库模型类。

目前，我们可以列表、 创建、 编辑和删除与联系人管理器应用程序的联系人记录。 换而言之，我们可以执行所有数据库驱动的 web 应用程序所需的基本数据库操作。

遗憾的是，我们的应用程序具有一些问题。 第一个，我不愿意再不得不承认，联系人管理器应用程序不是最具吸引力的应用程序。 它需要进行一些设计工作。 在下一个迭代中，我们将了解如何更改的默认视图母版页和级联样式表，以提高应用程序的外观。

其次，我们已不实现任何窗体验证。 例如，没有任何可以阻止你提交创建联系人窗体而无需任何窗体字段中输入值。 此外，你可以输入无效的电话号码和电子邮件地址。 我们首先为处理独特窗体验证迭代 #3 中的问题。

最后，并且最重要的是，联系人管理器应用程序的当前迭代无法轻松地修改或维护。 例如，数据库访问逻辑被融入右控制器操作。 这意味着，我们无法修改数据访问代码，而无需修改我们控制器。 在更高版本迭代中，我们将探讨我们可以实现以更具弹性，若要更改联系人管理器的软件设计模式。

>[!div class="step-by-step"]
[下一篇](iteration-2-make-the-application-look-nice-cs.md)
