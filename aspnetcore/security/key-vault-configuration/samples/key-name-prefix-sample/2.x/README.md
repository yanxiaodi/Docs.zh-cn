# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-2x"></a>前缀密钥保管库配置提供程序示例应用程序 (ASP.NET Core 2.x)

此示例演示如何使用 Azure 密钥保管库配置提供程序的 ASP.NET Core 2.x 使用密钥名称前缀。 有关 ASP.NET 核心 1.x 示例，请参阅[前缀密钥保管库配置提供程序示例应用程序 (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x)。

此示例的工作原理的详细信息，请参阅[Azure 密钥保管库配置提供程序](xref:security/key-vault-configuration)主题。

## <a name="using-the-sample"></a>使用示例
1. 创建密钥保管库并设置应用程序的指南的 Azure Active Directory (Azure AD)[开始使用 Azure 密钥保管库](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
  * 将机密添加到密钥保管库使用 Azure PowerShell 模块、 Azure 管理 API 或 Azure 门户。 机密创建为*手动*或*证书*机密。 *证书*机密是使用应用程序和服务的证书，但不是支持配置提供程序。 应使用*手动*选项可创建带有配置提供程序使用的名称-值对机密。
    * 分层值 （配置节） 使用`--`（两个短划线） 作为分隔符。
    * 有关示例应用中，创建两个*手动*具有以下名称-值对的机密：
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * 向 Azure Active Directory 注册示例应用程序。
  * 授权应用程序访问密钥保管库。 当你使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet 来授权应用程序访问密钥保管库，提供`List`和`Get`访问与机密`-PermissionsToSecrets list,get`。
2. 更新应用程序的*appsettings.json*的值的文件`Vault`， `ClientId`，和`ClientSecret`。
3. 运行示例应用程序，它可以通过获取其配置值从`IConfigurationRoot`带前缀的机密名称与同名。 在此示例中，该前缀是应用程序的版本，它提供给`PrefixKeyVaultSecretManager`添加 Azure 密钥保管库配置提供程序时。 值`AppSecret`一起被获取`config["AppSecret"]`。
4. 更改中的项目文件中的应用程序集版本`5.0.0.0`到`5.1.0.0`并再次运行该应用。 此时，返回的机密值是`5.1.0.0_secret_value`。
