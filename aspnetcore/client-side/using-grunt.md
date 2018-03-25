---
title: "在 ASP.NET 核心中使用 Grunt"
author: rick-anderson
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-grunt
ms.openlocfilehash: c23f170b36ac1b9623835337020f2b5ac9514971
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="using-grunt-in-aspnet-core"></a>在 ASP.NET 核心中使用 Grunt 

通过[了米](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt 是自动化脚本缩减、 TypeScript 编译、 代码质量"链接形式"工具、 CSS 预处理器和几乎任何重复繁琐的工作，需要采取措施来支持客户端开发的 JavaScript 任务运行程序。 尽管 ASP.NET 项目模板默认情况下使用 Gulp Visual Studio 中，完全支持 grunt (请参阅[使用 Gulp](using-gulp.md))。

此示例使用一个空的 ASP.NET Core 项目作为起始点，以显示如何从零开始的客户端生成过程中自动运行。

完成的示例清除目标部署目录、 将合并 JavaScript 文件，检查代码质量，会将 JavaScript 文件内容和将部署到 web 应用程序的根目录。 我们将使用下列包：

* **grunt**: Grunt 任务运行程序包。

* **grunt contrib 清理**： 删除文件或目录的插件。

* **grunt contrib jshint**： 查看 JavaScript 代码质量的插件。

* **grunt contrib concat**： 将文件联接成单个文件的插件。

* **grunt contrib uglify**: minifies JavaScript 以减少大小的插件。

* **grunt contrib 监视**： 监视文件活动的插件。

## <a name="preparing-the-application"></a>准备好应用程序

若要开始，设置新的空 web 应用程序，并添加 TypeScript 示例文件。 TypeScript 文件将会自动编译到 JavaScript 使用默认 Visual Studio 设置，并且将是我们原材料处理使用 Grunt。

1.  在 Visual Studio 中，创建一个新`ASP.NET Web Application`。

2.  在**新建 ASP.NET 项目**对话框中，选择 ASP.NET Core**空**模板，然后单击确定按钮。

3.  在解决方案资源管理器，查看的项目结构。 `\src`文件夹包含空`wwwroot`和`Dependencies`节点。

    ![空 web 解决方案](using-grunt/_static/grunt-solution-explorer.png)

4.  添加一个名为的新文件夹`TypeScript`到你的项目目录。

5.  在添加之前的任何文件，请确保 Visual Studio 具有选项编译保存 TypeScript 文件检查。 导航到**工具** > **选项** > **文本编辑器** > **Typescript**  > **项目**:

    ![设置自动 compliation TypeScript 文件的选项](using-grunt/_static/typescript-options.png)

6.  右键单击`TypeScript`目录，然后选择**添加 > 新项**从上下文菜单。 选择**JavaScript 文件**项并将该文件命名*Tastes.ts* (请注意\*.ts 扩展)。 将以下 TypeScript 代码的行复制到文件 (在您保存，新*Tastes.js*文件将出现与 JavaScript 源)。
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  添加的第二个文件，以便**TypeScript**目录并将其命名`Food.ts`。 将下面的代码复制到文件。

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }
    
      private _name: string;
      get Name() {
        return this._name;
      }
    
      private _calories: number;
      get Calories() {
        return this._calories;
      }
    
      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a>配置 NPM

接下来，配置 NPM 来下载 grunt 和 grunt 任务。

1. 在解决方案资源管理器，右键单击该项目并选择**添加 > 新项**从上下文菜单。 选择**NPM 配置文件**项，保留默认名称， *package.json*，然后单击**添加**按钮。

2. 在*package.json*文件内,`devDependencies`对象大括号中，输入"grunt"。 选择`grunt`从智能感知列表并按 Enter 键。 Visual Studio 将 quote grunt 包名称，并添加一个冒号。 从 Intellisense 列表的顶部中的冒号右侧，选择包的最新稳定版本 (按`Ctrl-Space`如果 Intellisense 不显示)。

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > 使用 NPM[语义版本控制](http://semver.org/)来组织依赖关系。 语义版本控制，也称为 SemVer，标识包的编号方案<major>。<minor>。<patch>.Intellisense 显示仅几个常用的选项，从而简化了语义版本控制。 Intellisense 列表 (在上面的示例 0.4.5) 中的顶级项被视为包的最新稳定版本。 脱字号 (^) 符号匹配的最新的主版本和波形符 （~） 与最新次要版本匹配。 请参阅[NPM semver 版本分析器参考](https://www.npmjs.com/package/semver)作为 SemVer 提供其完整表达的指南。

3. 添加更多的依赖关系，以加载 grunt-contrib-\*打包以*干净*， *jshint*， *concat*， *uglify*，和*监视*下面的示例中所示。 版本不需要与示例匹配。

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. 保存*package.json*文件。

将下载的每个 devDependencies 项包，以及每个包需要的任何文件。 你可以查找中的包文件`node_modules`通过启用目录**显示所有文件**在解决方案资源管理器的按钮。

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> 如果需要可以通过右键单击来手动还原解决方案资源管理器中的依赖关系`Dependencies\NPM`并选择**还原包**菜单选项。

![还原包](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>配置 Grunt

使用名为的清单配置 grunt *Gruntfile.js*的定义、 加载和注册任务可以手动运行或配置以自动基于运行 Visual Studio 中的事件。

1.  右键单击项目并选择**添加 > 新项**。 选择**Grunt 配置文件**选项，请保留默认名称， *Gruntfile.js*，然后单击**添加**按钮。

    初始代码包含了一个模块定义和`grunt.initConfig()`方法。 `initConfig()`用于为每个包中，设置选项和模块的剩余部分将会加载和注册任务。
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. 内部`initConfig()`方法，添加用于`clean`任务示例中所示*Gruntfile.js*下面。 Clean 任务，接受目录字符串的数组。 此任务从 wwwroot/lib 中删除文件，并将移除整个/临时目录。

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. 下面 initConfig() 方法中，添加对的调用`grunt.loadNpmTasks()`。 这将从 Visual Studio 来使该任务可运行。

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. 保存*Gruntfile.js*。 该文件应类似于下面的屏幕截图。

    ![初始 gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. 右键单击*Gruntfile.js*和选择**任务运行程序资源管理器**从上下文菜单。 任务运行程序资源管理器窗口将打开。

    ![任务运行程序资源管理器菜单](using-grunt/_static/task-runner-explorer-menu.png)

6. 验证`clean`下显示**任务**在任务运行程序资源管理器。

    ![任务运行程序资源管理器任务列表](using-grunt/_static/task-runner-explorer-tasks.png)

7. 右键单击 clean 任务，然后选择**运行**从上下文菜单。 命令窗口将显示任务的进度。

    ![任务运行程序资源管理器运行 clean 任务](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > 没有任何文件或目录尚未清理。 如果您愿意，你可以在解决方案资源管理器中手动创建它们，然后运行为测试的 clean 任务。
    
8. 在 initConfig() 方法中，添加一个条目`concat`使用下面的代码。

    `src`属性数组列出文件合并，它们应合并的顺序。 `dest`属性将路径分配给组合生成的文件。

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > `all`在上面的代码的属性为目标的名称。 某些 Grunt 任务中使用的目标用于允许多个生成环境。 你可以查看内置的目标使用 Intellisense，或指定你自己。
    
9. 添加`jshint`任务使用下面的代码。

    Jshint 代码质量实用工具运行的临时目录中找到的每个 JavaScript 文件。
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > 选项"-W069"一个错误时产生的 jshint JavaScript 使用方括号语法，即分配而不是点表示法，属性`Tastes["Sweet"]`而不是`Tastes.Sweet`。 选项关闭了警告，以允许进程继续的其余部分。

10.  添加`uglify`任务使用下面的代码。

    任务 minifies *combined.js*文件的临时目录中找到并在 wwwroot/lib 以下标准命名约定中创建结果文件*\<文件名\>。 min.js*.
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. 在加载 grunt contrib 清理调用 grunt.loadNpmTasks()，包括相同的调用，用于 jshint，concat 并 uglify 使用下面的代码使用。
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. 保存*Gruntfile.js*。 该文件应类似于下面的示例。

    ![完成 grunt 文件示例](using-grunt/_static/gruntfile-js-complete.png)

13. 请注意，该任务运行程序资源管理器任务列表包括`clean`， `concat`，`jshint`和`uglify`任务。 按顺序运行每个任务，并观察解决方案资源管理器中的结果。 每个任务应运行且未发生错误。
    
    ![任务运行程序资源管理器运行每个任务](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    Concat 任务创建一个新*combined.js*文件并将其放到临时目录。 Jshint 任务只需运行，但不生成输出。 Uglify 任务创建一个新*combined.min.js*文件并将其放到 wwwroot/lib。 完成时，解决方案应类似下面的屏幕截图：
    
    ![解决方案资源管理器在所有任务](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > 每个包的选项的详细信息，请访问[https://www.npmjs.com/](https://www.npmjs.com/)和查找在主页上的搜索框中的包名称。 例如，你可以查找 grunt contrib 清理包以获取说明它的所有参数的文档链接。

### <a name="all-together-now"></a>总的来说

使用 Grunt`registerTask()`方法以按特定顺序运行一系列任务。 例如，若要运行示例上述顺序干净的步骤-> concat-> jshint-> uglify，将下面的代码添加到该模块。 应将代码添加到与外部 initConfig loadNpmTasks() 调用相同的级别。

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

新任务将显示在任务运行程序资源管理器的别名任务中。 您可以右键单击，并运行它，就像其他任务。 `all`任务将运行`clean`， `concat`，`jshint`和`uglify`，按顺序。

![别名 grunt 任务](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>监视的更改

A`watch`任务保留文件和目录。 如果它检测到更改，监视将自动触发任务。 将下面的代码添加到 initConfig，若要监视的更改\*TypeScript 目录中的.js 文件。 如果更改 JavaScript 文件，`watch`将运行`all`任务。

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

添加对的调用`loadNpmTasks()`以显示`watch`任务运行程序资源管理器中的任务。

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

右键单击任务运行程序资源管理器中的监视任务，并从上下文菜单中选择运行。 显示运行的监视任务的命令窗口将显示"等待..." 消息。 打开一个 TypeScript 文件，添加一个空格，然后保存该文件。 这将触发监视任务并触发其他任务以按顺序运行。 下面的屏幕截图显示了示例运行。

![运行任务输出](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>绑定到 Visual Studio 事件

除非你想要手动启动你的任务，每次在 Visual Studio 中工作时，你可以将绑定到的任务**之前生成**，**后生成**，**清理**，和**项目打开**事件。

接下来将绑定`watch`以便使其运行每次 Visual Studio 将打开。 在任务运行程序资源管理器，右键单击监视任务并选择**绑定 > 项目打开**从上下文菜单。

![将任务绑定到在项目打开](using-grunt/_static/bindings-project-open.png)

卸载并重新加载项目。 在项目加载后再次，监视任务将开始自动运行。

## <a name="summary"></a>摘要

Grunt 是可以用于自动执行大多数客户端生成任务的功能强大的任务运行程序。 Grunt 利用 NPM 来提供其包和工具与 Visual Studio 的集成的功能。 Visual Studio 的任务运行程序资源管理器检测到配置文件的更改，并提供方便的界面以运行任务，查看正在运行的任务，并将任务绑定到 Visual Studio 事件。

## <a name="additional-resources"></a>其他资源

   * [使用 Gulp](using-gulp.md)
