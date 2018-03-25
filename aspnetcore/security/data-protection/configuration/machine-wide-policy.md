---
title: "在 ASP.NET 核心中支持的数据保护计算机范围的策略"
author: rick-anderson
description: "了解有关设置默认计算机范围策略对于使用 ASP.NET 核心数据保护的所有应用程序的支持。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 53ded37e9fd5f1a2eaa37935d1c52efb1e9231ac
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>在 ASP.NET 核心中支持的数据保护计算机范围的策略

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

Windows 上运行时，数据保护系统具有有限的支持设置默认计算机范围的所有应用程序使用 ASP.NET 核心数据保护策略。 常规的思路是管理员可能想要更改默认设置，如算法或密钥的生存期，而无需手动更新每个应用程序，在计算机上。

> [!WARNING]
> 系统管理员可以设置默认策略，但它们无法强制执行它。 应用程序开发人员始终可以重写其中一个自己的选择的任何值。 默认策略仅影响应用程序开发人员未指定显式设置值的位置。

## <a name="setting-default-policy"></a>设置默认策略

若要设置默认策略，管理员可以设置以下注册表项下系统注册表中的已知的值：

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

如果你是在 64 位操作系统上，并想要影响的 32 位应用程序的行为，请记住将上面的项的 Wow6432Node 等效项。

支持的值如下所示。

| “值”              | 类型   | 描述 |
| ------------------ | :----: | ----------- |
| EncryptionType     | 字符串 | 指定应使用哪些算法来进行数据保护。 值必须为 CNG CBC、 CNG 的 GCM 或托管和中更详细地介绍了。 |
| DefaultKeyLifetime | DWORD  | 指定新生成的键的生存期。 此值以天为单位指定，并且必须是 > = 7。 |
| KeyEscrowSinks     | 字符串 | 指定用于密钥证书的类型。 值是以分号分隔的密钥托管接收器，其中在列表中的每个元素都实现的类型的程序集限定名称列表[IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink)。 |

## <a name="encryption-types"></a>加密类型

如果 EncryptionType CNG CBC，系统配置为使用 CBC 模式对称的块密码来进行保密性和 HMAC 真实性有通过 Windows CNG 提供服务 (请参阅[指定自定义 Windows CNG 算法](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)为更多详细信息）。 支持以下的其他值，其中每个对应于 CngCbcAuthenticatedEncryptionSettings 类型上的属性。

| “值”                       | 类型   | 描述 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | 字符串 | 理解的 CNG 对称的块密码算法的名称。 此算法在 CBC 模式下打开。 |
| EncryptionAlgorithmProvider | 字符串 | 可以生成 EncryptionAlgorithm 的算法的 CNG 提供程序实现的名称。 |
| EncryptionAlgorithmKeySize  | DWORD  | 要派生的对称块加密算法的密钥长度 （以位为单位）。 |
| HashAlgorithm               | 字符串 | 理解的 CNG 哈希算法的名称。 此算法在 HMAC 模式打开。 |
| HashAlgorithmProvider       | 字符串 | 可以生成算法的 HashAlgorithm CNG 提供程序实现的名称。 |

如果 EncryptionType CNG GCM，系统配置为使用保密性和身份验证与服务提供的 Windows CNG Galois/计数器模式对称的块密码 (请参阅[指定自定义 Windows CNG 算法](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)有关详细信息）。 支持以下的其他值，其中每个对应于 CngGcmAuthenticatedEncryptionSettings 类型上的属性。

| “值”                       | 类型   | 描述 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | 字符串 | 理解的 CNG 对称的块密码算法的名称。 此算法将在 Galois/计数器模式中打开。 |
| EncryptionAlgorithmProvider | 字符串 | 可以生成 EncryptionAlgorithm 的算法的 CNG 提供程序实现的名称。 |
| EncryptionAlgorithmKeySize  | DWORD  | 要派生的对称块加密算法的密钥长度 （以位为单位）。 |

如果管理 EncryptionType，系统配置为用于托管的 SymmetricAlgorithm 保密性和 KeyedHashAlgorithm 真实性 (请参阅[指定自定义管理算法](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms)有关详细信息)。 支持以下的其他值，其中每个对应于 ManagedAuthenticatedEncryptionSettings 类型上的属性。

| “值”                      | 类型   | 描述 |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | 字符串 | 实现 SymmetricAlgorithm 的类型的程序集限定名称。 |
| EncryptionAlgorithmKeySize | DWORD  | 要派生的对称加密算法的密钥长度 （以位为单位）。 |
| ValidationAlgorithmType    | 字符串 | 实现 KeyedHashAlgorithm 的类型的程序集限定名称。 |

如果 EncryptionType 具有任何其他值非 null 或空时，数据保护系统启动时引发的异常。

> [!WARNING]
> 在配置涉及类型名称 （EncryptionAlgorithmType、 ValidationAlgorithmType、 KeyEscrowSinks） 的默认策略设置时，类型必须是可用于应用。 这意味着，对于在桌面 CLR 上运行的应用程序，包含这些类型的程序集应会显示在全局程序集缓存 (GAC) 中。 对于 ASP.NET Core 应用程序上运行[.NET 核心](https://www.microsoft.com/net/core)，应安装包含这些类型的包。
