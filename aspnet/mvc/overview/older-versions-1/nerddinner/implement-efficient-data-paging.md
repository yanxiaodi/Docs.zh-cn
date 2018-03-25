---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: "实现高效数据分页 |Microsoft 文档"
author: microsoft
description: "步骤 8 演示如何将分页支持添加到我们 /Dinners URL，以便而不是显示的一次晚餐 1000 秒内，我们将仅显示在 10 即将到来晚餐..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0b0fba604f97d3bb72d2d403e643b422b9ce48bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="implement-efficient-data-paging"></a>实现高效数据分页
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是一个免费的步骤 8 ["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 步骤 8 演示如何将分页支持添加到我们 /Dinners URL，以便而不是显示的一次晚餐 1000 秒内，我们将仅显示一次-10 即将到来晚餐并允许最终用户返回页面和 SEO 友好方式向前移动整个列表。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner 步骤 8： 分页支持

如果我们的站点操作成功，将会有数千个即将到来的晚餐。 我们需要确保我们的 UI 进行缩放以处理所有这些晚餐，并允许用户对其进行浏览。 若要启用此功能，我们将添加到的分页支持我们*/Dinners* URL，因此的晚餐 1000 秒内显示在一次，我们将仅显示一次-10 即将到来晚餐和允许最终用户页后和向前移动的完整列表SEO 友好的方式。

### <a name="index-action-method-recap"></a>Index （） 操作方法概述

我们 DinnersController 类中的 index （） 操作方法当前看起来像下面：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

当请求*/Dinners* URL，它将检索所有即将发布晚餐的列表，然后呈现所有的效果的列表：

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>了解 IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;* 是作为.NET 3.5 的一部分引入了 LINQ 的接口。 它使我们可以充分利用实现分页支持的功能强大的"延迟执行"方案。

在我们 DinnerRepository 我们要返回 IQueryable&lt;Dinner&gt;从我们 FindUpcomingDinners() 方法的序列：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

IQueryable&lt;Dinner&gt;我们 FindUpcomingDinners() 方法返回的对象所封装查询以从我们使用 LINQ to SQL 的数据库中检索 Dinner 对象。 重要的是，它不会执行对数据库的查询，直到我们尝试访问/循环访问的数据在查询中，或直到我们对其调用 tolist （） 方法。 调用我们 FindUpcomingDinners() 方法的代码可以根据需要选择将更多"链接"操作/筛选器添加到 IQueryable&lt;Dinner&gt;之前执行查询的对象。 LINQ to SQL 是然后足够智能，可执行对数据库的组合的查询请求数据时。

若要实现分页逻辑我们可以更新我们 DinnersController index （） 操作方法，以便它将其他"Skip"和"使用"运算符应用于返回 IQueryable&lt;Dinner&gt;之前对其调用 tolist （） 的序列：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

上面的代码会跳过前 10 的即将到来晚餐在数据库中，并返回然后 20 晚餐。 LINQ to SQL 是足够智能，可构造执行此跳过逻辑 SQL 数据库 – 中，不能在 web 服务器的优化的 SQL 查询。 这意味着，即使在数据库中具有数以百万计的即将到来的晚餐我们，我们希望的 10 将检索 （使其成为高效且可伸缩） 此请求的一部分。

### <a name="adding-a-page-value-to-the-url"></a>将"页"值添加到 URL

而不是硬编码的特定页范围，我们将需要我们 Url 以包括"页"参数，该值指示用户正在请求的 Dinner 范围。

#### <a name="using-a-querystring-value"></a>使用查询字符串值

下面的代码演示如何更新我们 index （） 的操作方法中支持的查询字符串参数和实现类似的 Url */Dinners？ 页 = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

上面的 index （） 操作方法具有一个名为"页"参数。 参数声明为可以为 null 的整数 (这是什么 int？ 指示)。 这意味着， */Dinners？ 页 = 2* URL 将会导致"2"作为参数值传递值。 */Dinners* URL （不带查询字符串值） 将导致将传递 null 值。

我们将乘以页值 （在这种情况下 10 行） 的页大小来确定多少晚餐跳过。 我们将使用[C# null"合并"运算符 （？）](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx)处理可以为 null 的类型时，这非常有用。 如果页参数为 null，上面的代码将页分配值为 0。

#### <a name="using-embedded-url-values"></a>使用嵌入的 URL 值

使用查询字符串值的替代方法是嵌入的实际 URL 本身中的页参数。 例如： */Dinners/Page/2*或*/晚餐/2*。 ASP.NET MVC 包括一个功能强大的 URL 路由引擎，可以轻松地支持与此类似的方案。

我们可以注册自定义的路由规则的映射任何传入的 URL 或 URL 格式，我们想要任何控制器类或操作方法。 我们只需待办事项是打开我们项目中的 Global.asax 文件：

![](implement-efficient-data-paging/_static/image2.png)

然后再注册新的映射规则使用如路由首次调用的 MapRoute() 帮助器方法。MapRoute() 下面：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

更高版本，我们将注册一个名为"UpcomingDinners"的新路由规则。 我们将它具有以下 URL 格式，表示"晚餐/页 / {页}"– 其中 {page} 是在 URL 中嵌入一个参数值。 MapRoute() 方法的第三个参数指示我们应映射匹配到 DinnersController 类上的 index （） 操作方法的这种格式的 Url。

我们可以使用我们在之前我们查询字符串的方案 – 但现在我们"页"参数将来自的 URL 和查询字符串不完全相同的 index （） 代码：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

现在我们运行应用程序并在键入*/Dinners*我们将看到前 10 个即将到来晚餐：

![](implement-efficient-data-paging/_static/image3.png)

当我们键入*/Dinners/Page/1*我们将看到晚餐的第二页：

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>添加页导航用户界面

完成我们分页的方案的最后一步是实现"下一步"和"早期/之前"中的导航 UI 我们查看模板，以使用户能够轻松地跳过 Dinner 数据。

若要正确实现此操作，我们将需要知道的晚餐总数在数据库中，以及如何数据的多个页面这将转换为。 然后，我们将需要以计算当前请求的"页"值的开头或末尾数据，并显示或隐藏"上一页"和"下一页"UI 相应地。 在我们 index （） 操作方法中，我们无法实现此逻辑。 或者我们可以向我们的更多重复使用的方式封装此逻辑的项目添加一个帮助器类。

下面是一个简单的"PaginatedList"帮助程序类，派生自列表&lt;T&gt;集合类内置于.NET Framework。 它可实现可用于分页 IQueryable 数据的任何序列的可重用集合类。 在我们 NerdDinner 应用程序中我们将用它处理通过 IQueryable&lt;Dinner&gt;结果，但它无法轻松地对使用 IQueryable&lt;产品&gt;或 IQueryable&lt;客户&gt;导致其他应用程序方案：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

此公式计算上面注意到，然后公开属性如"PageIndex"、"PageSize"、"TotalCount"和"TotalPages"。 它还公开两个帮助器属性"HasPreviousPage"和"HasNextPage"，指示集合中的数据的页的开头或原始序列的末尾。 上面的代码将导致两个 SQL 查询要运行的第一个要检索的 Dinner 对象总数的计数 （这不会返回的对象 – 它而是执行返回一个整数的"选择计数"语句），第二个检索只需的行我们需要从我们的数据的当前页的数据库的数据。

然后，我们可以更新我们 DinnersController.Index() helper 方法，以创建 PaginatedList&lt;Dinner&gt;从我们 DinnerRepository.FindUpcomingDinners() 结果，并将它传递给我们的视图模板：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

然后，我们可以更新 \Views\Dinners\Index.aspx 视图模板以继承 ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt;而不是 ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;，然后将以下代码添加到我们查看模板以显示或隐藏下一步和以前导航用户界面底部：

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

请注意上面如何我们将使用 Html.RouteLink() 帮助器方法来生成我们超链接。 此方法是类似于我们以前曾使用过的 Html.ActionLink() 帮助器方法。 不同之处在于，我们要生成使用"UpcomingDinners"路由规则我们我们 Global.asax 文件中设置的 URL。 这将确保我们将为我们的 index （） 操作方法中具有格式生成 Url: */Dinners/页 / {page}* – 其中 {页} 值是我们提供上面基于当前 PageIndex 的变量。

现在我们运行我们的应用程序时试我们将看到一次 10 晚餐在我们的浏览器中：

![](implement-efficient-data-paging/_static/image5.png)

我们还必须&lt; &lt; &lt;和&gt; &gt; &gt;导航，我们便可以跳过向前和向后通过我们的数据使用搜索引擎可访问的 Url 的页面底部的用户界面：

![](implement-efficient-data-paging/_static/image6.png)

| **端主题内容： 了解 IQueryable 的含义&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt;是非常强大的功能，使各种有趣的延迟的执行方案 （如分页和组合基于查询）。 作为与所有强大的功能，你想要谨慎使用它，并确保不被滥用。 务必要识别该返回 IQueryable&lt;T&gt;从存储库的结果使调用代码可以追加到其中，连锁的运算符方法，并使参与 ultimate 查询执行。 如果你不希望提供此功能的调用代码，则应返回备份 IList&lt;T&gt;或 IEnumerable&lt;T&gt;结果-包含已执行查询的结果。 分页方案中，这将要求你将实际数据分页逻辑推入所调用的存储库方法。 在此方案中，我们可能会更新我们 FindUpcomingDinners() finder 方法，请返回 PaginatedList 的签名： PaginatedList&lt; Dinner&gt; FindUpcomingDinners （int pageIndex，int pageSize） {} 或回到是 IList&lt;Dinner&gt;，然后使用"totalCount"out param 返回晚餐的总计数： IList&lt;Dinner&gt; FindUpcomingDinners （int pageIndex，int pageSize，出 int totalCount） {} |

### <a name="next-step"></a>下一步

让我们现在看一下我们可以如何添加身份验证和授权支持到我们的应用程序。

>[!div class="step-by-step"]
[上一页](re-use-ui-using-master-pages-and-partials.md)
[下一页](secure-applications-using-authentication-and-authorization.md)
