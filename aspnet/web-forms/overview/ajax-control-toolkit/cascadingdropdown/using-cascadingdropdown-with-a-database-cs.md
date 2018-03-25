---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: "使用数据库 (C#) CascadingDropDown |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 CascadingDropDown 控件扩展的 DropDownList 控件，使得一个 DropDownList 负载中的更改关联中 anoth 值..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 86d6b62259573433cff7054d50cc299da9e4f372
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-cascadingdropdown-with-a-database-c"></a>CascadingDropDown 使用数据库 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> AJAX 控件工具包中的 CascadingDropDown 控件扩展的 DropDownList 控件，使得一个 DropDownList 负载中的更改关联中另一个 DropDownList 值。 为了使这种方式生效，必须创建特殊的 web 服务。


## <a name="overview"></a>概述

AJAX 控件工具包中的 CascadingDropDown 控件扩展的 DropDownList 控件，使得一个 DropDownList 负载中的更改关联中另一个 DropDownList 值。 （例如，一个列表提供了一份我们状态，并且用处于该状态的主要城市然后填充下一个列表。）为了使这种方式生效，必须创建特殊的 web 服务。

## <a name="steps"></a>步骤

首先，数据源是必需的。 此示例使用 AdventureWorks 数据库和 Microsoft SQL Server 2005 Express Edition。 数据库是 （包括速成版） 的 Visual Studio 安装的可选部分，还可用作下单独下载[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 数据库是 SQL Server 2005 示例和示例数据库的一部分 (在下载[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 设置数据库的最简单方法是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`数据库文件。

对于此示例中，我们假定的 SQL Server 2005 Express Edition 实例称为`SQLEXPRESS`和驻留在与 web 服务器; 相同的计算机上也是默认设置。 如果你的设置不同，你必须调整数据库的连接信息。

为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`必须在页面上任意位置放置控件 (但内&lt; `form` &gt;元素):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

在下一步的步骤中，两个 DropDownList 控件是必需的。 在此示例中，我们使用从 AdventureWorks 的供应商和联系人信息，因此我们创建一个列表用于可用供应商，一个用于可用联系人：

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

然后，两个 CascadingDropDown 扩展程序必须添加到页面。 一个填充第一个 （供应商） 列表中，并且另一填充第二个 （联系人） 列表。 必须设置以下属性：

- `ServicePath`: 提供的列表项 web 服务的 URL
- `ServiceMethod`: Web 方法提供的列表项
- `TargetControlID`: ID 的下拉列表
- `Category`： 提交给 web 方法调用时的类别信息
- `PromptText`： 当以异步方式从服务器加载列表数据时显示的文本
- `ParentControlID`: (可选) 父下拉列表中的当前列表，触发器加载

具体取决于所用的编程语言，web 服务的名称会更改，但所有其他属性值是相同的。 此处是第一个下拉列表中的 CascadingDropDown 元素：

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

第二个列表控件的扩展器需要设置`ParentControlID`属性，以便加载供应商列表触发器中选择某一项关联的联系人列表中的元素。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

在 web 服务中，按以下方式设置，然后完成实际工作。 请注意，`[ScriptService]`使用属性，否则 ASP.NET AJAX 无法创建要从客户端脚本代码访问的 web 方法的 JavaScript 代理。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

由 CascadingDropDown 调用 web 方法的签名如下所示：

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

因此返回的值必须为类型的数组`CascadingDropDownNameValue`其定义了控件工具包。 `GetVendors()`方法可以很容易地实现： 代码连接到 AdventureWorks 数据库，并查询前 25 个供应商。 中的第一个参数`CascadingDropDownNameValue`构造函数的列表项第二个的默认标题是其值 (以 HTML 的 value 属性&lt; `option` &gt;元素)。 以下是代码：

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

获取供应商相关联的联系人 (方法名称： `GetContactsForVendor()`) 棘手。 首先，必须确定这已在第一个下拉列表中选择的供应商。 该控件工具包定义为该任务的帮助器方法：`ParseKnownCategoryValuesString()`方法返回`StringDictionary`与下拉列表中的数据的元素：

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

出于安全原因，必须首先验证此数据。 因此，如果供应商条目 (因为`Category`的第一个 CascadingDropDown 元素的属性设置为`"Vendor"`)，可能会检索所选供应商 ID:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

该方法的其余部分是相当简单，然后所示。 供应商的 ID 用作可检索为该供应商的所有相关联的联系人的 SQL 查询参数。 同样，该方法返回类型的数组`CascadingDropDownNameValue`。

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

加载 ASP.NET 页中，并在很短的时间后供应商列表填充包含 25 个条目。 选择一个条目，并注意如何填充数据的第二个下拉列表中。


[![第一个列表中自动填充](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

第一个列表中自动填充 ([单击以查看实际尺寸的图像](using-cascadingdropdown-with-a-database-cs/_static/image3.png))


[![第二个列表填充根据第一个列表中选择](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

第二个列表填充根据第一个列表中选择 ([单击以查看实际尺寸的图像](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

>[!div class="step-by-step"]
[上一页](filling-a-list-using-cascadingdropdown-cs.md)
[下一页](presetting-list-entries-with-cascadingdropdown-cs.md)
