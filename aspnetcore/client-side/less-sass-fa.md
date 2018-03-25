---
title: "小于，Sass 和 ASP.NET Core 中出色的字体"
author: ardalis
description: "了解如何在 ASP.NET Core 应用程序中使用较少，Sass，和字体出色。"
manager: wpickett
ms.author: tdykstra
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/less-sass-fa
ms.openlocfilehash: 979f5639e382560d952df45ba6e0b8af3b132c2d
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a>简介事半功倍的样式应用程序、 Sass，和在 ASP.NET 核心中出色的字体

作者：[Steve Smith](https://ardalis.com/)

Web 应用程序的用户有越来越高的期望时设置的样式和总体体验。 现代 web 应用程序频繁地利用丰富的工具和框架用于定义和管理其外观和感觉以一致的方式。 框架喜欢[Bootstrap](http://getbootstrap.com/)可以地长定义一组通用的样式和网站的布局选项。 但是，大多数重要站点还受益于能够有效地定义和维护样式和级联样式表 (CSS) 文件，以及能够轻松访问帮助使站点的接口更直观的非图像图标。 就是在此处的支持语言和工具[较少](http://lesscss.org/)和[Sass](http://sass-lang.com/)，库等，以及将[字体出色](http://fontawesome.io/)，进入。

## <a name="css-preprocessor-languages"></a>CSS 预处理器语言

为了改进的体验的使用基础的语言中，编译成其它语言的语言被称为预处理器。 有两个常用的预处理器针对 CSS： 不太和 Sass。  这些预处理器将功能添加到 CSS，如对变量和嵌套的规则，以改进可维护性的大型、 复杂的样式表的支持。 作为一种语言的 CSS 是非常基本，缺少那样简单变量，即使对于支持，这往往会重复和臃肿使 CSS 文件。 添加通过预处理器的实际编程语言功能可以帮助减少重复，提供更好地组织样式规则。 Visual Studio 提供这两个较低的内置支持和 Sass，以及可以进一步提高开发体验，使用这些语言时的扩展。

作为预处理器可以如何改进可读性和可维护性的样式信息的快速示例，请考虑以下 CSS:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

使用更少，这可以重写以消除所有重复，使用*mixin* (这样命名是因为它允许你"中混合使用"从一个类或到另一个规则集的属性):

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>小于

使用 Node.js 不太 CSS 预处理器运行。 若要安装小于，使用 Node 程序包管理器 (npm) 从命令提示符 (-g 意味着"全局"):

```console
npm install -g less
```

如果你使用 Visual Studio，你可以开始使用小于通过将一个或多个更少文件添加到你的项目，然后配置 Gulp （或 Grunt） 在编译时处理它们。 添加*样式*到你的项目的文件夹，然后添加新的名为的文件小于*main.less*到此文件夹。

![添加较少的文件](less-sass-fa/_static/add-less-file.png)

添加后，您的文件夹结构应如下所示：

![文件夹结构](less-sass-fa/_static/folder-structure.png)

现在你可以添加到文件中，这将编译到 CSS 并部署到的 wwwroot 文件夹 Gulp 的一些基本的样式。

修改*main.less*为包括以下内容，这将从一种基颜色创建一个简单的调色板。

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base` 和其他@-prefixed项是变量。 每个表示一种颜色。 除`@base`，设置它们并使用颜色函数： 加亮、 变暗，和旋转。 淡化和加深执行几乎你将预期;数值调节钮调整颜色色调的大量度 （围绕颜色盘中）。 较少处理器是足够智能，可忽略不使用的变量，因此若要展示了这些变量的工作原理，我们需要某个位置使用它们。 类`.baseColor`，等将演示每个生成的 CSS 文件中的变量的计算的值。

### <a name="get-started"></a>入门

创建**npm 配置文件**(*package.json*) 在你的项目文件夹并编辑它以引用`gulp`和`gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

在你的项目文件夹，或在 Visual Studio 中安装的依赖关系，请在命令提示符下**解决方案资源管理器**(**依赖项 > npm > 还原程序包**)。

```console
npm install
```

![VS 还原包](less-sass-fa/_static/restore-packages.png)

在项目文件夹中，创建**Gulp 配置文件**(*gulpfile.js*) 以定义自动的过程。  在更少，表示的文件及要运行更少的任务的顶部添加一个变量：

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

打开**任务运行程序资源管理器**(**视图 > 其他 Windows > 任务运行程序资源管理器**)。 在任务之间，你应看到一个名为的新任务`less`。 你可能必须刷新窗口。

运行`less`任务，并看到类似于此处所示的输出：

![较少的任务运行程序](less-sass-fa/_static/less-task-runner.png)

*Wwwroot/css*文件夹现在包含一个新的文件， *main.css*:

![创建主 css](less-sass-fa/_static/main-css-created.png)

打开*main.css* ，你将看到与下面类似：

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

添加到一个简单的 HTML 页面*wwwroot*文件夹，然后引用*main.css*若要查看操作中的调色板。

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

你可以看到上旋转 180 度`@base`用于生成`@background`导致相反颜色的颜色盘`@base`:

![不太测试示例](less-sass-fa/_static/less-test-screenshot.png)

小于还提供对嵌套的规则以及嵌套的媒体查询的支持。 例如，像菜单可能会导致详细 CSS 规则的定义嵌套层次结构像这样:

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

理想情况下的所有相关的样式规则将被放在一起在 CSS 文件中，但在实践中没有任何强制实施此规则约定和可能块注释除外。

定义使用小于这些相同的规则类似如下所示：

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

请注意，在此情况下，所有的从属元素`nav`包含在其作用域内。 不再父元素的任何重复 (`nav`， `li`， `a`)，并且总的行计数也已删除 （尽管某些就是将值放在第二个示例的同一行上的结果）。 它可以是非常有帮助，组织，若要查看的所有规则的给定的用户界面元素在显式限定范围内，在这种情况下设置从文件的其余部分由大括号。

`&`语法是较少的选择器功能，与 （&) 表示当前的选择器父级。 这样，在 {...} 块中，`&`表示`a`标记，因此`&:link`等效于`a:link`。

创建响应式设计非常有用的媒体查询还可能影响很大程度重复和 CSS 中的复杂性。 小于允许媒体查询嵌套在类，以便整个类定义不需要重复内不同顶级`@media`元素。 例如，下面是响应式菜单的 CSS:

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

这可以更好地定义以秒为：

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

小于我们已经看到的另一个功能是其支持的数学运算，允许从预定义的变量构造的样式特性。 这使得更新相关的样式容易得多，因为可以修改基变量和所有依赖值自动更改。

CSS 文件，尤其是对于大型站点 （和尤其是如果正在使用媒体查询），就会变得很大随着时间推移，从而使用它们难以操作。 Less 文件可以单独定义，然后一起使用，取出`@import`指令。 小于还可导入单个 CSS 文件，同样，如果需要。

*Mixins*可以接受参数，并且小于 mixin 防护，提供声明性方式定义某些 mixins 生效的形式支持条件逻辑。 Mixin 临界条件的一个常见用途是调整颜色如何细基于或深色的源颜色是。 Mixin 防护给定 mixin 接受颜色的参数，用于修改基于该颜色 mixin:

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

给定我们当前`@base`值`#663333`，小于此脚本将生成以下 CSS:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

小于提供许多其他功能，但这应为你提供此强大的一些思路预处理语言。

## <a name="sass"></a>Sass

Sass 类似于较少，是为许多相同功能，但使用略有不同的语法提供支持。 它使用 Ruby，而不是 JavaScript 中，生成，因此不同的安装要求。 原始 Sass 语言未使用大括号或分号，但改为定义使用空白和缩进的作用域。 在 Sass 3 版本中，引入了新的语法， **SCSS** ("Sassy CSS")。 SCSS 是类似于 CSS，可忽略缩进级别和空格，并改为使用分号和大括号。

若要安装 Sass，通常你将首先安装 Ruby （预安装在 macOS 上），，然后运行：

```console
gem install sass
```

但是，如果要运行 Visual Studio，你可以开始使用 Sass 在很大程度的相同方式就像对待小于。 打开*package.json*并添加到"gulp sass"包`devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

接下来，修改*gulpfile.js*添加 sass 变量和编译 Sass 文件并将结果放的 wwwroot 文件夹中的任务：

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

现在，您可以将 Sass 文件*main2.scss*到*样式*项目的根目录中的文件夹：

![添加 scss 文件](less-sass-fa/_static/add-scss-file.png)

打开*main2.scss* ，添加以下内容：

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

保存你所有文件。 现在刷新**任务运行程序资源管理器**，你看到`sass`任务。 运行它，并查找*/wwwroot/css*文件夹。 现在有了*main2.css*文件，使用以下内容：

```css
body {
    background-color: #CC0000;
}
```

Sass 支持嵌套大体相同时，小于存在，提供类似的好处。 文件可以由函数拆分和包含使用`@import`指令：

```sass
@import 'anotherfile';
```

Sass 支持 mixins 同样，使用`@mixin`关键字定义它们和`@include`以便将它们，包含从如此示例所示[sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Sass 除了 mixins，它还支持继承，的概念允许一个类，以扩展另一个。 它是从概念上讲类似于 mixin，但在更少的 CSS 代码中的结果。 它通过`@extend`关键字。 若要尝试 mixins，将以下代码添加到你*main2.scss*文件：

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

检查在输出*main2.css*运行之后`sass`任务中**任务运行程序资源管理器**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

请注意所有警报 mixin 的常见属性在每个类中有重复。 Mixin 好地帮助消除重复在开发时，但它仍包含中的重复，从而导致大于必要 CSS 文件-一个潜在的性能问题很多要创建 CSS。

现在将与警报 mixin`.alert`类，并更改`@include`到`@extend`(记住来扩展`.alert`，而不`alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

运行 Sass 次，并检查生成的 CSS:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

现在仅为根据需要多次定义属性，并更好地生成 CSS。

Sass 还包括函数和条件逻辑操作，类似于小于。 事实上，这两种语言的功能是非常相似。

## <a name="less-or-sass"></a>较低或 Sass？

仍没有关于是否通常更好的做法将占用更少或 Sass 没有共识 （或甚至是否首选原始 Sass 中的较新 SCSS 语法）。 可能的最重要的决定是**使用这些工具之一**，而不是只使用手工编码 CSS 文件。 一旦你所做的决策，这两个不太和 Sass 是不错的选择。

## <a name="font-awesome"></a>出色的字体

除了 CSS 预处理器样式现代 web 应用程序的另一个很好的办法是字体出色。 字体 Awesome 是一个工具包，提供 500 多个可以自由地使用 web 应用程序中的可缩放的向量图标。 它最初设计成可使用 Bootstrap，但它并不依赖于该框架或在任何 JavaScript 库。

若要开始使用字体出色的最简单方法是添加对它，使用其公用内容交付网络 (CDN) 位置的引用：

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

你可以还将其添加到你的 Visual Studio 项目通过将它添加到"依赖关系"中*bower.json*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

指向字体出色页上的引用后，你可以将图标添加到你的应用程序通过应用字体出色的类，通常前缀为"fa-"，为嵌入式 HTML 元素 (如`<span>`或`<i>`)。  例如，你可以将图标添加到简单列表和菜单使用如下代码：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

这将生成浏览器中的以下-请注意每个项旁边的图标：

![列表图标](less-sass-fa/_static/list-icons-screenshot.png)

你可以查看可用的图标的完整列表：

http://fontawesome.io/icons/

## <a name="summary"></a>总结

现代 web 应用程序越来越要求清晰、 直观，编写和更易于使用的各种设备从的响应速度快、 流体设计。 管理实现这些目标所需的 CSS 样式表的复杂性最好的做法是不太使用预处理器类似或 Sass。 此外，如字体出色的工具包快速提供的已知图标添加到文本导航菜单和按钮，提高整体用户体验的应用程序。
