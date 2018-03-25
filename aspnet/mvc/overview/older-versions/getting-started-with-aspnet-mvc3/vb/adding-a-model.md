---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: "添加模型 (VB) |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是一个 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a>添加模型 (VB)
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 的免费版本的 ASP.NET MVC Web 应用程序的基础知识。 在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，你可以单独安装系统必备组件，使用以下链接：
> 
> - [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）
> 
> 如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> Visual Web Developer 项目与 VB.NET 的源代码是可以附带本主题。 [下载 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果你愿意 C#，切换到[C# 版本](../cs/adding-a-model.md)本教程。


## <a name="adding-a-model"></a>添加模型

在本部分中，你将添加用于管理数据库中的电影某些类。 这些类将 ASP.NET MVC 应用程序的"模型"部分。

你将使用名为实体框架的.NET Framework 数据访问技术可以定义并使用这些模型类。 开发范例调用 （通常称为 EF） 的实体框架支持*Code First*。 代码首先，可通过编写简单的类来创建模型对象。 （这些也称为是 POCO 类，从"纯旧式 CLR 对象"。）然后，你可以这样非常干净且快速开发工作流从您的类，动态创建的数据库。

## <a name="adding-model-classes"></a>添加模型类

在**解决方案资源管理器**，右键单击*模型*文件夹，选择**添加**，然后选择**类**。

![](adding-a-model/_static/image1.png)

将类"电影"。

添加到以下五种属性`Movie`类：

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

我们将使用`Movie`类来表示数据库中的电影。 每个实例`Movie`对象将对应于数据库表中，并的每个属性中的行`Movie`类将映射到表中的列。

在同一文件中，添加以下`MovieDBContext`类：

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

`MovieDBContext`类表示的实体框架的电影数据库上下文，用于处理提取、 存储和更新`Movie`类数据库中的实例。 `MovieDBContext`派生自`DbContext`由实体框架提供的基类。 有关详细信息`DbContext`和`DbSet`，请参阅[实体框架的工作效率改进](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)。

若要引用`DbContext`和`DbSet`，你需要添加以下`imports`语句文件的顶部：

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

完整*Movie.vb*文件如下所示。

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>创建连接字符串和使用 SQL Server Compact

`MovieDBContext`你创建的类处理任务连接到数据库并映射`Movie`数据库记录的对象。 一个问题可能会问，不过，是如何指定它将连接到的数据库。 将执行该操作通过添加中的连接信息*Web.config*应用程序文件。

打开应用程序根目录*Web.config*文件。 (不*Web.config*文件中*视图*文件夹。)下图显示两者*Web.config*文件; 打开*Web.config*以红色圆圈显示文件。

![](adding-a-model/_static/image2.png)

## 

添加到以下连接字符串`<connectionStrings>`中的元素*Web.config*文件。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

下面的示例显示的一部分*Web.config*文件以添加新的连接字符串：

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

这种少量的代码和 XML 是你需要编写以便表示，并将影片数据存储在数据库中的所有内容。

接下来，你将生成一个新`MoviesController`类，该类可以用于显示影片数据，并允许用户创建新的影片列表。

>[!div class="step-by-step"]
[上一页](adding-a-view.md)
[下一页](accessing-your-models-data-from-a-controller.md)
