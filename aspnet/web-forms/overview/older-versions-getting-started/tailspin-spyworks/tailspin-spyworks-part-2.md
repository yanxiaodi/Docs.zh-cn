---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: "第 2 部分： 数据访问层 |Microsoft 文档"
author: JoeStagner
description: "本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 2 部分介绍如何添加数据访问层。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8b07b320640c1bb0074a4d3a04ca7c5b7e7bb6cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-2-data-access-layer"></a>第 2 部分： 数据访问层
====================
通过[Joe stagner 将](https://github.com/JoeStagner)

> Tailspin Spyworks 演示创建在.NET 平台的功能强大、 可扩展应用程序是如何非常简单。 它演示如何使用 ASP.NET 4 中出色的新功能构建在线商店，包括购物、 结帐和管理关闭。
> 
> 本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 2 部分介绍如何添加数据访问层。


## <a id="_Toc260221668"></a>添加数据访问层

我们电子商务应用程序将依赖于两个数据库。

有关客户信息中，我们将使用标准的 ASP.NET 成员资格数据库。 我们购物车和产品目录我们将实现 SQL Express 数据库，如下所示。

![](tailspin-spyworks-part-2/_static/image1.jpg)

具有在应用程序的应用程序中创建数据库 (Commerce.mdf)\_我们可以继续创建使用.NET Entity Framework 我们数据访问层的数据文件夹。

我们将创建名为的文件夹"数据\_访问"然后它们右键单击该文件夹并选择"添加新项"。

在"已安装的模板"项，然后选择"ADO.NET 实体数据模型"输入 EDM\_Commerce.edmx 作为名称，然后单击"添加"按钮。

![](tailspin-spyworks-part-2/_static/image2.jpg)

选择"从数据库生成"。

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

保存并生成。

现在我们已准备好添加我们第一项功能 – 产品类别菜单。

>[!div class="step-by-step"]
[上一页](tailspin-spyworks-part-1.md)
[下一页](tailspin-spyworks-part-3.md)
