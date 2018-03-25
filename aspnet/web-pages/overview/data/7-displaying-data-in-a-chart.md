---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: "在具有所需的 ASP.NET Web Pages (Razor) 的图表中显示数据 |Microsoft 文档"
author: microsoft
description: "本章介绍如何在图表中显示数据。 在前面的章节中，您学习了如何以手动并在网格中显示数据。 本章介绍..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: f252b74bc42d0ea65b8b1150973c4f3c50cc9cf4
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>具有 ASP.NET Web 页 (Razor) 的图表中显示数据
====================
by [Microsoft](https://github.com/microsoft)

> 此文章介绍了如何使用图表使用在 ASP.NET Web 页 (Razor) 网站中显示数据`Chart`帮助器。
> 
> **你将了解**:
> 
> - 如何在图表中显示数据。
> - 如何使用内置的主题的样式图表。
> - 如何保存图表以及如何其缓存以提高性能。
> 
> 这些是 ASP.NET 编程文章中引入的功能：
> 
> - `Chart`帮助器。
> 
> > [!NOTE]
> > 这篇文章中的信息适用于 ASP.NET Web Pages 1.0 和 Web Pages 2。


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>图表帮助器

如果你想要以图形方式显示数据，你可以使用`Chart`帮助器。 `Chart`帮助器可以呈现的图像中不同的图表类型显示数据。 它支持许多选项对于格式设置和标签。 `Chart`帮助器可以呈现 30 多个类型的图表，包括所有类型的图表，您可能需要从 Microsoft Excel 或其他工具熟悉&#8212;面积图、 条形图、 柱形图、 折线图中和饼图，以及的详细信息专用的图表，如股价图。

| **面积图**![说明： 面积图类型的图片](7-displaying-data-in-a-chart/_static/image1.jpg) | **条形图**![说明： 图片的条形图类型](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **柱形图**![说明： 图片的柱形图图表类型](7-displaying-data-in-a-chart/_static/image3.jpg) | **折线图**![说明： 图片线图图表类型](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **饼图**![说明： 饼形图类型的图片](7-displaying-data-in-a-chart/_static/image5.jpg) | **股价图**![说明： Stock 图表类型的图片](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>图表元素

图表显示数据和其他元素，如图例、 轴、 序列和等等。 下图显示了许多你可以自定义当你使用的图表元素`Chart`帮助器。 这篇文章演示如何设置一些 （并非所有） 的这些元素。

![描述： 图片显示的图表元素](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>从数据创建图表

在图表中显示的数据可以是从一个数组，从一个数据库，从返回的结果或从 XML 文件中的数据。

### <a name="using-an-array"></a>使用数组

中所述[ASP.NET 网页编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkId=202890)，数组，您可以在单个变量中存储的相似的项目集合。 可以使用数组以包含你想要包括在图表中的数据。

此过程演示如何创建图表数据在数组中，从使用默认图表类型。 它还演示如何以显示页中的图表。

1. 创建一个名为的新文件*ChartArrayBasic.cshtml*。
2. 将现有内容替换为以下： 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    首先，该代码将创建一个新图，并设置其宽度和高度。 使用指定图表标题`AddTitle`方法。 若要添加数据，请使用`AddSeries`方法。 在此示例中，你使用`name`， `xValue`，和`yValues`参数`AddSeries`方法。 `name`参数显示在图表图例中。 `xValue`参数包含沿着图表水平轴显示的数据的数组。 `yValues`参数包含用于绘制图表的垂直点的数据的数组。

    `Write`方法实际上将呈现的图表。 在这种情况下，这是因为未指定一个图表类型，`Chart`帮助器呈现其默认图表，这是柱形图。
3. 在浏览器中运行页面。 浏览器显示的图表。 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>使用适用于图表数据数据库查询

如果你想要显示的图表的信息在数据库中，你可以运行数据库查询，然后使用结果中的数据创建图表。 此过程演示如何读取和显示文章中创建的数据库中的数据[使用 ASP.NET Web Pages 站点中的数据库的简介](https://go.microsoft.com/fwlink/?LinkId=202893)。

1. 添加*应用\_数据*网站如果尚不存在文件夹的根目录的文件夹。
2. 在*应用\_数据*文件夹中，添加名为的数据库文件*SmallBakery.sdf*中描述的[使用 ASP.NET Web Pages 站点中的数据库的简介](https://go.microsoft.com/fwlink/?LinkId=202893).
3. 创建一个名为的新文件*ChartDataQuery.cshtml*。
4. 将现有内容替换为以下：   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    使用代码首先打开 SmallBakery 数据库，并将其分配给一个名为变量`db`。 此变量表示`Database`可以用于读取和写入到数据库的对象。 接下来，代码会运行 SQL 查询，以便获取的名称和每个产品的价格。 代码将创建新图表并传递给它的数据库查询，通过调用图表的`DataBindTable`方法。 此方法采用两个参数：`dataSource`参数是从查询中，数据和`xField`参数允许你设置的数据列用于图表的 x 轴。

    作为使用的替代方法`DataBindTable`方法，你可以使用`AddSeries`方法`Chart`帮助器。 `AddSeries`方法允许你设置`xValue`和`yValues`参数。 例如，而不是使用`DataBindTable`方法如下：

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    你可以使用`AddSeries`方法如下：

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    同时呈现相同的结果。 `AddSeries`方法是更灵活，因为你可以更显式指定的图表类型和数据但`DataBindTable`方法是易于使用，如果你不需要额外的灵活性。
5. 在浏览器中运行页面。 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>使用 XML 数据

图表的第三个选项是图表的数据使用 XML 文件。 这要求的 XML 文件还具有架构文件 (*.xsd*文件)，它描述 XML 结构。 此过程演示如何从 XML 文件读取数据。

1. 在*应用\_数据*文件夹中，创建名为的新 XML 文件*data.xml*。
2. 将现有的 XML 替换为以下内容，这是有关在一家虚构公司的员工的一些 XML 数据。 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. 在*应用\_数据*文件夹中，创建名为的新 XML 文件*data.xsd*。 (请注意，扩展此时间是*.xsd*。)
4. 现有的 XML 替换为以下代码： 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. 在网站的根目录，创建一个名为的新文件*ChartDataXML.cshtml*。
6. 将现有内容替换为以下： 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    此代码首先创建`DataSet`对象。 此对象用于管理从 XML 文件中读取和组织根据架构文件中的信息的数据。 (请注意代码的顶部包括语句`using SystemData`。 这为了能够使用所必需`DataSet`对象。 有关详细信息，请参阅[&quot;使用&quot;语句和完全限定的名称](#SB_UsingStatements)这篇文章中更高版本。)

    接下来，该代码创建`DataView`对象基于数据集。 数据视图提供图表可以将绑定到对象&#8212;即读取和绘制。 图表将绑定到数据使用`AddSeries`方法，与你此前看到的时图表数组数据，但这次`xValue`和`yValues`参数设置为`DataView`对象。

    此示例还演示了如何指定特定的图表类型。 在添加数据时`AddSeries`方法，`chartType`参数也将设置为显示饼图。
7. 在浏览器中运行页面。 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP] 
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Using"语句和完全限定的名称
> 
> 带有 Razor 语法的 ASP.NET Web Pages 基于.NET Framework 包括的数千个组件 （类）。 若要使其易于管理，若要使用所有这些类，它们组织成*命名空间*，这是有些类似库。 例如，`System.Web`命名空间包含支持的浏览器/服务器通信的类`System.Xml`命名空间包含用于创建和读取 XML 文件的类和`System.Data`命名空间包含用于处理的类使用数据。
> 
> 若要访问.NET Framework 中的任何给定的类，代码需要知道不只是类名称，但也在类所在的命名空间。 例如，若要使用`Chart`帮助器，代码需要找到`System.Web.Helpers.Chart`类，它组合了命名空间 (`System.Web.Helpers`) 与类名称 (`Chart`)。 这称为类的*完全限定*名称&#8212;其完整的明确位置内 vastness 的.NET framework。 在代码中，这将显示如下：
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> 但是，很繁琐 （，而且容易出错） 不必每次你想要引用的类或帮助器使用这些 long、 完全限定名称。 因此，若要更加轻松地使用的类名，您可以*导入*你感兴趣，这通常是命名空间是只需少量从.NET Framework 中的多个命名空间。 如果你已导入命名空间，则可以使用只是类名称 (`Chart`) 而不是完全限定名称 (`System.Web.Helpers.Chart`)。 当你的代码运行和遇到的类名称时，它可以查看在刚导入以查找该类的命名空间中。
> 
> 当你使用的 ASP.NET Web Pages 使用 Razor 语法创建网页时，你通常使用同一组类的每个时间，包括`WebPage`类，各种帮助器，依次类推。 若要保存的工作的导入相关的命名空间，每次创建网站时，ASP.NET 配置以便自动导入一组核心命名空间为每个网站。 这就是为什么你尚未需要处理命名空间或导入最现在;你已使用过的所有类都是已经为你导入的命名空间中。
> 
> 但是，有时你需要使用不会自动为您导入的命名空间中的类。 在这种情况下，你可以使用该类的完全限定名称，也可以手动导入包含类的命名空间。 若要导入命名空间，请使用`using`语句 (`import`在 Visual Basic 中)，如前面所看到的示例中的文章。
> 
> 例如，`DataSet`类位于`System.Data`命名空间。 `System.Data`命名空间不会自动提供给 ASP.NET Razor 页。 因此，若要使用`DataSet`类使用其完全限定的名称，则可以使用如下代码：
> 
> `var dataSet = new System.Data.DataSet();`
> 
> 如果你需要使用`DataSet`重复类可以导入如下的命名空间，然后使用在代码中的仅类名称：
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> 你可以添加`using`你想要引用任何其他.NET Framework 命名空间的语句。 但是，如所述，你无需执行此操作通常情况下，因为您将使用的类的大多数都都会自动导入由 ASP.NET 在中使用的命名空间中*.cshtml*和*.vbhtml*页。


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>显示 Web 页面内的图表

在示例中你已了解到目前为止，创建一个图表，然后再将图表呈现直接向图形的浏览器。 在许多情况下，不过，想要显示一个图表作为一部分页上，而不仅仅是通过在浏览器本身。 若要执行此操作需要两步过程。 第一步是创建页生成图表，如已看到的一样。

第二步是在另一个页中显示生成的映像。 若要显示图像，你使用 HTML`<img>`中相同的元素，就会显示任何图像。 但是，而不是引用*.jpg*或*.png*文件，`<img>`元素引用*.cshtml*文件，其中包含`Chart`帮助器，创建图表。 显示页的运行时，则`<img>`元素获取的输出`Chart`帮助器，并将呈现的图表。

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. 创建名为的文件*ShowChart.cshtml*。
2. 将现有内容替换为以下： 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    该代码使用`<img>`元素显示的图表的更早版本中，你创建*ChartArrayBasic.cshtml*文件。
3. 在浏览器中运行网页。 *ShowChart.cshtml*文件显示基于中包含的代码的图表图像*ChartArrayBasic.cshtml*文件。

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>设置图表样式

`Chart`帮助器支持大量的选项允许你自定义图表的外观。 你可以设置颜色、 字体、 边框和等等。 自定义图表的外观的简单办法是使用*主题*。 主题是信息的集合以指定如何呈现使用字体、 颜色、 标签、 调色板、 边框和效果的图表。 （请注意图表的样式不指示的图表类型。）

下表列出了内置的主题。

| 主题 | 描述 |
| --- | --- |
| `Vanilla` | 白色背景上显示红色柱形。 |
| `Blue` | 显示蓝色蓝色渐变背景上的列。 |
| `Green` | 显示蓝色绿色渐变背景上的列。 |
| `Yellow` | 黄色渐变背景上显示橙色的列。 |
| `Vanilla3D` | 白色背景上显示三维红色柱形。 |

你可以指定要在创建新图表时使用的主题。

1. 创建一个名为的新文件*ChartStyleGreen.cshtml*。
2. 在页中的现有内容替换为以下：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    此代码是前面示例的数据，使用数据库，但将添加相同`theme`参数在创建时`Chart`对象。 下面显示了已更改的代码：

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. 在浏览器中运行页面。 你看到相同的数据之前，但该图表看上去更加完善： 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>保存图表

当你使用`Chart`作为你的帮助器目前为止看到在本文中，帮助器在每次调用它时都会重新创建从头图表。 如有必要，图表的代码还重新查询数据库或重新读取要获取的数据的 XML 文件。 在某些情况下，执行此操作可以是一个复杂的操作，例如如果你要查询的数据库较大，或者，如果 XML 文件包含大量数据。 即使图表不涉及大量数据，动态创建映像的过程将占用服务器资源，并且如果很多用户请求或多个显示图表的页面，可以影响你的网站的性能。

若要帮助你减少创建的图表的潜在性能影响，你可以创建图表第一个时间用到它并将其保存。 当图表再次需要时，而不是重新生成它，可以提取已保存的版本和呈现的。

你可以通过以下方式保存图表：

- 缓存 （在服务器上） 的计算机内存中的图表。
- 将图表保存为图像文件。
- 为 XML 文件中保存该图表。 此选项可让你在保存之前修改图表。

### <a name="caching-a-chart"></a>缓存图表

在创建图表后，你可以对其进行缓存。 缓存图表意味着，它不一定是重新创建，如果它需要再次显示。 在缓存中保存图表时，你赋予它必须是唯一的该图表的密钥。

如果服务器运行低内存，可能会删除保存到缓存的图表。 此外，如果你的应用程序重启出于任何原因清除了缓存。 因此，标准的方法，可以使用缓存的图表是以始终首先检查它是否可在缓存中，并且如果没有，则可创建或重新创建它。

1. 在你的网站的根目录，创建名为的文件*ShowCachedChart.cshtml*。
2. 将现有内容替换为以下： 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>`标记包含`src`属性指向*ChartSaveToCache.cshtml*文件并将密钥传递给查询字符串形式的页。 该密钥包含值&quot;myChartKey&quot;。 *ChartSaveToCache.cshtml*文件包含`Chart`帮助程序创建图表。 你将在稍后创建此页。

    末尾的页是一个名为页的链接*ClearCache.cshtml*。 这是一个页，你还将创建很快。 你需要*ClearCache.cshtml*仅用于测试此示例中的缓存-不是链接或使用缓存的图表时您通常要包括的页。
3. 在你的网站的根目录，创建一个名为的新文件*ChartSaveToCache.cshtml*。
4. 将现有内容替换为以下：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    代码将首先检查是否任何内容传递作为查询字符串中的密钥值。 如果这样，该代码尝试通过调用读取缓存图表`GetFromCache`方法并将其传递该密钥。 如果事实证明，无需进行任何 （可能发生第一次请求图表） 该注册表项下缓存中，该代码将按常规方式创建图表。 完成图表后，代码将其保存到缓存通过调用`SaveToCache`。 该方法要求密钥 （以便图表可以请求更高版本），并且应在缓存中保存图表的时间量。 （你将缓存图表的确切时间都依赖于频率认为它所代表的数据可能会更改。）`SaveToCache`方法还要求`slidingExpiration`参数&#8212;如果此值设置为 true，超时计数器将重置每次访问图表。 在这种情况下，这实际上意味着数据透视图报表的缓存条目过期 2 分钟后用户访问图表的最后一个时间。 （相对过期机制的替代方法是绝对过期，这意味着缓存条目将过期恰好 2 分钟之后它放入缓存中，无论何种频率已被访问,。）

    最后，代码使用`WriteFromCache`方法以提取和呈现来自缓存的图表。 请注意，此方法是外部`if`检查缓存中，因为它将从缓存获取图表，该图表已开始时还是必须生成并保存在缓存中的块。

    请注意，在此示例中，`AddTitle`方法包括时间戳。 (它将当前日期和时间添加&#8212; `DateTime.Now` &#8212;标题。)
5. 创建一个名为的新页*ClearCache.cshtml*和其内容替换为以下代码：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    此页使用`WebCache`用于删除缓存在图表帮助*ChartSaveToCache.cshtml*。 如前文所述，通常不必具有如下页面。 你要创建此处只是为了更加轻松地测试缓存。
6. 运行*ShowCachedChart.cshtml*在浏览器中的网页。 页面显示基于中包含的代码的图表图像*ChartSaveToCache.cshtml*文件。 记下的时间戳显示图表标题中。 

    ![描述： 时间戳为图表标题中的基本图表图片](7-displaying-data-in-a-chart/_static/image13.jpg)
7. 关闭浏览器。
8. 运行*ShowCachedChart.cshtml*试。 请注意，时间戳是相同和前面一样，这指示图表未重新生成，但已改为从缓存中读取。
9. 在*ShowCachedChart.cshtml*，单击**清除缓存**链接。 将进入*ClearCache.cshtml*，该报告已清除缓存。
10. 单击**返回到 ShowCachedChart.cshtml**链接，或重新运行*ShowCachedChart.cshtml*从 WebMatrix。 请注意，此时间已更改的时间戳，因为缓存已被清除。 因此，代码必须重新生成图表并将其放回缓存。

### <a name="saving-a-chart-as-an-image-file"></a>将图表另存为图像文件

你还可以将图表保存为图像文件 (例如，作为*.jpg*文件) 在服务器上。 您然后可以使用图像文件的任何图像那样。 优点是该文件是存储而不是保存到临时缓存。 你可以将新的图表图像保存在不同时间 （例如，每隔一小时），然后保持的随着时间的推移发生的更改的永久记录。 请注意，你必须确保你的 web 应用程序有权将文件保存到文件夹，你想要将图像文件放在服务器上。

1. 在你的网站的根目录，创建名为的文件夹 *\_ChartFiles*如果不存在。
2. 在你的网站的根目录，创建一个名为的新文件*ChartSave.cshtml*。
3. 将现有内容替换为以下：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    代码首先检查以查看是否*.jpg*文件存在通过调用`File.Exists`方法。 如果文件不存在，代码将创建一个新`Chart`从数组。 这一次，该代码调用`Save`方法并传递`path`参数来指定的文件路径和文件名称保存该图表的位置。 中的页上，正文`<img>`元素使用的路径指向*.jpg*文件以显示。
4. 运行*ChartSave.cshtml*文件。
5. 返回到 WebMatrix。 请注意，映像文件命名为*chart01.jpg*已保存在 *\_ChartFiles*文件夹。

### <a name="saving-a-chart-as-an-xml-file"></a>将图表另存为 XML 文件

最后，你可以将图表保存为服务器上的 XML 文件。 通过缓存图表或保存到文件的图表中使用此方法的一个优点是，无法显示图表，如果您希望对之前修改 XML。 你的应用程序必须具有你想要将图像文件放在服务器上文件夹的读/写权限。

1. 在你的网站的根目录，创建一个名为的新文件*ChartSaveXml.cshtml*。
2. 将现有内容替换为以下：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    此代码将类似于你此前看到的针对存储在缓存中，图表，只不过它使用的 XML 文件的代码。 代码首先检查以查看 XML 文件是否存在通过调用`File.Exists`方法。 如果存在该文件，代码将创建一个新`Chart`对象并将传递作为文件名`themePath`参数。 这将创建基于任何位于 XML 文件中的图表。 如果 XML 文件尚不存在，代码将创建照常图，然后调用`SaveXml`将其保存。 使用呈现图表`Write`方法，当你如上所示。

    与显示缓存的页面，此代码图表标题中包含时间戳。
3. 创建一个名为的新页*ChartDisplayXMLChart.cshtml*并向其中添加以下标记： 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. 运行*ChartDisplayXMLChart.cshtml*页。 图表将显示。 记下在图表的标题中的时间戳。
5. 关闭浏览器。
6. 在 WebMatrix 中，右键单击 *\_ChartFiles*文件夹中，单击**刷新**，，然后打开文件夹。 *XMLChart.xml*此文件夹中的文件由`Chart`帮助器。 

    ![描述： 显示的图表帮助程序创建的 XMLChart.xml 文件 _ChartFiles 文件夹。](7-displaying-data-in-a-chart/_static/image14.jpg)
7. 运行*ChartDisplayXMLChart.cshtml*再次页。 该图表显示相同的时间戳作为第一次运行页面。 这是因为正在从以前保存的 XML 生成图表。
8. 在 WebMatrix 中，打开 *\_ChartFiles*文件夹并删除*XMLChart.xml*文件。
9. 运行*ChartDisplayXMLChart.cshtml*页上一次。 此时，由于更新时间戳`Chart`帮助程序不得不重新创建 XML 文件。 如果你想，检查 *\_ChartFiles*文件夹，然后请注意，XML 文件已恢复。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源

- [使用在 ASP.NET 网页中的数据库的简介页站点](https://go.microsoft.com/fwlink/?LinkId=202893)
- [使用缓存在 ASP.NET Web 页站点中以提高性能](https://go.microsoft.com/fwlink/?LinkId=202903)
- [图表类](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99))（MSDN 上的 ASP.NET Web Pages API 参考）
