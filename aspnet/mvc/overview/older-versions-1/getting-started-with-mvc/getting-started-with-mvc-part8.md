---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: "将列添加到模型 |Microsoft 文档"
author: shanselman
description: "这是初学者本教程介绍 ASP.NET MVC 的基础知识。 创建一个简单的 web 应用程序读取和写入数据库中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 17ee105f596319423ac83cf718683ed293f952f3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-column-to-the-model"></a>向模型添加列
====================
通过[Scott Hanselman](https://github.com/shanselman)

> 这是初学者本教程介绍 ASP.NET MVC 的基础知识。 将创建一个简单的 web 应用程序读取和写入数据库中。 请访问[ASP.NET MVC 学习中心](../../../index.md)若要查找其他 ASP.NET MVC 教程和示例。


在本部分中我们将演练如何我们可以对我们的数据库的架构进行更改，并在我们的应用程序内处理所做的更改。

让我们将"级别"的列添加到影片表。 返回到 IDE，并单击数据库资源管理器。 右键单击电影表并选择打开表定义。

添加"分级"列，如下所示。 我们现在没有任何级别，因为此列可以允许 null 值。 单击保存。

[![编辑电影表](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

接下来，返回到解决方案资源管理器并打开 Movies.edmx 文件 （这是 \Models 文件夹中）。 右键单击设计图面 （白色区域） 上并从数据库中选择更新模型。

[![电影-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

这将启动"更新向导"。 单击刷新选项卡中的，单击完成。 然后将新列更新我们电影模型的类。

![更新向导 (2)](getting-started-with-mvc-part8/_static/image5.png)

单击完成，后，你可以看到新的分级列已添加到我们的模型中的电影实体。

[![电影实体](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

我们在数据库模型中，添加了列，但是视图不知道有关它。

## <a name="update-views-with-model-changes"></a>使用模型更改更新视图

有几种方法，我们无法更新我们的视图模板，以反映新的分级列。 由于我们创建这些视图通过添加视图对话框通过生成它们，我们无法将其删除并再次重新创建它们。 但是，通常的人员将具有已进行了修改对其视图模板从初始的基架生成并且将想要添加或删除字段，手动，只需与我们创建相同的 ID 字段。

打开 \Views\Movies\Index.aspx 模板并添加&lt;th&gt;评级&lt;/th&gt;到影片表的开头。 我添加挖掘之后风格。 然后，在相同的列位置但较低位置添加行以便输出我们新级别。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

我们最终 Index.aspx 模板将如下所示：

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

让我们然后打开 \Views\Movies\Create.aspx 模板，然后添加标签和文本框中为我们新分级属性：

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

我们最终 Create.aspx 模板将如下所示，并让我们更改我们浏览器的标题和辅助&lt;h2&gt;标题为类似于"创建电影"当我们在此处解决 ！

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

运行你的应用，现在你已生成已添加到创建页面数据库中的新字段。 添加新的影片的分级-此时间，并单击创建。

[![创建电影-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

单击创建后，你要发送到索引页在你新影片列出与数据库中的新级别列

[![影片列表-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

这一基本教程了你可以开始进行控制器，将其与视图关联并传递关于硬编码数据。 则我们创建和设计数据库，并将一些数据放在。 我们从数据库中检索数据，并显示在 HTML 表。 然后，我们添加创建窗体，让用户将数据添加到数据库本身从 Web 应用程序中。 我们添加验证，则进行客户端上使用 JavaScript 的验证。 最后，我们更改了数据库，以包括新列的数据，然后更新我们的两个页来创建和显示此新数据。

我现在鼓励你转到我们中级教程"[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)"以及许多视频和资源在[https://asp.net/mvc](https://asp.net/mvc)若要了解更多有关 ASP.NET MVC ！

请尽情体验吧！

- Scott Hanselman- [http://hanselman.com](http://hanselman.com)和[ @shanselman ](http://twitter.com/shanselman)在 Twitter 上。

>[!div class="step-by-step"]
[上一篇](getting-started-with-mvc-part7.md)
