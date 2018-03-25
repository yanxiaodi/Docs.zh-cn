---
title: "安全存储在 ASP.NET Core 在开发过程中的应用程序机密"
author: rick-anderson
description: "演示如何在开发过程中安全地存储机密"
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: a23c9dc9ee1e20c0e0551a372e1cd706bb82070e
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a>安全存储在 ASP.NET Core 在开发过程中的应用程序机密

通过[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)，和[Scott Addie](https://scottaddie.com) 

本文档说明如何在开发中使用密钥管理器工具需要机密放在代码外部。 最重要的一点是你应永远不会将密码或其他敏感数据存储在源代码中，且你不应在开发和测试模式下使用生产机密。 你可以改用[配置](xref:fundamentals/configuration/index)系统环境变量读取这些值或从值存储使用密钥管理器工具。 密码管理器工具可帮助避免敏感数据被签入源代码管理。 [配置](xref:fundamentals/configuration/index)系统可以读取此文章中所述的密钥管理器工具与存储的机密。

密码管理器工具仅用于开发。 您可以保护与 Azure 的测试和生产机密[Microsoft Azure 密钥保管库](https://azure.microsoft.com/services/key-vault/)配置提供程序。 请参阅[Azure 密钥保管库配置提供程序](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration)有关详细信息。

## <a name="environment-variables"></a>环境变量

若要避免在代码中或在本地配置文件中存储应用程序机密，请在环境变量中存储机密。 你可以设置[配置](xref:fundamentals/configuration/index)框架，以读取从环境变量的值，通过调用`AddEnvironmentVariables`。 然后可以使用环境变量来重写的所有以前指定的配置源的配置值。

例如，如果使用单个用户帐户创建新的 ASP.NET 核心 web 应用程序，它将添加到的默认连接字符串*appsettings.json*与键项目文件中的`DefaultConnection`。 默认连接字符串为安装程序以使用 LocalDB，在用户模式下运行，而且不需要密码。 当你部署到测试或生产服务器应用程序时，你可以重写`DefaultConnection`密钥用于测试或生产数据库包含 （可能有敏感凭据） 的连接字符串的环境变量设置的值服务器。

>[!WARNING]
> 环境变量通常以明文形式存储，并且不加密。 如果计算机或进程受到攻击，则可以通过不受信任方访问环境变量。 可能仍需要更多措施，防止用户的机密信息泄露。

## <a name="secret-manager"></a>密码管理器

密钥管理器工具存储在项目树之外的开发工作的敏感数据。 密码管理器工具是一个项目工具，可以用于存储机密信息，以便[.NET 核心](https://www.microsoft.com/net/core)在开发过程中的项目。 使用密钥管理器工具中，可以将应用程序机密关联与特定项目，并共享跨多个项目。

>[!WARNING]
> 密码管理器工具不加密存储的机密信息，并不应被视为受信任存储区。 它是仅限开发目的。 键和值存储在用户配置文件目录中的 JSON 配置文件中。

## <a name="installing-the-secret-manager-tool"></a>安装机密管理器工具

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

右键单击解决方案资源管理器中的项目并选择**编辑\<文件的内容\>.csproj**从上下文菜单。 添加到突出显示的行将*.csproj*文件中，并保存以还原相关联的 NuGet 程序包：

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

再次右键单击解决方案资源管理器中的项目，然后选择**管理用户的机密信息**从上下文菜单。 添加一个新的该笔势`UserSecretsId`中的节点`PropertyGroup`的*.csproj*文件，那么在下面的示例中突出显示：

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

保存已修改*.csproj*文件还将打开`secrets.json`在文本编辑器中的文件。 内容替换`secrets.json`文件替换为以下代码：

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

添加`Microsoft.Extensions.SecretManager.Tools`到*.csproj*文件，运行[dotnet 还原](/dotnet/core/tools/dotnet-restore)。 可以使用相同的步骤来安装命令行使用该密钥管理器工具。

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

通过运行以下命令来测试机密管理器工具：

```console
dotnet user-secrets -h
```

使用情况、 选项和命令的帮助，将显示在密码管理器工具。

> [!NOTE]
> 你必须在与位于同一目录*.csproj*文件以运行中定义的工具*.csproj*文件的`DotNetCliToolReference`节点。

密码管理器工具进行存储用户配置文件中的项目的特定配置设置操作。 若要使用用户的机密信息，项目必须指定`UserSecretsId`值在其*.csproj*文件。 值`UserSecretsId`是任意的但通常唯一的项目。 开发人员通常应生成的 GUID `UserSecretsId`。

添加`UserSecretsId`为你的项目中*.csproj*文件：

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

密码管理器工具用于设置机密。 例如，在命令窗口从项目目录中，输入以下信息：

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

你可以从其他目录，运行密钥管理器工具，但必须使用`--project`选项的路径中传递*.csproj*文件：
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

你可以使用密钥管理器工具能够列出、 删除和清除应用程序密钥。

-----

## <a name="accessing-user-secrets-via-configuration"></a>访问通过配置的用户机密

可以通过配置系统访问机密 Manager 机密。 添加`Microsoft.Extensions.Configuration.UserSecrets`打包和运行[dotnet 还原](/dotnet/core/tools/dotnet-restore)。

添加到用户机密配置源`Startup`方法：

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

您可以访问用户的机密信息的配置 API 通过：

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>密码管理器工具的工作原理

密码管理器工具避开实现详细信息，例如哪里和如何存储值。 无需知道这些实现的详细信息，可以使用该工具。 在当前版本中，这些值存储在[JSON](http://json.org/)用户配置文件目录中的配置文件：

* Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

值`userSecretsId`来自中指定的值*.csproj*文件。

你不应编写代码所依赖的位置或使用密码管理器工具中，保存的数据格式，如这些实现的详细信息可能会更改。 例如，密钥的值是当前*不*今天，加密，但可能有一天。

## <a name="additional-resources"></a>其他资源

* [配置](xref:fundamentals/configuration/index)
