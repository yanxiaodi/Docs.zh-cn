---
title: "在 ASP.NET Core 中向 Razor 页面应用添加模型"
author: rick-anderson
description: "在 ASP.NET Core 中向 Razor 页面应用添加模型"
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0ce7693bfdc37d930488304b329dbcd533a5ec1d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a>向 Razor 页面应用添加模型

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>添加数据模型

在解决方案资源管理器中，右键单击“RazorPagesMovie”项目 >“添加” > “新建文件夹”。 将文件夹命名为“Models”。

右键单击“Models”文件夹。 选择“添加” > “类”。 将类命名为“Movie”，并添加以下属性：

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>添加数据库连接字符串

将连接字符串添加到 appsettings.json 文件。

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>注册数据库上下文

使用 Startup.cs 文件中的[依存关系注入](xref:fundamentals/dependency-injection)容器注册数据库上下文。

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

生成项目以验证有没有任何错误存在。

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>添加基架工具并执行初始迁移

在本部分中，使用程序包管理器控制台 (PMC) 执行以下操作：

* 添加 Visual Studio Web 代码生成包。 必须添加此包才能运行基架引擎。
* 添加初始迁移。
* 使用初始迁移来更新数据库。

从“工具”菜单中，选择“NuGet 包管理器” > “包管理器控制台”。

  ![PMC 菜单](../first-mvc-app/adding-model/_static/pmc.png)

在 PMC 中，输入以下命令：

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

`Install-Package` 命令安装运行基架引擎所需的工具。

`Add-Migration` 命令生成用于创建初始数据库架构的代码。 此架构以（Models/MovieContext.cs 文件中的）`DbContext` 中指定的模型为基础。 `Initial` 参数用于为迁移命名。 可以使用任意名称，但是按照惯例应选择描述迁移的名称。 有关详细信息，请参阅[迁移简介](xref:data/ef-mvc/migrations#introduction-to-migrations)。

`Update-Database` 命令在用于创建数据库的 Migrations/\<time-stamp>_InitialCreate.cs 文件中运行 `Up` 方法。

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a>测试应用

* 运行应用并将 `/Movies` 追加到浏览器中的 URL (`http://localhost:port/movies`)。
* 测试“创建”链接。

 ![创建页面](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* 测试“编辑”、“详细信息”和“删除”链接。

如果收到 SQL 异常，则验证是否已运行迁移并更新了数据库：

下一个教程介绍由基架创建的文件。

>[!div class="step-by-step"]
[上一篇：入门](xref:tutorials/razor-pages/razor-pages-start)
[下一篇：已搭建基架的 Razor 页面](xref:tutorials/razor-pages/page)    
