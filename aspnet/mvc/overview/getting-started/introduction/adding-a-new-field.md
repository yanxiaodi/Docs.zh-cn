---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: "添加新字段 |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 453fbf68aa2f3a1d9ea708355c06c53d4f1eabd0
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
<a name="adding-a-new-field"></a>添加新字段
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

在本部分中，你将使用 Entity Framework Code First 迁移来迁移到的模型类的一些更改，因此更改应用到数据库。

默认情况下，当你使用 Entity Framework Code First 自动创建的数据库，就像此前在本教程中，Code First 表向数据库添加来帮助跟踪数据库的架构是否与从生成的模型类同步。 如果它们不同步，实体框架将引发错误。 这使得更轻松地在开发期间，你可能会否则仅发现 （通过晦涩的错误） 在运行时跟踪问题。

## <a name="setting-up-code-first-migrations-for-model-changes"></a>设置用于模型更改的 Code First 迁移

导航到解决方案资源管理器。 右键单击*Movies.mdf*文件，然后选择**删除**删除电影数据库。 如果看不到*Movies.mdf*文件中，单击**显示所有文件**下面显示为红色轮廓的图标。

![](adding-a-new-field/_static/image1.png)

生成应用程序以确保没有任何错误。

从**工具**菜单上，单击**NuGet 包管理器**然后**程序包管理器控制台**。

![添加包 Man](adding-a-new-field/_static/image2.png)

在**程序包管理器控制台**窗口`PM>`提示输入

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

**Enable-migrations** （如上所示） 的命令创建*Configuration.cs*在新的文件*迁移*文件夹。

![](adding-a-new-field/_static/image4.png)

Visual Studio 将打开*Configuration.cs*文件。 替换`Seed`中的方法*Configuration.cs*文件替换为以下代码：

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

将鼠标悬停在下红色的波浪线`Movie`单击`Show Potential Fixes`，然后单击**使用** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

这样将添加以下 using 语句：

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE] 
> 
> 代码优先迁移调用`Seed`每个迁移后的方法 (即，调用**更新数据库**在 Package Manager Console)，和此方法更新行，具有已插入，或将其插入如果它们尚不存在。
> 
> [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)下面的代码中的方法执行的"upsert"操作：
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> 因为[种子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法运行与每个迁移时，不能只是插入数据，因为你尝试添加的行已将存在后创建数据库的初始迁移。 "[Upsert](http://en.wikipedia.org/wiki/Upsert)"操作可以防止如果你尝试插入行已存在，会发生的错误，但它将重写任何测试应用程序时可能已做的数据更改。 使用某些表中的测试数据您可能不希望这种情况： 在某些情况下在测试期间更改数据时所需更改后数据库更新保留。 在这种情况下要执行条件的插入操作： 仅当它尚不存在插入行。   
>   
> 第一个参数传递给[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法指定要用于检查行是否已存在的属性。 为你提供的，测试影片数据`Title`属性可以用于此目的，由于每个列表中的标题都是唯一：
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> 此代码假定： 标题都是唯一的。 如果你手动添加重复的标题，则会将得到以下异常下次执行迁移。   
>   
>  *序列包含多个元素*  
>   
> 有关详细信息[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法，请参阅[负责使用 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**按 CTRL-SHIFT-B 生成项目。**（如果不在此时生成以下步骤即会成功。）

下一步是创建`DbMigration`类初始迁移。 这种迁移创建一个新的数据库，这就是为什么你删除*movie.mdf*上一步中的文件。

在**程序包管理器控制台**窗口中，输入命令`add-migration Initial`创建初始迁移。 "初始"的名称是任意参数并用于命名创建的迁移文件。

![](adding-a-new-field/_static/image6.png)

Code First 迁移创建中的另一个类文件*迁移*文件夹 (同名*{日期时间戳}\_Initial.cs* )，并且此类包含创建数据库架构的代码。 迁移 filename 是预先固定的使用时间戳来帮助进行排序。 检查*{日期时间戳}\_Initial.cs*文件，它包含的说明创建`Movies`电影 DB 的表。 下面，这说明中的数据库的更新时*{日期时间戳}\_Initial.cs*文件将运行并创建数据库架构。 则**种子**方法将运行，以填充使用测试数据的数据库。

在**程序包管理器控制台**，输入命令`update-database`若要创建数据库并运行`Seed`方法。

![](adding-a-new-field/_static/image7.png)

如果你收到的错误消息指示表已存在，无法创建，则可能是因为你运行应用程序删除了数据库后，在你执行之前`update-database`。 在这种情况下，删除*Movies.mdf*再次文件，然后重试`update-database`命令。 如果仍然收到错误，删除 migrations 文件夹及其内容，则使用此页顶部的说明启动 (即删除*Movies.mdf*文件，然后转到 Enable-migrations)。 如果仍然收到项错误而失败，打开 SQL Server 对象资源管理器，并从列表中删除数据库。

运行应用程序并导航到*/Movies* URL。 将显示种子数据。

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>向电影模型添加分级属性

添加一个新的启动`Rating`到现有的属性`Movie`类。 打开*Models\Movie.cs*文件并添加`Rating`属性如下所示：

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

完整`Movie`类现在如下所示的以下代码：

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

生成应用程序 (Ctrl + Shift + B)。

因为你已将新字段添加到`Movie`类，你还需要更新绑定*白名单*以便将包含此新属性。 更新`bind`属性，则为`Create`和`Edit`操作方法，以包含`Rating`属性：

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

还需要更新视图模板，以便在浏览器视图中显示、创建和编辑新的 `Rating` 属性。

打开*\Views\Movies\Index.cshtml*文件并添加`<th>Rating</th>`列标题之后**价格**列。 然后添加`<td>`结尾处的模板来呈现的列`@item.Rating`值。 下面是哪些更新*Index.cshtml*视图模板如下所示：

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

接下来，打开*\Views\Movies\Create.cshtml*文件并添加`Rating`替换为以下 highlighed 标记字段。 这会呈现一个文本框，以便创建新的影片时，可以指定的分级。

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

你现在已更新应用程序代码以支持新`Rating`属性。

运行应用程序并导航到*/Movies* URL。 执行此操作，不过，你将看到以下错误之一：

![](adding-a-new-field/_static/image9.png)  
  
数据库创建后，备份 MovieDBContext 上下文的模型已更改。 请考虑使用 Code First 迁移更新数据库 (https://go.microsoft.com/fwlink/?LinkId=238269)。

![](adding-a-new-field/_static/image10.png)

你看到此错误，因为更新`Movie`应用程序中的模型类现在是不同的架构`Movie`现有数据库的表。 （数据库表中没有 `Rating` 列。）


可通过几种方法解决此错误：

1. 让 Entity Framework 自动丢弃，并基于新的模型类架构重新创建数据库。 在测试数据库上进行开发时，此方法在开发周期早期很方便；通过它可以一起快速改进模型和数据库架构。 缺点，不过，是，则会失去在数据库中的现有数据，以便你*不*想要在生产数据库上使用此方法 ！ 使用初始值设定项，以使用测试数据自动设定数据库种子，这通常是开发应用程序的有效方式。 有关实体框架数据库初始值设定项的详细信息，请参阅[ASP.NET MVC/实体框架教程](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
2. 对现有数据库架构进行显式修改，使它与模型类相匹配。 此方法的优点是可以保留数据。 可以手动或通过创建数据库更改脚本进行此更改。
3. 使用 Code First 迁移更新数据库架构。


本教程使用 Code First 迁移。

更新的 Seed 方法，使其提供了值的新列。 打开 Migrations\Configuration.cs 文件并将分级字段添加到每个电影对象。

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

生成解决方案，，然后打开**程序包管理器控制台**窗口并输入以下命令：

`add-migration Rating`

`add-migration`命令指示迁移框架在检查当前的电影模型与当前的电影 DB 架构并创建必要的代码以将数据库迁移到新的模型。 名称*评级*是任意参数并用于命名迁移文件。 是很有帮助，若要使用的迁移步骤有意义的名称。

此命令完成时，Visual Studio 将打开定义新的类文件`DbMigration`派生类，然后在`Up`方法你可以看到创建的新列的代码。

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

生成解决方案，，然后输入`update-database`命令**程序包管理器控制台**窗口。

下图显示中的输出**程序包管理器控制台**窗口 (日期戳预先计算*评级*将不同。)

![](adding-a-new-field/_static/image11.png)

重新运行该应用程序并导航到 /Movies URL。 您可以看到新的分级字段。

![](adding-a-new-field/_static/image12.png)

单击**新建**链接添加新的影片。 请注意，你可以添加一个级别。

![7_CreateRioII](adding-a-new-field/_static/image13.png)

单击 **“创建”**。 新的影片，包括评级，现在显示在电影列出：

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

现在，项目使用迁移，不需要以添加新字段或更新的架构，否则时删除数据库。 在下一步的部分中，我们将进行更多的架构更改，并使用迁移来更新数据库。

此外应添加`Rating`编辑、 详细信息，并删除视图模板字段。

您可以输入中的"更新数据库"命令**程序包管理器控制台**再次窗口和任何迁移代码将运行，因为架构与匹配的模型。 但是，运行"更新数据库"将运行`Seed`方法再次，并且如果你更改任何种子数据，所做的更改将会丢失，因为`Seed`方法 upserts 数据。 你可以阅读更多有关`Seed`方法在 Tom Dykstra 的常用[ASP.NET MVC/实体框架教程](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

在本部分中您将了解如何修改模型对象和保留数据库与更改同步。 你还了解了一种方法来填充新创建的数据库使用示例数据，因此你还可以尝试方案。 这是只需向 Code First 的快速介绍，请参阅[为 ASP.NET MVC 应用程序创建实体框架数据模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)有关主题的更完整教程。 接下来，让我们看一下如何将更丰富的验证逻辑添加到模型类和启用一些业务规则，以强制执行。

>[!div class="step-by-step"]
[上一页](adding-search.md)
[下一页](adding-validation.md)
