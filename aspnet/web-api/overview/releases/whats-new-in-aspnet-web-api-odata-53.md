---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: "什么是 ASP.NET Web API OData 5.3 中的新增功能 |Microsoft 文档"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>什么是 ASP.NET Web API OData 5.3 中的新增功能
====================
通过[Microsoft](https://github.com/microsoft)

本主题介绍什么是用于 ASP.NET Web API OData 5.3 的新功能。

- [下载](#download)
- [文档](#documentation)
- [OData 核心库](#corelib)
- [新功能](#newf)
- [已知的问题和重大更改](#known-issues)
- [Bug 修复](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>下载

运行时功能将发布为 NuGet 库上的 NuGet 包。 你可以安装或使用 NuGet 程序包管理器控制台更新到已发布的 NuGet 程序包：

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>文档

您可找到教程和有关在 ASP.NET Web API OData 的其他文档[ASP.NET 网站](../odata-support-in-aspnet-web-api/index.md)。

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>OData 核心库

对于 OData v4，Web API 现在使用 ODataLib 版本 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>ASP.NET 中的新增功能的 Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>支持在 $ $levels 展开

你可以使用 $levels 中 $ 选项展开查询的查询。 例如: 

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

此查询等效于：

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>打开实体类型的支持

*打开键入*是 stuctured 类型包含动态属性，除了在类型定义中声明的任何属性。 开放类型允许你添加到数据模型的灵活性。 有关详细信息，请参阅 xxxx。

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>对动态集合属性中打开类型的支持

以前，动态属性必须是单个值。 在 5.3，动态属性可以具有集合值。 例如，在下面的 JSON 负载，`Emails`属性是动态属性，并且是字符串类型的集合：

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>对于复杂类型的继承支持

现在复杂类型可以继承自基类型。 例如，OData 服务可以定义以下的复杂类型：

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

下面是此示例 EDM:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

有关详细信息，请参阅[OData 复杂类型继承示例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)。

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的问题和重大更改

本部分介绍已知的问题和 ASP.NET Web API OData 5.3 中的重大更改。

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>查询选项

问题： 使用嵌套的 $展开与 $levels = max 导致不正确的扩展深度。

例如，给定以下请求：

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

如果`MaxExpansionDepth`为 5，此查询将导致 6 的扩展深度。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Bug 修复和细微的功能更新

此版本还包括几个 bug 修复和细微的功能更新。 您可以找到完整的列表：

- [Bug 修复](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

在此版本中[bug 修复](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All)某些 AllowedFunctions 枚举。 此版本没有任何其他 bug 修复或新的功能。
