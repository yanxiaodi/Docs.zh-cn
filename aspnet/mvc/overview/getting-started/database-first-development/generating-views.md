---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: "首先使用 ASP.NET MVC 的 EF 数据库： 生成视图 |Microsoft 文档"
author: tfitzmac
description: "使用 MVC、 实体框架和 ASP.NET 基架，可以创建的 web 应用程序提供了一个接口到现有数据库。 此教程系列..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 5fccb3c56af0945ec448becff777a3e92dc160d7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>首先使用 ASP.NET MVC 的 EF 数据库： 生成视图
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 实体框架和 ASP.NET 基架，可以创建的 web 应用程序提供了一个接口到现有数据库。 本系列教程演示了如何自动生成代码，使用户能够显示、 编辑、 创建和删除驻留在数据库表中的数据。 生成的代码对应于数据库表中的列。
> 
> 此系列的一部分的重点介绍使用 ASP.NET 基架以生成控制器和视图。


## <a name="add-scaffold"></a>添加基架

你就可以生成将提供的模型类的标准数据操作的代码。 通过添加基架项添加代码。 有许多选项可以添加; 基架的类型在本教程中，基架将包括一个控制器和对应于你在上一节中创建了学生和注册模型的视图。

若要维护你的项目中的一致性，您将新控制器添加到现有**控制器**文件夹。 右键单击**控制器**文件夹，并选择**添加**–**新建基架项**。

![添加基架](generating-views/_static/image1.png)

选择**数据与视图，MVC 5 控制器使用 Entity Framework**选项。 此选项将生成控制器和视图更新、 删除、 创建和显示您的模型中的数据。

![添加 mvc 控制器](generating-views/_static/image2.png)

选择**学生**模型类，然后选择**ContosoUniversityEntities**上下文类。 保留作为控制器名称**StudentsController**，

![指定控制器](generating-views/_static/image3.png)

单击 **“添加”**。

如果你收到错误，则可能是因为未生成上一节中的项目。 如果是这样，请尝试生成项目，然后再添加基架的项。

此代码生成过程完成后，你将看到你项目中的新控制器和视图。

![显示视图](generating-views/_static/image4.png)

同样，执行相同的步骤，但添加基架注册类。 完成后，你应具有**EnrollmentsController.cs**文件和文件夹在**视图**名为**注册**与创建、 删除、 详细信息、 编辑和索引视图。

![显示视图](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>将链接添加到新视图

若要使你更轻松地导航到你新视图，可以在索引视图中添加几个超链接，学生和注册。 处打开此文件**Views/Home/Index.cshtml**，这是你的站点的主页。 添加 jumbotron 下面的代码。

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

对于 ActionLink 方法中，第一个参数是要显示的链接中的文本。 第二个参数是操作，第三个参数是控制器的名称。 例如，第一个链接将指向 StudentsController 中的索引操作。 实际的超链接将从这些值构造。 第一个链接最终将用户导航到**Index.cshtml**文件内**视图/学生**文件夹。

## <a name="display-student-views"></a>显示学生视图

你将验证正确添加到你的项目的代码显示学生的列表，并且使用户能够编辑、 创建或删除数据库中的学生记录。

右键单击**Views/Home/Index.cshtml**文件中，并选择**用浏览器查看**。 在此页上，单击链接获取学生列表。

![](generating-views/_static/image6.png)

在此页上，请注意若要修改此数据的学生和链接的列表。

![学生列表](generating-views/_static/image7.png)

单击**新建**链接并为新学生提供一些值。

![创建新的学生](generating-views/_static/image8.png)

单击**创建**，并留意新的学生添加到列表。

![与新的名学生的列表](generating-views/_static/image9.png)

选择**编辑**链接，并将某些值更改为一名学生。

![编辑学生](generating-views/_static/image10.png)

单击**保存**，并注意学生记录已更改。

最后，选择**删除**链接并确认你想要删除的记录，通过单击**删除**按钮。

![删除学生](generating-views/_static/image11.png)

无需编写任何代码，你已经添加对数据的常见操作执行学生表中的视图。

你可能已经注意到字段的文本标签，基于数据库属性 (如**LastName**) 这不一定是你想要在网页上显示。 例如，您可能想要的标签**姓氏**。 你将在本教程的更高版本中修复此显示问题。

## <a name="display-enrollment-views"></a>显示注册视图

你的数据库包括学生和注册表中和课程和注册表之间的一个对多关系之间的一个对多关系。 注册的视图正确处理这些关系。 导航到站点并选择主页**列表注册**链接，然后**新建**链接。 该视图显示用于创建新的注册记录的表单。 具体而言，请注意该窗体包含两个相关表中的值填充的下拉列表。

![创建注册](generating-views/_static/image12.png)

此外，验证提供的值将自动应用基于字段对数据类型。 年级需要一个数字，因此如果你尝试提供不兼容的值显示错误消息。

![验证消息](generating-views/_static/image13.png)

你已验证的自动生成的视图，使用户能够处理数据库中的数据。 在本系列中下一步教程中，将更新该数据库，并在 web 应用程序中进行相应更改。

>[!div class="step-by-step"]
[上一页](creating-the-web-application.md)
[下一页](changing-the-database.md)
