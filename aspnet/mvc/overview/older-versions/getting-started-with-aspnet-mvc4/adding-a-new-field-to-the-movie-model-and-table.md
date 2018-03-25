---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: "将新字段添加到的电影模型和表 |Microsoft 文档"
author: Rick-Anderson
description: "注意： 本教程的更新的版本此处提供了使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照和演示要简单得多..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 9965c8a755857a8e8cb8ecbc6c467a6c856aa83d
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>将新字段添加到的电影模型和表
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教程的更新的版本可[此处](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照要简单得多，并演示更多的功能。


在本部分中，你将使用 Entity Framework Code First 迁移来迁移到的模型类的一些更改，因此更改应用到数据库。

默认情况下，当你使用 Entity Framework Code First 自动创建的数据库，就像此前在本教程中，Code First 表向数据库添加来帮助跟踪数据库的架构是否与从生成的模型类同步。 如果它们不同步，实体框架将引发错误。 这使得更轻松地在开发期间，你可能会否则仅发现 （通过晦涩的错误） 在运行时跟踪问题。

## <a name="setting-up-code-first-migrations-for-model-changes"></a>设置用于模型更改的 Code First 迁移

如果你正在使用 Visual Studio 2012，双击*Movies.mdf*从解决方案资源管理器打开数据库工具的文件。 Visual Studio Express for Web 将显示数据库资源管理器，Visual Studio 2012 将显示服务器资源管理器。 如果你使用的 Visual Studio 2010，使用 SQL Server 对象资源管理器。

在数据库工具 （数据库资源管理器、 服务器资源管理器或 SQL Server 对象资源管理器） 中，右键单击`MovieDBContext`和选择**删除**若要删除电影数据库。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

向后定位到解决方案资源管理器。 右键单击*Movies.mdf*文件，然后选择**删除**删除电影数据库。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

生成应用程序以确保没有任何错误。

从**工具**菜单上，单击**库程序包管理器**然后**程序包管理器控制台**。

![添加包 Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

在**程序包管理器控制台**窗口`PM>`提示符处输入"Enable-migrations-ContextTypeName MvcMovie.Models.MovieDBContext"。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Enable-migrations** （如上所示） 的命令创建*Configuration.cs*在新的文件*迁移*文件夹。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio 将打开*Configuration.cs*文件。 替换`Seed`中的方法*Configuration.cs*文件替换为以下代码：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

右键单击红色波浪线`Movie`和选择**解决**然后**使用** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

这样将添加以下 using 语句：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> 代码优先迁移调用`Seed`每个迁移后的方法 (即，调用**更新数据库**在 Package Manager Console)，和此方法更新行，具有已插入，或将其插入如果它们尚不存在。


**按 CTRL-SHIFT-B 生成项目。**(以下步骤将失败，如果你不在此时生成。)

下一步是创建`DbMigration`类初始迁移。 此迁移到创建一个新的数据库，这就是为什么你删除*movie.mdf*上一步中的文件。

在**程序包管理器控制台**窗口中，输入命令"Initial"以创建初始迁移。 "初始"的名称是任意参数并用于命名创建的迁移文件。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First 迁移创建中的另一个类文件*迁移*文件夹 (同名*{日期时间戳}\_Initial.cs* )，并且此类包含创建数据库架构的代码。 迁移 filename 是预先固定的使用时间戳来帮助进行排序。 检查*{日期时间戳}\_Initial.cs*文件，它包含的说明进行操作，对于影片 DB 创建电影表。 下面，这说明中的数据库的更新时*{日期时间戳}\_Initial.cs*文件将运行并创建数据库架构。 则**种子**方法将运行，以填充使用测试数据的数据库。

在**程序包管理器控制台**，输入命令"更新数据库"创建数据库并运行**种子**方法。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

如果你收到的错误消息指示表已存在，无法创建，则可能是因为你运行应用程序删除了数据库后，在你执行之前`update-database`。 在这种情况下，删除*Movies.mdf*再次文件，然后重试`update-database`命令。 如果仍然收到错误，删除 migrations 文件夹及其内容，则使用此页顶部的说明启动 (即删除*Movies.mdf*文件，然后转到 Enable-migrations)。

运行应用程序并导航到*/Movies* URL。 将显示种子数据。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>向电影模型添加分级属性

添加一个新的启动`Rating`到现有的属性`Movie`类。 打开*Models\Movie.cs*文件并添加`Rating`属性如下所示：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

完整`Movie`类现在如下所示的以下代码：

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

生成应用程序使用**生成** &gt;**生成电影**菜单命令或通过按 CTRL SHIFT。

现在，你已更新`Model`类，你还需要更新*\Views\Movies\Index.cshtml*和*\Views\Movies\Create.cshtml*查看模板，以显示新`Rating`在浏览器视图的属性。

打开*\Views\Movies\Index.cshtml*文件并添加`<th>Rating</th>`列标题之后**价格**列。 然后添加`<td>`结尾处的模板来呈现的列`@item.Rating`值。 下面是哪些更新*Index.cshtml*视图模板如下所示：

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

接下来，打开*\Views\Movies\Create.cshtml*文件并添加以下标记窗体末尾附近。 这会呈现一个文本框，以便创建新的影片时，可以指定的分级。

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

你现在已更新应用程序代码以支持新`Rating`属性。

现在运行应用程序并导航到*/Movies* URL。 执行此操作，不过，你将看到以下错误之一：

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

你看到此错误，因为更新`Movie`应用程序中的模型类现在是不同的架构`Movie`现有数据库的表。 （数据库表中没有 `Rating` 列。）


可通过几种方法解决此错误：

1. 让 Entity Framework 自动丢弃，并基于新的模型类架构重新创建数据库。 这种方法是非常方便时进行活动开发上测试数据库;它允许您快速发展的模型和数据库架构在一起。 缺点，不过，是，则会失去在数据库中的现有数据，以便你*不*想要在生产数据库上使用此方法 ！ 使用初始值设定项自动种子设定具有测试数据的数据库通常是开发应用程序的高效方法。 有关实体框架数据库初始值设定项的详细信息，请参阅 Tom Dykstra 的[ASP.NET MVC/实体框架教程](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
2. 对现有数据库架构进行显式修改，使它与模型类相匹配。 此方法的优点是可以保留数据。 可以手动或通过创建数据库更改脚本进行此更改。
3. 使用 Code First 迁移更新数据库架构。


本教程使用 Code First 迁移。

更新的 Seed 方法，使其提供了值的新列。 打开 Migrations\Configuration.cs 文件并将分级字段添加到每个电影对象。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

生成解决方案，，然后打开**程序包管理器控制台**窗口并输入以下命令：

`add-migration AddRatingMig`

`add-migration`命令指示迁移框架在检查当前的电影模型与当前的电影 DB 架构并创建必要的代码以将数据库迁移到新的模型。 AddRatingMig 是任意参数并用于命名迁移文件。 是很有帮助，若要使用的迁移步骤有意义的名称。

此命令完成时，Visual Studio 将打开定义新的类文件`DbMIgration`派生类，然后在`Up`方法你可以看到创建的新列的代码。

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

生成解决方案，，然后输入中的"更新数据库"命令**程序包管理器控制台**窗口。

下图显示中的输出**程序包管理器控制台**窗口 （预先计算 AddRatingMig 日期戳将不同。）

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

重新运行该应用程序并导航到 /Movies URL。 您可以看到新的分级字段。

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

单击**新建**链接添加新的影片。 请注意，你可以添加一个级别。

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

单击 **“创建”**。 新的影片，包括评级，现在显示在电影列出：

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

此外应添加`Rating`字段编辑、 详细信息和 SearchIndex 查看模板。

您可以输入中的"更新数据库"命令**程序包管理器控制台**再次窗口以及任何更改将进行，因为架构与匹配的模型。

在本部分中您将了解如何修改模型对象和保留数据库与更改同步。 你还了解了一种方法来填充新创建的数据库使用示例数据，因此你还可以尝试方案。 接下来，让我们看一下如何将更丰富的验证逻辑添加到模型类和启用一些业务规则，以强制执行。

>[!div class="step-by-step"]
[上一页](examining-the-edit-methods-and-edit-view.md)
[下一页](adding-validation-to-the-model.md)
