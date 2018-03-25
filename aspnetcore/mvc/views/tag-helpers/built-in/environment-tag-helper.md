---
title: "ASP.NET Core 中的环境标记帮助程序"
author: pkellner
description: "定义的 ASP.NET Core 环境标记帮助程序（包括所有属性）"
manager: wpickett
ms.author: riande
ms.date: 07/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 7a99ee0e59c7f49a3208d2c86c11cabce4294889
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a>ASP.NET Core 中的环境标记帮助程序

作者：[Peter Kellner](http://peterkellner.net) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya)

环境标记帮助程序根据当前宿主环境，有条件地呈现其包含的内容。 其单一属性 `names` 是一个以逗号分隔的环境名称列表，如果其中的任何名称与当前环境匹配，就会触发已包含内容的呈现。

## <a name="environment-tag-helper-attributes"></a>环境标记帮助程序属性

### <a name="names"></a>名称

采用单个宿主环境名称或以逗号分隔的宿主环境名称列表，用于触发已包含内容的呈现。

这些值与从 ASP.NET Core 静态属性 `HostingEnvironment.EnvironmentName` 返回的当前值进行比较。  此值为以下值之一：**Staging**、**Development** 或 **Production**。 比较不区分大小写。

有效的 `environment` 标记帮助程序示例为：

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>include 和 exclude 属性

ASP.NET Core 2.x 添加了 `include` & `exclude` 属性。 这些属性基于已包括或已排除的宿主环境名称，控制已包含内容的呈现。

### <a name="include-aspnet-core-20-and-later"></a>include ASP.NET Core 2.0 及更高版本

`include` 属性的行为与 ASP.NET Core 1.0 中 `names` 属性的行为类似。

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>exclude ASP.NET Core 2.0 及更高版本

相反，`exclude` 属性允许 `EnvironmentTagHelper` 为指定名称以外的所有宿主环境名称呈现已包含内容。

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>其他资源

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
