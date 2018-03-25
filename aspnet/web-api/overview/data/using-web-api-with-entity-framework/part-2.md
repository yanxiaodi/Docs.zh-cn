---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: "添加模型和控制器 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 7e09316f0faaf0731e4cdda48040fdaedc0f244a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="add-models-and-controllers"></a>添加模型和控制器
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://github.com/MikeWasson/BookService)

在本部分中，你将添加定义数据库实体的模型类。 然后你将添加执行对这些实体的 CRUD 操作的 Web API 控制器。

## <a name="add-model-classes"></a>添加模型类

在本教程中，我们将通过使用"代码优先"方法到 Entity Framework (EF) 创建数据库。 使用 Code First 数据库表中写入对应的 C# 类和 EF 创建数据库。 (有关详细信息，请参阅[实体框架开发方法](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf)。)

我们首先我们域对象定义为 POCOs （纯旧式 CLR 对象）。 我们将创建以下 POCOs:

- 作者
- 书籍

在解决方案资源管理器，右键单击模型文件夹。 选择**添加**，然后选择**类**。 将此类命名为 `Author`。

![](part-2/_static/image1.png)

将所有中 Author.cs 的样板文件代码替换下面的代码。

[!code-csharp[Main](part-2/samples/sample1.cs)]

添加名为的另一个类`Book`，替换为以下代码。

[!code-csharp[Main](part-2/samples/sample2.cs)]

实体框架将使用这些模型创建数据库表。 对于每个模型，`Id`属性将成为数据库表的主键列。

在簿类中，`AuthorId`定义外的键到`Author`表。 （为简单起见，我将假定每本书具有单个作者。）Book 类还包含指向相关的导航属性`Author`。 你可以使用导航属性访问相关`Author`在代码中。 我说第 4，部分中的导航属性有关的详细信息[处理实体关系](part-4.md)。

## <a name="add-web-api-controllers"></a>添加 Web API 控制器

在本部分中，我们将添加 Web API 控制器支持 CRUD 操作 （创建、 读取、 更新和删除）。 控制器将使用实体框架与数据库层进行通信。

首先，你可以删除文件 Controllers/ValuesController.cs。 此文件所包含的示例 Web API 控制器，但你不在本教程需要它。

![](part-2/_static/image2.png)

接下来，生成项目。 Web API 基架使用反射来查找模型类，因此它需要编译的程序集。

在解决方案资源管理器，右键单击 Controllers 文件夹。 选择**添加**，然后选择**控制器**。

![](part-2/_static/image3.png)

在**添加基架**对话框中，选择"Web API 2 控制器其操作使用 Entity Framework"。 单击 **“添加”**。

![](part-2/_static/image4.png)

在**添加控制器**对话框中，执行以下操作：

1. 在**模型类**下拉列表中，选择`Author`类。 （如果你未看到它在下拉列表中列出，请确保生成项目。）
2. 检查"使用异步控制器操作"。
3. 将作为控制器名称&quot;AuthorsController&quot;。
4. 单击加号 （+） 按钮旁边**数据上下文类**。

![](part-2/_static/image5.png)

在**新建数据上下文**对话框中，保留默认名称，然后单击**添加**。

![](part-2/_static/image6.png)

单击**添加**完成**添加控制器**对话框。 该对话框将添加到你的项目的两个类：

- `AuthorsController`定义一个 Web API 控制器。 控制器实现 REST API 客户端用来执行 CRUD 操作列表上的作者。
- `BookServiceContext`在运行期间，其中包括填充对象具有从数据库、 更改跟踪和将数据永久保存到数据库的数据管理实体对象。 它继承自`DbContext`。

![](part-2/_static/image7.png)

此时，重新生成项目。 现在，通过添加为一个 API 控制器的相同步骤转`Book`实体。 此时，选择`Book`模型类，然后选择现有`BookServiceContext`数据上下文类的类。 （不创建新的数据上下文）。单击**添加**将控制器添加。

![](part-2/_static/image8.png)

>[!div class="step-by-step"]
[上一页](part-1.md)
[下一页](part-3.md)
