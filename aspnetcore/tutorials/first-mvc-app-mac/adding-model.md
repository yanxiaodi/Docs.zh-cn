---
title: "将模型添加到 ASP.NET Core MVC 应用"
author: rick-anderson
description: "将模型添加到简单的 ASP.NET Core 应用。"
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.devlang: csharp
ms.prod: .net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: bf4d5d289266b585cbdfbb70c7482620fd4ced54
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* 右键单击“Models”文件夹，然后选择“添加” > “新建文件”。 
* 在“新建文件”对话框中：

  * 在左侧窗格中，选择“常规”。
  * 在中间窗格中，选择“空类”。
  * 将此类命名为“Movie”，然后选择“新建”。

向 `Movie` 类添加以下属性：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

数据库需要 `ID` 字段以获取主键。

生成项目以验证有没有任何错误存在。 现在 MVC 应用中已具有模型。

## <a name="prepare-the-project-for-scaffolding"></a>准备项目以搭建基架

- 右键单击项目文件，然后选择“工具”>“编辑文件”。

  ![上述步骤的视图](adding-model/_static/1.png)

- 将以下突出显示的 NuGet 包添加到 MvcMovie.csproj 文件：
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- 保存该文件。

- 创建 Models/MvcMovieContext.cs 文件并添加以下 `MvcMovieContext` 类：[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- 打开 Startup.cs 文件并添加两个 using：[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- 将数据库上下文添加到 Startup.cs 文件：

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  这会告诉实体框架，数据模型中包含哪些模型类。 现在定义一个 Movie 对象实体集，此实体集会表示为数据库中的一个 Movie 表。

- 生成项目并确定没有任何错误。

## <a name="scaffold-the-moviecontroller"></a>为 MovieController 搭建基架

在项目文件夹中打开终端窗口，然后运行以下命令：

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
如果收到错误 `No executable found matching command "dotnet-aspnet-codegenerator", verify`：

 * 你现在位于项目目录中。 项目目录中包含 Program.cs、Startup.cs、和 .csproj 文件。
 * 你的 dotnet 版本是 1.1 或更高版本。 运行 `dotnet` 获取版本。
 * 你已将 `<DotNetCliToolReference>` 元素添加到 [MvcMovie.csproj 文件](#prepare-the-project-for-scaffolding)。
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

基架引擎创建以下组件：

* 电影控制器 (Controllers/MoviesController.cs)
* “创建”、“删除”、“详细信息”、“编辑”和“索引”页面的 Razor 视图文件 (Views/Movies/\*.cshtml)

自动创建 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)（创建、读取、更新和删除）操作方法和视图的过程称为“搭建基架”。 你很快将具有功能完整的 Web 应用程序，可使用此应用程序管理电影数据库。

### <a name="add-the-files-to-visual-studio"></a>将文件添加到 Visual Studio

* 将 MovieController.cs 文件添加到 Visual Studio 项目：

  * 右键单击“控制器”文件夹，然后选择“添加”>“添加文件”。
  * 选择 MovieController.cs 文件。

* 添加“电影”文件夹和视图：

  * 右键单击“视图”文件夹，然后选择“添加”>“添加现有文件夹”。
  * 导航到“视图”文件夹，选择“视图\电影”，然后选择“打开”。
  * 在“从‘电影’选择要添加的文件”对话框中，选择“包括全部”，然后选择“确定”。

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

现在你已拥有用于显示、编辑、更新和删除数据的数据库和页面。 在下一个教程中，我们将使用此数据库。

## <a name="additional-resources"></a>其他资源

* [标记帮助程序](xref:mvc/views/tag-helpers/intro)
* [全球化和本地化](xref:fundamentals/localization)

>[!div class="step-by-step"]
[上一篇：添加视图](adding-view.md)
[下一篇：使用 SQL](working-with-sql.md)  
