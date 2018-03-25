---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: "ASP.NET 网页 (Razor) API 快速参考 |Microsoft 文档"
author: tfitzmac
description: "此页包含的最常用的对象、 属性和方法进行编程的 ASP.NET Web Pages 使用 Razor 语法的简短示例的列表。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 35f91f4dbea4881d9dabc4ab7c6b96dbb6a01ea2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a>ASP.NET 网页 (Razor) API 快速参考
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此页包含的最常用的对象、 属性和方法进行编程的 ASP.NET Web Pages 使用 Razor 语法的简短示例的列表。
> 
> 标记为"(v2)"的说明引入了在 ASP.NET Web Pages 版本 2。
> 
> API 参考文档，请参阅[ASP.NET 网页参考文档](https://go.microsoft.com/fwlink/?LinkId=208659)MSDN 上。
> 
> ## <a name="software-versions"></a>软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2 和 ASP.NET Web Pages 1.0 （除外标记 v2 的功能）。


此页包含有关以下参考信息：

- [类](#Classes)
- [Data](#Data)
- [帮助器](#Helpers)
- [验证](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>类

### `AppState[key], AppState[index],App`

包含可由任何页共享应用程序中的数据。 你可以使用动态`App`属性来访问相同的数据，如以下示例所示：

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

将一个字符串值转换为布尔值 (true/false)。 返回 false 或指定的值如果字符串不表示 true/false。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

将转换日期/时间的字符串值。 返回`DateTime.MinValue`或如果字符串不表示为日期/时间的指定的值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

将一个字符串值转换为十进制值。 返回 0.0 或如果字符串不表示一个十进制值的指定的值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

将一个字符串值转换为浮点数。 返回 0.0 或如果字符串不表示一个十进制值的指定的值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

将一个字符串值转换为整数。 返回 0 或如果字符串不表示一个整数，指定的值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

从本地文件路径，具有可选的附加路径部分创建一个与浏览器兼容的 URL。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

呈现*值*作为 HTML 标记，而不是呈现为 HTML 编码输出。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

如果可以将值从字符串转换为指定的类型，则返回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

如果该对象或变量不具有任何值，则返回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

如果请求是 POST，则返回 true。 （初始请求通常是 GET。）

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

指定要应用于此页所需的布局页的路径。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

包含在当前请求中的页、 布局页和部分页面之间共享的数据。 你可以使用动态`Page`属性来访问相同的数据，如以下示例所示：

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

（布局页）呈现不在任何命名的部分内容页面的内容。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

呈现内容的页面，使用指定的路径和可选的额外数据。 你可以获取来自的额外参数的值`PageData`按照位置 （例如 1） 或键 （示例 2）。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

（布局页）呈现内容的部分具有名称。 设置*必需*为 false 使一个节可选。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

获取或设置 HTTP cookie 的值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

获取当前请求过程中上载的文件。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

获取窗体中 （作为字符串） 已发布的数据。 `Request[key]`检查同时`Request.Form`和`Request.QueryString`集合。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

获取指定的 URL 查询字符串中的数据。 `Request[key]`检查同时`Request.Form`和`Request.QueryString`集合。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

有选择性地禁用请求验证窗体元素、 查询字符串值、 cookie，或标头值。 请求验证默认处于启用状态，并且可以防止用户发布标记或其他潜在危险的内容。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

将 HTTP 服务器标头添加到响应中。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

将页面输出缓存指定的时间。 可以选择设置*滑动*重置在每次页访问的超时和*varyByParams*缓存的页面请求中每个不同的查询字符串的页的不同版本。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

将浏览器请求重定向到新位置。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

设置发送到浏览器的 HTTP 状态代码。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

内容写入*数据*到具有可选的 MIME 类型的响应。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

将响应写入的文件的内容。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

（布局页）定义一个内容段中，有一个名称。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

对 HTML 编码的字符串进行解码。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

将编码的字符串中的 HTML 标记的呈现。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

返回指定的虚拟路径的服务器物理路径。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

将文本从 URL 解码。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

将文本将在 URL 中的编码。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

获取或设置一个值，用户关闭浏览器一直存在。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

显示的字符串表示形式的对象的值。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

获取其他数据从 URL (例如， */MyPage/ExtraData*)。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

更改指定的用户的密码。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

确认使用帐户确认令牌的帐户。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

使用指定的用户名和密码创建新的用户帐户。 如果需要确认令牌，请将传递适用*requireConfirmationToken。*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

获取当前登录的用户的整数标识符。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

获取当前登录的用户的名称。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

生成可通过发送电子邮件给用户，以便用户可以重置密码的密码重置标记。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

用户名中返回的用户 ID。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

如果当前用户登录，则返回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

如果用户已确认 （例如，通过确认电子邮件），则返回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

如果当前用户的名称与指定的用户名称匹配，则返回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

通过在 cookie 中设置身份验证令牌记录中的用户。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

通过删除身份验证令牌 cookie 缩小记录用户。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

如果用户未通过身份验证，则设置的 HTTP 状态为 401 （未授权）。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

如果当前用户不是某个指定的角色的成员，设置 HTTP 状态为 401 （未授权）。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

如果当前用户不是按指定的用户*用户名*，将 HTTP 状态设置为 401 （未授权）。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

如果密码重置令牌有效，请为新密码更改用户的密码。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>数据

### `Database.Execute(SQLstatement [,parameters]`

执行*SQLstatement* （带可选参数） 如 INSERT、 DELETE 或 UPDATE 并返回受影响的记录计数。

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

从最近插入的行返回的标识列。

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

打开指定的数据库文件或使用已命名的连接字符串中指定的数据库*Web.config*文件。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

打开使用连接字符串的数据库。 (这点与`Database.Open`，它使用的连接字符串名称。)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

查询数据库使用*SQLstatement* （根据需要传递参数） 并以集合形式返回结果。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

执行*SQLstatement* （带可选参数），并返回单个记录。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

执行*SQLstatement* （带可选参数），并返回单个值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>帮助器

### `Analytics.GetGoogleHtml(webPropertyId)`

为指定的 id。 呈现 Google 分析 JavaScript 代码

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

呈现指定项目的 StatCounter 分析 JavaScript 代码。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

呈现为指定的帐户的 Yahoo 分析 JavaScript 代码。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

将搜索传递给必应。 若要指定站点到搜索并搜索框的标题，可以设置`Bing.SiteUrl`和`Bing.SiteTitle`属性。 在中设置这些属性通常 *\_AppStart*页。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

初始化一个图表。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

向图表添加图例。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

向图表中添加一系列值。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

返回指定的数据的哈希值。 默认算法是`sha256`。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

允许连接到页的 Facebook 用户。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

将文件上载呈现用户界面。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

呈现指定的 Xbox 游戏标记。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

呈现为指定的电子邮件地址 Gravatar 图像。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

将数据对象转换为 JavaScript 对象表示法 (JSON) 格式的字符串。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

将 JSON 编码的输入的字符串转换为可以循环访问或向数据库中插入的数据对象。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

呈现使用指定的标题和可选的 URL 的社交网络链接。

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

将一条错误消息与窗体字段相关联。 使用`ModelState`帮助器访问该成员。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

将一条错误消息表单与相关联。 使用`ModelState`帮助器访问该成员。

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

如果没有任何验证错误，则返回 true。 使用`ModelState`帮助器访问该成员。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

呈现的属性和值的对象和任何子对象。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

呈现 reCAPTCHA 验证测试。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

设置 reCAPTCHA 服务的公共和私有密钥。 在中设置这些属性通常 *\_AppStart*页。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

返回 reCAPTCHA 测试的结果。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

呈现有关 ASP.NET Web 页的状态信息。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

呈现为指定的用户 Twitter 流。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

呈现一个 Twitter 流以便指定的搜索文本。

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

呈现闪存视频播放器与可选的宽度和高度指定的文件。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

呈现与可选的宽度和高度指定的文件的 Windows 媒体播放器。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

呈现为指定的 Silverlight 播放器*.xap*文件所需的宽度和高度。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

返回指定的对象*密钥*，或如果找不到对象为 null。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

删除指定的对象*密钥*从缓存。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

放入*值*到下指定的名称缓存*密钥*。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

创建一个新`WebGrid`对象使用查询中的数据。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

呈现标记以 HTML 表中显示数据。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

呈现页导航`WebGrid`对象。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

从指定的路径加载图像。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

将指定的映像添加为水印。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

将指定的文本添加到映像。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

水平或垂直翻转图像。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

加载图像，图像文件上载过程中发送到页面时。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

调整图像。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

将向左或向右图像的旋转。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

将图像保存到指定的路径。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

为 SMTP 服务器设置的密码。 在设置此属性通常 *\_AppStart*页。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

发送电子邮件。

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

设置 SMTP 服务器名称。 在设置此属性通常*\_AppStart*页。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

设置 SMTP 服务器的用户名称。 通常应在设置此属性 *\_AppStart*页。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>验证

### `Html.ValidationMessage(field)`

(v2)呈现指定字段的验证错误消息。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2)显示所有验证错误的列表。

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2)注册的指定类型的验证的用户输入的元素。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2)动态呈现客户端验证的 CSS 类特性，以便你可以设置验证错误消息的格式。 （需要引用相应的客户端脚本库和定义 CSS 类。）

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2)启用客户端验证用户输入字段。 （需要引用相应的客户端脚本库。）

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2)如果是用于验证注册的所有用户输入的元素都包含有效的值，则返回 true。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2)指定用户必须提供用户输入元素的值。

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2)指定用户必须为每个用户输入元素提供值。 此方法不允许您指定的自定义错误消息。

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2)当你使用指定的验证测试`Validation.Add`方法。

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
