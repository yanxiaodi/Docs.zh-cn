---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: "创建数据库 |Microsoft 文档"
author: shanselman
description: "这是初学者本教程介绍 ASP.NET MVC 的基础知识。 创建一个简单的 web 应用程序读取和写入数据库中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 22f1042555d7cdd0ca8d75f1cbde65fbb65d81cc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="creating-a-database"></a>创建数据库
====================
通过[Scott Hanselman](https://github.com/shanselman)

> 这是初学者本教程介绍 ASP.NET MVC 的基础知识。 将创建一个简单的 web 应用程序读取和写入数据库中。 请访问[ASP.NET MVC 学习中心](../../../index.md)若要查找其他 ASP.NET MVC 教程和示例。


在本部分中我们将创建我们将使用它来存储和检索我们影片数据的新 SQL Express 数据库。 从 Visual Web Developer IDE 中，选择视图 |服务器资源管理器。 右键单击数据连接，然后单击添加连接...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

在选择数据源对话框中，选择 Microsoft SQL Server，然后选择继续。

![](getting-started-with-mvc-part4/_static/image2.png)

在添加连接对话框中，输入"。 \SQLEXPRESS"为你的服务器名称，并输入"电影"作为新数据库的名称。

[![添加连接对话框](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

单击确定，并将你想要创建该数据库要求你。 选择是。

[![创建电影？](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

现在你已在服务器资源管理器中生成空数据库。

![添加新表](getting-started-with-mvc-part4/_static/image7.png)

右键单击表，然后单击添加表。 表设计器将显示。 为 Id、 标题、 ReleaseDate、 Genre 和价格添加列。 右键单击 ID 列，然后单击设置为主键。 下面是哪些我设计方面的问题如下所示。

[![数据库表编辑器](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

此外，选择 Id 列，并且在下面的列属性下更改"标识规范"为"是"。

[![IsIdentity 的列属性](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

获得它完成后，单击工具栏中的保存图标，或选择文件 |从菜单中，保存并命名你的表"**电影**"（采用单数形式）。 我们已经有了数据库和表 ！

[![选择名称](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

返回到服务器资源管理器并右键单击电影表，然后选择"显示表数据"。 输入少量的电影，因此我们的数据库都有一些数据。

[![数据库表编辑](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>创建模型

现在，切换回 IDE 右侧的解决方案资源管理器并右键单击 Models 文件夹然后选择添加 |新项。

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

我们将创建我们新数据库中的实体模型。 这将向我们轻松地让我们来查询和操作我们的数据库内的数据的项目中添加一组类。 选择对话框中，左侧的数据节点，然后选择 ADO.NET 实体数据模型项模板。 将其命名 Movies.edmx。

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

单击"添加"按钮。 然后，这将启动"实体数据模型向导"。

在新建对话框弹出，从数据库中选择生成。 由于我们只需已进行了数据库，我们将仅需要告诉我们新的数据库和它的表的实体框架。 保存我们在我们的 web 应用程序的配置中的数据库连接，单击下一步。 现在，检查表和电影复选框，然后单击完成。

[![实体数据模型向导](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

现在，我们可以看到我们新的影片表中实体框架设计器，并从代码访问它。

[![电影-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

在设计图面上，你可以看到"电影"类。 此类将映射到我们的数据库，在"电影"表和其中的每个属性将映射到表的列。 "电影"类的每个实例将对应于"电影"表中的行。

如果你不喜欢的默认命名和映射实体框架使用的约定，你可以使用实体框架设计器更改或自定义它们。 为此应用程序中，我们将使用默认值和只需将文件另存的是。

现在，让我们使用一些实际数据 ！

>[!div class="step-by-step"]
[上一页](getting-started-with-mvc-part3.md)
[下一页](getting-started-with-mvc-part5.md)
