---
title: "非 DI 感知的情境中 ASP.NET Core 的数据保护"
author: rick-anderson
description: "了解如何支持数据保护方案不能或不想使用提供的依赖关系注入服务的位置。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: d878bd20489876f919f2a8e0149f3f000cbf72d8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>非 DI 感知的情境中 ASP.NET Core 的数据保护

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET 核心数据保护系统通常是[添加到服务容器](xref:security/data-protection/consumer-apis/overview)并且供通过依赖关系注入 (DI) 的从属组件。 但是，一些情况下，这不可行或所需，尤其是在将系统导入到现有应用程序。

若要支持这些方案中， [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)包提供具体类型， [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider)，它提供一种简单的方法为使用数据保护而不依赖于 DI。 `DataProtectionProvider`类型实现[IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider)。 构造`DataProtectionProvider`只需提供[DirectoryInfo](/dotnet/api/system.io.directoryinfo)实例，以指示应存储提供程序的加密密钥的位置，如下面的代码示例中所示：

[!code-none[](non-di-scenarios/_static/nodisample1.cs)]

默认情况下，`DataProtectionProvider`具体的类型不加密原始密钥材料之前将其保存到文件系统。 这是为了支持开发人员将指向网络共享和数据保护系统无法自动推导适当静态密钥加密机制的方案。

此外，`DataProtectionProvider`具体的类型不[隔离应用程序](xref:security/data-protection/configuration/overview#per-application-isolation)默认情况下。 使用相同密钥的目录的所有应用程序可以共享负载长达其[目的参数](xref:security/data-protection/consumer-apis/purpose-strings)匹配。

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)构造函数接受一个可选配置回调，可以用于调整的系统行为。 下面的示例演示与显式调用还原隔离[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)。 此示例还演示将系统配置为自动加密持久化的密钥使用 Windows DPAPI。 如果目录指向 UNC 共享，可能想要在所有相关计算机间分发共享的证书还可将系统配置为使用基于证书的加密和调用[ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)。

[!code-none[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> 实例`DataProtectionProvider`具体类型是创建开销很大。 如果应用程序维护此类型的多个实例，并且如果他们正在使用相同的密钥存储目录，应用程序性能可能会降低。 如果你使用`DataProtectionProvider`类型，我们建议你一次创建此类型，并重复使用尽可能多地它。 `DataProtectionProvider`类型及其所有[IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)从它创建的实例是线程安全的多个调用方。
