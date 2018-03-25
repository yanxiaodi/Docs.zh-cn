---
title: "数据保护密钥管理和 ASP.NET Core 的生存时间"
author: rick-anderson
description: "了解有关数据保护密钥管理和 ASP.NET Core 的生存时间。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: b43c14af015d5e03f46200c51a1218a581b1de0c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>数据保护密钥管理和 ASP.NET Core 的生存时间

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>密钥管理

应用程序将尝试检测其操作环境并处理其自己的密钥配置。

1. 如果应用程序承载于[Azure Apps](https://azure.microsoft.com/services/app-service/)，密钥保存到*%HOME%\ASP.NET\DataProtection-Keys*文件夹。 此文件夹由网络存储提供支持，并且在托管应用的所有计算机同步。
   * 密钥未保护静止。
   * *数据保护密钥*文件夹提供密钥环在单个部署槽中的应用程序的所有实例。
   * 单独的部署槽，如过渡和生产环境，请不要共享密钥链。 更换之间部署槽，如交换过渡到生产环境或使用 A / B 测试，使用数据保护任何应用将无法解密使用密钥链内前一槽的存储的数据。 这会导致对正在注销的应用程序使用标准的 ASP.NET Core cookie 身份验证，因为它使用数据保护来保护其 cookie 的用户。 如果你需要独立于槽的密钥环，使用 Azure Blob 存储，Azure 密钥保管库，SQL 存储区中，之类的外部密钥环提供程序，或 Redis 缓存。

1. 如果可用的用户配置文件，将密钥保存到*%LOCALAPPDATA%\ASP.NET\DataProtection-Keys*文件夹。 如果操作系统是 Windows，密钥被加密使用 DPAPI 对静止。

1. 如果应用程序托管在 IIS 中，密钥会保留到 HKLM 注册表仅对辅助进程帐户是 ACLed 某个特殊的注册表项中。 使用 DPAPI 对密钥静态加密。

1. 如果没有这些条件匹配，密钥不保留在当前进程之外。 进程关闭时，生成所有密钥都都将丢失。

开发人员可始终完全控制，并且可重写如何和密钥的存储位置。 上面的前三个选项应提供很好的默认值对于大多数应用程序类似于如何 ASP.NET  **\<machineKey >**过去可以正常运行自动生成例程。 最终的回退选项是要求开发人员指定的唯一情形[配置](xref:security/data-protection/configuration/overview)前部如果他们想要密钥持久性，但此回退仅发生在极少数情况下。

当承载 Docker 容器中时，应是 （共享的卷或容器的生存期结束后仍然存在的主机装入的卷） 的 Docker 卷的文件夹中保留密钥或在外部提供程序，如[Azure 密钥保管库](https://azure.microsoft.com/services/key-vault/)或[Redis](https://redis.io/)。 如果应用程序不能访问共享的网络卷，可能也是在 web 场方案中有用外部提供程序 (请参阅[PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem)有关详细信息)。

> [!WARNING]
> 如果开发人员重写上面所述的规则和点的特定的密钥存储库的数据保护系统，则会禁用自动加密对静止的密钥。 在 rest 保护可能会通过重新启用[配置](xref:security/data-protection/configuration/overview)。

## <a name="key-lifetime"></a>密钥的生存期

默认情况下，密钥具有 90 天的生存期。 密钥过期时，该应用将自动生成新密钥，并将新的密钥设置为活动密钥。 只要已停用的密钥保留在系统上，你的应用程序可以解密受保护的与之任何数据。 请参阅[密钥管理](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)有关详细信息。

## <a name="default-algorithms"></a>默认的算法

使用的默认负载保护算法是 AES 256 CBC 机密性和 HMACSHA256 真实性。 512 位主密钥，每隔 90 天更改一次用于派生用于每个负载基于这些算法的两个子键。 请参阅[子项派生](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation)有关详细信息。

## <a name="see-also"></a>请参阅

* [密钥管理扩展性](xref:security/data-protection/extensibility/key-management)
