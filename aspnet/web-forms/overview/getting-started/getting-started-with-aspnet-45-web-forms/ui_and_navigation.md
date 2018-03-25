---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: "UI 和导航 |Microsoft 文档"
author: Erikre
description: "本系列教程将教您生成有关我们使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 的 ASP.NET Web 窗体应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: 7f1d8a1a473820a7c8da4c8086904cc41c86fd2a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="ui-and-navigation"></a>UI 和导航
====================
通过[艾力克 Reitan](https://github.com/Erikre)

[下载 Wingtip Toys 示例项目 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下载电子书 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教程将教您构建使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web 窗体应用程序的基础知识。 Visual Studio 2013[包含 C# 源代码项目](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)是可以附带本系列教程。


在本教程中，你将要修改默认 Web 应用程序以支持 Wingtip Toys 应用商店前端应用程序的功能的 UI。 此外，你将添加简单和数据绑定导航。 本教程以前一教程"创建数据访问层"上构建，并为 Wingtip Toys 教程系列的一部分。

## <a name="what-youll-learn"></a>你将学习：

- 如何更改 UI 以支持 Wingtip Toys 应用商店前端应用程序的功能。
- 如何配置包括页面导航中的 HTML5 元素。
- 如何创建数据驱动的控件，若要导航到特定的产品数据。
- 如何显示来自数据库使用 Entity Framework Code First 创建的数据。

ASP.NET Web 窗体，可以创建 Web 应用程序的动态内容。 每个 ASP.NET Web 页创建的方式类似到静态 HTML 网页上 （不包括基于服务器的处理页），但 ASP.NET Web 页包括额外的元素，ASP.NET 识别并处理页将运行时生成 HTML。

与静态 HTML 页面 (*.htm*或*.html*文件)，服务器担当`Web`通过读取文件并将其作为发送的请求-在浏览器。 有人与此相反，当请求 ASP.NET 网页 (*.aspx*文件)，该页在 Web 服务器上运行的程序。 在运行页面时，它可以执行您的网站需要，包括计算值、 读取或写入数据库信息，或调用其他程序的任何任务。 作为其输出，该页面动态生成标记 （例如 HTML 中的元素），并将此动态输出发送到浏览器。

## <a name="modifying-the-ui"></a>修改 UI

你将通过修改继续本教程系列*Default.aspx*页。 您将修改用于创建应用程序的默认模板已建立的 UI。 创建任何 Web 窗体应用程序时，你将执行的修改的类型是典型的。 你将执行此更改的标题、 替换一些内容，并删除不需要的默认内容。

1. 打开或切换到*Default.aspx*页。
2. 如果在显示的页面**设计**视图中，切换到**源**视图。
3. 在中的页面顶部`@Page`指令，更改`Title`属性设为"欢迎"，如下所示在下面的黄色突出显示。 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. 同样，在*Default.aspx*页上，将替换默认的内容中包含的所有`<asp:Content>`添加标记以便标记显示为下面。 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. 保存*Default.aspx*页上通过选择**保存 Default.aspx**从**文件**菜单。

 生成*Default.aspx*页将出现，如下所示： 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

在示例中，设置`Title`属性`@Page`指令。 在浏览器中，服务器代码中显示 HTML 时`<%: Page.Title %>`解析为中包含的内容`Title`属性。

该示例页面包括构成 ASP.NET 网页的基本元素。 此页包含静态文本，你可能可以在 HTML 页，以及特定于 ASP.NET 的元素。 中包含的内容*Default.aspx*将与母版页的内容，它将在本教程后面部分所述集成页。

### <a name="page-directive"></a>@Page指令

ASP.NET Web 窗体通常包含允许你指定的页的页属性和配置信息的指令。 指令用作 asp.net 说明有关如何为进程页面，但它们不会呈现到浏览器发送的标记的一部分。

最常使用的指令`@Page`指令，该对话框可以指定多个配置选项页，其中包括：

1. 编程语言在页中，如 C# 代码的服务器。
2. 该页是否是用服务器代码直接在页中，这称为单文件页，页，或者它是否是在单独的类文件，称为代码隐藏页中使用代码页。
3. 是否页都有关联的母版页，并因此会被视为内容页。
4. 调试和跟踪选项。

如果不包括`@Page`指令在页中，或如果该指令不包括特定设置，将从继承设置*Web.config*配置文件或从*Machine.config*配置文件。 *Machine.config*文件提供对所有应用程序的计算机上运行的其他配置设置。

> [!NOTE] 
> 
> *Machine.config*还提供有关所有可能的配置设置的详细信息。


### <a name="web-server-controls"></a>Web 服务器控件

在大多数的 ASP.NET Web 窗体应用程序，你将添加允许用户与页面，如按钮、 文本框、 列表和等等交互的控件。 这些 Web 服务器控件都类似于 HTML 按钮和输入的元素。 但是，它们进行处理在服务器上，让你能够使用服务器代码设置其属性。 这些控件还引发你可以在服务器代码中处理的事件。

服务器控件使用 ASP.NET 识别运行页面时的特殊语法。 ASP.NET 服务器控件的标记名称开头`asp:`前缀。 这使 ASP.NET 可以识别并处理这些服务器控件。 如果控件不是.NET Framework 的一部分，前缀可能有所不同。 除了`asp:`前缀，ASP.NET 服务器控件还包括`runat="server"`属性和`ID`，你可以使用引用服务器代码中的控件。

页运行时，ASP.NET 标识服务器控件，并运行的代码，这些控件与相关联。 在浏览器中显示时，很多控件呈现到页一些 HTML 或其他标记。

### <a name="server-code"></a>服务器代码

大多数 ASP.NET Web 窗体应用程序包括在页处理时，在服务器运行的代码。 如上所述，服务器代码可以用于执行各种操作，例如将数据添加到 ListView 控件。 ASP.NET 支持的许多语言包括 C#、 Visual Basic、 J# 中，和其他人在服务器上运行。

ASP.NET 支持用于编写用于网页的服务器代码的两个模型。 在单文件模型中，页的代码是在脚本元素，其中包括开始标记中`runat="server"`属性。 或者，你可以在单独的类文件中，称为代码隐藏模型创建页的代码。 在这种情况下，ASP.NET Web 窗体页通常不包含服务器代码。 相反，`@Page`指令包含的信息的链接*.aspx*与其关联的代码隐藏文件的页。

`CodeBehind`属性中包含`@Page`指令指定单独的类文件的名称和`Inherits`特性指定对应于页的代码隐藏文件中的类的名称。

### <a name="updating-the-master-page"></a>更新母版页

在 ASP.NET Web 窗体、 主控页，可以在你的应用程序中创建一致的布局的页。 使用单个母版页定义应用程序中的外观和感觉和所需的所有页 （或一组页） 的标准行为。 然后可以创建包含你想要显示，如上文所述的内容的各个内容页。 当用户请求内容页时，ASP.NET 会将其合并以生成将主控页的布局与内容页中的内容相结合的输出与该母版页。

新站点需要要显示每一页上的单个徽标。 若要添加此徽标，你可以修改母版页上的 HTML。

1. 在**解决方案资源管理器**，找到并打开**Site.Master**页。
2. 如果该页面的是**设计**视图中，切换到**源**视图。
3. 更新母版页由**修改或添加**以黄色突出显示的标记： 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

以下 HTML 将显示名为映像*logo.jpg*从*映像*稍后将添加的 Web 应用程序的文件夹。 使用母版页的页显示时在浏览器中，将显示的徽标。 如果用户单击徽标，用户将向后定位到*Default.aspx*页。 HTML 定位点标记`<a>`包装映像服务器控件，并允许图像的链接的一部分。 `href`属性定位点标记指定的根目录"`~/`"作为链接位置的网站。 默认情况下， *Default.aspx*页将显示当用户导航到 Web 站点的根目录。 **映像**`<asp:Image>`服务器控件包括添加属性，如`BorderStyle`，，呈现为 HTML 浏览器中显示时。

### <a name="master-pages"></a>母版页

母版页为具有扩展.master 的 ASP.NET 文件 (例如， *Site.Master*) 有一个可以包含静态文本、 HTML 元素和服务器控件的预定义布局。 母版页由特殊`@Master`替换的指令`@Page`适用于普通的指令*.aspx*页。

除了`@Master`指令，主控页还包含的所有页，顶层的 HTML 元素例如`html`， `head`，和`form`。 例如，在前面添加主页面上的情况下，你使用 HTML`table`对于布局，`img`公司徽标、 静态文本和服务器控件以处理你的站点的公共成员身份的元素。 作为你母版页的一部分，可以使用任何 HTML 和 ASP.NET 的任何元素。

除了静态文本和控件，将出现在所有页上，该主页面还包括一个或多个**ContentPlaceHolder**控件。 这些占位符控件定义的区域会显示可替换的内容。 反过来，可替换的内容定义在内容页中，如*Default.aspx*，使用**内容**服务器控件。

#### <a name="adding-image-files"></a>添加图像文件

徽标图像引用更高版本，以及所有产品映像，必须添加到 Web 应用程序，以便在浏览器中显示该项目时，可以看到它们。

#### <a name="download-from-msdn-samples-site"></a>从 MSDN 示例站点下载：

[Getting Started with ASP.NET 4.5 Web 窗体和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

下载内容还包括中的资源*WingtipToys 资产*用于创建示例应用程序的文件夹。

1. 如果你尚未这样做，下载压缩的示例文件从 MSDN 示例站点使用上面的链接。
2. 下载完成后，打开.zip 文件并将内容复制到您的计算机上的本地文件夹。
3. 查找和打开*WingtipToys 资产*文件夹。
4. 通过拖放，复制*目录*从您的本地文件夹中的 Web 应用程序项目的根文件夹**解决方案资源管理器**的 Visual Studio。 

    ![UI 和导航-复制文件](ui_and_navigation/_static/image1.png)
5. 接下来，创建一个名为的新文件夹*映像*通过右键单击**WingtipToys**项目中**解决方案资源管理器**并选择**添加** - &gt; **新文件夹**。
6. 复制*logo.jpg*文件从*WingtipToys 资产*文件夹中的**文件资源管理器**到*映像*的 Web 应用程序的文件夹项目中**解决方案资源管理器**的 Visual Studio。
7. 单击**显示所有文件**顶部的选项**解决方案资源管理器**更新的文件的列表，如果你看不到新文件。  
  
    **解决方案资源管理器**现在显示更新的项目文件。 

    ![UI 和导航的解决方案资源管理器](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>添加网页

在添加之前导航到 Web 应用程序，你将首先添加你将导航到的两个新页。 稍后在本教程系列中，你将在这些新的页面上显示产品和产品详细信息。

1. 在**解决方案资源管理器**，右键单击**WingtipToys**，单击**添加**，然后单击**新项**。   
 随即出现“添加新项”对话框。
2. 选择**Visual C#**  - &gt; **Web**左侧的模板组。 然后，选择**包含母版页的 Web 窗体**从中间列表并将其命名*ProductList.aspx*。 

    ![UI 和导航的添加新项对话框](ui_and_navigation/_static/image3.png)
3. 选择**Site.Master**要附加到新创建的主控页*.aspx*页。 

    ![UI 和导航-选择母版页](ui_and_navigation/_static/image4.png)
4. 添加名为另一个页面*ProductDetails.aspx*按照相同的步骤。

### <a name="updating-bootstrap"></a>更新 Bootstrap

Visual Studio 2013 项目模板使用[Bootstrap](http://getbootstrap.com/)，由 Twitter 的布局和主题框架。 Bootstrap 使用 CSS3 提供响应灵敏的设计，这意味着布局可以动态调整以适应不同的浏览器窗口大小。 Bootstrap 的主题功能还可用于轻松地影响应用程序的外观和行为的更改。 默认情况下，Visual Studio 2013 中的 ASP.NET Web 应用程序模板包括 Bootstrap 作为 NuGet 程序包。

在本教程中，你将替换的 Bootstrap CSS 文件来更改 Wingtip Toys 应用程序的外观和感觉。

1. 在**解决方案资源管理器**，打开*内容*文件夹。
2. 右键单击*bootstrap.css*文件并将其命名为*bootstrap original.css*。
3. 重命名*bootstrap.min.css*到*bootstrap original.min.css*。
4. 在**解决方案资源管理器**，右键单击*内容*文件夹，然后选择**在文件资源管理器中打开文件夹**。  
 将显示文件资源管理器。 会将下载的 bootstrap CSS 文件保存到此位置。
5. 在浏览器中，转到[http://Bootswatch.com](http://bootswatch.com/)。
6. 滚动浏览器窗口，直到看到 Cerulean 主题。 

    ![UI 和导航-Cerulean 主题](ui_and_navigation/_static/image5.png)
7. 下载同时*bootstrap.css*文件和*bootstrap.min.css*文件为*内容*文件夹。 使用中显示的内容文件夹的路径**文件资源管理器**以前打开的窗口。
8. 在**Visual Studio**顶部**解决方案资源管理器**，选择**显示所有文件**选项以显示新的文件的内容文件夹中。 

    ![UI 和导航的解决方案资源管理器](ui_and_navigation/_static/image6.png)

 你将看到两个新 CSS 文件中的**内容**文件夹中，但请注意，每个文件名旁边的图标灰显。这意味着，该文件尚未尚未添加到项目。
9. 右键单击*bootstrap.css*和*bootstrap.min.css*文件，然后选择**包括在项目**。   
 本教程中稍后运行 Wingtip Toys 应用程序时，将显示新的用户界面。

> [!NOTE] 
> 
> ASP.NET Web 应用程序模板使用*Bundle.config*在项目存储的 Bootstrap CSS 文件的路径的根目录的文件。


### <a name="modifying-the-default-navigation"></a>修改默认导航

可通过更改中的无序的导航列表元素修改应用程序中的每一页的默认导航*Site.Master*页。

1. 在**解决方案资源管理器**，找到并打开*Site.Master*页。
2. 添加以到未经排序的列表如下所示的黄色突出显示的其他导航链接：   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

正如您可以看到上述 HTML 中，修改每个行项`<li>`包含的定位点标记`<a>`的链接`href`属性。 每个`href`指向 Web 应用程序中的页。 在浏览器中，当用户单击这些链接之一 (如**产品**)，它们将导航到中包含的页`href`(如**ProductList.aspx**)。 在本教程末尾，将运行应用程序。

> [!NOTE] 
> 
> 波形符 (`~`) 字符用于指定`href`路径匹配位置始于项目的根目录。


### <a name="adding-a-data-control-to-display-navigation-data"></a>添加一个数据控件来显示导航数据

接下来，你将添加一个控件来显示所有数据库中的类别。 每个类别将作为一个链接到*ProductList.aspx*页。 当用户单击浏览器中的类别链接时，它们将导航到产品页并查看仅与所选类别关联的产品。

你将使用**ListView**控件来显示数据库中包含的所有类别。 若要添加**ListView**到母版页的控件：

1. 在*Site.Master*页上，添加以下突出显示`<div>`元素**后**`<div>`元素，其中包含`id="TitleContent"`你先前添加：  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

此代码将显示从数据库的所有类别。 **ListView**控件将每个类别名称显示为链接文本，并包括指向的*ProductList.aspx*包含查询字符串值包含页面`ID`的类别。 通过设置`ItemType`中的属性**ListView**控制，数据绑定表达式`Item`内可用时`ItemTemplate`节点和控件将成为强类型。 你可以选择的详细信息`Item`对象使用 IntelliSense，例如，指定`CategoryName`。 此代码包含在容器内`<%#: %>`标记数据绑定表达式。 通过将 （:） 添加到末尾`<%#`前缀，数据绑定表达式的结果经过 HTML 编码。 如果结果为 HTML 编码，你的应用程序更好地抵御跨站点脚本注入 (XSS) 和 HTML 注入攻击。

> [!NOTE] 
> 
> **Tip**
> 
> 通过键入在开发过程中添加代码，可以将某些对象的有效成员找到因为强类型数据控件显示可用的成员，基于智能感知。 键入代码，例如属性、 方法和对象时，IntelliSense 将提供相应上下文的代码选择。


在下一步，你将实现`GetCategories`方法来检索数据。

### <a name="linking-the-data-control-to-the-database"></a>将数据控件链接到数据库

数据控件中显示数据之前，你需要将数据控件链接到数据库。 若要使该链接，你可以修改代码隐藏的*Site.Master.cs*文件。

1. 在**解决方案资源管理器**，右键单击*Site.Master*页，然后单击**查看代码**。 *Site.Master.cs*在编辑器中打开文件。
2. 附近的开头*Site.Master.cs*文件中，添加两个其他命名空间，以便所有包含的命名空间显示，如下所示：  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. 添加突出显示`GetCategories`方法之后`Page_Load`事件处理程序，如下所示：  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

使用母版页的任何页面加载浏览器中时，将执行上面的代码。 `ListView`本教程中前面添加的控件 （名为"任一个"） 使用模型绑定来选择数据。 中的标记`ListView`你设置的控件的控件`SelectMethod`属性`GetCategories`上面所示的方法。 `ListView`控制调用`GetCategories`方法在适当的时间在页生命周期，并自动将绑定返回的数据。 你将了解有关数据绑定在下一教程中的详细信息。

### <a name="running-the-application-and-creating-the-database"></a>运行应用程序和创建数据库

本系列教程中前面创建的初始值设定项类 （名为"ProductDatabaseInitializer"） 和指定中的此类*global.asax.cs*文件。 实体框架应用程序运行的第一个时间，因为时将生成数据库`Application_Start`方法中包含*global.asax.cs*文件将调用初始值设定项类。 初始值设定项类将使用的模型类 (`Category`和`Product`) 之前在本系列教程来创建数据库中添加。

1. 在**解决方案资源管理器**，右键单击*Default.aspx*页，选择**设为起始页**。
2. 在 Visual Studio 中按**F5**。   
 需要一些时间来在此首次运行期间设置的所有内容。   
    ![UI 和导航-浏览器窗口](ui_and_navigation/_static/image7.png)  
 运行应用程序时，将编译应用程序和数据库名为*wingtiptoys.mdf*将在其中创建*应用\_数据*文件夹。 在浏览器中，你将看到类别导航菜单。 此菜单是通过从数据库检索类别生成的。 在下一步的教程中，你将实现导航。
3. 关闭浏览器来停止正在运行的应用程序。

### <a name="reviewing-the-database"></a>查看数据库

打开*Web.config*文件并查看连接字符串部分。 你可以看到，`AttachDbFilename`连接字符串中的值指向`DataDirectory`为 Web 应用程序项目。 值`|DataDirectory|`是保留的值，表示*应用\_数据*项目中的文件夹。 此文件夹是已从实体类创建的数据库所在的位置。

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> 如果*应用\_数据*文件夹是不可见，或如果文件夹为空，则选择**刷新**图标，然后**显示所有文件**顶部图标**解决方案资源管理器**窗口。 展开的宽度**解决方案资源管理器**windows 可能需要显示所有可用的图标。


现在你可以检查中包含的数据*wingtiptoys.mdf*数据库文件使用**服务器资源管理器**窗口。

1. 展开*应用\_数据*文件夹。 如果*应用\_数据*文件夹是不可见，请参阅上面的说明。
2. 如果*wingtiptoys.mdf*数据库文件不可见，选择**刷新**图标，然后**显示所有文件**顶部的图标**解决方案资源管理器**窗口。
3. 右键单击*wingtiptoys.mdf*数据库文件并选择**打开**。  
    **服务器资源管理器**显示。 

    ![UI 和导航-服务器资源管理器](ui_and_navigation/_static/image8.png)
4. 展开*表*文件夹。
5. 右键单击**产品**表，然后选择**显示表数据**。  
 **产品**显示表。 

    ![UI 和导航的产品表](ui_and_navigation/_static/image9.png)
6. 此视图允许你查看和修改中的数据**产品**手动表。
7. 关闭**产品**表窗口。
8. 在**服务器资源管理器**，右键单击**产品**再次表，然后选择**打开表定义**。  
 数据设计为**产品**显示表。 

    ![UI 和导航-产品设计](ui_and_navigation/_static/image10.png)
9. 在**T-SQL**选项卡上，你将看到用于创建表的 SQL DDL 语句。 你还可以使用中的用户界面**设计**选项卡以修改架构。
10. 在**服务器资源管理器**，右键单击**WingtipToys**数据库并选择**关闭连接**。   
 通过从 Visual Studio 将数据库分离，将能够更高版本在本教程系列中修改数据库架构。
11. 返回到**解决方案资源管理器**通过选择**解决方案资源管理器**选项卡底部的**服务器资源管理器**窗口。

## <a name="summary"></a>摘要

在本教程中的序列已添加一些基本 UI、 图形、 页和导航。 此外，你已运行的 Web 应用程序，从你在前面的教程中添加的数据类创建数据库。 你还查看的内容*产品*通过直接查看数据库的数据库的表。 在下一步的教程中，将显示数据项和从数据库的详细信息。

## <a name="additional-resources"></a>其他资源

[ASP.NET 网页编程简介](https://msdn.microsoft.com/library/ms178125.aspx)   
[ASP.NET Web 服务器控件概述](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[CSS 教程](http://www.w3schools.com/css/default.asp)

>[!div class="step-by-step"]
[上一页](create_the_data_access_layer.md)
[下一页](display_data_items_and_details.md)
