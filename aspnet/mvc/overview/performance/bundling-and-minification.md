---
uid: mvc/overview/performance/bundling-and-minification
title: "绑定和缩减 |Microsoft 文档"
author: Rick-Anderson
description: "绑定和缩减是两种技术可以在 ASP.NET 4.5 中使用，来提高请求加载时间。 绑定和缩减提高 reducin 加载时间..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 7192481de46c36f7de71164766e68afdbba74f6d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="bundling-and-minification"></a>绑定和缩减
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 绑定和缩减是两种技术可以在 ASP.NET 4.5 中使用，来提高请求加载时间。 绑定和缩减提高加载时通过减少到服务器的请求数和减少的大小的请求资产 （如 CSS 和 JavaScript。）


大部分当前主要浏览器都限制的数量[同时连接](http://www.browserscope.org/?category=network)每六个每个主机名。 这意味着，在处理六个请求，而在主机上的资产的其他请求将排队等候，由浏览器。 下图中，在 IE F12 开发人员工具网络选项卡显示所需的一个示例应用程序的关于视图资产的计时。

![B/M](bundling-and-minification/_static/image1.png)

灰色条显示该请求排队等待的六个连接限制上的浏览器的时间。 黄色的条为至第一字节的请求时间，即发送请求和从服务器接收第一个响应所花费的时间。 蓝色条显示从服务器收到的响应数据所花的时间。 你可以双击资产中，以获取详细的计时信息。 例如下, 图显示加载的计时详细信息*/Scripts/MyScripts/JavaScript6.js*文件。

![](bundling-and-minification/_static/image2.png)

前面的图所示**启动**事件，其中给出了由于浏览器的请求已排入队列的时间限制同时连接数。 在这种情况下，请求已排队等待 46 毫秒内等待另一个请求完成。

## <a name="bundling"></a>绑定

绑定是可以轻松地合并或捆绑到单个文件的多个文件的 ASP.NET 4.5 中的新功能。 你可以创建 CSS、 JavaScript 和其他捆绑包。 少选一些文件意味着少数几个 HTTP 请求，可以提高第一个页面加载性能。

下图显示的相同的计时视图的显示以前，但这一次绑定和缩减启用具有的关于视图。

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>缩减

缩减执行各种不同的代码优化脚本或 css，如删除不必要的空白区域和注释和缩短为一个字符的变量名。 请考虑下面的 JavaScript 函数。

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

后缩减，该函数将都会减少到以下：

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

除了删除注释和多余的空格，以下参数和变量名已重命名 （缩短），如下所示：

| **源语言** | **重命名** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>影响的绑定和缩减

下表显示单独列出所有资产，然后在示例程序中使用绑定和缩减 (B/M) 之间的几个重要区别。

|  | **使用 B/M** | **而无需 B/M** | **更改** |
| --- | --- | --- | --- |
| **文件请求** | 9 | 34 | 256% |
| **KB Sent** | 3.26 | 11.92 | 266% |
| **KB Received** | 388.51 | 530 | 36% |
| **加载时间** | 510 MS | 780 MS | 53% |

发送的字节数必须是与绑定因为浏览器是与它们在请求应用的 HTTP 标头非常详细，大大减少。 接收的字节减少不是最大因为最大的文件 (*Scripts\jquery-ui-1.8.11.min.js*和*Scripts\jquery-1.7.1.min.js*) 已缩减。 注意： 的使用的示例程序计时[Fiddler](http://www.fiddler2.com/fiddler2/)工具来模拟慢速网络。 (从 Fiddler**规则**菜单上，选择**性能**然后**模拟调制解调器速度**。)

## <a name="debugging-bundled-and-minified-javascript"></a>调试捆绑和缩减的 JavaScript

很容易地调试你在开发环境中的 JavaScript (其中[编译元素](https://msdn.microsoft.com/library/s10awwz0.aspx)中*Web.config*文件设置为`debug="true"`) 因为 JavaScript 文件中未绑定或缩减。 你还可以调试在其中捆绑和缩减的你 JavaScript 文件的发布版本。 使用 IE F12 开发人员工具，调试使用以下方法缩减捆绑中包含的 JavaScript 函数：

1. 选择**脚本**选项卡上，然后选择**开始调试**按钮。
2. 选择包含你想要使用的资产按钮调试 JavaScript 函数捆绑。  
    ![](bundling-and-minification/_static/image4.png)
3. 通过选择设置格式缩减的 JavaScript**配置按钮** ![](bundling-and-minification/_static/image5.png)，然后选择**格式 JavaScript**。
4. 在**搜索脚本**t 输入框中，选择你想要调试的函数的名称。 在下图中， **AddAltToImg**已进入**搜索脚本**t 输入的框。  
    ![](bundling-and-minification/_static/image6.png)

有关使用 F12 开发人员工具进行调试的详细信息，请参阅 MSDN 文章[使用 F12 开发人员工具为调试 JavaScript 错误](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx)。

## <a name="controlling-bundling-and-minification"></a>控制绑定和缩减

启用或禁用通过设置中的调试属性的值绑定和缩减[编译元素](https://msdn.microsoft.com/library/s10awwz0.aspx)中*Web.config*文件。 在下面的 XML，`debug`设置为 true，绑定和缩减处于禁用状态。

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

若要启用绑定和缩减，设置`debug`为"false"的值。 您可以重写*Web.config*设置，并`EnableOptimizations`属性`BundleTable`类。 以下代码启用绑定和缩减，并将替代在任何设置*Web.config*文件。

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> 除非`EnableOptimizations`是`true`或中的调试属性[编译元素](https://msdn.microsoft.com/library/s10awwz0.aspx)中*Web.config*文件设置为`false`，不会捆绑在一起或缩减的文件。 此外，将不使用的文件的.min 版本，将选择的完整的调试版本。 `EnableOptimizations`重写中的调试属性[编译元素](https://msdn.microsoft.com/library/s10awwz0.aspx)中*Web.config*文件


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>使用绑定和缩减与 ASP.NET Web 窗体和 Web 页

- 有关 Web 页，请参阅博客文章[添加到网页站点的 Web 优化](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx)。
- Web 窗体，请参阅博客文章[添加绑定和缩减到 Web 窗体](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx)。

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>使用绑定和缩减使用 ASP.NET MVC

在本部分中我们将创建 ASP.NET MVC 项目以检查捆绑和缩减。 首先，创建一个名为的新 ASP.NET MVC internet 项目**MvcBM**而无需更改任何默认值。

打开*应用\_Start\BundleConfig.cs*文件并检查`RegisterBundles`方法用于创建、 注册和配置捆绑包。 下面的代码演示了一部分`RegisterBundles`方法。

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

前面的代码创建名为的新 JavaScript 捆绑*~/bundles/jquery* ，包括所有相应 (调试或缩减的但不是。*vsdoc*) 中的文件*脚本*匹配通配符字符串"~/Scripts/jquery-{version}.js"的文件夹。 对于 ASP.NET MVC 4 中，这意味着使用调试配置，该文件*jquery 1.7.1.js*将添加到该绑定。 在版本配置中， *jquery 1.7.1.min.js*将添加。 捆绑 framework 如遵循几个常见约定：

- 当"FileX.min.js"和"FileX.js"存在，请选择".min"版本的文件。
- 选择用于调试的非".min"版本。
- 忽略"-vsdoc"文件 （例如 jquery-1.7.1-vsdoc.js)，它仅由 IntelliSense。

`{version}`通配符匹配上面所示用于自动创建相应版本的中的 jQuery jQuery 捆绑你*脚本*文件夹。 在此示例中，使用通配符提供了以下好处：

- 可以使用 NuGet 来更新到较新的 jQuery 版本，而无需更改的前面的捆绑代码或 jQuery 引用视图页面中。
- 自动选择调试配置的完整版本和版本的".min"版本生成。

## <a name="using-a-cdn"></a>使用 CDN

 以下代码替换 CDN jQuery 捆绑本地 jQuery 捆绑。

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

在上面的代码中，将 jQuery 请求从 CDN 这是在版本模式和 jQuery 的调试版本将获取本地在调试模式下时。 在使用 CDN 时，你应在 CDN 请求失败的情况下具有回退机制。 以下标记片段从添加请求 jQuery 应在 CDN 失败的布局文件显示脚本的末尾。

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>创建包

[捆绑](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)类`Include`方法采用数组的字符串，其中每个字符串是资源的虚拟路径。 下面的代码中的 RegisterBundles 方法从*应用\_Start\BundleConfig.cs*文件说明了如何将多个文件添加到了捆绑包：

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[捆绑](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)类`IncludeDirectory`方法可用于添加一个目录 （以及所有子目录 （可选）） 中与搜索模式匹配的所有文件。 [捆绑](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)类`IncludeDirectory`API 如下所示：

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

使用 Render 方法中，视图中引用捆绑包 ( `Styles.Render` css 和`Scripts.Render`javascript)。 从以下标记*views/shared\\_Layout.cshtml*文件演示如何在默认 ASP.NET internet 项目视图引用 CSS 和 JavaScript 的捆绑包。

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

请注意呈现方法采用数组的字符串，因此你可以在某行代码中添加多个捆绑包。 通常，你将想要使用创建必要 HTML 中以引用资产的呈现器方法。 你可以使用`Url`方法以生成资产的 URL，而引用资产所需的标记。 假设你想要使用新的 HTML5[异步](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async)属性。 下面的代码演示如何引用 modernizr 使用`Url`方法。

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>使用"\*"通配符，以选择文件

中指定的虚拟路径`Include`方法并且搜索模式中`IncludeDirectory`方法可以接受一个"\*"作为前缀或后缀中最后一个路径段的通配符字符。 区分大小写的搜索字符串。 `IncludeDirectory`方法具有的搜索子目录选项。

使用以下 JavaScript 文件，请考虑一个项目：

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

下表显示添加到使用通配符，如下所示的捆绑的文件：

| **Call** | **添加文件或引发异常** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js, ToggleDiv.js, ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | 无效模式异常。 通配符字符只能在前缀或后缀上。 |
| Include("~/Scripts/Common/\*og.\*") | 无效模式异常。 允许只有一个通配符字符。 |
| "Include("~/Scripts/Common/T\*") | *ToggleDiv.js ToggleImg.js* |
| "Include("~/Scripts/Common/\*") | 无效模式异常。 纯通配符段不是有效的。 |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js ToggleImg.js* |
| IncludeDirectory("~/Scripts/Common", "T\*",true) | *ToggleDiv.js，ToggleImg.js，ToggleLinks.js* |

捆绑包中显式添加每个文件是通常首选通过通配符加载的下列原因造成的文件：

- 将脚本通配符默认情况下添加到在字母顺序排列，这通常不是你希望中加载它们。 经常需要添加特定 （非字母） 顺序 CSS 和 JavaScript 文件。 你可以通过添加自定义来缓解此风险[IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx)实现，但显式添加每个文件都不容易出错。 例如，可以添加到将来的文件夹的新资产，这可能会要求你修改你[IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx)实现。
- 查看特定文件添加到目录中使用通配符加载可以包含在引用此绑定的所有视图。 如果查看特定脚本添加到捆绑包，您可能会在其他引用捆绑包的视图的 JavaScript 错误。
- 导入其他文件的 CSS 文件会产生两次加载导入文件。 例如，下面的代码与大多数两次加载 jQuery UI 主题 CSS 文件创建包。 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

 通配符选择器"\*.css"的文件夹中，每个 CSS 文件中将包括*Content\themes\base\jquery.ui.all.css*文件。 *Jquery.ui.all.css*文件导入其他 CSS 文件。

## <a name="bundle-caching"></a>捆绑缓存

捆绑包设置创建包时从 HTTP 过期标头一年。 如果你导航到以前查看过的页面，Fiddler 显示 IE 不会进行捆绑包，条件请求，即有的捆绑包的 IE 没有 HTTP GET 请求以及来自服务器的任何 HTTP 304 响应。 你可以强制 IE 以便每个捆绑包的条件请求 （导致每个捆绑包的 HTTP 304 响应） F5 键。 你可以通过使用强制完全刷新 ^ F5 （导致每个捆绑包的 HTTP 200 响应）。

下图显示**Caching** Fiddler 响应窗格的选项卡：

![fiddler 缓存图像](bundling-and-minification/_static/image8.png)

请求   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 是捆绑包**AllMyScripts**和包含查询字符串对**v = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**。 查询字符串**v**令牌，它是用于缓存的唯一标识符的值。 ASP.NET 应用程序，只要捆绑包不会更改，将请求**AllMyScripts**捆绑在一起使用此令牌。 如果捆绑中的任何文件发生更改，ASP.NET 优化框架将生成一个新的令牌，捆绑包的浏览器请求将获取最新的捆绑包，从而保证。

如果你运行 IE9 F12 开发人员工具，并导航到先前加载页时，即错误地显示为每个捆绑包和返回 HTTP 304 服务器所做的条件性 GET 请求。 你可以阅读 IE9 具有确定是否在博客文章中进行了一个条件的请求的问题的原因[使用 Cdn 和提高网站性能的 Expires](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)。

## <a name="less-coffeescript-scss-sass-bundling"></a>更少，CoffeeScript、 SCSS，Sass 绑定。

绑定和缩减 framework 提供一种机制，如处理中间语言[SCSS](http://sass-lang.com/)， [Sass](http://sass-lang.com/)，[较少](http://www.dotlesscss.org/)或[Coffeescript](http://coffeescript.org/)，并将如缩减的转换应用到生成的捆绑包。 例如，若要添加[.less](http://www.dotlesscss.org/)到 MVC 4 项目的文件：

1. 创建你较少的内容的文件夹。 下面的示例使用*Content\MyLess*文件夹。
2. 添加[.less](http://www.dotlesscss.org/) NuGet 包**无点**到你的项目。  
    ![NuGet 无点安装](bundling-and-minification/_static/image9.png)
3. 添加一个类，实现[IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx)接口。 用于.less 转换到你的项目中添加以下代码。

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. 创建具有更少文件的捆绑包`LessTransform`和[CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx)转换。 以下代码添加到`RegisterBundles`中的方法*应用\_Start\BundleConfig.cs*文件。

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. 将以下代码添加到任何视图，即在引用较少捆绑包。

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>捆绑注意事项

较好的做法，在创建包时应遵循是作为前缀的捆绑包名称包含"捆绑"。 这将防止可能发生的[路由冲突](https://forums.asp.net/post/5012037.aspx)。

后更新捆绑中的一个文件，为捆绑查询字符串参数生成新的令牌，并必须客户端请求包含捆绑包的页在下一步时下载完整的捆绑包。 在其中将单独列出每个资产的传统标记中，会下载已更改的文件。 批的情况下，经常发生变化的资产可能不是很好的候选。

绑定和缩减主要提高第一个页面请求加载时间。 一旦已请求了网页，浏览器缓存的资产 （JavaScript、 CSS 和图像） 因此绑定和缩减在请求同一页面时，不会提供任何性能提升，或在同一站点请求相同的资产。 如果未设置过期标头在你的资产上正常和不使用绑定和缩减、 浏览器新鲜度试探方法会将标记为资产陈旧在几天后和浏览器将需要为每个资产的验证请求。 在这种情况下，绑定和缩减后首次页请求提供性能提升。 有关详细信息，请参阅博客[使用 Cdn 和提高网站性能的 Expires](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)。

每个主机名每六个同时连接的浏览器限制可以通过使用减轻[CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)。 CDN 将有不同的主机名与你的托管站点，因为从 CDN 的资产请求将不计入你的托管环境的六个并发连接限制。 常见包缓存和缓存优点的边缘，还可以提供 CDN。

捆绑包应进行分区需要它们的页。 例如，默认的 internet 应用程序的 ASP.NET MVC 模板创建 jQuery 验证捆绑分开 jQuery。 创建的默认视图不产生任何影响，并不发布值，因为它们不包括验证捆绑。

`System.Web.Optimization`在 System.Web.Optimization.DLL 中实现命名空间。 它利用 WebGrease 库 (WebGrease.dll) 对于缩减功能，这反过来使用 Antlr3.Runtime.dll。

*我使用 Twitter 进行快速的文章和共享链接。我 Twitter 句柄是*:[@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>其他资源

- 视频：[绑定和优化](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing)通过[Howard Dierking](https://twitter.com/#!/howard_dierking)
- [将添加到的网页的 Web 优化](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx)。
- [添加绑定和缩减到 Web 窗体](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx)。
- [性能影响的绑定和缩减在 Web 浏览](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx)通过[Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen)[@frystyk](https://twitter.com/frystyk)
- [使用 Cdn 和过期以提高网站性能](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)由 Rick Anderson[@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [最大程度减少 RTT （往返时间）](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>参与者

- Hao 永远
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
