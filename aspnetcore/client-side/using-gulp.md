---
title: "在 ASP.NET 核心中使用 Gulp"
author: rick-anderson
description: "了解如何在 ASP.NET 核心中使用 Gulp。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: 0a3443e8187d46992f55dc537d0f400c6771c50c
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a>在 ASP.NET 核心中使用 Gulp 简介 

通过[艾力克 Reitan](https://github.com/Erikre)， [Scott Addie](https://scottaddie.com)， [Daniel Roth](https://github.com/danroth27)，和[Shayne 贝叶](https://twitter.com/spboyer)

在典型的现代 web 应用中，可能会生成过程：

* 捆绑和 minify JavaScript 和 CSS 文件。
* 运行工具以调用之前每个生成的绑定和缩减任务。
* 小于编译或 SASS 文件复制到 CSS。
* 编译 CoffeeScript 或 TypeScript 文件添加到 JavaScript。

A*任务运行程序*是一种工具，可以自动进行这些例程开发任务和的详细信息。 Visual Studio 为两个流行的基于 JavaScript 的任务流道提供内置支持： [Gulp](https://gulpjs.com/)和[Grunt](using-grunt.md)。

## <a name="gulp"></a>gulp

Gulp 是一个基于 JavaScript 的流式处理生成工具包，客户端代码。 它通常用于在生成环境中触发特定事件时流通过一系列的进程的客户端文件。 例如，使用 Gulp 来自动执行[绑定和缩减](bundling-and-minification.md)或清理新生成前的开发环境。

在中定义一组 Gulp 任务*gulpfile.js*。 以下 JavaScript 包括 Gulp 模块，并指定文件路径，以在即将推出的任务中引用：

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

上面的代码中指定的节点模块所需。 `require`函数导入每个模块，以便依赖任务可以使用其功能。 每个导入的模块被分配给变量。 模块可以位于按名称或路径。 在此示例中，模块名为`gulp`， `rimraf`， `gulp-concat`， `gulp-cssmin`，和`gulp-uglify`按名称检索。 此外，创建一系列的路径，以便可以重复使用并在任务中引用的 CSS 和 JavaScript 文件的位置。 下表提供了的模块中包含的描述*gulpfile.js*。

| 模块名 | 描述 |
| ----------- | ----------- |
| gulp        | Gulp 流式处理生成系统中。 有关详细信息，请参阅[gulp](https://www.npmjs.com/package/gulp)。 |
| rimraf      | 节点删除模块。 有关详细信息，请参阅[rimraf](https://www.npmjs.com/package/rimraf)。 |
| gulp-concat | 一个连接基于操作系统的换行字符的文件的模块。 有关详细信息，请参阅[gulp concat](https://www.npmjs.com/package/gulp-concat)。 |
| gulp-cssmin | Minifies CSS 文件模块。 有关详细信息，请参阅[gulp cssmin](https://www.npmjs.com/package/gulp-cssmin)。 |
| gulp-uglify | Minifies 模块*.js*文件。 有关详细信息，请参阅[gulp uglify](https://www.npmjs.com/package/gulp-uglify)。 |

必备项的模块将导入后，可以指定任务。 此处有六项任务注册，表示通过以下代码：

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

下表提供了在上面的代码中指定的任务的说明：

|任务名称|描述|
|--- |--- |
|clean:js|使用 rimraf 节点删除模块删除 site.js 文件的缩减的版本的任务。|
|clean:css|使用 rimraf 节点删除模块删除 site.css 文件的缩减的版本的任务。|
|清理|一个任务，它调用`clean:js`任务后, 跟`clean:css`任务。|
|min:js|Minifies 和串联的 js 文件夹中的所有.js 文件的任务。 。 排除 min.js 文件。|
|min:css|Minifies 和串联的 css 文件夹中的所有.css 文件的任务。 。 排除 min.css 文件。|
|min|一个任务，它调用`min:js`任务后, 跟`min:css`任务。|

## <a name="running-default-tasks"></a>正在运行的默认任务

如果你尚未创建新的 Web 应用程序，请在 Visual Studio 中创建新的 ASP.NET Web 应用程序项目。

1.  将新的 JavaScript 文件添加到你的项目并将其命名*gulpfile.js*，然后将以下代码复制。

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  打开*package.json*文件 (添加如果不是存在) 并添加以下。

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  在**解决方案资源管理器**，右键单击*gulpfile.js*，然后选择**任务运行程序资源管理器**。
    
    ![从解决方案资源管理器中打开任务运行程序资源管理器](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **任务运行程序资源管理器**显示 Gulp 任务的列表。 (你可能需要单击**刷新**显示项目名称左侧的按钮。)
    
    ![任务运行程序资源管理器](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  在此之下**任务**中**任务运行程序资源管理器**，右键单击**干净**，然后选择**运行**从弹出菜单。

    ![任务运行程序资源管理器清理任务](using-gulp/_static/04-TaskRunner-clean.png)

    **任务运行程序资源管理器**将创建名为的新选项卡**干净**和执行清理任务，如中定义*gulpfile.js*。

5.  右键单击**干净**任务，然后选择**绑定** > **之前生成**。

    ![任务运行程序资源管理器绑定 BeforeBuild](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **之前生成**绑定将配置 clean 任务，以在每次生成项目之前自动运行。

绑定您设置**任务运行程序资源管理器**顶部的注释的形式存储在你*gulpfile.js*和仅在 Visual Studio 中有效。 不需要 Visual Studio 的替代方法是配置 gulp 中的任务自动执行你*.csproj*文件。 例如，将这个放你*.csproj*文件：

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

现在 Visual Studio 中或从命令提示符处使用运行项目时执行清理任务[dotnet 运行](/dotnet/core/tools/dotnet-run)命令 (运行`npm install`第一个)。

## <a name="defining-and-running-a-new-task"></a>定义和运行新任务

若要定义新的 Gulp 任务，修改*gulpfile.js*。

1.  末尾添加以下 JavaScript *gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    此任务名为`first`，和它只需显示的字符串。

2.  保存*gulpfile.js*。

3.  在**解决方案资源管理器**，右键单击*gulpfile.js*，然后选择*任务运行程序资源管理器*。

4.  在**任务运行程序资源管理器**，右键单击**第一个**，然后选择**运行**。

    ![任务运行程序资源管理器运行第一个任务](using-gulp/_static/06-TaskRunner-First.png)

    你将看到，输出文本已显示。 如果你有兴趣基于一种常见方案的示例，请参阅的 Gulp 配方。

## <a name="defining-and-running-tasks-in-a-series"></a>定义和一系列中运行任务

当你运行多个任务时，任务将默认情况下同时运行。 但是，如果你需要以特定顺序运行任务，你必须指定每个任务过程何时完成，以及为哪些任务依赖于另一个任务完成。

1.  若要定义要按顺序运行的任务的一系列，替换`first`中前面添加的任务*gulpfile.js*替换为以下：

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    你现在具有三个任务： `series:first`， `series:second`，和`series`。 `series:second`任务包括指定任务要运行和完成之前的数组的第二个参数`series:second`任务将运行。 根据上面，唯一的代码中的指定`series:first`任务必须完成之前`series:second`任务将运行。

2.  保存*gulpfile.js*。

3.  在**解决方案资源管理器**，右键单击*gulpfile.js*和选择**任务运行程序资源管理器**如果尚未打开。

4.  在**任务运行程序资源管理器**，右键单击**系列**和选择**运行**。

    ![任务运行程序资源管理器运行系列任务](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense 提供了代码完成、 参数说明和其他功能来提高工作效率，并减少错误。 用 JavaScript; 编写 gulp 任务因此，IntelliSense 可以提供开发时的协助。 当你使用 JavaScript，IntelliSense 也会列出对象、 函数、 属性和可用的参数基于当前的上下文。 从 intellisense 来完成代码提供的弹出列表中选择编码选项。

![IntelliSense 的 gulp](using-gulp/_static/08-IntelliSense.png)

Intellisense 的详细信息，请参阅[JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense)。

## <a name="development-staging-and-production-environments"></a>开发、 过渡和生产环境

当 Gulp 用于优化过渡和生产客户端文件时，处理的文件将保存到本地的过渡和生产位置。 *_Layout.cshtml*文件使用**环境**标记帮助器提供两个不同版本的 CSS 文件。 CSS 文件的一个版本是用于开发和另一个版本进行了优化的过渡和生产。 在 Visual Studio 2017，当你更改**ASPNETCORE_ENVIRONMENT**环境变量`Production`，Visual Studio 将生成 Web 应用程序和链接到的最小化 CSS 文件。 下面的标记演示**环境**标记帮助程序包含将标记与`Development`CSS 文件和缩减`Staging, Production`CSS 文件。

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a>环境之间切换

若要针对不同的环境编译之间切换，请修改**ASPNETCORE_ENVIRONMENT**环境变量的值。

1.  在**任务运行程序资源管理器**，验证**min**任务已设置为运行**之前生成**。

2.  在**解决方案资源管理器**，右键单击项目名称并选择**属性**。

    在显示属性表为 Web 应用。

3.  单击“调试”选项卡。

4.  设置的值**宿主： 环境**环境变量`Production`。

5.  按**F5**在浏览器中运行该应用程序。

6.  在浏览器窗口中，右击该页并选择**查看源**若要查看的 HTML 页。

    请注意，样式表链接指向的缩减的 CSS 文件。

7.  关闭浏览器来停止 Web 应用。

8.  在 Visual Studio 中，返回到 Web 应用的属性表，并更改**宿主： 环境**环境变量回`Development`。

9.  按**F5**再次在浏览器中运行该应用程序。

10. 在浏览器窗口中，右击该页并选择**查看源**若要查看的 HTML 页。

    请注意，样式表链接指向 CSS 文件的 unminified 版本。

有关 ASP.NET 核心中的环境的详细信息，请参阅[使用多个环境](../fundamentals/environments.md)。

## <a name="task-and-module-details"></a>任务和模块的详细信息

Gulp 任务已注册到的函数名称。 如果其他任务都必须运行在当前任务之前，你可以指定依赖关系。 其他函数，您可以运行和监视 Gulp 任务，以及将源设置 (*src*) 和目标 (*dest*) 正在修改的文件。 以下是主 Gulp API 函数：

|Gulp 函数|语法|描述|
|---   |--- |--- |
|任务  |`gulp.task(name[, deps], fn) { }`|`task`函数创建的任务。 `name`参数定义任务的名称。 `deps`参数包含要运行此任务之前完成的任务的数组。 `fn`参数表示执行任务的操作的回调函数。|
|监视 |`gulp.watch(glob [, opts], tasks) { }`|`watch`发生文件更改时，函数会监视文件和运行任务。 `glob`参数是`string`或`array`，它确定要监视哪些文件。 `opts`参数提供了监视选项的其他文件。|
|src   |`gulp.src(globs[, options]) { }`|`src`函数提供了 glob 值匹配的文件。 `glob`参数是`string`或`array`，它确定哪些文件读取。 `options`参数提供附加文件选项。|
|目标  |`gulp.dest(path[, options]) { }`|`dest`函数定义可以向其写入文件的位置。 `path`参数是字符串或确定目标文件夹的函数。 `options`参数是一个对象，指定输出文件夹的选项。|

有关其他的 Gulp API 参考信息，请参阅[Gulp 文档 API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)。

## <a name="gulp-recipes"></a>Gulp 配方

Gulp 社区提供 Gulp[配方](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md)。 这些配方包含 Gulp 任务用于针对常见情况。

## <a name="additional-resources"></a>其他资源

* [Gulp 文档](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [绑定和缩减中 ASP.NET 核心](bundling-and-minification.md)
* [在 ASP.NET 核心中使用 Grunt](using-grunt.md)
