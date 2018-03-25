---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: "母版页 |Microsoft 文档"
author: microsoft
description: "成功的网站的关键组件之一是一致的外观和感觉。 在 ASP.NET 中 1.x，开发人员用于用户控件复制常见页 elem...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: bd9effd4b73a014d4d7bb825b382b8db34d636f1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="master-pages"></a>母版页
====================
通过[Microsoft](https://github.com/microsoft)

> 成功的网站的关键组件之一是一致的外观和感觉。 在 ASP.NET 中 1.x，开发人员使用用户控件在 Web 应用程序上复制常见页面元素。 虽然这的确是一个可行的解决方案，则使用用户控件也具有一些缺点。 例如，用户控件的位置中的更改需要在站点之间对多个页的更改。 用户控件也不会呈现在之后插入页面上的设计视图中。


成功的网站的关键组件之一是一致的外观和感觉。 在 ASP.NET 中 1.x，开发人员使用用户控件在 Web 应用程序上复制常见页面元素。 虽然这的确是一个可行的解决方案，则使用用户控件也具有一些缺点。 例如，用户控件的位置中的更改需要在站点之间对多个页的更改。 用户控件也不会呈现在之后插入页面上的设计视图中。

ASP.NET 2.0 引入了 Master 页作为一种维护一致的外观和感觉，而且如你很快就会看到，Master 页通过用户控件方法表示的重大进步。

## <a name="why-master-pages"></a>为什么主页？

你可能想知道为什么主控页所需在 ASP.NET 2.0。 毕竟，网站开发人员已使用用户控件在 ASP.NET 中 1.x 共享页面之间的内容区域。 实际原因有多种原因用户控件是用于创建通用布局的不太比最佳解决方案。

用户控件实际上未定义页面布局。 相反，它们定义的布局和页面的一个部分的功能。 这两者之间的差异非常重要，因为它使用户控制解决方案的可管理性困难得多。 例如，如果你想要更改你的页面上的用户控件的位置，你必须编辑用户控件在其显示的实际页。 如果你有仅几个页面，但在大型站点中，我们很快就会站点管理非常困难，这是微调 ！

用于定义常用布局使用用户控件的另一个缺点 ASP.NET 本身的体系结构中已取得 root 权限。 如果更改用户控件的任何公共成员，它将要求你重新编译所有使用该用户控件的页面。 反过来，ASP.NET 将然后重新 JIT 页面首次访问。 此操作，请再次生成不可扩展的体系结构和较大的站点的站点管理问题。

通过在 ASP.NET 2.0 的主控页，这两个这些问题 （以及更多） 可以很好地进行寻址。

## <a name="how-master-pages-work"></a>如何母版页的工作原理

母版页是类似于其他页面的模板。 应在其他页 （即菜单、 边框、 等） 之间共享的页面元素添加到母版页。 当新页面添加到该站点时，你可以将其与母版页中关联。 调用页与母版页的**内容页**。 默认情况下，内容页面采用从主控页的外观。 但是，当你创建母板页，你可以定义内容页可以将其自己的内容替换页面的部分。 这些部分定义使用在 ASP.NET 2.0; 中引入一个新的控件**ContentPlaceHolder**控件。

母版页可以包含任意数量的 ContentPlaceHolder 控件 （或全部都不。）在内容页上会出现的内部的 ContentPlaceHolder 控件中的内容**内容**控件，在 ASP.NET 2.0 中的另一个新控件。 默认情况下，内容控件的内容页为空，以便你可以提供自己的内容。 如果你想要使用内容控件内母版页中的内容，则可以执行因此，当你将看到更高版本在此模块。 内容控件映射到内容控件的 ContentPlaceHolderID 属性通过 ContentPlaceHolder 控件。 地图下面的代码到母版页上名为 mainBody ContentPlaceHolder 控件的内容控件。

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> 通常，你将能听到描述的其他页面的基类为母版页的人员。 这是实际事实并非如此。 母版页和内容页之间的关系不是继承。


**图 1**显示母版页和关联的内容页，在 Visual Studio 2005 中显示。 你可以看到在母版页和相应的 ContentPlaceHolder 控件内容在内容页中的控件。 请注意，在 ContentPlaceHolder 之外的母版页内容可见但在内容页中灰显。 可以通过内容页取代仅 ContentPlaceHolder 内的内容。 所有其他来自母版页的内容不可变。


![主控页和其关联的内容页](master-pages/_static/image1.jpg)

**图 1**： 主控页和其关联的内容页


## <a name="creating-a-master-page"></a>创建 Master 页

若要创建新的母版页：

1. 打开 Visual Studio 2005 并创建新网站。
2. 单击新建文件，文件。
3. 从添加新项对话框中选择主文件中所示**图 2**。
4. 单击添加。


![创建一个新的主页面](master-pages/_static/image2.jpg)

**图 2**： 创建一个新的主页面


请注意，母版页的文件扩展名是*.master*。 这是一个母版页上与普通页不同的方式。 其他主要差别在于，替代@Page指令，母版页包含@Master指令。 切换到源视图的主页面你刚刚创建，并检查代码。

默认情况下，新的母版页将具有一个 ContentPlaceHolder 控件。 在大多数情况下，它更有意义要先创建常见的页面元素，然后插入 ContentPlaceHolder 控件需要自定义内容。 在这些情况下，开发人员将想要删除默认 ContentPlaceHolder 控件并将插入新记录，由于页开发。 ContentPlaceHolder 控件不调整它们显示调整大小控点的情况下。 ContentPlaceHolder 控件大小自动基于有一个例外; 它包含的内容如果块元素内的 ContentPlaceHolder 控件置于如表单元格上时，它将根据元素的大小的大小。

## <a name="lab-1-working-with-master-pages"></a>实验室 1 使用母版页

在本实验中，将创建新的主页面，并定义三个 ContentPlaceHolder 控件。 然后，你将创建一个新的内容页，并将中至少一个 ContentPlaceHolder 控件的内容。

1. 创建母版页和插入 ContentPlaceHolder 控件。 

    1. 创建新的母版页上按上文所述。
    2. 删除默认 ContentPlaceHolder 控件。
    3. 通过单击控件的阴影上边框选择 ContentPlaceHolder 控件，然后删除它按你键盘上的 DEL 键。
    4. 插入新表使用*标头和端*模板图 3 中所示。 将宽度和高度更改为 90%，以使整个表设计器中可见。


![](master-pages/_static/image3.jpg)

**图 3**


1. 将光标置于表的每个单元格并设置*垂直*属性*顶部*。
2. 从工具箱中，将 ContentPlaceHolder 控件插入顶部的单元格的表 （标头单元。）
3. 当插入此 ContentPlaceHolder 控件时，你将注意到行高将几乎整个页面占用，如图 4 中所示。 别担心，在此点。


![空的空间是在相同单元作为 ContentPlaceHolder](master-pages/_static/image1.gif)

**图 4**： 空白区域是在相同单元作为 ContentPlaceHolder


1. 将 ContentPlaceHolder 控件放在其他两个单元格。 后已插入其他 ContentPlaceHolder 控件，如你所料，应为表格单元格的大小。 页面现在应如下所示中显示的网页**图 5**。


![与所有 ContentPlaceHolder 控件 Master。 请注意标题单元格的单元格高度现它应该是什么](master-pages/_static/image2.gif)

**图 5**： 所有 ContentPlaceHolder 控件母版。 请注意标题单元格的单元格高度现它应该是什么


1. 输入所选的一些文本到每个三个 ContentPlaceHolder 控件。
2. Exercise1.master 另存母版页。
3. 创建新的 Web 窗体并将其与 exercise1.master 母版页。
4. 选择文件，新、 文件在 Visual Studio 2005 中。
5. 选择**Web 窗体**在添加新项对话框。
6. 请确保选中选择母版页复选框，如图 6 中所示。


![添加一个新的内容页](master-pages/_static/image3.gif)

**图 6**： 添加一个新的内容页


1. 单击添加。
2. 选择 exercise1.master 在选择母版页对话框图 7 中所示。
3. 单击确定以添加新的内容页。

新内容页将显示在 Visual Studio 在主页上每个 ContentPlaceHolder 控件的一个内容控件。 默认情况下，内容控件是空的以便您可以添加自己的内容。 如果想再使用母版页上的 ContentPlaceHolder 控件中的内容，只需单击智能标记符号 （控件的右上角的小黑色箭头），并选择*默认为母版内容*从所示的智能标记**图 8**。 执行此操作时，菜单项更改为*创建自定义内容*。 此时，单击删除母版页让你能够定义为该特定的内容控件的自定义内容的内容。


![设置默认为母版页页内容的内容控件](master-pages/_static/image4.gif)

**图 7**： 将内容控件设置为默认为母版页页内容


## <a name="connecting-master-page-and-content-pages"></a>连接母版页和内容页

可以四种不同方式之一中配置母版页和内容页之间的关联：

- **MasterPageFile**属性@Page指令
- 设置**Page.MasterPageFile**在代码中的属性。
- **&lt;页&gt;**应用程序配置文件 (web.config 应用程序的根文件夹中) 中的元素
- **&lt;页&gt;**子文件夹配置文件 (web.config 的子文件夹中) 中的元素

## <a name="masterpagefile-attribute"></a>MasterPageFile 属性

MasterPageFile 属性，可以轻松将母版页应用于特定的 ASP.NET 页面。 它也是用于检查时应用主控页的方法**选择母版页**复选框为你未在练习 1 中。

## <a name="setting-pagemasterpagefile-in-code"></a>在代码中设置 Page.MasterPageFile

通过在代码中设置 MasterPageFile 属性，你可以在运行时你的内容应用特定的母版页。 这可在情况下，你可能需要将特定母版页基于用户角色或某些其他条件应用。 MasterPageFile 属性必须设置 PreInit 方法中。 如果设置 PreInit 方法后，将引发 InvalidOperationException。 在其设置此属性页还必须具有内容的控件为顶级控件页。 否则 MasterPageFile 属性设置时，将引发 HttpException。

## <a name="using-the-ltpagesgt-element"></a>使用&lt;页&gt;元素

你可以通过将 masterPageFile 属性设置配置页母版页&lt;页&gt;web.config 文件的元素。 使用此方法时，记住应用程序结构中较低级别的 web.config 文件可以重写此设置。 在中设置任何 MasterPageFile 属性@Page指令还将重写此设置。 使用&lt;页&gt;元素可以很容易地创建*master*母版页可以重写如有必要在特定文件夹或文件中。

## <a name="properties-in-master-pages"></a>母版页中的属性

母版页可以公开只需使这些属性母版页中的公共属性。 例如，下面的代码定义一个名为 SomeProperty 属性：

[!code-csharp[Main](master-pages/samples/sample2.cs)]

若要从内容页访问 SomeProperty 属性，你将需要使用主属性如下所示：

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>嵌套母版页

母版页是完美的解决方案，确保跨大型 Web 应用程序的常见的外观和感觉。 但是，不少见具有大型站点共享一个公共接口的某些部分，而其他部分共享一个不同的接口。 若要解决这种需求，多个母版页是完美的解决方案。 但是，这样仍然不解决大型应用程序可能具有在所有页之间共享某些组件 （如菜单，例如） 和其他仅在站点的某些部分之间共享的组件。 对于该类型的情况下，嵌套的母版页精美填充需要。 如你所见，正常母版页由母版页和内容页组成。 在嵌套的母版页的情况下，有两个主页面; 例如：父主服务器并子 master。 子母版页也是内容页并且其主父母版页。

下面是典型的主控页的代码：

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

在嵌套的主方案中，这将是父 master。 另一个母版页将使用此页为其主页面，并且该代码将如下所示：

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

请注意，在此方案中，子主也是父级主页的内容页。 所有子母版页的内容显示在从父 ContentPlaceHolder 控件获取其内容的内容控件内。

> [!NOTE]
> 设计器支持不可用于嵌套的母版页。 当你正在开发使用嵌套母版时，你将需要使用源视图。


本视频介绍了使用嵌套的母版页的演练。


![](master-pages/_static/image1.png)


[打开全屏幕视频](master-pages/_static/nested1.wmv)


![选择 Master 页](master-pages/_static/image4.jpg)

**图 8**： 选择母版页
