---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: "创建 MVC 3 具有 Razor 和非介入式 JavaScript 应用程序 |Microsoft 文档"
author: microsoft
description: "用户列表的示例 web 应用程序演示如何创建使用 Razor 视图引擎的 ASP.NET MVC 3 应用程序是多么简单。 示例应用程序 s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 29b45c07b5498542abbf22c4c3001b1cee41edc9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>创建 MVC 3 具有 Razor 和非介入式 JavaScript 应用程序
====================
by [Microsoft](https://github.com/microsoft)

> 用户列表的示例 web 应用程序演示如何创建使用 Razor 视图引擎的 ASP.NET MVC 3 应用程序是多么简单。 示例应用程序演示如何使用新的 Razor 视图引擎，使用 ASP.NET MVC 版本 3 和 Visual Studio 2010 创建虚构的用户列表网站包含功能，如创建、 显示、 编辑和删除用户。
> 
> 本教程介绍已执行的以生成用户列表示例 ASP.NET MVC 3 应用程序的步骤。 与 C# 和 VB 的源代码的 Visual Studio 项目是用本主题可以附带：[下载](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114)。 如果你有关于本教程的问题，请将其发布到[MVC 论坛](https://forums.asp.net/1146.aspx)。


## <a name="overview"></a>概述

你将构建的应用程序是一个简单的用户列表网站。 用户可以输入、 查看和更新用户信息。

![示例站点](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

你可以下载 VB 和 C# 已完成的项目[此处](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f)。

## <a name="creating-the-web-application"></a>创建 Web 应用程序

若要开始本教程，请打开 Visual Studio 2010 并创建新项目使用*ASP.NET MVC 3 Web 应用程序*模板。 将该应用程序&quot;Mvc3Razor&quot;。

[![新的 MVC 3 项目](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

在**新建 ASP.NET MVC 3 项目**对话框中，选择**Internet 应用程序**，选择 Razor 视图引擎，，然后单击**确定**。

![新建 ASP.NET MVC 3 项目对话框](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

在本教程中你将不使用 ASP.NET 成员资格提供程序，因此您可以删除与登录和成员资格相关联的所有文件。 在**解决方案资源管理器**，删除以下文件和目录：

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* （和此目录中的所有文件）

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

编辑 *\_Layout.cshtml*文件并将内部标记`<div>`元素名为`logindisplay`并显示消息 *&quot;*登录名已禁用&quot;. 下面的示例显示新的标记：

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>添加模型

在**解决方案资源管理器**，右键单击*模型*文件夹，选择**添加**，然后单击**类**。

![新用户 Mdl 类](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

将此类命名为 `UserModel`。 内容替换*UserModel*文件替换为以下代码：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel`类表示用户。 每个成员的类进行批注[所需](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)属性从[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间。 中的特性[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间提供对于 web 应用程序的自动客户端和服务器端验证。

打开`HomeController`类，并添加`using`指令，以便可以访问`UserModel`和`Users`类：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

紧后面`HomeController`声明，添加以下注释和对引用`Users`类：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users`类是你将使用在本教程中的简化、 内存中数据存储。 在实际应用中将使用数据库来存储用户信息。 前的几行`HomeController`文件显示在下面的示例：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

生成应用程序，从而使用户模型将在下一步基架向导中提供。

## <a name="creating-the-default-view"></a>创建的默认视图

下一步是添加操作方法和视图以显示用户。

删除现有*Views\Home\Index*文件。 你将创建一个新*索引*文件以显示用户。

在`HomeController`类中的内容替换`Index`方法替换为以下代码：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

右键单击内部`Index`方法，然后单击**添加视图**。

![添加视图](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

选择**创建强类型化视图**选项。 有关**查看数据类**，选择**Mvc3Razor.Models.UserModel**。 (如果看不到**Mvc3Razor.Models.UserModel**中**查看数据类**框中，你需要以生成项目。)请确保视图引擎设置为**Razor**。 设置**查看内容**到**列表**，然后单击**添加**。

![添加索引视图](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

新视图自动 scaffolds 传递给的用户数据`Index`视图。 检查新生成*Views\Home\Index*文件。 **新建**，**编辑**，**详细信息**，和**删除**链接不起作用，但是页面的其余部分正常工作。 运行页面。 你看到用户的列表。

![索引页](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

打开*Index.cshtml*文件并将`ActionLink`标记**编辑**，**详细信息**，和**删除**替换为以下代码:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

用户名称用作 ID 以查找中的所选的记录**编辑**，**详细信息**，和**删除**链接。

## <a name="creating-the-details-view"></a>创建详细信息视图

下一步是添加`Details`操作方法和视图中，即可显示用户详细信息。

![详细信息](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

添加以下`Details`向主控制器的方法：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

右键单击内部`Details`方法，然后选择**添加视图**。 验证**查看数据类**框包含 **Mvc3Razor.Models.UserModel***。* 设置**查看内容**到**详细信息**，然后单击**添加**。

![添加详细信息视图](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

运行应用程序并选择的详细信息链接。 自动的基架显示模型中的每个属性。

![详细信息](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>创建编辑视图

添加以下`Edit`向主控制器的方法。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

添加视图如下所示的上一个步骤，但设置**查看内容**到**编辑**。

![添加编辑视图](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

运行应用程序和编辑的其中一个用户的第一个和最后一个名称。 如果违反任何`DataAnnotation`已应用于的约束`UserModel`类，当您在提交窗体中，你将看到生成的服务器代码的验证错误。 例如，如果你更改名字&quot;Ann&quot;到&quot;A&quot;，当你提交该表单，窗体上显示以下错误：

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

在本教程中，你要将用户名称视为为主键。 因此，不能更改了用户名属性。 在*Edit.cshtml*文件中，紧后面`Html.BeginForm`语句中，设置要隐藏的字段的用户名称。 这将导致要在模型中传递的属性。 下面的代码段演示如何放置`Hidden`语句：

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

替换`TextBoxFor`和`ValidationMessageFor`具有的用户名称标记`DisplayFor`调用。 `DisplayFor`方法将属性显示为只读的元素。 下面的示例演示已完成的标记。 原始`TextBoxFor`和`ValidationMessageFor`调用注释掉使用 Razor 开始注释和结束注释字符 (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>启用客户端验证

若要启用 ASP.NET MVC 3 中的客户端验证，必须设置两个标志，并且必须包含三个 JavaScript 文件。

打开应用程序的*Web.config*文件。 验证`that ClientValidationEnabled`和`UnobtrusiveJavaScriptEnabled`设置为 true 在应用程序设置。 以下片段中根*Web.config*文件可用于显示正确的设置：

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

设置`UnobtrusiveJavaScriptEnabled`为 true，允许非介入式 Ajax 和非介入式客户端验证。 当你使用非介入式验证时，验证规则将转换为 HTML5 属性。 HTML5 属性名称可以包含小写字母、 数字和短划线。

设置`ClientValidationEnabled`为 true，则启用客户端验证。 通过应用程序中设置这些密钥*Web.config*文件中，启用客户端验证和整个应用程序的非介入式 JavaScript。 你还可以启用或禁用这些设置在单个视图中或在使用下面的代码的控制器方法：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

你还需要在呈现的视图中包含多个 JavaScript 文件。 在所有视图中包括了 JavaScript 的简单办法是将其添加到*views/shared\\_Layout.cshtml*文件。 替换`<head>`元素 *\_Layout.cshtml*文件替换为以下代码：

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

前两个 jQuery 脚本托管的 Microsoft Ajax 内容交付网络 (CDN)。 通过利用 Microsoft Ajax CDN，就可以显著提高你的应用程序的第一个命中性能。

运行应用程序，然后单击编辑链接。 在浏览器中查看该页面的源。 浏览器源显示在窗体的许多特性`data-val`（用于数据验证）。 当启用客户端验证和非介入式 JavaScript 时，包含与客户端验证规则的输入的字段`data-val="true"`属性触发非介入式客户端验证。 例如，`City`模型中的字段使用修饰[所需](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)属性，这会导致下面的示例所示的 HTML:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

对于每个客户端验证规则，具有窗体添加属性`data-val-rulename="message"`。 使用`City`所需的客户端验证规则将生成更早版本，显示的字段示例`data-val-required`属性和消息&quot;城市字段是必填&quot;。 运行应用程序、 编辑的其中一个用户，并清除`City`字段。 当不使用 tab 键字段外时，你将看到客户端验证错误消息。

![所需的城市](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

同样，对于客户端验证规则中每个参数，将某个属性添加具有窗体`data-val-rulename-paramname=paramvalue`。 例如，`FirstName`属性进行批注[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性并指定 3 的最小长度和最大长度为 8。 名为的数据验证规则`length`具有参数名称`max`和参数值 8。 下面的示例演示为生成的 HTML`FirstName`字段时编辑用户之一：

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

有关非介入式客户端验证的详细信息，请参阅文章[ASP.NET MVC 3 中的非介入式客户端验证](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)Brad wilson 制作的博客中。

> [!NOTE]
> 在 ASP.NET MVC 3 Beta，有时需要以启动客户端验证提交表单。 这可能会在最终发布版本进行更改。


## <a name="creating-the-create-view"></a>创建创建视图

下一步是添加`Create`操作方法和要使用户能够创建新用户的视图。 添加以下`Create`向主控制器的方法：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

添加视图如下所示的上一个步骤，但设置**查看内容**到**创建**。

![创建视图](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

运行应用程序中，选择**创建**链接，并添加新用户。 `Create`方法自动利用客户端和服务器端验证。 尝试输入的用户名称，包含空格，如&quot;Ben X&quot;。 当用户名称字段中，客户端验证错误外选项卡 (`White space is not allowed`) 显示。

## <a name="add-the-delete-method"></a>添加 Delete 方法

若要完成本教程，添加以下`Delete`向主控制器的方法：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

添加`Delete`视图如下所示的上一个步骤，设置**查看内容**到**删除**。

![删除视图](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

你现在具有简单但完全正常运行的 ASP.NET MVC 3 应用程序，与验证。
