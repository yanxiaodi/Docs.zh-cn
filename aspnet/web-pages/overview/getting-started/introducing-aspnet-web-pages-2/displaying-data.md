---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: "引入的 ASP.NET Web Pages-显示数据 |Microsoft 文档"
author: tfitzmac
description: "本教程演示了如何在 WebMatrix 中创建数据库以及如何使用 ASP.NET Web 页 (Razor) 时，在页中显示数据库数据。 它假定 y..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: fdb9af0ba87c7802c63451ac7aa422e0020b5719
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>引入了 ASP.NET Web 页的显示数据
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程演示了如何在 WebMatrix 中创建数据库以及如何使用 ASP.NET Web 页 (Razor) 时，在页中显示数据库数据。 它假定你已完成通过系列[为 ASP.NET 网页编程的介绍](../introducing-razor-syntax-c.md)。
> 
> 你将学习：
> 
> - 如何使用 WebMatrix 工具来创建数据库和数据库表。
> - 如何使用 WebMatrix 工具将数据添加到数据库。
> - 如何在页上显示来自数据库的数据。
> - 如何运行 ASP.NET 网页中的 SQL 命令。
> - 如何自定义`WebGrid`帮助器以更改显示的数据并将添加分页和排序。
>   
> 
> 功能/技术讨论：
> 
> - WebMatrix 数据库工具。
> - `WebGrid`帮助器。


## <a name="what-youll-build"></a>你将生成

在前面的教程，你已引入到 ASP.NET Web Pages (*.cshtml*文件)、 Razor 语法的基础知识和帮助器。 在本教程中，你将开始创建的实际 web 应用程序将使用序列的其余部分。 应用程序是一个简单的影片应用程序，你可以查看、 添加、 更改和删除电影有关的信息。

完成本教程后，你将能够查看类似于此页的影片列表：

![包含参数的 WebGrid 显示设置为 CSS 类名称](displaying-data/_static/image1.png)

但是，若要开始，你必须创建数据库。

## <a name="a-very-brief-introduction-to-databases"></a>对数据库的非常简短介绍

本教程将提供到数据库仅 briefest 的简介。 如果你有数据库经验，则可以跳过此较短的节。

数据库包含一个或多个表包含的信息&mdash;例如，表为客户、 订单和供应商或学生、 教师、 类和年级。 在结构上，数据库表就像电子表格。 假设一个典型的通讯簿。 每个条目的通讯簿中 (即，每人) 具有以下几条如名字、 姓氏、 地址、 电子邮件地址和电话号码的信息。

![为简单网格示例数据库表](displaying-data/_static/image2.png)

(行有时称为*记录*，和列有时称为*字段*。)

对于大多数数据库表，表必须有一列以包含一个唯一值，如客户编号、 帐户号，依次类推。 此值称为表的*主键*，并使用它来标识表中的每个行。 在示例中，ID 列是在前面的示例所示的通讯簿的主键。

Web 应用程序中所做的工作大部分组成读取出数据库的信息以及在页面上显示它。 你通常还将从用户收集信息并将其添加到数据库，或将修改已存在于数据库中的记录。 （我们将介绍所有这些操作在此教程的集的过程中。）

数据库工作可以大大地复杂，可能需要专用的知识。 此教程的集，不过，您需要了解仅基本概念，这些概念将所有能解释根据自己的意愿。

## <a name="creating-a-database"></a>创建数据库

WebMatrix 包含工具，可以轻松创建数据库，并在数据库中创建表。 (数据库的结构称为数据库的*架构*。)此教程的集，你将创建在其中具有只有一个表的数据库&mdash;电影。

打开 WebMatrix，如果尚未这样做并打开你在前面的教程中创建的 WebPagesMovies 站点。

在左窗格中，单击**数据库**工作区。

![WebMatrix 数据库工作区选项卡](displaying-data/_static/image3.png)

功能区更改为显示与数据库相关的任务。 在功能区中，单击**新数据库**。

![在 WebMatrix 功能区的新数据库按钮](displaying-data/_static/image4.png)

WebMatrix 创建 SQL Server CE 数据库 ( *.sdf*文件) 作为你的站点具有相同名称&mdash; *WebPagesMovies.sdf*。 (不会执行此操作在这里，但你可以将重命名该文件为您喜欢的任何，只要它具有*.sdf*扩展。)

## <a name="creating-a-table"></a>创建表

在功能区中，单击**新表**。 WebMatrix 将在新选项卡中打开表设计器。（如果新的表选项不可用，请确保在左侧树视图中选择新的数据库。）

![在 WebMatrix 功能区的新表按钮](displaying-data/_static/image5.png)

在顶部 （其中水印显示"输入表名称"） 的文本框，输入"电影"。

![在 WebMatrix 数据库设计器中输入一个表名](displaying-data/_static/image6.png)

表名称下方窗格是你在其中定义单独的列。 有关*电影*表在本教程中，你将创建仅少量的列： *ID*，*标题*，*流派*，和*年*.

在**名称**框中，输入"ID"。 输入一个值，则会激活的新列的所有控件。

Tab 键移动到**数据类型**列表并选择**int**。此值指定 ID 列将包含整数 （数字） 数据。

> [!NOTE]
> 我们不会调出任何此处详细 （得多），但你可以使用标准的 Windows 键盘手势在此网格中进行导航。 例如，你可以使用 tab 键字段之间，就可以开始键入以便在列表中，选择一个项，依次类推。


过去的选项卡上**默认值**框 （即，将其留空）。 Tab 键移动到**是 Primary Key**复选框，然后选择它。 此选项指示数据库*ID*列将包含标识单个行的数据。 （这就是，每一行将具有一个唯一的值可用来查找该行 ID 列中。）

选择**是标识**选项。 此选项指示数据库，它应自动生成每个新行的下一个顺序编号。 (**是标识**选项仅当你已选择**是 Primary Key**选项。)

在下一步的网格行中，单击或按 tab 键两次以完成当前行。 任一笔势保存当前行，并启动下一个。 请注意，**默认值**列现在显示**Null**。 （null 是默认值为默认值，这样说。）

当你已定义新**ID**列中，设计器将类似于此图中：

![WebMatrix 定义电影表的 ID 列后的数据库设计器](displaying-data/_static/image7.png)

若要创建下一列，请单击框中**名称**列。 为列中输入"Title"，然后选择**nvarchar**为**数据类型**值。 "Var"属于**nvarchar**告诉数据库此列的数据将为其大小可能有所不同记录之间的字符串。 ("N"前缀表示"国家/地区，"指示字段可以包含任何字母或书写体系的字符数据-即，该字段将持有 Unicode 数据。)

当你选择**nvarchar**，另一个框中将出现，为该字段输入的最大长度。 输入 50，假定您将在本教程中使用无影片标题将会超过 50 个字符。

跳过**默认值**和清除**允许 null 值**选项。 你不想要允许任何以输入到数据库过的电影没有标题的数据库。

当你完成此操作后，并移动到下一行时，设计器将类似于此图中：

![WebMatrix 定义电影表的标题列后的数据库设计器](displaying-data/_static/image8.png)

重复以下步骤创建一个名为除外长度的"Genre"列将其设置为只是 30。

创建名为"Year。"的另一个列 对于数据类型中，选择**nchar** (不**nvarchar**) 并将长度设置为 4。 年，你要使用 4 位数字，如"1995"或"2010"，因此不需要的大小可变的列。

下面是已完成的设计如下所示：

![WebMatrix 数据库设计器后为电影表定义的所有字段](displaying-data/_static/image9.png)

按 Ctrl + S 或单击**保存**中快速访问工具栏按钮。 通过关闭选项卡来关闭数据库设计器。

## <a name="adding-some-example-data"></a>添加了一些示例数据

稍后在本教程系列中，你将创建可以在窗体中输入新影片的页。 但是，现在，可以添加一些示例数据，然后可以显示在页面上。

在**数据库**在 WebMatrix 中，请注意，没有一个树，它显示你的工作区*.sdf*前面创建的文件。 打开新的节点*.sdf*文件中，并随后打开**表**节点。

![目录树到电影表打开 WebMatrix 数据库工作区](displaying-data/_static/image10.png)

右键单击**电影**节点，然后选择**数据**。 WebMatrix 将打开一个网格，你可以在其中输入数据*电影*表：

![在 WebMatrix 中 （空） 的数据库条目网格](displaying-data/_static/image11.png)

单击**标题**列并输入"时 Harry 满足 Sally"。 将移动到**流派**（你可以使用 Tab 键） 的列，然后输入"浪漫喜剧"。 将移动到**年**列并输入"1989":

![在 WebMatrix 中使用一条记录的数据库条目网格](displaying-data/_static/image12.png)

按 Enter 和 WebMatrix 将保存新的影片。 请注意， **ID**填写列。

![在 WebMatrix 中使用一条记录和自动生成的 ID 的数据库条目网格](displaying-data/_static/image13.png)

输入另一电影 （例如，"进入与风暴"、"戏曲"，"1939"）。 再次填写 ID 列：

![在 WebMatrix 中使用两条记录和自动生成 Id 的数据库条目网格](displaying-data/_static/image14.png)

输入第三个影片 （例如，"Ghostbusters"、"喜剧"）。 作为试验，使**年**列留空，然后按 Enter。 因为你未选定的**允许 null 值**选项，数据库将显示错误：

![如果所需的列的值为空，将显示无效的数据错误](displaying-data/_static/image15.png)

单击**确定**以返回并修复 （"Ghostbusters"的年份是 1984年） 的条目，然后按 Enter。

填写几个电影，直到有 8 或以便。 （输入 8 便于更轻松地使用分页更高版本。 但如果是太多，输入只是几个现在。）实际数据并不重要。

![在 WebMatrix 中包含的任一记录的数据库条目网格](displaying-data/_static/image16.png)

如果输入未出现任何错误的所有影片，ID 值是连续的。 如果尝试保存一个不完整的电影记录，可能不是连续的 ID 号。 如果是这样，没关系。 数字没有任何固有的含义，又是重要的唯一操作是它们在中唯一*电影*表。

关闭包含数据库设计器的选项卡。

现在你可以启用在网页上显示此数据。

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>通过使用 WebGrid 帮助器在页上显示数据

若要在页中显示数据，你将使用`WebGrid`帮助器。 此帮助器生成的网格或表 （行和列） 中显示。 如你所见，你将能够调整网格与格式设置和其他功能。

若要运行网格，你将需要编写几行代码。 以下几行将充当一种类型的几乎所有在本教程中执行的数据访问模式。

> [!NOTE]
> 实际上有许多选项在页上显示数据`WebGrid`帮助器只是其中一个。 我们选择它本教程中，因为它显示数据的最简单方法并且因为它是相当灵活。 在下一步教程组中，你将了解如何使用详细的"手动"方式处理在页中，这为你提供更直接控制如何显示数据的数据。


在 WebMatrix 中左窗格中，单击**文件**工作区。

你创建的新数据库处于*应用\_数据*文件夹。 如果文件夹尚不存在，WebMatrix 将创建它为新的数据库。 （文件夹可能已存在如果以前已安装了帮助器。）

在树视图中，选择在网站的根目录。 您必须选择网站; 的根目录否则，可能会向应用添加新文件\_数据文件夹。

在功能区中，单击**新建**。 在**选择文件类型**框中，选择**CSHTML**。

在**名称**框中，将新该页"Movies.cshtml":

![选择文件类型对话框中显示电影页](displaying-data/_static/image17.png)

单击**确定**按钮。 WebMatrix 打开具有在其中一些主干元素的新文件。 首先，你将编写一些代码，可从数据库中获取数据。 然后你将添加到页后，可以实际显示数据的标记。

### <a name="writing-the-data-query-code"></a>编写数据查询代码

在页上，顶部之间`@{`和`}`字符，输入下面的代码。 （请确保你输入此代码，开始标记和右大括号之间。）

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

第一行将打开更早版本，创建的数据库，这是始终第一个步骤之前执行某项与数据库。 你判断`Database.Open`要打开的数据库的方法名称。 请注意，不包含*.sdf*名称中。 `Open`方法假设它正在寻找*.sdf*文件 (即， *WebPagesMovies.sdf*) 并且*.sdf*文件位于*应用\_数据*文件夹。 (前面我们注意到，*应用\_数据*保留文件夹; 这种情况下是 ASP.NET 其中假定该名称的位置之一。)

当打开该数据库时，对它的引用被放置到命名的变量`db`。 （这可以命名为任何内容。）`db`变量是如何将显示与数据库交互。

第二行实际上会获取数据库数据，通过使用`Query`方法。 请注意此代码的工作原理：`db`变量具有对打开的数据库的引用和调用`Query`方法通过使用使用`db`变量 (`db.Query`)。

查询本身是一个 SQL`Select`语句。 （有关少量 SQL 有关背景信息，请参阅说明更高版本）。在语句中，`Movies`标识要查询表。 `*`字符指定查询应返回表中的所有列。 （你可能还列出列单独，并用逗号隔开。）

结果的查询，如果有的话，将返回，并且中可`selectedData`变量。 同样，该变量可以命名为任何内容。

最后，第三行是告诉 ASP.NET，你想要使用的实例`WebGrid`帮助器。 创建 (*实例化*) 使用的帮助程序对象`new`关键字并将其传递通过查询结果`selectedData`变量。 新`WebGrid`对象，以及数据库查询的结果可在`grid`变量。 在一段时间来实际在页中显示数据，你将需要该结果。

在此阶段中，打开该数据库，你已收到数据，并已准备好`WebGrid`与该数据的帮助器。 下一步是在页中创建的标记。

> [!TIP] 
> 
> **结构化的查询语言 (SQL)**
> 
> SQL 是一种语言，可在大多数关系数据库中用于管理数据库中的数据。 它包括能够让你检索数据和更新它，并能够让你创建、 修改和管理数据库表中的数据的命令。 SQL 是不同于编程语言 （如 C# 中)。 使用 SQL 告诉数据库所需的并且数据库的作业，以了解如何获取数据或执行任务。 下面是一些 SQL 命令的示例和它们执行的操作：
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> 第一个`Select`语句获取所有列 (由指定`*`) 从*电影*表。
> 
> 第二个`Select`语句从记录中提取的 ID、 名称和价格列*产品*其价格列的值是 10 个以上的表。 此命令根据名称列的值的按字母顺序返回结果。 如果没有记录匹配价格条件，该命令将返回空集。
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> 此命令将插入新记录到*产品*表，将"Croissant"、"A 发生喜欢广为"，说明列和价格的名称列设置为 1.99。
> 
> 请注意，当你要指定一个非数字值，该值将用单引号引起来 （不双引号，如下所示 C#）。 你使用围绕文本或日期值，但不是解决数字这些引号引起来。
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> 此命令删除中的记录*产品*其到期日期列是早于 2008 年 1 月 1 日的表。 (该命令假定*产品*表具有这样的列，这是当然的。)采用 MM/DD/YYYY 格式，在此处输入日期，但它应该用于你的区域设置的格式输入。
> 
> `Insert`和`Delete`命令不返回结果集。 相反，它们返回一个数字，告诉你该命令影响了多少条记录。
> 
> 对于某些这些操作 （例如插入和删除记录），请求操作的过程必须在数据库中具有适当的权限。 这就是为什么对于生产数据库通常具有以便在连接到数据库时提供用户名和密码。
> 
> 有多个 SQL 命令，但它们都遵循与你在此处看到命令一样模式。 SQL 命令可用于创建数据库表、 计算的表中的记录数、 计算价格，和执行许多的更多操作。


### <a name="adding-markup-to-display-the-data"></a>添加标记以显示数据

内部`<head>`元素，集内容`<title>`"电影"的元素：

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

内部`<body>`元素的页上，添加以下：

[!code-html[Main](displaying-data/samples/sample3.html)]

介绍完毕。 `grid`变量是在创建时创建的值`WebGrid`前面代码中的对象。

在 WebMatrix 树视图中，右击该页并选择**在浏览器中启动**。 你将看到类似于此页：

![电影表中的默认 WebGrid 帮助器输出](displaying-data/_static/image18.png)

单击列标题链接以按该列排序。 能够作为排序依据单击标题是一项功能内置于**WebGrid**帮助器。

`GetHtml`方法，顾名思义，生成显示的数据的标记。 默认情况下，`GetHtml`方法将生成一个 HTML`<table>`元素。 （如果你想，你可以验证呈现通过查看浏览器中的页面的源。）

## <a name="modifying-the-look-of-the-grid"></a>修改网格的外观

使用`WebGrid`帮助程序只需像是变得简单，但生成的显示为 plain。 `WebGrid`帮助程序具有各种类型的选项，可让你控制如何显示数据。 有太多，以浏览本教程中，但是此部分可让你了解一些这些选项。 将在后面的这一系列教程中介绍几个其他选项。

### <a name="specifying-individual-columns-to-display"></a>指定显示的单个列

若要开始，可以指定你想要只显示某些列。 默认情况下，如你所见，该网格显示中的列的所有四个*电影*表。

在*Movies.cshtml*文件中，将`@grid.GetHtml()`刚添加替换为以下标记：

[!code-css[Main](displaying-data/samples/sample4.css)]

若要指示帮助器要显示的列，则包含`columns`参数`GetHtml`方法并传入集合中的列。 在集合中，你可以指定要包括每个列。 指定各个列以显示通过包括`grid.Column`对象，并传入所需的数据列的名称。 (这些列必须包含在 SQL 查询结果-`WebGrid`帮助器无法显示未由查询返回的列。)

启动*Movies.cshtml*浏览器中的同样，和获取以下所示显示此时间 （请注意，显示没有 ID 列）：

![显示仅所选的列的 WebGrid 显示](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>更改查找范围的网格

有相当多的更多用于显示的列，将此集中的更高版本教程中介绍其中一些选项。 现在，本节将向你介绍你可以在其中设置样式为整个网格的方式。

内部`<head>`的页上，只需在关闭之前的部分`</head>`标记中，添加以下`<style>`元素：

[!code-css[Main](displaying-data/samples/sample5.css)]

此 CSS 标记定义名为的类`grid`， `head`，依次类推。 您还可以通过以下方式将这些样式定义在单独*.css*文件并将该链接到页。 （事实上，您将执行的更高版本中此教程的集。）但是，为了方便起见出于本教程，它们很内显示的数据的同一页面。

现在你可以获取`WebGrid`帮助器以使用这些样式类。 帮助器具有多个属性 (例如， `tableStyle`) 只需这样 — 为它们，分配的 CSS 样式类名称并作为一部分的帮助程序呈现的标记呈现该类的名称。

更改`grid.GetHtml`标记以便它现在看起来像此代码：

[!code-css[Main](displaying-data/samples/sample6.css)]

什么是新此处为你添加的`tableStyle`， `headerStyle`，和`alternatingRowStyle`参数`GetHtml`方法。 这些参数已设置为添加前一段时间的 CSS 样式的名称。

运行此页面，和此时会显示一个网格，看起来比大大降低纯之前：

![包含参数的 WebGrid 显示设置为 CSS 类名称](displaying-data/_static/image20.png)

若要了解`GetHtml`生成的方法，你可以查看在浏览器中的页面的源。 我们将不会进行详细信息在这里，但重要的一点是，通过指定类似于的参数`tableStyle`，导致网格以生成 HTML 标记，如下所示：

`<table class="grid">`

`<table>`标记已`class`引用了一个以前添加的 CSS 规则的属性添加到它。 此代码演示了基本模式&mdash;不同参数`GetHtml`方法让你引用 CSS 类方法然后生成以及标记。 你使用 CSS 类所执行的操作是由您决定。

## <a name="adding-paging"></a>添加分页

作为本教程的最后一个任务，你将在网格中添加分页。 现在，它是要每次显示所有电影没问题。 但如果你添加数百个电影，页面显示将获得长。

在网页代码中，更改创建的行`WebGrid`于下面的代码的对象：

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

与之前的唯一差异是你已添加`rowsPerPage`设置为 3 的参数。

运行页面。 在网格中显示在时间，以及让电影的你的数据库中翻页的导航链接 3 行：

![使用分页 WebGrid 显示](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>接下来

在下一步的教程中，你将了解如何使用 Razor 和 C# 代码来获取用户输入的窗体。 以便您可以找到按标题或风格的影片，你将添加到电影页搜索框。

## <a name="complete-listing-for-movies-page"></a>电影页的完整列表

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>其他资源

- [使用 Razor 语法的 ASP.NET Web 编程简介](https://go.microsoft.com/fwlink/?LinkID=202890)

>[!div class="step-by-step"]
[上一页](intro-to-web-pages-programming.md)
[下一页](form-basics.md)
