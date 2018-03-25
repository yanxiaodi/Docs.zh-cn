---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: "创建连接字符串和使用 SQL Server LocalDB |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 25d1c1c9954baaca9ef91eff3dd3c853930a5893
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>创建连接字符串和使用 SQL Server LocalDB
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>创建连接字符串和使用 SQL Server LocalDB

`MovieDBContext`你创建的类处理任务连接到数据库并映射`Movie`数据库记录的对象。 一个问题可能会问，不过，是如何指定它将连接到的数据库。 您不必实际来指定要使用的数据库，实体框架将默认为使用[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)。 在本部分中我们将显式添加中的连接字符串*Web.config*应用程序文件。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)是 SQL Server Express 数据库引擎的按需启动并在用户模式下运行的轻量版本。 LocalDB 运行中的 SQL Server Express，可用于处理数据库作为一种特殊的执行模式*.mdf*文件。 通常，将 LocalDB 数据库文件保存在*应用\_数据*web 项目的文件夹。

不建议在生产 web 应用程序中使用 SQL Server Express。 LocalDB 尤其应不用于与 web 应用程序的生产因为它不是使用 IIS。 但是，LocalDB 数据库可以轻松地迁移到 SQL Server 或 SQL Azure。

在 Visual Studio 2017，默认情况下，使用 Visual Studio 安装 LocalDB。

默认情况下，实体框架将查找名为与对象上下文类相同的连接字符串 (`MovieDBContext`为此项目)。 有关详细信息请参阅[ASP.NET Web 应用程序的 SQL Server 连接字符串](https://msdn.microsoft.com/library/jj653752.aspx)。

打开应用程序根目录*Web.config*文件如下所示。 (不*Web.config*文件中*视图*文件夹。)

![](creating-a-connection-string/_static/image1.png)

查找`<connectionStrings>`元素：

![](creating-a-connection-string/_static/image2.png)

添加到以下连接字符串`<connectionStrings>`中的元素*Web.config*文件。

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

下面的示例显示的一部分*Web.config*文件以添加新的连接字符串：

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

两个连接字符串是非常相似。 第一个连接字符串命名为`DefaultConnection`和用于成员资格数据库以控制可以访问应用程序。 你已添加的连接字符串指定一个名为的 LocalDB 数据库*Movie.mdf*位于*应用\_数据*文件夹。 我们不会在本教程中，有关详细信息成员身份、 身份验证和安全中使用成员资格数据库，请参阅我的教程[使用身份验证和 SQL 数据库中创建的 ASP.NET MVC 应用并部署到 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。

连接字符串的名称必须与匹配的名称[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)类。

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

你实际上不需要添加`MovieDBContext`连接字符串。 如果未指定连接字符串，实体框架将带有完全限定名称的用户目录中创建 LocalDB 数据库[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)类 (在这种情况下`MvcMovie.Models.MovieDBContext`)。 你可以将数据库命名您喜欢的任何，只要它具有*。MDF*后缀。 例如，我们无法将数据库命名*MyFilms.mdf*。

接下来，你将生成一个新`MoviesController`类，该类可以用于显示影片数据，并允许用户创建新的影片列表。

>[!div class="step-by-step"]
[上一页](adding-a-model.md)
[下一页](accessing-your-models-data-from-a-controller.md)
