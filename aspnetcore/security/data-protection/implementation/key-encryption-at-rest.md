---
title: "密钥加密对静止"
author: rick-anderson
description: "本文档概述了 ASP.NET 核心数据保护密钥加密对静止的实现详细信息。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: c66430bfe547cf061e9e79a703ac665a968bbe0b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="key-encryption-at-rest"></a>密钥加密对静止

<a name="data-protection-implementation-key-encryption-at-rest"></a>

默认情况下，数据保护系统[使用启发式方法](xref:security/data-protection/configuration/default-settings)来确定如何加密的密钥材料应加密对静止。 开发人员可以重写启发式方法，并手动指定应如何静态加密密钥。

> [!NOTE]
> 如果指定在 rest 机制显式密钥加密时，数据保护系统将取消注册启发式方法提供的默认密钥存储机制。 你必须[指定显式的密钥存储机制](key-storage-providers.md#data-protection-implementation-key-storage-providers)，否则数据保护系统将无法启动。

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

数据保护系统都附带有三个框中的密钥加密机制。

## <a name="windows-dpapi"></a>Windows DPAPI

*仅在 Windows 上，此机制是可用。*

当使用 Windows DPAPI 时，将通过加密密钥材料[CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx)之前保存到存储。 DPAPI 是一种用于将永远不会读取当前计算机之外的数据的合适的加密机制 (尽管它可以备份到 Active Directory 这些密钥，请参阅[DPAPI 和漫游配置文件](https://support.microsoft.com/kb/309408/#6))。 例如，若要配置 DPAPI 密钥在 rest 加密。

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

如果`ProtectKeysWithDpapi`调用不带任何参数，仅当前 Windows 用户帐户，才能解密持久化的密钥材料。 你可以选择指定中所示，应导致计算机 （而不仅仅是当前用户帐户） 上的任何用户帐户能够解密的密钥材料，下面的示例。

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a>X.509 证书

*此机制不可用于`.NET Core 1.0`或`1.1`。*

如果你的应用程序分布在多台计算机，则可能很方便多台计算机之间分发共享的 X.509 证书并配置应用程序，此证书用于加密存放的密钥。 有关示例，请参阅下文。

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

由于.NET Framework 限制支持仅使用 CAPI 私钥的证书。 请参阅[基于证书的加密使用 Windows DPAPI NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng)下面这些限制的可能解决方法。

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

*此机制是仅适用于 Windows 8 / Windows Server 2012 和更高版本。*

操作系统从 Windows 8 开始，支持 DPAPI NG （也称为 CNG DPAPI）。 Microsoft 是布局其使用方案，如下所示。

   但是，云计算，通常需要该内容的加密在一台计算机进行在另一台解密。 因此，从开始 Windows 8，Microsoft 扩展的使用相对比较简单的 API 以覆盖云方案的想法。 称为 DPAPI NG，此新 API，可安全地通过保护它们的一组可用于取消这些不同的计算机上保护后正确的身份验证和授权的主体到共享机密 （密钥、 密码、 密钥材料） 和消息。

   从[有关 CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)

主体编码为保护描述符规则。 请考虑以下示例中，该加密密钥材料，以便仅已加入域的用户具有指定 SID 都可以解密密钥材料。

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

另外，还有的无参数重载`ProtectKeysWithDpapiNG`。 这是用于指定规则的便捷方法"SID = 挖掘"，其中挖掘是当前 Windows 用户帐户的 SID。

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

在此方案中，AD 域控制器负责分发 DPAPI NG 操作所使用的加密密钥。 目标用户将能够解密从任何加入域的计算机的加密的负载 （前提是进程正在其标识下运行时）。

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>基于证书的加密使用 Windows DPAPI NG

如果在 Windows 8.1 上运行 / Windows Server 2012 R2 或更高版本，你可以使用 Windows DPAPI NG 执行基于证书的加密，即使在上运行应用程序[.NET 核心](https://www.microsoft.com/net/core)。 若要充分利用此功能，使用规则描述符字符串"证书 = HashId:thumbprint"，其中指纹是要使用的证书的十六进制编码 SHA1 指纹。 有关示例，请参阅下文。

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

在此存储库指向任何应用程序必须在 Windows 8.1 上运行 / Windows Server 2012 R2 或更高版本能够解密此密钥。

## <a name="custom-key-encryption"></a>自定义密钥加密

如果不适合的内置机制，开发人员可以通过提供自定义指定自己的密钥加密机制`IXmlEncryptor`。
