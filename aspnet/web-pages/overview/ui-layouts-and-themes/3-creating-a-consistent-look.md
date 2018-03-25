---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: "在 ASP.NET 网页中创建一致的布局页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "若要使其更高效地创建你的站点的 web 页面，可以创建可重用块的内容 （如页眉和页脚） 的网站，然后你 c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>在 ASP.NET Web 页 (Razor) 站点中创建一致的布局
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何在 ASP.NET Web 页 (Razor) 网站中使用布局页，以创建可重用块的内容 （如页眉和页脚） 并在站点中创建的所有页一致的外观。
> 
> **你将学习：** 
> 
> - 如何创建可重用块的内容，例如页眉和页脚。
> - 如何在你使用的布局的站点中创建的所有页一致的外观。
> - 如何将数据在运行时传递给布局页。
> 
> 这些是文章中引入的 ASP.NET 功能：
> 
> - 内容块，是包含要插入多个页面的 HTML 格式的内容文件。
> - 包含可以共享网站上的页的 HTML 格式的内容页的布局页。
> - `RenderPage`， `RenderBody`，和`RenderSection`方法，其作用是告诉 ASP.NET 页元素的插入位置。
> - `PageData`允许您在共享内容块和布局页之间的数据的字典。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


## <a name="about-layout-pages"></a>有关布局页

许多网站未在每个页上，如页眉和页脚或告知用户他们在登录框中显示的内容。 ASP.NET，可以使用内容块可以包含文本、 标记和代码，就像常规 web 页一样创建一个单独的文件。 然后，你可以在想要显示的信息的站点上的其他页中插入该内容块。 这样无需复制并粘贴到每一页的相同的内容。 创建这样的常见内容还可以更轻松地更新你的站点。 如果你需要更改的内容，你可以仅更新单个文件，并且所做的更改将反映无处不在已插入内容。

下图显示内容如何阻止工作。 当浏览器从 web 服务器请求页面时，ASP.NET 会将内容块插入点处其中`RenderPage`在主页面中调用方法。 然后，完成 （合并） 页被发送到浏览器。

![显示如何 RenderPage 方法将一个引用的页插入到当前页的概念图。](3-creating-a-consistent-look/_static/image1.jpg)

在此过程中，你将创建引用两个内容块 （页眉和页脚），它们位于单独的文件中的页。 在你的站点中的任意页中，可以使用这些相同的内容块。 完成后，你将获得如下页：

![显示在浏览器中运行包括对 RenderPage 方法的调用的页面的页面的屏幕截图。](3-creating-a-consistent-look/_static/image2.jpg)

1. 在你的网站的根文件夹中，创建名为的文件*Index.cshtml*。
2. 现有的标记替换为以下代码：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. 在根文件夹中，创建名为的文件夹*共享*。

    > [!NOTE]
    > 常见的做法，用于存储在名为的文件夹中的 web 页之间共享的文件是*共享*。
4. 在*共享*文件夹中，创建名为的文件 *\_Header.cshtml*。
5. 任何现有内容替换为以下代码：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    请注意，文件名是 *\_Header.cshtml*，以下划线 (\_) 作为前缀。 ASP.NET 不会发送到浏览器页面，如果其名称以下划线开头。 这可以防止用户请求 （无意中或其他） 这些页面直接。 它是一个好办法使用下划线名称页中，具有内容块，因为你实际上不希望用户能够请求这些页面 &#8212;它们严格存在要插入到其他页。
6. 在*共享*文件夹中，创建名为的文件 *\_Footer.cshtml*和替换为以下内容：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. 在*Index.cshtml*页上，添加到两个调用`RenderPage`方法，如下所示：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    此示例演示如何将内容块插入到网页。 你调用`RenderPage`方法并将其传递你想要在该点插入其内容的文件的名称。 在这里，要插入的内容 *\_Header.cshtml*和 *\_Footer.cshtml*文件到*Index.cshtml*文件。
8. 运行*Index.cshtml*在浏览器中的页。 (在 WebMatrix 中，在**文件**工作区中，右键单击该文件，然后选择**在浏览器中启动**。)
9. 在浏览器中查看页面源文件。 (例如，在 Internet Explorer 中，右击该页，然后单击**查看源**。)

    这样，你可以查看发送到浏览器，将索引页标记与内容块相结合的网页标记。 下面的示例演示为呈现页面源文件*Index.cshtml*。 对调用`RenderPage`插入到*Index.cshtml*已被替换为页眉和页脚文件的实际内容。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>创建一致的外观使用布局页

到目前为止，你已了解很容易地包含在多个页相同的内容。 一种创建站点的一致的外观更结构化的方法是使用布局页。 布局页定义的 web 网页、 的结构，但不包含任何实际内容。 创建布局页后，可以创建包含的内容，然后将它们链接到布局页的网页。 这些页面显示时，它们将采用格式根据布局页。 （在这个意义上来说，布局页充当一种类型的其他页中定义的内容的模板。）

布局页就像任何 HTML 页中，只是它包含对的调用`RenderBody`方法。 位置`RenderBody`布局页中的方法确定内容页中的信息将在其中包含。

下图显示了如何在内容页并布局页将合并在运行时生成完成的网页。 浏览器请求内容页。 内容页中，指定要用于该页面的结构的布局页有代码。 在布局页中，内容插入点处其中`RenderBody`调用方法。 内容块还可以插入到布局页通过调用`RenderPage`方法上, 一节中采用的方式。 完成网页时，则将它发送到浏览器。

![显示在浏览器中运行包括对 RenderBody 方法的调用的页面的页面的屏幕截图。](3-creating-a-consistent-look/_static/image3.jpg)

以下过程演示如何创建网页并链接到它的内容页的布局。

1. 在*共享*文件夹中你的网站，创建名为的文件 *\_Layout1.cshtml*。
2. 任何现有内容替换为以下代码：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    你使用`RenderPage`布局页后，可以将插入内容块中的方法。 布局页可以包含对只有一个调用`RenderBody`方法。
3. 在*共享*文件夹中，创建名为的文件 *\_Header2.cshtml*和任何现有内容替换为以下代码：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. 在根文件夹中，创建一个新文件夹并将其命名*样式*。
5. 在*样式*文件夹中，创建名为的文件*Site.css*并添加以下样式定义：

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    这些样式定义目前仅用于显示布局页中，如何可以使用样式表。 如果你想，你可以定义你自己的这些元素的样式。
6. 在根文件夹中，创建名为的文件*Content1.cshtml*和任何现有内容替换为以下代码：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    这是将使用布局页的页。 在页面顶部的代码块指示要用于此内容进行格式设置的布局页。
7. 运行*Content1.cshtml*在浏览器。 呈现的页面使用的格式和样式表中定义 *\_Layout1.cshtml*和中定义的文本 （内容） *Content1.cshtml*。

    ![[image]](3-creating-a-consistent-look/_static/image4.jpg)

    您可以重复步骤 6 以创建然后可以共享相同的布局页的其他内容页。

    > [!NOTE]
    > 你可以设置你的站点，以便为文件夹中的所有内容页对自动使用相同的布局页。 有关详细信息，请参阅[自定义站点范围的行为的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)。

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>具有多个内容区域的设计布局页

内容页可以有多个部分中，这非常有用，如果你想要使用具有多个区域可替换内容的布局。 在内容页中，你为每个部分的唯一名称。 (保持默认部分未命名。)在布局页中，你将添加`RenderBody`方法，以指定的未命名 （默认值） 部分应出现的位置。 然后添加单独`RenderSection`方法以逐一呈现命名的部分。

下图显示 ASP.NET 处理分为多个部分的内容的方式。 每个命名的部分包含在内容页中的部分块中。 (它们在名为`Header`和`List`在示例中。)框架将内容部分插入点处的布局页，其中`RenderSection`调用方法。 未命名 （默认值） 部分插入点处其中`RenderBody`调用方法，如前面所看到。

![显示如何 RenderSection 方法将引用部分插入到当前页的概念图。](3-creating-a-consistent-look/_static/image5.jpg)

此过程介绍如何创建有多个内容节的内容页以及如何使其使用支持多个内容区域的布局页。

1. 在*共享*文件夹中，创建名为的文件 *\_Layout2.cshtml*。
2. 任何现有内容替换为以下代码：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    你使用`RenderSection`方法呈现的标头和列表部分。
3. 在根文件夹中，创建名为的文件*Content2.cshtml*和任何现有内容替换为以下代码：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    此内容页包含在页面顶部的代码块。 每个命名的部分包含在部分块中。 页面的其余部分包含 （未命名） 的默认内容部分。
4. 运行*Content2.cshtml*在浏览器。

    ![显示在浏览器中运行包括对 RenderSection 方法的调用的页面的页面的屏幕截图。](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>使内容的部分可选

通常情况下，在内容页中创建的部分必须匹配布局页中定义的部分。 如果发生以下任一情况，你会遇到错误：

- 内容页包含布局页中没有相应部分的部分。
- 布局页包含的部分没有任何内容。
- 该布局页面包括尝试多次呈现相同的部分的方法调用。

但是，可以通过声明部分是可选中布局页来重写此行为的命名的节。 这样可以定义多个内容页，可以共享布局页，但是，可能会也可能没有特定部分的内容。

1. 打开*Content2.cshtml*并删除以下部分：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. 保存页，然后在浏览器中运行它。 显示错误消息，因为内容页面上不提供布局页，即标头部分中定义的节的内容。

    ![显示如果运行某个页面则会发生的错误的屏幕截图调用 RenderSection 方法但不是提供的相应部分。](3-creating-a-consistent-look/_static/image7.jpg)
3. 在*共享*文件夹中，打开 *\_Layout2.cshtml*页上，将以下行：

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    替换为以下代码：

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    作为替代方法，无法替换下面的代码块生成相同的结果之前的代码行：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. 运行*Content2.cshtml*再次浏览器。 （如果你仍然使用浏览器中打开此页，你可以只刷新它。）此时将显示页不显示错误，即使该类具有没有标头。

## <a name="passing-data-to-layout-pages"></a>将数据传递给布局页

您可能需要在布局页中引用的内容页中定义的数据。 如果是这样，你需要将从内容页的数据传递到布局页。 例如，你可能想要显示的用户的登录状态，或你可能想要显示或隐藏根据用户输入的内容区域。

若要将数据从内容页传递给布局页中，你可以将值放入`PageData`属性的内容页。 `PageData`属性是保存你想要在页面之间传递数据的名称/值对的集合。 在布局页中，然后可以读取外的值`PageData`属性。

下面是另一个关系图。 本示例显示如何使用 ASP.NET`PageData`属性要从内容页的值传递给布局页。 当 ASP.NET 开始生成 web 页面时，它将创建`PageData`集合。 在内容页中，你编写代码以将数据放入`PageData`集合。 中的值`PageData`通过内容页中的其他章节或其他内容块，也可以访问集合。

![演示如何填充 PageData 字典并将该信息传递给布局页内容页的概念图。](3-creating-a-consistent-look/_static/image8.jpg)

以下过程演示如何将数据从内容页传递给布局页。 页运行时，它将显示一个按钮，使用户可以隐藏或显示在布局页中定义的列表。 当用户单击按钮时，它设置中的 true/false （布尔值） 值`PageData`属性。 布局页读取该值，并如果为 false，将隐藏列表。 值还用于在内容页确定是否显示**隐藏列表**按钮或**显示列表**按钮。

![[image]](3-creating-a-consistent-look/_static/image9.jpg)

1. 在根文件夹中，创建名为的文件*Content3.cshtml*和任何现有内容替换为以下代码：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    代码将存储中的数据的两条`PageData`属性 &#8212; 网页和 true 或 false 以指定是否显示列表的标题。

    请注意，ASP.NET 将允许你放入有条件地使用代码块的页的 HTML 标记。 例如，`if/else`页的正文中的块确定哪个窗体，具体取决于是否显示`PageData["ShowList"]`设置为 true。
2. 在*共享*文件夹中，创建名为的文件 *\_Layout3.cshtml*和任何现有内容替换为以下代码：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    布局页包括中的表达式`<title>`获取从的标题值的元素`PageData`属性。 它还使用`ShowList`值`PageData`属性来确定是否显示该列表内容块。
3. 在*共享*文件夹中，创建名为的文件 *\_List.cshtml*和任何现有内容替换为以下代码：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. 运行*Content3.cshtml*在浏览器中的页。 在页面左侧上可见的列表将显示的页和**隐藏列表**底部的按钮。

    ![显示包含列表和一个按钮，将显示为隐藏列表的页的屏幕截图。](3-creating-a-consistent-look/_static/image10.jpg)
5. 单击**隐藏列表**。 列表将消失，该按钮改为**显示列表**。

    ![显示不包含列表和一个按钮，将显示为显示列表页的屏幕截图。](3-creating-a-consistent-look/_static/image11.jpg)
6. 单击**显示列表**再次显示按钮和列表。

## <a name="additional-resources"></a>其他资源


[为 ASP.NET 网页自定义站点范围的行为](https://go.microsoft.com/fwlink/?LinkId=202906)
