---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: "首先使用 ASP.NET MVC 的 EF 数据库： 增强数据的有效性 |Microsoft 文档"
author: tfitzmac
description: "使用 MVC、 实体框架和 ASP.NET 基架，可以创建的 web 应用程序提供了一个接口到现有数据库。 此教程系列..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 842496c2d3ec56fb81f2409dd7d05d800f83799b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>首先使用 ASP.NET MVC 的 EF 数据库： 增强数据验证
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 实体框架和 ASP.NET 基架，可以创建的 web 应用程序提供了一个接口到现有数据库。 本系列教程演示了如何自动生成代码，使用户能够显示、 编辑、 创建和删除驻留在数据库表中的数据。 生成的代码对应于数据库表中的列。
> 
> 此系列的一部分的侧重于将数据注释添加到数据模型以指定验证要求并显示格式设置。 它已从注释部分中的用户的反馈以提高基于。


## <a name="add-data-annotations"></a>添加数据批注

正如你看到更早版本的主题中，一些数据的验证规则将自动应用到用户输入。 例如，你可以仅提供大量年级属性。 若要指定多个数据验证规则，你可以将数据批注添加到您的模型类。 这些批注将应用于整个 web 应用程序的指定属性。 你还可以应用更改的属性将显示如何; 的格式设置特性例如，更改用于文本标签的值。

在本教程中，你将添加数据注释来限制为 FirstName、 LastName 和 MiddleName 属性提供的值的长度。 在数据库中，这些值是限制为 50 个字符;但是，web 应用程序中该字符限制当前未实施。 如果用户提供超过 50 个字符，这些值之一，尝试将值保存到数据库时将崩溃页。 你将为介于 0 和 4 之间的值来限制级。

打开**Student.cs**文件中**模型**文件夹。 将以下突出显示的代码添加到类。

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

在 Enrollment.cs，添加以下突出显示的代码。

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

生成解决方案。

浏览到编辑或创建一名学生的页。 如果你尝试输入超过 50 个字符，将显示一条错误消息。

![显示的错误消息](enhancing-data-validation/_static/image1.png)

浏览到页以进行编辑注册，并尝试提供上面 4 级。

![年级范围错误](enhancing-data-validation/_static/image2.png)

可以将应用于属性和类的数据验证注释的完整列表，请参阅[System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)。

## <a name="add-metadata-classes"></a>添加元数据类

你不希望要更改; 的数据库时直接向模型类添加验证特性的工作原理但是，如果你的数据库更改，并且您需要重新生成模型类，则将失去所有已应用于模型的类的属性。 此方法可能会非常低效并且容易导致丢失重要的验证规则。

若要避免此问题，可以添加一个包含属性的元数据类。 将关联到元数据类的模型类，这些特性被应用于模型中。 在此方法中，模型类可以重新生成而不会丢失所有已应用于元数据类的属性。

在**模型**文件夹中，添加一个名为类**Metadata.cs**。

![添加元数据类](enhancing-data-validation/_static/image3.png)

Metadata.cs 中的代码替换为以下代码。

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

这些元数据类包含所有先前应用到的模型类的验证属性。 **显示**属性用于更改用于文本标签的值。

现在，必须将模型类关联的元数据类。

在**模型**文件夹中，添加一个名为类**PartialClasses.cs**。

文件的内容替换为以下代码。

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

请注意，每个类标记为`partial`类，并且每个匹配的名称和命名空间与自动生成的类。 通过将元数据属性应用到该分部类，可以确保数据验证特性，将应用于自动生成类。 当你重新生成模型类，因为元数据属性在应用中不会重新生成的分部类，这些属性不会丢失。

若要重新生成的自动生成的类，请打开 ContosoModel.edmx 文件。 同样，右键单击设计图面并选择**从数据库更新模型**。 即使你未更改数据库，此过程将重新生成类。 在**刷新**选项卡上，选择**表**和**完成**。

![刷新表](enhancing-data-validation/_static/image4.png)

保存 ContosoModel.edmx 文件才能应用更改。

打开 Student.cs 文件或 Enrollment.cs 文件，并请注意您先前应用的数据验证属性将不再在文件中。 但是，运行该应用程序，并且请注意当输入数据时，仍应用验证规则。

>[!div class="step-by-step"]
[上一页](customizing-a-view.md)
[下一页](publish-to-azure.md)
