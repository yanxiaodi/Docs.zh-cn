---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: "生成章下载 EF 5 MVC 的 4 教程 |Microsoft 文档"
author: Rick-Anderson
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 912a1383ed170b49782657372abc1801140df8dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>生成章下载 EF 5 MVC 的 4 教程
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


## <a name="building-the-chapter-downloads"></a>生成章下载

1. 下载并解压缩项目示例的 zip 文件。 在已解压缩的下载包中，你将找到其他 zip 文件，其中一个为完成的每一章。
2. 右键单击所需的 zip 文件中，单击**属性**，然后单击**解除阻止**按钮。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. 将文件解压缩。
4. 双击*CUx.sln*文件以启动 Visual Studio。
5. 从**工具**菜单上，单击**库程序包管理器**，然后**程序包管理器控制台**。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. 在包管理器控制台 (PMC) 中，单击**还原**。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. 退出 Visual Studio。
8. 重新启动 Visual Studio 中，打开在前面步骤中你关闭的解决方案文件。
9. 在包管理器控制台 (PMC) 中，输入`Update-Database`命令：  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > 如果你收到以下错误：  
    >   
    >  *术语更新数据库未被识别为 cmdlet、 函数、 脚本文件或可操作程序的名称。请检查拼写的名称，或如果包括路径，请验证路径正确，然后重试。*  
    > 退出并重新启动 Visual Studio。

    每个迁移要在运行，然后将运行 seed 方法。 你现在可以运行应用程序。

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

>[!div class="step-by-step"]
[上一篇](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
