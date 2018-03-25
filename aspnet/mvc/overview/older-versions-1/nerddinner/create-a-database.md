---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: "创建数据库 |Microsoft 文档"
author: microsoft
description: "步骤 2 显示的步骤来创建包含所有 dinner 数据库 RSVP 我们 NerdDinner 应用程序的数据。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 7635722fc357356edd06fb4cff301a8c4dfebbef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="create-a-database"></a>创建数据库
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是一个免费的第 2 步["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 步骤 2 显示的步骤来创建包含所有 dinner 数据库 RSVP 我们 NerdDinner 应用程序的数据。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner 步骤 2： 创建数据库

我们将使用数据库来存储所有的 Dinner 和回复数据我们 NerdDinner 为应用程序。

以下步骤说明了创建使用免费的 SQL Server Express 版的数据库 (你可以轻松地安装使用的 V2 [Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx))。 我们将编写的代码的所有适用于 SQL Server Express 和完整的 SQL Server。

### <a name="creating-a-new-sql-server-express-database"></a>创建新的 SQL Server Express 数据库

我们将开始通过我们的 web 项目，右键单击，然后选择**添加-&gt;新项**菜单命令：

![](create-a-database/_static/image1.png)

这将显示 Visual Studio 的"添加新项"对话框。 我们将按"数据"类别筛选，并选择"SQL Server 数据库"项模板：

![](create-a-database/_static/image2.png)

我们将命名我们想要创建"NerdDinner.mdf"和确定命中的 SQL Server Express 的数据库。 Visual Studio 将然后问是否我们想要将此文件添加到我们 \App\_数据目录 （即一个目录已设置设置为这两个读取和写入安全 Acl）：

![](create-a-database/_static/image3.png)

我们将单击"是"并将创建我们新数据库，并将其添加到我们的解决方案资源管理器：

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>创建我们数据库中的表

我们现在有新的空数据库。 让我们向其添加一些表。

若要这样做我们将导航到 Visual Studio 中，这使我们能够管理数据库和服务器内的"服务器资源管理器"选项卡窗口。 SQL Server Express 数据库存储在 \App\_的我们的应用程序数据文件夹将自动显示在服务器资源管理器。 我们可以选择使用"连接到数据库"图标"服务器资源管理器"窗口的顶部将其他 SQL Server 数据库 （本地和远程） 添加到列表以及：

![](create-a-database/_static/image5.png)

我们将向我们的 NerdDinner 数据库 – 一个以用于存储我们晚餐，另一个跟踪对它们的回复接受添加两个表。 我们可以通过我们的数据库中的"表"文件夹中右键单击并选择"添加新表"菜单命令来创建新表：

![](create-a-database/_static/image6.png)

这将会打开一个表设计器，使我们可以配置我们表的架构。 对于我们"晚餐"表中，我们将添加 10 列的数据：

![](create-a-database/_static/image7.png)

我们想要将表的唯一主键的"DinnerID"列。 我们可以通过右键单击"DinnerID"列并选择"设置主键"菜单项对此进行配置：

![](create-a-database/_static/image8.png)

除了进行 DinnerID 为主键，我们还希望将其配置为向表添加新的数据行时，其值将自动递增"标识"列 （这意味着第插入的 Dinner 一行将包含 1 DinnerID，第二个插入行将具有 DinnerID 为 2，等等)。

我们可以通过选择"DinnerID"列执行此操作，然后使用"列属性"编辑器以将"（是标识）"属性设置为"是"列。 我们将使用标准身份默认值 （从 1 开始和递增 1 上的每个新的 Dinner 行）：

![](create-a-database/_static/image9.png)

我们将通过键入 CTRL-S 或使用保存我们表**文件-&gt;保存**菜单命令。 这将提示我们命名表。 我们将其"晚餐"命名：

![](create-a-database/_static/image10.png)

我们的新晚餐表将显示我们在服务器资源管理器中的数据库中。

我们然后重复上述步骤，创建"回复"表。 带有此表具有 3 列。 我们将设置为主键，RsvpID 列，并还将为标识列：

![](create-a-database/_static/image11.png)

我们将其保存，将其命名为"回复"。

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>设置表之间的外键关系

我们现在有两个表，我们的数据库中。 我们最后一个架构设计步骤将设置这两个表 – 之间的"一到多"关系，以便我们可以将每个 Dinner 行与零个或多个应用于它的回复行相关联。 我们将配置的回复表的"DinnerID"列"晚餐"表中有"DinnerID"列的外键关系来执行此操作。

若要执行此操作我们将通过在服务器资源管理器中双击它来打开表设计器中的回复表。 然后，我们将选择"DinnerID"列中，右键单击，然后选择"Relationshps..." 上下文菜单命令：

![](create-a-database/_static/image12.png)

此时会弹出一个对话框，我们可以使用安装程序表之间的关系：

![](create-a-database/_static/image13.png)

我们将单击"添加"按钮，以向对话框添加新的关系。 一旦添加关系，我们将右侧的对话框中，展开属性网格中的"表和列规范"树视图节点，然后单击右侧的"..."按钮：

![](create-a-database/_static/image14.png)

单击"..."按钮将显示另一个对话框，从中我们需要指定哪些表和列所涉及的关系，以及使我们能够命名关系。

我们将更改为"晚餐"主键表并选择晚餐表中的"DinnerID"列为主键。 我们的回复表将外键表中，并回复。DinnerID 列将作为外键相关联：

![](create-a-database/_static/image15.png)

现在的回复表中的每一行将 Dinner 表中的行与相关联。 SQL Server 将为我们 – 维护引用完整性，并使我们无法添加新的回复行，如果它不指向有效的 Dinner 行。 它还将删除 Dinner 行仍 RSVP 引用它的行是否阻止我们。

### <a name="adding-data-to-our-tables"></a>将数据添加到我们表

让我们通过将一些示例数据添加到我们晚餐表完成。 在服务器资源管理器在其上右键单击并选择"显示表数据"命令，我们可以向表中添加数据：

![](create-a-database/_static/image16.png)

我们将添加几行 Dinner 我们可以在以后当我们开始实现应用程序使用的数据：

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>下一步

我们已经完成了创建我们的数据库。 现在让我们创建我们可用于查询和更新它的模型类。

>[!div class="step-by-step"]
[上一页](create-a-new-aspnet-mvc-project.md)
[下一页](build-a-model-with-business-rule-validations.md)
