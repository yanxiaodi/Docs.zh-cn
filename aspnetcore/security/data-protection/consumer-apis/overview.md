---
title: "使用者 Api 概述"
author: rick-anderson
description: "本文档提供各种使用者 Api ASP.NET 核心数据保护库中可用的简要概述。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: 3aa0c4bc8d009147dd15571da4d7d63402e4c512
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="consumer-apis-overview"></a>使用者 Api 概述

`IDataProtectionProvider`和`IDataProtector`接口是通过该使用者使用数据保护系统的基本接口。 它们位于[Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/)包。

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

提供程序接口表示数据保护系统的根目录。 它不能直接用于保护或取消保护数据。 相反，使用者必须获得对引用`IDataProtector`通过调用`IDataProtectionProvider.CreateProtector(purpose)`，其中，目的是描述预期的使用者用例的字符串。 请参阅[目的字符串](purpose-strings.md)着眼于此参数以及如何选择适当的值的更多的信息。

## <a name="idataprotector"></a>IDataProtector

保护程序接口将返回通过调用`CreateProtector`，和它的使用者可以用于执行此接口保护或取消保护操作。

若要保护的数据片段，将数据传递给`Protect`方法。 基本接口定义的将 byte []-> byte [] 的方法，但没有还将字符串转换的重载 （提供作为扩展方法）-> 字符串。 提供两种方法的安全性是相同的;开发人员应选择任何重载是最方便对其用例。 而不考虑的重载选择，则返回保护方法现在受保护 （enciphered 和防考验） 和应用程序可以将其发送到不受信任的客户端。

若要取消对以前受保护的数据片段的保护，将传递到受保护的数据`Unprotect`方法。 (有 byte []-基于和基于字符串的重载，为开发人员方便起见。)如果受保护的负载由以前调用生成`Protect`此同一`IDataProtector`、`Unprotect`方法将返回原始的未受保护的负载。 如果已被篡改或已由不同的受保护的负载`IDataProtector`、`Unprotect`方法会引发 CryptographicException。

与不同的相同的概念`IDataProtector`ties 回目的的概念。 如果两个`IDataProtector`实例生成从同一根`IDataProtectionProvider`但通过不同的用途的调用中的字符串`IDataProtectionProvider.CreateProtector`，则它们正在被视为[不同的保护程序](purpose-strings.md)，和一个将无法取消保护由其他生成的负载。

## <a name="consuming-these-interfaces"></a>使用这些接口

对于 DI 感知的组件，预期的用法是，该组件需要`IDataProtectionProvider`在其构造函数的参数，并实例化组件时，DI 系统可以自动提供此服务。

> [!NOTE]
> 某些应用程序 （如控制台应用程序或 ASP.NET 4.x 应用程序） 可能不是 DI 感知因此不能使用此处所述的机制。 有关这些方案，请查阅[非 DI 感知的情境](../configuration/non-di-scenarios.md)有关获取的实例的详细信息的文档`IDataProtection`而无需通过 DI 的提供程序。

下面的示例演示三个概念：

1. [添加数据保护系统](../configuration/overview.md)到服务容器

2. 使用 DI 接收的实例`IDataProtectionProvider`，和

3. 创建`IDataProtector`从`IDataProtectionProvider`并使用它来保护和取消保护数据。

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

包 Microsoft.AspNetCore.DataProtection.Abstractions 包含的扩展方法`IServiceProvider.GetDataProtector`为开发人员方便起见。 它作为单个操作封装这两个检索`IDataProtectionProvider`从服务提供商和调用`IDataProtectionProvider.CreateProtector`。 下面的示例演示其用法。

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> 实例`IDataProtectionProvider`和`IDataProtector`是线程安全的多个调用方。 它具有一个组件获取的引用后应，`IDataProtector`通过调用`CreateProtector`，它将使用该引用，以便多个调用`Protect`和`Unprotect`。 调用`Unprotect`将引发 CryptographicException，如果无法验证或中译解出来的受保护的负载。 某些组件可能想要忽略错误期间取消保护操作;组件它读取身份验证 cookie 可能处理此错误和请求则将视为根本具有任何 cookie，而不使迫切地请求失败。 需要此行为的组件应专门捕获 CryptographicException，而不是忽略所有异常。
