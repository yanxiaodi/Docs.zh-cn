---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: "使用模型的绑定和 web 窗体中集成 JQuery UI Datepicker |Microsoft 文档"
author: tfitzmac
description: "本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互详细直接-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: da3c8f347a709a4c9a47fd0ecce5201d9b0cd1b1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>使用模型的绑定和 web 窗体中集成 JQuery UI Datepicker
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教程演示使用模型绑定的 ASP.NET Web 窗体项目的基本方面。 模型绑定使数据交互更直接的比处理数据源对象 （如 ObjectDataSource 或 SqlDataSource）。 这一系列开头介绍性材料，更高版本的教程中移动到更高级的概念。
> 
> 本教程演示如何将添加 JQuery UI [Datepicker 小组件](http://jqueryui.com/datepicker/)到 Web 窗体，并使用模型绑定，使其使用所选值更新数据库。
> 
> 本教程中创建的项目上[第一个](retrieving-data.md)和[第二个](updating-deleting-and-creating-data.md)的序列部分。
> 
> 你可以[下载](https://go.microsoft.com/fwlink/?LinkId=286116)在 C# 或 VB.完整项目 可下载的代码适用于 Visual Studio 2012 或 Visual Studio 2013。 它使用 Visual Studio 2012 模板，这是本教程中所示的 Visual Studio 2013 模板过程稍有不同。


## <a name="what-youll-build"></a>你将生成

在本教程中，你将：

1. 将属性添加到你的模型来记录学生的注册日期
2. 使用户可选择使用 JQuery UI Datepicker 小组件的注册日期
3. 注册日期实施验证规则

JQuery UI Datepicker 小组件使用户能够轻松地从在用户交互的字段时弹出日历中选择一个日期。 使用此小组件可能会更便于用户比手动键入日期。 将包含 Datepicker 小组件集成到的数据操作中使用模型绑定的页面要求只有少量的额外的工作。

## <a name="add-a-new-property-to-the-model"></a>将新属性添加到模型

首先，你将添加**Datetime**属性设置为你学生模型和迁移到数据库所做的更改。 打开**UniversityModels.cs**，并将突出显示的代码添加到学生模型。

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute**目的是为了实施的属性的验证规则。 对于本教程中，我们将假定 Contoso 大学成立于 2013 年 1 月 1 日，并因此早期注册日期不是有效。

在包管理窗口中，通过运行命令添加迁移**添加迁移 AddEnrollmentDate**。 请注意，迁移代码会将新的日期时间列添加到学生表。 以匹配你在 RangeAttribute 中指定的值，添加以下突出显示的代码中所示的新列的默认值。

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

将所做的更改保存到的迁移文件。

不需要再次种子数据。 因此，打开**Configuration.cs** Migrations 文件夹中，删除或注释掉中的代码**种子**方法。 保存并关闭文件。

现在，运行命令**更新数据库**。 请注意，此列现在存在于数据库并且所有现有记录 EnrollmentDate 对于具有默认值。

## <a name="add-dynamic-controls-for-enrollment-date"></a>注册日期添加动态控件

现在，你将添加用于显示和编辑注册日期的控件。 此时，通过在文本框中编辑该值。 更高版本在教程中，将更改文本框中，为 JQuery Datepicker 小组件。

首先，务必请注意，不需要进行任何更改到**AddStudent.aspx**文件。 DynamicEntity 控件会自动呈现新属性。

打开**Students.aspx**，并添加以下突出显示的代码。

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

运行应用程序，并请注意，你可以通过键入日期设置注册日期的值。 当添加新的学生：

![设置的日期](integrating-jquery-ui/_static/image1.png)

或者，编辑现有值：

![编辑日期](integrating-jquery-ui/_static/image2.png)

键入日期工作原理，但它可能不是你想要提供的客户体验。 在下一步的部分中，将启用选择通过日历日期。

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>安装 NuGet 包才能使用 JQuery UI

**汁 UI** NuGet 包更方便地将 JQuery UI 小组件集成到 web 应用程序。 若要使用此包，请通过 NuGet 安装它。

![添加汁 UI](integrating-jquery-ui/_static/image3.png)

你安装的汁 UI 的版本可能与你的应用程序中的 JQuery 版本冲突。 然后再继续进行本教程，请尝试运行应用程序。 如果你遇到 JavaScript 错误，则需要进行对帐 JQuery 版本。 你可以将 JQuery 的预期的版本添加到脚本文件夹 （版本 1.8.2 在编写本教程的时），或在 Site.master 指定 JQuery 文件的路径。

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>自定义 DateTime 模板以包括包含 Datepicker 小组件

将将包含 Datepicker 小组件添加到动态数据模板进行编辑的日期时间值。 通过将小组件添加到模板，它会自动呈现在添加新的学生的这两个窗体并在网格视图中编辑学生版。 打开**DateTime\_Edit.ascx**，并添加以下突出显示的代码。

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

在代码隐藏文件中，你将会包含 DatePicker 设置的最小和最大日期。 通过设置这些值，将阻止用户导航到无效的日期。 将检索的最小和最大值**RangeAttribute** DateTime 属性，如果其中一个提供。 打开**DateTime\_Edit.ascx.cs**，并将以下突出显示的代码添加到页\_加载方法。

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

运行 web 应用程序并导航到 AddStudent 页。 为字段提供值，请注意当你在文本框中单击针对注册日期时，会显示日历。

![日期选取器](integrating-jquery-ui/_static/image4.png)

选择日期，并单击**插入**。 RangeAttribute 强制进行服务器上的验证。 通过设置 Datepicker minDate 属性，您在客户端还应用验证。 日历不允许用户导航到之前的 minDate 值的日期。

当您编辑网格视图中的记录时，还会显示日历。

![在 GridView 的 Datepicker](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>结束语

在本教程中，您学习了如何将 JQuery 小组件合并到使用模型绑定的 web 窗体。

在下一个[教程](using-query-string-values-to-retrieve-data.md)，选择数据时，将使用的查询字符串值。

>[!div class="step-by-step"]
[上一页](sorting-paging-and-filtering-data.md)
[下一页](using-query-string-values-to-retrieve-data.md)
