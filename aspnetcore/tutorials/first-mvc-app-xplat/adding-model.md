---
title: "将模型添加到 ASP.NET Core MVC 应用。"
author: rick-anderson
description: "将模型添加到简单的 ASP.NET Core 应用。"
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 81511b05a3cc11a58b93452d3c6e5305e7ee4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* 将类添加到名为“Movie.cs”的“Models”文件夹。
* 将以下代码添加到“Models/Movie.cs”文件：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

数据库需要 `ID` 字段以获取主键。 

生成应用以确认没有任何错误，最后将 Model 添加到你的 MVC 应用。

## <a name="prepare-the-project-for-scaffolding"></a>准备项目以搭建基架

- 将以下突出显示的 NuGet 包添加到 MvcMovie.csproj 文件：
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- 保存该文件，并对信息性消息“存在未解析的依赖项”选择“还原”。
- 创建 Models/MvcMovieContext.cs 文件并添加以下 `MvcMovieContext` 类：

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- 打开 Startup.cs 文件并添加两个 using：

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- 将数据库上下文添加到 Startup.cs 文件：

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  这会告诉实体框架，数据模型中包含哪些模型类。 现在定义一个 Movie 对象实体集，此实体集会表示为数据库中的一个 Movie 表。

- 生成项目并确定没有任何错误。

## <a name="scaffold-the-moviecontroller"></a>为 MovieController 搭建基架

在项目文件夹中打开终端窗口，然后运行以下命令：

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
基架引擎创建以下组件：

* 电影控制器 (Controllers/MoviesController.cs)
* “创建”、“删除”、“详细信息”、“编辑”和“索引”页面的 Razor 视图文件 (Views/Movies/\*.cshtml)

自动创建 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)（创建、读取、更新和删除）操作方法和视图的过程称为“搭建基架”。 你很快就会拥有一个功能完备的 Web 应用程序，并且你可以使用它管理电影数据库。

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

现在你已拥有用于显示、编辑、更新和删除数据的数据库和页面。 在下一个教程中，我们将使用此数据库。

### <a name="additional-resources"></a>其他资源

* [标记帮助程序](xref:mvc/views/tag-helpers/intro)
* [全球化和本地化](xref:fundamentals/localization)

>[!div class="step-by-step"]
[上一篇 - 添加视图](adding-view.md)
[下一篇 - 使用 SQLite](working-with-sql.md)
