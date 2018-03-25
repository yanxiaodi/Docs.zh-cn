---
title: "基于使用单个用户帐户创建的项目的文章"
author: rick-anderson
description: "本文档列出了一些文章基于使用单个用户帐户创建的项目。"
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: aee18fa08fbc5c8452ca2b401d32858edaf55e7c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>基于使用单个用户帐户创建的项目的文章

ASP.NET 核心标识包含在 Visual Studio 中使用"单个用户帐户"选项的项目模板中。

中使用的.NET 核心 CLI 中可用的身份验证模板`-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

以下文章演示了如何使用 ASP.NET Core 使用单个用户帐户的模板中生成的代码：

* [使用 SMS 设置双因素身份验证](xref:security/authentication/2fa)
* [ASP.NET Core 中的帐户确认和密码恢复](xref:security/authentication/accconfirm)
* [使用受授权的用户数据创建 ASP.NET Core 应用](xref:security/authorization/secure-data)
