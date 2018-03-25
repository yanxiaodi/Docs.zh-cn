---
title: "数据保护简介"
author: rick-anderson
description: "本文档介绍的数据保护的概念，并概述了相关联的 ASP.NET 核心 Api 设计原则。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/introduction
ms.openlocfilehash: acd38679390b92705703111b72816f1a5d3ba848
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-data-protection"></a>数据保护简介

Web 应用程序通常需要存储安全敏感数据。 Windows 提供 DPAPI 用于桌面应用程序，但这不适用于 web 应用程序。 ASP.NET 核心数据保护堆栈提供一个简单、 易于使用的加密 API，开发人员可以使用来保护数据，包括密钥管理和旋转。

ASP.NET 核心数据保护堆栈旨在用作的长期替代<machineKey>在 ASP.NET 中的元素 1.x-4.x。 它旨在解决许多旧加密堆栈的不足之处同时为大多数现代应用程序都可能会遇到的使用情况下提供的现成可用的解决方案。

## <a name="problem-statement"></a>问题陈述

总体问题语句中的单个句子可以用于简单地所述： 我需要保持受信任的信息，以便于以后检索，但我不信任的持久性机制。 在 web 术语中，这可能会写"我需要为通过不受信任的客户端往返受信任的状态。"

此规范的示例是身份验证 cookie 或持有者令牌。 服务器会生成"我是 Groot，具有 xyz 权限"令牌并将它传递到客户端。 在将来的某个日期，客户端将显示该令牌发送回服务器，但服务器需要某种类型的客户端尚未伪造令牌的保证。 因此第一个要求： 真实性 （也称为 完整性、 防校对）。

由于所保持的状态由服务器信任的我们预计这种状态，可能包含特定于操作环境的信息。 这可能是或的服务器特定数据的其他部分的文件路径、 权限、 句柄或其他间接引用的形式。 此类信息应通常不得泄露给不受信任的客户端。 因此第二个要求： 保密性。

最后，由于现代应用程序已被组件化，我们已了解是各个组件将想要利用此系统而不考虑其他组件在系统中。 例如，如果持有者令牌组件使用此堆栈，它应运行，而不从一种反 CSRF 机制，也可能使用相同的堆栈的干扰。 因此最后一项要求： 隔离。

我们可以提供进一步的约束为了缩小我们的要求的范围。 我们假定加密系统内运行的所有服务都都同样受信任而数据不需要使用外部下我们直接控制服务或生成。 此外，我们需要操作是尽可能快，因为 web 服务的每个请求可能会经过加密系统一个或多个时间。 这样，可以对称加密适合我们的方案，而且我们可以如具有所需的时间之前折扣非对称加密。

## <a name="design-philosophy"></a>设计理念

通过标识具有现有堆栈的问题，我们已开始。 我们有的我们调查了现有的解决方案的布局，并结束，任何现有解决方案相当了我们查找的功能。 我们然后工程处理基于几个指导原则的解决方案。

* 系统应提供简单的配置。 理想情况下，系统将零配置并开发人员无法按完全运行。 在开发人员需要配置 （如密钥存储库） 的特定方面的情况下，应特别注意简化这些特定的配置。

* 提供一个简单的面向使用者的 API。 Api 应易于正确使用且难以不正确地使用。

* 开发人员不应了解密钥管理原则。 系统应处理算法选择和开发人员代表密钥的生存期。 理想情况下开发人员甚至不应有权访问原始密钥材料。

* 应存放在可能的情况保护密钥。 系统应找出适当的默认保护机制，并自动应用。

牢记这些原则与我们开发一个简单、[易于使用](using-data-protection.md)数据保护堆栈。

ASP.NET 核心数据保护 Api 主要不用于机密负载的无限期持久性。 其他技术喜欢[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)和[Azure Rights Management](https://docs.microsoft.com/rights-management/)更适合于以下场景： 无限期存储，并且它们的相应强密钥管理功能。 也就是说，无需进行任何开发人员禁止使用 ASP.NET Core 数据保护 Api 进行长期保护的机密数据。

## <a name="audience"></a>读者

数据保护系统分为五个主要的软件包。 这些 Api 的各个方面的目标三个主要受众;

1. [使用者 Api 概述](consumer-apis/overview.md)目标应用程序和 framework 开发人员。

   "我不想要了解有关堆栈的运行方式或配置方式。 我只想要执行某项操作中的作为简单的方式尽可能与高概率的成功使用 Api。"

2. [配置 Api](configuration/overview.md)面向应用程序开发人员和系统管理员。

   "我需要告诉我的环境需要非默认路径或设置数据保护系统。"

3. 扩展性 Api 目标开发人员负责实现自定义策略。 这些 api 的使用情况将限制为极少数情况下，并且可以出现，安全感知开发人员。

   "我需要更换系统中的整个组件，因为我具有真正独特的行为要求。 我愿意以了解极其使用组成部分的 API 图面以生成满足我的要求的插件。"

## <a name="package-layout"></a>包布局

数据保护堆栈包含五个的包。

* Microsoft.AspNetCore.DataProtection.Abstractions 包含基本的 IDataProtectionProvider 和 IDataProtector 接口。 它还包含有助于使用这些类型 （例如，重载 IDataProtector.Protect） 的有用的扩展方法。 请参阅使用者接口部分以了解更多信息。 如果其他人负责实例化数据保护系统，并且你只需使用 Api，则需要引用 Microsoft.AspNetCore.DataProtection.Abstractions。

* Microsoft.AspNetCore.DataProtection 包含数据保护系统，包括核心加密操作、 密钥管理、 配置和可扩展性的核心的实现。 如果你要负责实例化数据保护系统 （例如，将其添加到 IServiceCollection） 或修改或扩展其行为，你将希望引用 Microsoft.AspNetCore.DataProtection。

* Microsoft.AspNetCore.DataProtection.Extensions 包含其他 Api 的开发人员可能会发现很有用，但这不属于核心包中。 例如，此程序包包含一个简单的"实例化指向没有依赖关系注入安装程序的特定密钥的存储目录系统"API （详细信息）。 它还包含用于限制受保护负载 （详细信息） 的生存期的扩展方法。

* Microsoft.AspNetCore.DataProtection.SystemWeb 可安装到现有 ASP.NET 4.x 应用程序将重定向其<machineKey>操作改为使用新的数据保护堆栈。 请参阅[兼容性](compatibility/replacing-machinekey.md#compatibility-replacing-machinekey)有关详细信息。

* Microsoft.AspNetCore.Cryptography.KeyDerivation 提供的哈希例程 PBKDF2 密码的实现，并且可以由系统需要安全地处理用户密码。 请参阅[密码哈希](consumer-apis/password-hashing.md)有关详细信息。
