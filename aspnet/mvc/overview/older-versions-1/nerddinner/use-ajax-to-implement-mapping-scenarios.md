---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: "AJAX 用于实现映射方案 |Microsoft 文档"
author: microsoft
description: "步骤 11 演示如何将 AJAX 映射支持集成到我们 NerdDinner 的应用程序，使得用户创建、 编辑或查看晚餐若要查看 l..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: cc55560ce691826b6d52971b16d0515ed73d72a6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>AJAX 用于实现映射方案
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是个免费的步骤 11 ["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 步骤 11 演示如何将 AJAX 映射支持集成到我们 NerdDinner 的应用程序，使得用户创建、 编辑或查看晚餐若要以图形方式查看 dinner 的位置。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner 步骤 11： 集成 AJAX 映射

我们现在将使我们的应用程序少量更为直观振奋人心通过集成 AJAX 映射支持。 这将启用用户创建、 编辑或查看晚餐若要以图形方式查看 dinner 的位置。

### <a name="creating-a-map-partial-view"></a>创建映射分部视图

我们将使用我们的应用程序中的多个位置中映射功能。 使我们代码干我们将封装在单个部分模板，我们可以重新使用跨多个控制器操作和视图中的常见映射功能。 我们将此分部视图"map.ascx"，创建 \Views\Dinners 目录中。

我们可以创建 map.ascx 部分 \Views\Dinners 目录上右键单击并选择添加-&gt;查看菜单命令。 我们将视图命名为"Map.ascx"、 检查为分部视图，以及指示我们要将其传递一个强类型化"Dinner"模型类：

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

当单击"添加"按钮时，我们将创建我们部分的模板。 然后，我们将更新 Map.ascx 文件具有以下内容：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

第一个&lt;脚本&gt;引用指向 Microsoft 虚拟地球 6.2 映射库。 第二个&lt;脚本&gt;引用到 map.js 我们很快将创建该文件将封装我们常见 Javascript 映射逻辑点。 &lt;Div id ="theMap"&gt;元素是 HTML 容器 Virtual Earth 将用于托管代码图。

接下来是一个嵌入&lt;脚本&gt;包含特定于此视图的两个 JavaScript 函数的块。 第一个函数使用 jQuery 网络型时页已准备好运行客户端脚本执行的函数。 调用 LoadMap() helper 函数，我们将在要加载虚拟地球地图控件我们 Map.js 脚本文件内定义。 第二个函数是回调事件处理程序将 pin 添加到的映射中标识的位置。

请注意，我们如何使用服务器端&lt;%= %&gt;在嵌入的纬度和经度 Dinner 我们想要将映射到 JavaScript 客户端脚本块的内部块。 这是一种用于输出可以 （而无需单独 AJAX 调用返回到服务器以检索值 – 使其更快地） 使用的客户端脚本的动态值的有用技术。 &lt;%= %&gt;视图呈现在服务器上时，将执行块并因此的 html 输出只最终将得到嵌入的 JavaScript 值 (例如： var 纬度 = 47.64312;)。

### <a name="creating-a-mapjs-utility-library"></a>创建 Map.js 实用程序库

让我们现在创建 Map.js 文件，我们可以用来封装我们映射的 JavaScript 功能 （和实现 LoadMap 和 LoadPin 上述两种方法）。 我们可以执行此操作通过 \Scripts 目录在我们的项目中，右键单击，然后选择"添加-&gt;新项"菜单命令，选择 JScript 项目中，并将其命名为"Map.js"。

下面是我们将添加的 JavaScript 代码将与 Virtual Earth 以显示我们映射并为我们晚餐向其中添加位置 pin 进行交互的 Map.js 文件：

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>与创建和编辑窗体中集成映射

我们现在将与我们现有的创建和编辑方案集成的映射支持。 好消息是这是非常简单的待办事项，不要求我们要更改任何我们控制器代码。 由于我们创建和编辑视图共享了常见的"DinnerForm"分部视图实现 dinner 窗体用户界面，因此我们可以在一个位置中添加地图并让使用它的这两种我们创建和编辑方案。

我们只需待办事项是打开 \Views\Dinners\DinnerForm.ascx 分部视图并将其更新为包括部分我们新映射。 下面是添加地图后更新的 DinnerForm 外观 (注意： 以下代码段为简洁起见中省略了 HTML 窗体元素):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

上面部分 DinnerForm 将类型"DinnerFormViewModel"的对象作为其模型类型，（因为它需要一个 Dinner 对象，以及此时若要填充的国家/地区 dropdownlist）。 我们映射部分只要求类型"Dinner"的对象作为其模型类型，并因此当我们呈现地图部分我们是仅将该 Dinner 子属性的 DinnerFormViewModel 向方法传递：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

JavaScript 函数我们已添加到部分使用 jQuery 要附加到"Address"HTML 文本框中的"模糊"事件。 您之前可能听对象"焦点"当用户单击时触发的事件或选项卡到文本框。 相反是当用户退出文本框中，将引发一个"模糊"事件。 在这种情况下，然后绘制我们图上的新地址位置时，上述的事件处理程序清除纬度和经度文本框中值。 我们在 map.js 文件内定义一个回调事件处理程序然后将更新的经度和纬度的文本框，我们使用由虚拟地球基于我们赋予了它的地址返回的值的窗体上。

现在我们运行一次，我们的应用程序并单击"主机 Dinner"选项卡，我们将看到默认的映射与我们的标准 Dinner 窗体元素一起显示：

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

当我们在地址中，键入，然后跳转消失时，映射将会动态更新以显示位置，然后我们事件处理程序将填充的位置值纬度/经度文本框中：

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

如果我们保存新 dinner，然后重新打开以进行编辑，我们将找到加载页面时，显示地图位置：

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

每次更改地址字段后，将更新映射和纬度/经度坐标。

现在，代码图显示了 Dinner 位置，我们还可以从正在可见的文本框中 （因为该映射自动更新其每次输入一个地址） 更改为隐藏的元素更改纬度和经度窗体字段。 待办事项这我们将从使用使用 Html.Hidden() 帮助器方法的 Html.TextBox() HTML 帮助程序切换：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

现在我们窗体将并变得更加用户友好避免显示原始纬度/经度 （此时，仍将它们存储在数据库中每个 Dinner 与）：

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>将与详细信息视图中集成映射

现在，我们已与我们创建和编辑方案集成的映射，让我们还将它与我们的详细信息的方案。 我们只需待办事项是调用&lt;%html.renderpartial("map"); %&gt;详细信息视图中。

下面是源代码到 （具有映射集成） 的完整详细信息视图如下所示：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

现在当某个用户导航到 /Dinners/详细信息 / [id] URL 他们将看到有关 dinner dinner 图上的位置的详细信息和 （完成，但图钉图标，当悬停在显示的标题 dinner 和它的地址），并且使用到的回复 fo AJAX 链接r 它：

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>在我们的数据库和存储库中实现位置搜索

若要完成关闭我们 AJAX 实现，让我们将地图添加到主页上的应用程序允许用户以图形方式搜索晚餐接近它们。

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

首先，我们将实现中我们来高效地执行晚餐基于位置的 radius 搜索的数据库和数据的存储库层的支持。 我们无法使用新[地理空间功能的 SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx)来实现此操作，或者我们可以使用一种 SQL 函数方法，在此处文章中讨论 Gary Dryden 或： [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx)和有关此处使用 LINQ to SQL Rob Conery 博客： [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

若要实现此方法，我们将打开"服务器资源管理器"在 Visual Studio 中，选择 NerdDinner 数据库中，，然后右键单击其下的"函数"子节点，选择创建一个新"标量值函数":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

我们将然后粘贴以下 DistanceBetween 函数：

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

然后，我们将在 SQL Server 中，我们将调用"NearestDinners"创建新的表值函数：

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

此"NearestDinners"表函数使用 DistanceBetween 帮助器函数返回所有晚餐中 100 英里的纬度和经度我们提供它：

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

若要调用此函数，我们将首先打开 LINQ to SQL 设计器通过双击我们 \Models 目录中的 NerdDinner.dbml 文件：

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

然后，我们将将的 NearestDinners 和 DistanceBetween 函数拖动到 LINQ 到 SQL 设计器中，这将导致它们将作为我们 linq 的方法添加到 SQL NerdDinnerDataContext 类：

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

我们可以在我们使用 NearestDinner 函数返回即将到来的 DinnerRepository 类上公开"FindByLocation"查询方法内的指定位置的 100 英里的晚餐：

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>实现基于 JSON 的 AJAX 搜索操作方法

我们现在将实施充分利用新 FindByLocation() 存储库的方法返回可以用于填充地图的 Dinner 数据的列表的控制器操作方法。 我们将使此操作方法返回回 Dinner 数据以 JSON （JavaScript 对象表示法） 格式，以便它可以容易地操作在客户端使用 JavaScript。

若要实现这一点，我们将创建一个新"SearchController"类 \Controllers 目录上右键单击并选择添加-&gt;控制器菜单命令。 然后，我们将实施区内新 SearchController 类，如下面的"SearchByLocation"操作方法：

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

SearchController SearchByLocation 操作方法内部调用 DinnerRespository 若要获取的附近晚餐列表 FindByLocation 方法。 而不是直接向客户端返回 Dinner 对象，不过，它而是返回 JsonDinner 对象。 JsonDinner 类公开 Dinner 属性的子集 (例如： 出于安全考虑，它不会公开为 dinner 答复具有活动的人员的名称)。 它还包括一个 RSVPCount 属性，不在 Dinner – 上存在且其动态计算通过计算与特定 dinner 关联的回复对象的数量。

我们然后将控制器基类上使用 Json() 帮助器方法来返回晚餐使用将基于 JSON 的连网格式的序列。 JSON 是用于表示简单的数据结构以标准文本格式。 下面是 JSON 格式的两个 JsonDinner 对象列表如下所示从我们操作方法返回时的示例：

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>调用使用 jQuery 的基于 JSON 的 AJAX 方法

现在我们就可以更新用于 SearchController SearchByLocation 操作方法的 NerdDinner 应用程序的主页。 待办事项，我们将打开 /Views/Home/Index.aspx 视图模板并更新其具有文本框中，搜索按钮，我们映射和&lt;div&gt;名为 dinnerList 元素：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

然后，我们可以将两个 JavaScript 函数添加到页面：

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

首先加载页面时，第一个 JavaScript 函数加载映射。 向上 JavaScript 的第二个 JavaScript 函数连线单击搜索按钮的事件处理程序。 当按下了按钮它将调用我们将添加到我们的 Map.js 文件的 FindDinnersGivenLocation() JavaScript 函数：

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

此 FindDinnersGivenLocation() 函数将调用映射。Find （) 上虚拟地球控件中心上输入的位置。 当虚拟地球映射服务返回，该映射。Find （） 方法将调用我们作为最后一个参数传递它 callbackUpdateMapDinners 回调方法。

CallbackUpdateMapDinners() 方法是这样的实际工作的位置。 它使用 jQuery 的 $.post() 帮助器方法来执行 AJAX 调用我们 SearchController SearchByLocation() 操作方法 – 将其传递的纬度和经度新为中心的映射的对象。 它定义 $.post() 帮助器方法完成时，将调用一个内联函数，并且从操作方法将传递它使用名为"晚餐"SearchByLocation() 返回 JSON 格式 dinner 结果。 然后，它将通过每个返回 dinner 未 foreach 并使用 dinner 的纬度和经度和其他属性图上添加一个新的 pin。 它还将 dinner 条目添加到地图右侧晚餐 HTML 列表。 它然后电线型图钉和 HTML 列表悬停事件，以便当用户悬停在它们上时，将显示有关 dinner 的详细信息：

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

而且，现在我们运行该应用程序并访问主页上我们将看到一幅地图。 当我们输入一个市县名称图将显示即将到来晚餐办公电话附近：

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

悬停在 dinner 将显示有关它的详细信息。

单击 Dinner 标题在气泡或在 HTML 列表中的右侧将导航我们到 dinner – 我们可以然后 （可选) RSVP 为：

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>下一步

现在，我们已实施我们 NerdDinner 的应用程序的所有应用程序的功能。 让我们现在看如何我们可以启用自动单元测试它。

>[!div class="step-by-step"]
[上一页](use-ajax-to-deliver-dynamic-updates.md)
[下一页](enable-automated-unit-testing.md)
