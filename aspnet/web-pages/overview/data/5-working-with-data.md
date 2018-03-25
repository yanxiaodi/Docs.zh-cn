---
uid: web-pages/overview/data/5-working-with-data
title: "使用在 ASP.NET 网页中的数据库的简介页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "本章介绍如何从数据库访问数据并将其使用的 ASP.NET Web Pages 显示。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 460af471a1b0650f8d782d582ce6cd9a06664d5c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>使用在 ASP.NET 网页中的数据库的简介页 (Razor) 站点
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文介绍如何使用 Microsoft WebMatrix 工具创建的数据库在一个 ASP.NET Web 页 (Razor) 的网站，以及如何创建网页，让你显示、 添加、 编辑和删除数据。
> 
> **你将学习：** 
> 
> - 如何创建数据库。
> - 如何连接到数据库。
> - 如何在网页上显示数据。
> - 如何插入、 更新和删除数据库记录。
> 
> 这些是文章中引入的功能：
> 
> - 使用 Microsoft SQL Server Compact Edition 数据库。
> - 使用 SQL 查询。
> - `Database` 类。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> 本教程还适用于 WebMatrix 3。 你可以使用 ASP.NET Web Pages 3 和 Visual Studio 2013 （或 Visual Studio Express 2013 for Web）;但是，用户界面将会不同。


## <a name="introduction-to-databases"></a>数据库简介

假设一个典型的通讯簿。 每个条目的通讯簿中 (即，每人) 具有以下几条如名字、 姓氏、 地址、 电子邮件地址和电话号码的信息。

如下图数据的典型方法是为包含行和列的表。 在数据库术语中，每个行通常称为记录。 每个列 （有时称为字段） 包含每种数据类型的值： 第一个名称，最后一个名称和等等。

| **ID** | **FirstName** | **LastName** | **Address** | **Email** | **Phone** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main St.西雅图市华盛顿州 99011 | terry@cohowinery.com | 555 0101 |

对于大多数数据库表，该表必须有一列以包含的唯一标识符，如客户编号、 帐户号等。这称为表的*主键*，并使用它来标识表中的每个行。 在示例中，ID 列是通讯簿的主键。

此数据库的基本的了解，你就可以了解如何创建一个简单的数据库并执行如添加、 修改和删除数据的操作。

> [!TIP] 
> 
> **关系数据库**
> 
> 你可以将数据存储在大量方法，包括文本文件和电子表格。 对于大多数的业务使用，不过，数据存储在关系数据库中。
> 
> 本文不非常深进入数据库。 但是，你可能会发现它有助于了解一些有关它们的信息。 在关系数据库中，信息在逻辑上划分为单独的表。 例如，一所学校的数据库可能包含单独的表，学生版和类产品。 数据库软件 （如 SQL Server) 支持强大命令，使您动态建立表之间的关系。 例如，关系数据库可用于逻辑之间建立关系学生和类以创建计划。 在单独的表中存储数据可减少表结构的复杂性，而无需将冗余数据保留在表。


## <a name="creating-a-database"></a>创建数据库

此过程演示如何创建名 SmallBakery 为使用 SQL Server Compact 数据库设计工具在 WebMatrix 中包含的数据库。 虽然你可以创建使用代码的数据库，它是更常见的做法创建数据库和数据库表使用 WebMatrix 之类的设计工具。

1. 启动 WebMatrix，然后在快速启动页上，单击**站点从模板**。
2. 选择**空网站**，然后在**站点名称**框中输入"SmallBakery"，然后单击**确定**。 创建站点并将其显示在 WebMatrix 中。
3. 在左窗格中，单击**数据库**工作区。
4. 在功能区中，单击**新数据库**。 使用与你的站点相同的名称创建一个空数据库。
5. 在左窗格中，展开**SmallBakery.sdf**节点，然后单击**表**。
6. 在功能区中，单击**新表**。 WebMatrix 打开表设计器。

    ![[image]](5-working-with-data/_static/image1.jpg)
7. 在单击**名称**列和输入&quot;Id&quot;。
8. 在**数据类型**列中，选择**int**。
9. 设置**是主键？**和**是识别？**选项到**是**。

    顾名思义，**是 Primary Key**告诉数据库，这将是表的主键。 **是标识**告诉数据库自动创建新的每个记录的 ID 号并将其分配下一步的序列号 （从 1 开始）。
10. 单击下一步的行中。 此时将启动一个新的列定义编辑器。
11. 对于名称值中，输入&quot;名称&quot;。
12. 有关**数据类型**，选择&quot;nvarchar&quot;和长度设置为 50。 *Var*属于`nvarchar`告诉数据库此列的数据将为其大小可能有所不同记录之间的字符串。 ( *N*前缀表示*国家/地区*、，该值指示字段可以保存表示任何字母的字符数据或写入系统&#8212;，即，该字段将持有 Unicode 数据。)
13. 设置**允许 null 值**选项设为**否**。 这会强制此要求*名称*列不会保留空白。
14. 使用此相同的过程中，创建一个名为列*说明*。 设置**数据类型**"nvarchar"和长度，并设置 50**允许 null 值**为 false。
15. 创建一个名为列*价格*。 设置**数据类型设置为"货币"**并设置**允许 null 值**为 false。
16. 在顶部的框中，命名表&quot;产品&quot;。

    完成后，定义将如下所示：

    ![[image]](5-working-with-data/_static/image2.jpg)
17. 按 Ctrl + S 以保存表。

## <a name="adding-data-to-the-database"></a>向数据库添加数据

现在你可以添加到你的数据库，你将在本文的后面使用的一些示例数据。

1. 在左窗格中，展开**SmallBakery.sdf**节点，然后单击**表**。
2. 右键单击产品表，然后单击**数据**。
3. 在编辑窗格中，输入以下记录：

    | **名称** | **说明** | **价格** |
    | --- | --- | --- |
    | 痕迹 | 由新颖度每日。 | 2.99 |
    | 草莓 Shortcake | 从我们园使用环保 strawberries 进行。 | 9.99 |
    | Apple 饼图 | 仅为你的妈妈饼图的第二个。 | 12.99 |
    | Pecan 饼图 | 如果你喜欢 pecans，这是为你。 | 10.99 |
    | 柠檬饼图 | 使用世界上的最佳柠檬进行。 | 11.99 |
    | 蛋糕 | 你的孩子和在你的孩子将喜欢这些。 | 7.99 |

    请记住，你不必输入任何内容的*Id*列。 你在创建时*Id*列中，设置其**是标识**属性为 true，这会导致它自动填充的。

    当完成输入数据时，表设计器将如下所示：

    ![[image]](5-working-with-data/_static/image3.jpg)
4. 关闭包含的数据库数据的选项卡。

## <a name="displaying-data-from-a-database"></a>显示数据库中的数据

一旦在其中配置了具有数据的数据库，你可以在 ASP.NET 网页中显示数据。 若要选择要显示的表行，你可以使用 SQL 语句，这是一个可将传递到数据库的命令。

1. 在左窗格中，单击**文件**工作区。
2. 在网站的根目录，创建一个名为的新 CSHTML 页*ListProducts.cshtml*。
3. 现有的标记替换为以下代码：

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    你可以在第一个代码块中，打开*SmallBakery.sdf*前面创建的文件 （数据库）。 `Database.Open`方法假设*.sdf*文件位于你的网站*应用\_数据*文件夹。 (请注意，不需要指定*.sdf*扩展&#8212;事实上，如果这样做，`Open`方法将不起作用。)

    > [!NOTE]
    > *应用\_数据*文件夹是用于存储数据文件的 ASP.NET 中的特殊文件夹。 有关详细信息，请参阅[连接到数据库](#SB_ConnectingToADatabase)本文后续部分中。

    然后发出请求以查询数据库中使用以下 SQL`Select`语句：

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    在语句中，`Product`标识要查询表。 `*`字符指定查询应返回表中的所有列。 （你可能还列出列单独，由逗号分隔，如果你想要看到仅某些列。）`Order By`子句指示数据的排序方式&#8212;在这种情况下，通过*名称*列。 这意味着对数据进行排序的值按字母顺序基于*名称*每一行的列。

    在页的正文中，标记将创建一个 HTML 表以将用于显示数据。 内部`<tbody>`元素，你使用`foreach`循环单独获取由查询返回的每个数据行。 对于每个数据行，创建一个 HTML 表行 (`<tr>`元素)。 然后创建 HTML 表格单元格 (`<td>`元素) 为每个列。 每次执行循环时，请转到下一步的可用行从数据库已处于`row`变量 (你对此进行设置`foreach`语句)。 若要获取行中的单个列，可以使用`row.Name`或`row.Description`或者的列的任何名称是你想。
4. 在浏览器中运行页面。 (请确保页中选择**文件**工作区之前运行它。)该页显示如下所示的列表：

    ![[image]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **结构化的查询语言 (SQL)**
> 
> SQL 是一种语言，可在大多数关系数据库中用于管理数据库中的数据。 它包括能够让你检索数据和更新它，并能够让你创建、 修改和管理数据库表的命令。 SQL 是不同于编程语言 （如你在 WebMatrix 中使用的一个） 因为使用 SQL 的想法是告诉数据库所需的并且在各数据库的作业，以了解如何获取数据或执行任务。 下面是一些 SQL 命令的示例和它们执行的操作：
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> 这提取*Id*，*名称*，和*价格*中记录的列*产品*表如果的值*价格*大于 10，并将结果返回的值为基础的按字母顺序*名称*列。 此命令将返回包含满足条件或为空集，如果没有与匹配记录的记录的结果集。
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> 这会插入新记录到*产品*表，设置*名称*列&quot;Croissant&quot;、*说明*列&quot;发生异常喜欢广为&quot;，到 1.99 与价格。
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> 此命令删除中的记录*产品*其到期日期列是早于 2008 年 1 月 1 日的表。 (此操作假定*产品*表具有这样的列，这是当然的。)采用 MM/DD/YYYY 格式，在此处输入日期，但它应该用于你的区域设置的格式输入。
> 
> `Insert Into`和`Delete`命令不返回结果集。 相反，它们返回一个数字，告诉你该命令影响了多少条记录。
> 
> 对于某些这些操作 （例如插入和删除记录），请求操作的过程必须在数据库中具有适当的权限。 这是通常必须提供用户名和密码连接到数据库时的生产数据库的原因。
> 
> 有多个 SQL 命令，但它们都遵循如下模式。 SQL 命令可用于创建数据库表、 计算的表中的记录数、 计算价格，和执行许多的更多操作。


## <a name="inserting-data-in-a-database"></a>在数据库中插入数据

本部分演示如何创建一个允许用户添加到新的产品页面*产品*数据库表。 插入新的产品记录后，页面将显示更新后的表使用*ListProducts.cshtml*你在上一节中创建的页。

该页面包括验证以确保用户输入的数据有效的数据库。 例如，在页中的代码可确保已为所有所需的列输入值。

1. 在网站中，创建一个名为的新 CSHTML 文件*InsertProducts.cshtml*。
2. 现有的标记替换为以下代码：

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    页的正文包含使用户可以输入名称、 描述和价格的三个文本框一个 HTML 窗体。 当用户单击**插入**按钮，在页面顶部的代码打开的连接*SmallBakery.sdf*数据库。 您会收到用户通过使用已提交的值`Request`对象并将这些值分配给本地变量。

    若要验证用户为每个所需的列输入一个值，你注册每个`<input>`你想要验证的元素：

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    `Validation`帮助器检查每个已注册的字段中是否有一个值。 你可以测试是否所有字段通过检查已都通过验证`Validation.IsValid()`，后者通常执行之前处理从用户获取的信息：

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (`&&`运算符表示 AND-此测试*如果这是提交窗体和所有字段已都通过验证*。)

    如果验证了所有列 （无为空），请继续并创建一个 SQL 语句来将数据插入，然后执行它，如下所示：

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    要插入的值，包括参数占位符 (`@0`， `@1`， `@2`)。

    > [!NOTE]
    > 作为安全措施，始终将值传递给 SQL 语句使用参数，如上所示在前面的示例。 这使你可以首先验证用户的数据，以及它有助于保护计算机免受恶意命令发送到数据库 （有时称为 SQL 注入式攻击） 的尝试。

    若要执行查询时，你使用此语句中，将传递到它包含要替换的占位符的值的变量：

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    后`Insert Into`已执行语句，将用户发送给页，其中列出的产品使用以下行：

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    如果验证未成功，则跳过插入。 相反，你可以显示累积的错误消息 （如果有） 的页中具有帮助器：

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    请注意，在标记中的样式块包括 CSS 类定义名为`.validation-summary-errors`。 这是默认情况下，为使用 CSS 类的名称`<div>`包含任何验证错误的元素。 在这种情况下，CSS 类指定验证摘要错误显示为红色和显示为粗体，但你可以定义`.validation-summary-errors`类来显示任何您喜欢的格式设置。

### <a name="testing-the-insert-page"></a>测试插入页

1. 在浏览器中查看的页面。 该页面显示类似于下图中所示的一个窗体。

    ![[image]](5-working-with-data/_static/image5.jpg)
2. 对于所有列，输入值，但请确保你保留*价格*列保留为空。
3. 单击“插入” 。 页面显示错误消息，如下面的插图中所示。 （不创建任何新的记录。）

    ![[image]](5-working-with-data/_static/image6.jpg)
4. 完全，填写表单，然后单击**插入**。 此时， *ListProducts.cshtml*页会显示，并显示新的记录。

## <a name="updating-data-in-a-database"></a>更新数据库中的数据

数据输入到表后，你可能需要更新它。 此过程演示如何创建类似于前面的数据插入操作创建的两个页面。 第一页显示产品，并使用户可以选择一个计数器以更改。 可以在第二个页的实际进行的编辑和保存它们的用户。

> [!NOTE] 
> 
> **重要**在生产网站中，你通常限制允许哪些人具有对数据进行更改。 有关如何将成员资格设置以及如何授权用户的站点上执行任务的信息，请参阅[添加安全和 ASP.NET Web Pages 站点的成员资格](https://go.microsoft.com/fwlink/?LinkId=202904)。


1. 在网站中，创建一个名为的新 CSHTML 文件*EditProducts.cshtml*。
2. 文件中的现有标记替换为以下代码：

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    此页之间唯一的区别和*ListProducts.cshtml*页从更早版本是在此页中的 HTML 表包含的额外的列显示**编辑**链接。 单击此链接时，它将带你访问*UpdateProducts.cshtml*页面 （其中你将创建下一步） 可以在其中编辑选定的记录。

    查看创建的代码**编辑**链接：

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    这将创建 HTML`<a>`元素其`href`动态设置属性。 `href`属性指定当用户单击该链接时要显示的页。 它还将传递`Id`链接到当前行的值。 当运行页面时，页面源文件可能包含类似这样的链接：

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    请注意，`href`属性设置为`UpdateProducts/n`，其中*n*是产品编号。 当用户单击这些链接之一时，生成的 URL 将如下所示：

    `http://localhost:18816/UpdateProducts/6`

    换而言之，将在 URL 中传递的产品编号，以对其进行编辑。
3. 在浏览器中查看的页面。 该页显示数据的格式如下：

    ![[image]](5-working-with-data/_static/image7.jpg)

    接下来，你将创建页面，从而让用户实际更新的数据。 更新页面包括验证来验证用户输入的数据。 例如，在页中的代码可确保已为所有所需的列输入值。
4. 在网站中，创建一个名为的新 CSHTML 文件*UpdateProducts.cshtml*。
5. 将替换为以下文件中的现有标记。

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    页面的正文包含 HTML 窗体，其中显示产品和用户可以在其中编辑。 若要获取要显示的产品，你可以使用此 SQL 语句：

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    这将选择其 ID 与传入的值匹配的产品`@0`参数。 (因为*Id*是主键，因此必须是唯一的只有一个产品记录可以不断选择这种方式。)若要获取的 ID 值传递给这`Select`语句中，你可以读取的值传递到页的 url 的一部分使用以下语法：

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    若要实际提取的产品记录，请使用`QuerySingle`方法，它将返回一个记录：

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    中的单个行返回到`row`变量。 你可以获取超出每个列的数据，并将其分配给本地变量如下：

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    在窗体标记中，这些值则会自动显示在单个文本框中通过使用嵌入的代码如下所示：

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    该代码的一部分显示要更新的产品记录。 一旦已显示该记录，用户可以编辑单个列。

    当用户通过单击提交窗体**更新**按钮中的代码`if(IsPost)`块将运行。 这将获取用户的值从`Request`对象，将值存储在变量，并验证每个列已填写。 如果通过验证，该代码将创建以下 SQL Update 语句：

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    在 SQL`Update`语句，您可以指定更新和的值将它设置为每个列。 在此代码中，使用参数占位符指定的值`@0`， `@1`， `@2`，依次类推。 （如前文所述，为安全起见，你应始终将值传递给 SQL 语句中使用的参数。）

    当调用`db.Execute`方法，将变量传递包含对应于 SQL 语句中的参数的顺序中的值：

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    后`Update`执行语句，以便将用户重定向回编辑页调用以下方法：

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    其效果是用户将数据库中的数据的更新的列表，并且可以编辑另一种产品。
6. 保存页。
7. 运行*EditProducts.cshtml*页 （不是在更新页面），然后单击**编辑**以选择要编辑的产品。 *UpdateProducts.cshtml*页面显示时，显示你选择的记录。

    ![[image]](5-working-with-data/_static/image8.jpg)
8. 进行更改，然后单击**更新**。 使用更新的数据，则需再次显示产品列表文件。

## <a name="deleting-data-in-a-database"></a>删除数据库中的数据

本部分演示如何使用户可以删除从产品*产品*数据库表。 此示例由两个页组成。 在第一页中，用户选择要删除的记录。 要删除的记录随后显示在第二页，以便确认他们想要删除的记录。

> [!NOTE] 
> 
> **重要**在生产网站中，你通常限制允许哪些人具有对数据进行更改。 有关如何将成员资格设置以及方式来授予用户在站点上执行任务的信息，请参阅[添加安全和 ASP.NET Web Pages 站点的成员资格](https://go.microsoft.com/fwlink/?LinkId=202904)。


1. 在网站中，创建一个名为的新 CSHTML 文件*ListProductsForDelete.cshtml*。
2. 现有的标记替换为以下代码：

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    此页是类似于*EditProducts.cshtml*从前面的页。 但是，而不是显示**编辑**链接为每个产品，它显示**删除**链接。 **删除**标记中使用下面的嵌入的代码创建链接：

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    这将创建如下所示，当用户单击此链接的 URL:

    `http://<server>/DeleteProduct/4`

    URL 调用一个名为页*DeleteProduct.cshtml* （这将在下一步创建） 并将其传递要删除的产品 ID （此处，4）。
3. 保存该文件，但使它保持打开状态。
4. 创建名为的另一个 CHTML 文件*DeleteProduct.cshtml*。 将现有内容替换为以下：

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    此页由*ListProductsForDelete.cshtml*并允许用户确认他们想要删除产品。 若要列出要删除的产品，可以从 URL 中删除该产品的 ID 使用下面的代码：

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    然后，页会询问用户单击一个按钮以实际删除的记录。 这是一项重要的安全措施： 当在更新或删除数据等网站执行敏感的操作，应始终执行这些操作使用 POST 操作，而不是 GET 操作。 如果你的站点设置，以便可以使用 GET 操作执行删除操作，任何人都可以传递的 URL，如`http://<server>/DeleteProduct/4`和从数据库中删除任何操作。 通过添加确认和编码页，以便可以只能通过使用 POST 执行删除，向网站添加安全的措施。

    使用以下代码，以首先确认这是 post 操作 ID 不为空来执行实际的删除操作：

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    代码将运行 SQL 语句中删除指定的记录，然后将用户重定向回列表页。
5. 运行*ListProductsForDelete.cshtml*在浏览器。

    ![[image]](5-working-with-data/_static/image9.jpg)
6. 单击**删除**链接，获取的产品之一。 *DeleteProduct.cshtml*显示页以确认你想要删除该记录。
7. 单击**删除**按钮。 删除该产品记录，并使用更新的产品列表刷新页面。

> [!TIP] 
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>连接到数据库
> 
> 你可以连接到两种方法中的数据库。 第一种是使用`Database.Open`方法并指定数据库文件的名称 (较少*.sdf*扩展):
> 
> `var db = Database.Open("SmallBakery");`
> 
> `Open`方法假设。*sdf*文件是在网站的*应用\_数据*文件夹。 此文件夹被专为保存数据。 例如，它具有适当的权限以允许网站以读取和写入数据，以及作为一种安全措施，WebMatrix 不允许访问到文件从该文件夹。
> 
> 第二种方法是使用连接字符串。 连接字符串包含有关如何连接到数据库的信息。 这可以包括文件路径，也可以包括本地或远程服务器，以及用户名和密码以连接到该服务器上的 SQL Server 数据库的名称。 （如果你在集中管理版本的 SQL Server 中保留数据，例如在托管提供商的网站上，则始终使用连接字符串来指定数据库连接信息。）
> 
> 在 WebMatrix 中，连接字符串通常存储在名为 XML 文件*Web.config*。顾名思义，你可以使用*Web.config*你的网站以存储站点的配置信息，包括你的站点可能需要的任意连接字符串的根目录中的文件。 中的连接字符串的示例*Web.config*文件可能如下所示：
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> 在示例中，连接字符串指向的某处的服务器运行的 SQL Server 实例中的数据库 (而不是本地*.sdf*文件)。 你将需要进行替换为相应的名称`myServer`和`myDatabase`，并指定 SQL Server 登录名值`username`和`password`。 （用户名和密码值不一定是相同作为您的 Windows 凭据或你托管提供商已授予你用于登录到其服务器的值。 与管理员一起检查所需的确切值。）
> 
> `Database.Open`方法非常灵活，因为它允许您将数据库名称传递*.sdf*文件或存储在连接字符串的名称*Web.config*文件。 下面的示例演示如何使用连接到数据库的连接字符串，在前面的示例所示：
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> 如前所述，`Database.Open`方法使你可以将数据库名称或连接字符串，传递和它将找出使用哪一个。 在部署时，这是非常有用 （发布） 网站。 你可以使用*.sdf*文件中*应用\_数据*文件夹时你开发和测试你的站点。 然后，将你的站点移到生产服务器时，可以使用中的连接字符串*Web.config*具有同名的文件您*.sdf*文件，但该点添加到托管提供商的数据库&#8212;所有而无需更改代码。
> 
> 最后，如果你想要直接使用连接字符串，则可以调用`Database.OpenConnectionString`方法并传入该实际的连接字符串而不只在中的名称，它*Web.config*文件。 这可能会在其中出于某种原因不具备访问权限的连接字符串的情况下很有用 (或值，例如*.sdf*文件名称) 之前运行页面。 但是，对于大多数情况下，你可以使用`Database.Open`这篇文章中所述。


## <a name="additional-resources"></a>其他资源

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [连接到 SQL Server 或 MySQL 数据库在 WebMatrix 中](https://go.microsoft.com/fwlink/?LinkId=208661)
- [在 ASP.NET 网站中验证用户输入](https://go.microsoft.com/fwlink/?LinkId=253002)
