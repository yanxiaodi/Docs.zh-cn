---
title: "使用 SQL Server LocalDB"
author: rick-anderson
description: "结合简单的 MVC 应用使用 SQL Server LocalDB"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: a0aa6fdfa51650628021a4ba6d0533e7e0e39200
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-sql-server-localdb"></a>使用 SQL Server LocalDB

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` 对象处理连接到数据库并将 `Movie` 对象映射到数据库记录的任务。 在 Startup.cs 文件的 `ConfigureServices` 方法中向[依赖关系注入](xref:fundamentals/dependency-injection)容器注册数据库上下文：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

ASP.NET Core [配置](xref:fundamentals/configuration/index)系统会读取 `ConnectionString`。 为了进行本地开发，它会从 appsettings.json 文件获取连接字符串：

[!code-json[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

将应用部署到测试或生产服务器时，可使用环境变量或另一种方法将连接字符串设置为实际的 SQL Server。 有关详细信息，请参阅[配置](xref:fundamentals/configuration/index)。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB 是轻型版的 SQL Server Express 数据库引擎，以程序开发为目标。 LocalDB 按需启动并在用户模式下运行，因此没有复杂的配置。 默认情况下，LocalDB 数据库在 C:/Users/\<user\> 目录中创建“\*.mdf”文件。

* 从“视图”菜单中，打开“SQL Server 对象资源管理器”(SSOX)。

  ![“视图”菜单](working-with-sql/_static/ssox.png)

* 右键单击 `Movie` 表，然后单击“视图设计器”

  ![Movie 表上打开的上下文菜单](working-with-sql/_static/design.png)

  ![设计器中打开的 Movie 表](working-with-sql/_static/dv.png)

请注意 `ID` 旁边的密钥图标。 默认情况下，EF 将名为 `ID` 的属性设置为主键。

* 右键单击 `Movie` 表，然后单击“查看数据”

  ![Movie 表上打开的上下文菜单](working-with-sql/_static/ssox2.png)

  ![显示表数据的打开的 Movie 表](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a>设定数据库种子

在 Models 文件夹中创建一个名为 `SeedData` 的新类。 将生成的代码替换为以下代码：

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

如果 DB 中没有任何电影，则会返回种子初始值设定项，并且不会添加任何电影。

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>添加种子初始值设定项

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

将种子初始值设定项添加 Program.cs 文件中的 `Main` 方法：

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

将种子初始值设定项添加到 Startup.cs 文件中的 `Configure` 方法末尾：

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

测试应用

* 删除 DB 中的所有记录。 可以使用浏览器中的删除链接，也可从 SSOX 执行此操作。
* 强制应用初始化（调用 `Startup` 类中的方法），使种子方法能够正常运行。 若要强制进行初始化，必须先停止 IIS Express，然后再重新启动它。 可以使用以下任一方法来执行此操作：

  * 右键单击通知区域中的 IIS Express 系统任务栏图标，然后点击“退出”或“停止站点”

    ![IIS Express 系统任务栏图标](working-with-sql/_static/iisExIcon.png)

    ![上下文菜单](working-with-sql/_static/stopIIS.png)

   * 如果是在非调试模式下运行 VS 的，请按 F5 以在调试模式下运行
   * 如果是在调试模式下运行 VS 的，请停止调试程序并按 F5
   
应用将显示设定为种子的数据。

![在 Microsoft Edge 中打开的显示电影数据的 MVC 电影应用程序](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
[上一页](adding-model.md)
[下一页](controller-methods-views.md)  
