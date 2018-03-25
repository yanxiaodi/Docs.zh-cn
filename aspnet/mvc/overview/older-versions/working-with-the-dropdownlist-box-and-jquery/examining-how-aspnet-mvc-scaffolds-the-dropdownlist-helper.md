---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: "检查如何 ASP.NET MVC scaffolds DropDownList 帮助器 |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: abd9b5c09e942b966eb3eaaebe1b315c30b8e0c0
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>检查如何 ASP.NET MVC scaffolds DropDownList 帮助器
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

在**解决方案资源管理器**，右键单击*控制器*文件夹，然后选择**添加控制器**。 控制器**StoreManagerController**。 设置选项**添加控制器**下图中所示的对话框。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

编辑*StoreManager\Index.cshtml*查看和删除`AlbumArtUrl`。 删除`AlbumArtUrl`将使演示文稿更具可读性。 完成的代码如下所示。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

打开*Controllers\StoreManagerController.cs*文件并查找`Index`方法。 添加`OrderBy`子句，以便唱片集将按价格进行排序。 完整的代码所示。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

价格按排序将使其可以轻松地测试对数据库的更改。 当你要测试的编辑，并创建方法时，可以使用较低成本，因此将第一个显示保存的数据。

打开*StoreManager\Edit.cshtml*文件。 图例标记的后面添加以下行。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

下面的代码演示此更改的上下文：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId`需对唱片集记录进行更改。

按 Ctrl+F5 运行应用程序。 选择**管理员**链接，然后选择**新建**链接创建新唱片集。 验证已保存的唱片集信息。 编辑唱片集并验证你所做的更改将保留。

### <a name="the-album-schema"></a>唱片集架构

`StoreManager`控制器创建的 MVC 基架机制允许专辑 CRUD （创建、 读取、 更新、 删除） 访问音乐存储数据库中。 唱片集信息的架构如下所示：

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums`表不会存储唱片集 genre 和描述，它将存储的外键`Genres`表。 `Genres`表包含的流派名称和描述。 同样，`Albums`表不包含唱片集艺术家名称，但外的键与`Artists`表。 `Artists`表包含艺术家的名称。 如果检查中的数据`Albums`表，你可以看到每一行都包含外的键与`Genres`表和外的键与`Artists`表。 下图显示的某些表数据来自`Albums`表。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>HTML 选择标记

HTML`<select>`元素 (由 HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)帮助程序) 用于显示值 （如风格的列表） 的完整列表。 对于编辑窗体中，当已知的当前值时，选择列表可以显示的当前值。 我们已了解此以前当我们设置为所选的值**喜剧**。 选择列表非常适合于显示类别或外键数据。 `<select>`流派外键的元素显示名称的列表可能流派，但时保存该窗体流派属性将更新为流派外键值，而不是显示的流派名称。 在下图中，选择流派是**Disco**艺术家且**Donna 夏天**。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>检查 ASP.NET MVC 基架代码

打开*Controllers\StoreManagerController.cs*文件并查找`HTTP GET Create`方法。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create`方法添加两个[此时](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx)对象添加到`ViewBag`，另一个用于包含流派信息，另一个用于包含艺术家信息。 [此时](https://msdn.microsoft.com/library/dd505286.aspx)上面使用的构造函数重载采用三个自变量：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *项*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)包含列表中的项。 在上面的示例中，返回的风格列表`db.Genres`。
2. *dataValueField*： 中的属性的名称**IEnumerable**包含密钥的值的列表。 在上例中，`GenreId`和`ArtistId`。
3. *dataTextField*： 中的属性的名称**IEnumerable**列表，其中包含要显示的信息。 在专业人员和风格表`name`使用字段。

打开*Views\StoreManager\Create.cshtml*文件并检查`Html.DropDownList`genre 字段的帮助器标记。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

第一行显示创建视图所`Album`模型。 在`Create`上面所示方法，不传递任何模型，因此视图获取**null** `Album`模型。 此时我们将创建新唱片集，因此我们没有任何`Album`为它的数据。

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)上面所示的重载采用要绑定到模型的字段的名称。 它还使用此名称来查找**ViewBag**对象，其中包含[此时](https://msdn.microsoft.com/library/dd505286.aspx)对象。 使用此重载，将要求你将为名称**ViewBag 此时**对象`GenreId`。 第二个参数 (`String.Empty`) 是要在未不选定任何项时显示的文本。 这正是我们想要创建新唱片集时。 如果你删除第二个参数，并使用下面的代码：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

在我们的示例情况下，选择列表将默认为第一个元素或 Rock。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

检查`HTTP POST Create`方法。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

此重载`Create`方法采用`album`创建 ASP.NET MVC 模型绑定系统从窗体值发布的对象。 当你提交新唱片集，模型状态有效并且没有数据库错误时，将数据库被添加新唱片集。 下图显示新唱片集的创建。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

你可以使用[fiddler 工具](http://www.fiddler2.com/fiddler2/)检查已发布的窗体值绑定的 ASP.NET MVC 模型使用来创建唱片集对象。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)。

### <a name="refactoring-the-viewbag-selectlist-creation"></a>重构 ViewBag 此时创建

这两个`Edit`方法和`HTTP POST Create`方法具有相同的代码，若要设置**此时**中**ViewBag**。 中的设计理念[干](http://en.wikipedia.org/wiki/Don't_repeat_yourself)，我们将重构此代码。 我们将使此使用重构代码更高版本。

创建一个新方法以便添加 genre 和艺术家**此时**到**ViewBag**。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

将两个行设置`ViewBag`中每个`Create`和`Edit`方法通过调用`SetGenreArtistViewBag`方法。 完成的代码如下所示。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

创建新唱片集和编辑唱片集以验证更改正常。

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>显式将此时传递到下拉列表

创建和编辑视图创建的 ASP.NET MVC 基架使用以下**DropDownList**重载：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList`创建视图标记，如下所示。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

因为`ViewBag`属性`SelectList`名为`GenreId`、 **DropDownList**帮助程序将使用`GenreId`**此时**中**ViewBag**. 在下面的示例**DropDownList**超负荷运转，`SelectList`显式传递中。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

打开*Views\StoreManager\Edit.cshtml*文件，并将更改**DropDownList**调用中显式传递**此时**，使用上面的重载。 执行此操作为流派类别。 已完成的代码所示：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

运行应用程序，然后单击**管理员**链接，然后导航到爵士乐唱片集并选择**编辑**链接。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

而不被显示为当前所选风格爵士乐，将显示 Rock。 当字符串参数 （要绑定的属性） 和**此时**对象具有相同的名称，则不会使用所选的值。 中的第一个元素时没有提供所选的值，默认浏览器**此时**(即**Rock**在上面的示例)。 这是已知的限制**DropDownList**帮助器。

打开*Controllers\StoreManagerController.cs*文件并将更改**此时**对象名称传递给`Genres`和`Artists`。 已完成的代码所示：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

风格和艺术家更好名称是为类别，因为它们具有以外的其他每个类别的 ID。 重构我们此前回报。 而不是更改**ViewBag**在四个方法中，我们更改了隔离到`SetGenreArtistViewBag`方法。

更改**DropDownList**调用在创建和编辑视图以使用新**此时**名称。 编辑视图的新标记如下所示：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

创建视图需要一个空字符串，以防止显示此时的第一项。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

创建新唱片集和编辑唱片集以验证更改正常。 通过选择与以外 Rock 流派唱片集测试编辑代码。

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>使用视图模型具有 DropDownList 帮助程序

名为的 Viewmodel 文件夹中创建一个新类`AlbumSelectListViewModel`。 中的代码替换`AlbumSelectListViewModel`替换为以下类：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel`构造函数采用唱片集、 专业人员和风格的列表，并创建一个包含唱片集的对象和一个`SelectList`风格和艺术家。

生成项目因此`AlbumSelectListViewModel`当我们在下一步中创建视图时才可用。

添加`EditVM`方法`StoreManagerController`。 完成的代码如下所示。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

右键单击`AlbumSelectListViewModel`，选择**解决**，然后**使用 MvcMusicStore.ViewModels;**。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

或者，可以添加以下 using 语句：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

右键单击`EditVM`和选择**添加视图**。 使用下面所示的选项。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

选择**添加**，然后替换内容*Views\StoreManager\EditVM.cshtml*替换为以下文件：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM`标记是非常类似于原始`Edit`标记有以下例外。

- 模型中的属性`Edit`视图是在窗体`model.property`(例如， `model.Title` )。 模型中的属性`EditVm`视图是在窗体`model.Album.property`(例如， `model.Album.Title`)。 这是因为`EditVM`视图为传递容器`Album`，而不`Album`作为 in`Edit`视图。
- **DropDownList**第二个参数不是来自视图模型， **ViewBag**。
- **BeginForm**中的帮助器`EditVM`视图显式回发到`Edit`操作方法。 通过发布回`Edit`操作，我们不需要编写`HTTP POST EditVM`操作并可以重用`HTTP POST``Edit`操作。

运行应用程序和编辑唱片集。 将 URL 更改为使用`EditVM`。 更改的字段，并按**保存**按钮来验证代码能够工作。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>你应使用哪种方法？

显示的所有三种方法是可接受的。 许多开发人员更愿意使用 explictily 传递`SelectList`到`DropDownList`使用`ViewBag`。 此方法的优点添加了为您提供使用找不到更合适的名称的灵活性。 一个需要注意的是不能将命名`ViewBag SelectList`对象模型属性与相同的名称。

一些开发人员更喜欢 ViewModel 方法。 其他请考虑更详细的标记，并且生成 HTML ViewModel 方法的缺点。

在本部分中我们已了解到使用的三种方法**DropDownList**类别数据。 在下一步的部分中，我们将展示如何添加新类别。

>[!div class="step-by-step"]
[上一页](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[下一页](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
