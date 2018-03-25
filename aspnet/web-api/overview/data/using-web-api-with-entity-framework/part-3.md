---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "使用 Code First 迁移植入到数据库 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 1ca627397f0f100d13388f9afc27ff481886e098
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="use-code-first-migrations-to-seed-the-database"></a>使用 Code First 迁移植入到数据库
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://github.com/MikeWasson/BookService)

在本部分中，你将使用[Code First 迁移](https://msdn.microsoft.com/data/jj591621)EF 植入使用测试数据到数据库中。

从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在 Package Manager Console 窗口中，输入以下命令：

[!code-console[Main](part-3/samples/sample1.cmd)]

此命令将添加到项目中，名为迁移的文件夹以及一个名为 Configuration.cs Migrations 文件夹中的代码文件。

![](part-3/_static/image1.png)

打开 Configuration.cs 文件。 添加以下**使用**语句。

[!code-csharp[Main](part-3/samples/sample2.cs)]

然后添加以下代码到**Configuration.Seed**方法：

[!code-csharp[Main](part-3/samples/sample3.cs)]

在 Package Manager Console 窗口中，键入以下命令：

[!code-console[Main](part-3/samples/sample4.cmd)]

第一个命令生成代码来创建数据库，并第二个命令执行该代码。 数据库在本地创建，使用[LocalDB](https://msdn.microsoft.com/library/hh510202.aspx)。

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>浏览 API （可选）

按 F5 在调试模式下运行该应用程序。 Visual Studio 启动 IIS Express，并运行你的 web 应用。 然后，visual Studio 启动浏览器，并打开应用程序的主页。

当 Visual Studio 运行 web 项目时，它将分配一个端口号。 在下图中，端口号是 50524。 运行应用程序时，你将看到不同的端口号。

![](part-3/_static/image3.png)

使用 ASP.NET MVC 实现主页页面。 在页面顶部，没有指出"API"的链接。 此链接使你可以自动生成的帮助页，web API。 (若要了解如何生成此帮助页，以及如何向页面添加你自己的文档，请参阅[创建 ASP.NET Web api 的帮助页](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。)你可以单击帮助页链接以查看有关 API，包括请求和响应格式的详细信息。

![](part-3/_static/image4.png)

API 使数据库上的 CRUD 操作。 下面汇总了 API。

| 作者 |  |
| --- | -- |
| 获取 api/作者 | 获取所有作者。 |
| GET api/作者 / {id} | 获取由 ID 作者 |
| POST/api/作者 | 创建新的作者。 |
| PUT /api/作者 / {id} | 更新现有作者。 |
| DELETE /api/authors/{id} | 删除作者。 |

| 图书 |  |
| --- | -- |
| 获取 /api/books | 获取所有书籍。 |
| 获取 /api/丛书 / {id} | 获取一本书的 id。 |
| 发布/api/丛书 | 创建新的书籍。 |
| PUT /api/丛书 / {id} | 更新现有书籍。 |
| 删除 /api/丛书 / {id} | 删除一本书。 |

## <a name="view-the-database-optional"></a>查看数据库 （可选）

当你运行 Update-database 命令时，EF 创建数据库，并调用`Seed`方法。 本地运行应用程序时，使用 EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)。 你可以在 Visual Studio 中查看数据库。 从**视图**菜单上，选择**SQL Server 对象资源管理器**。

![](part-3/_static/image5.png)

在**连接到服务器**对话框，请在**服务器名称**编辑框中，键入"(localdb) \v11.0"。 保留**身份验证**作为"Windows 身份验证"选项。 单击“连接” 。

![](part-3/_static/image6.png)

Visual Studio 连接到 LocalDB，并显示了 SQL Server 对象资源管理器窗口中的现有数据库。 你可以展开节点以查看 EF 创建的表。

![](part-3/_static/image7.png)

若要查看数据，右击某个表，然后选择**查看数据**。

![](part-3/_static/image8.png)

下面的屏幕截图显示了丛书表的结果。 请注意，EF 填充数据库种子数据，并且表中包含的外键和作者表。

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
[上一页](part-2.md)
[下一页](part-4.md)
