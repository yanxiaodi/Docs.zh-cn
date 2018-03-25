---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: "将新类别添加到使用 jQuery UI DropDownList |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: de661616ff3ca83052ae74d3ae6810d014aff764
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>将新类别添加到使用 jQuery UI DropDownList
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

HTML`Select`标记非常适合于显示一系列的固定的类别数据，但通常情况下你需要添加新类别。 假设我们想要添加到我们的数据库中的类别的流派"Opera"？ 在本部分中，我们将使用 jQuery UI 添加对话框中，我们可以使用来添加新类别。 下图显示如何 UI 将显示在浏览器中。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

当用户选择**添加新流派**链接，弹出对话框中提示用户输入新的流派名称 （和可选说明）。 下面显示的图像**添加流派**弹出对话框。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

当输入新的流派名称和**保存**按钮后，会发生以下情况：

1. AJAX 调用将数据发送到流派控制器，它将新的流派保存到数据库，并返回新流派信息 （流派名称和 ID） 为 JSON 的 Create 方法。
2. JavaScript 将新的流派数据添加到选择列表。
3. JavaScript 将使新流派所选的项目。

 在下图所示， **Opera**已添加到数据库，并选择在**流派**下拉列表。 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

打开*Views\StoreManager\Create.cshtml*文件并将流派标记替换为以下下面的代码：

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre`分部视图将包含所有的逻辑以挂钩的 JavaScript 和 jQuery 用于实现新添加的流派功能。 我们完成后将简单相同，但有艺术家 UI 的代码。

在解决方案资源管理器，右键单击*Views\StoreManager*文件夹，然后选择**添加**，然后**视图**。 在**视图名称**输入、 输入`_ChooseGenre`然后选择**添加**。 替换中的标记*Views\StoreManager\\_ChooseGenre.cshtml*替换为以下文件：

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

第一行声明我们正在传入的`Album`作为我们的模型中完全相同模型在创建视图中找到的语句。 下一步的几行是**标签**帮助器标记。 下一行是**DropDownList**帮助器调用，如下所示的原始创建视图完全相同。 下一步的行添加一个指向具有名称`Add New Genre`，和样式类似于按钮。 行包含`ValidationMessageFor`复制直接从创建视图。 以下行：

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

创建隐藏的 div 中，id 为`genreDialog`。 我们将使用 jQuery 挂钩我们**添加流派**id 对话框`genreDialog`中此分区 最后两个脚本标记包含我们将使用来实现新添加的流派功能的 JavaScript 文件的链接。 */Scripts/chooseGenre.js*文件是为你项目中，我们将检查它在教程后面部分提供。

运行应用程序并单击**添加新流派**按钮。 在**添加流派**对话框框中，输入**Opera**中**名称**输入的框。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

单击**保存**按钮。 AJAX 调用创建 Opera 类别然后填充下拉列表中的与 Opera，并将 Opera 设置为所选风格。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

输入艺术家、 标题和价格，然后选择**创建**按钮。 如果你输入的价格小于 $8.99，新唱片集将出现在索引视图的顶部。 验证新唱片集条目已保存到数据库中。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

请尝试使用只有一个字母创建新的风格。 下面的代码中*Models\Genre.cs*文件设置流派名称的最小和最大长度。

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

客户端验证报告您必须为 2 到 20 个字符输入字符串。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>检查如何新风格将添加到数据库和选择列表中。

打开*Scripts\chooseGenre.js*文件并检查代码。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

第二行使用 ID`genreDialog`上的 div 标记在中创建一个对话框*Views\StoreManager\\_ChooseGenre.cshtml*文件。 命名参数的大部分都是自解释性。 `autoOpen`参数设置为 false，选择**创建流派**按钮将显式打开对话框 （这将介绍在后一种）。 该对话框有两个按钮，**保存**和**取消**。 **取消**按钮关闭对话框。 下面的代码演示**保存**按钮函数。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm`从选择`createGenreForm`id。 `createGenreForm` ID 已在下面的代码中找到中设置*Views\Genre\\_CreateGenre.cshtml*文件。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx)中使用的帮助器重载*Views\Genre\\_CreateGenre.cshtml*文件具有一个包含要提交表单的 URL 的操作属性生成 HTML。 可以通过在浏览器中显示创建唱片集页并在浏览器中选择显示源来查看这种情况。 以下标记显示生成包含窗体标记的 HTML。

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery`$.post`一行使对操作属性的 AJAX 调用 (`/StoreManager/Create`) 并且将中的数据传入**创建流派**对话框。 数据包含新 genre 和可选说明的名称。 如果 AJAX 调用成功，新的流派名称和值添加到选择的标记，和新流派设置为所选值。 由于这是动态生成的标记，你无法看到新选择的选项，通过在浏览器中查看源。 你可以看到与 IE 9 F12 开发人员工具的新 HTML。 若要查看在 Internet Explorer 9 中的新选择的选项，按 F12 键以启动 F12 开发人员工具。 导航到创建页和添加新流派，因此流派选择列表中选择新的风格。 中的 F12 开发人员工具：

1. 选择 HTML 选项卡。
2. 点击刷新图标。  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. 在搜索框中，输入 GenreID。
4. 使用下一步图标，   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
 导航到以下选择标记：

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. 展开的最后一个选项值。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

下面的代码中*Scripts\chooseGenre.js*文件演示如何**添加新流派**按钮获取连接到的 click 事件，以及如何**添加新流派**对话框创建。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

第一行创建附加到一个单击函数**添加新流派**按钮。 以下标记从 Views\StoreManager\\_ChooseGenre.cshtml 文件演示如何**添加新流派**创建按钮：

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Load 方法创建和打开添加 Genre 对话框并调用 jQuery`parse`方法，以便在对话框中输入的数据时执行客户端验证。

在本部分中，你已学习如何创建一个对话框，可用来将新类别数据添加到选择列表。 你可以遵循相同的过程来创建用于向艺术家选择列表中添加新艺术家 UI。 本教程已授予的使用 ASP.NET MVC HTML 帮助程序概述**DropDownList**。 有关其他信息使用**DropDownList**，请参阅添加引用部分下面。 请让我们知道是否本教程中对你有所帮助。

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>其他参考

- [ASP.NET MVC – 级联下拉列表中列出教程](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx)通过[Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [选择](http://harvesthq.github.com/chosen/)JavaScript 插件支持多选择和筛选。

### <a name="contributors"></a>参与者

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean Sébastien Goupil
- [Brad wilson 制作](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>审阅者

- Jean Sébastien Goupil
- [Brad wilson 制作](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

>[!div class="step-by-step"]
[上一篇](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
