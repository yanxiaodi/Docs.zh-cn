---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: "拖放通过 ReorderList (VB) |Microsoft 文档"
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: e193a31fc86b7e8733d0b2fba371d99c62783d6c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="drag-and-drop-via-reorderlist-vb"></a>拖放通过 ReorderList (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> AJAX 控件工具包中的 ReorderList 控件提供可由用户通过拖放重新排序的列表。 应在服务器保留对当前的列表顺序。


## <a name="overview"></a>概述

`ReorderList` AJAX 控件工具包中的控件提供可由用户通过拖放重新排序的列表。 应在服务器保留对当前的列表顺序。

## <a name="steps"></a>步骤

`ReorderList`控件支持将数据从数据库绑定到列表。 最重要的是，它还支持将更改写入回数据存储区的列表元素的顺序。

此示例使用作为数据存储区的 Microsoft SQL Server 2005 Express Edition。 数据库是包括速成版的 Visual Studio 安装的可选 （和可用） 组成部分。 此外，还可以作为单独的下载下[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)。 对于此示例中，我们假定的 SQL Server 2005 Express Edition 实例称为`SQLEXPRESS`和驻留在与 web 服务器; 相同的计算机上也是默认设置。 如果你的设置不同，你必须调整数据库的连接信息。

将数据库设置的最简单方法是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。 连接到服务器，请双击`Databases`并创建一个新的数据库 (右键单击并选择`New Database`) 调用`Tutorials`。

在此数据库中，创建名为的新表`AJAX`与以下四列：

- `id`(主键，整数，标识，不为 NULL)
- `char`（char （1)，NULL）
- `description`(varchar(50)，NULL)
- `position`(int、 NULL)


[![AJAX 表的布局](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

AJAX 表的布局 ([单击以查看实际尺寸的图像](drag-and-drop-via-reorderlist-vb/_static/image3.png))


接下来，使用几个值填充表。 请注意，`position`的列包含的元素的排序顺序。


[![AJAX 表中的初始数据](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

AJAX 表中的初始数据 ([单击以查看实际尺寸的图像](drag-and-drop-via-reorderlist-vb/_static/image6.png))


下一步需要生成`SqlDataSource`控件与新的数据库和它的表进行通信。 数据源必须支持`SELECT`和`UPDATE`SQL 命令。 当列表元素的顺序更高版本发生更改时，`ReorderList`控件会自动将提交到数据源的两个值`Update`命令： 新的位置和元素的 ID。 因此，数据源需要`<UpdateParameters>`这两个值的部分：

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

`ReorderList`控件需要设置以下属性：

- `AllowReorder`： 是否可以重新排列列表项
- `DataSourceID`： 数据源的 ID
- `DataKeyField`： 的名称的数据源中主键列
- `SortOrderField`： 提供的列表项的排序顺序数据源列

在`<DragHandleTemplate>`和`<ItemTemplate>`部分中，列表的布局可以细微调整。 此外，数据绑定是可能使用`Eval()`方法，如下所示：

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

以下 CSS 样式信息 (中引用`<DragHandleTemplate>`部分`ReorderList`控件) 可确保，鼠标指针将相应更改当悬停拖动句柄：

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

最后，`ScriptManager`控件初始化 ASP.NET AJAX 页：

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

在浏览器中运行此示例和有点重新排列列表项。 然后，重新加载该页面和/或具有在数据库的外观。 更改的位置所保留的天数和中的值也会反映`position`列数据库中的所有无需任何代码，只使用标记。


[![中的新列表项顺序根据数据库更改的数据](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

根据新列表的数据库更改数据项顺序 ([单击以查看实际尺寸的图像](drag-and-drop-via-reorderlist-vb/_static/image9.png))

>[!div class="step-by-step"]
[上一篇](using-postbacks-with-reorderlist-vb.md)
