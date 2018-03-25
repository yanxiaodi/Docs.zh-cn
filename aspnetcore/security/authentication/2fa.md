---
title: "在 ASP.NET Core SMS 的双因素身份验证"
author: rick-anderson
description: "了解如何设置双因素身份验证 (2FA) 与 ASP.NET Core 应用。"
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: c328c6f4b674695dd1f2db8145a7ac1b8f12d36d
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>在 ASP.NET Core SMS 的双因素身份验证

通过[Rick Anderson](https://twitter.com/RickAndMSFT)和[瑞士开发人员](https://github.com/Swiss-Devs)

本教程适用于 ASP.NET Core 仅 1.x。 请参阅[ASP.NET Core 中的身份验证器应用启用 QR 代码生成](xref:security/authentication/identity-enable-qrcodes)ASP.NET 核心 2.0 及更高版本。

本教程演示如何设置双因素身份验证 (2FA) 使用短信。 为提供的说明[twilio](https://www.twilio.com/)和[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)，但你可以使用任何其他 SMS 提供程序。 我们建议你完成[帐户确认和密码恢复](accconfirm.md)之前开始学习本教程。

视图[已完成的示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)。 [如何下载](xref:tutorials/index#how-to-download-a-sample)。

## <a name="create-a-new-aspnet-core-project"></a>创建新的 ASP.NET Core 项目

创建新的 ASP.NET 核心 web 应用名为`Web2FA`与单个用户帐户。 按照中的说明[在 ASP.NET Core 应用程序强制实施 SSL](xref:security/enforcing-ssl)才能设置，并且需要 SSL。

### <a name="create-an-sms-account"></a>创建 SMS 帐户

创建 SMS 帐户，例如，从[twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)。 记录身份验证凭据 (twilio: accountSid 和 authToken 的 ASPSMS： 用户密钥和密码)。

#### <a name="figuring-out-sms-provider-credentials"></a>了解 SMS 提供程序凭据

**Twilio:**  
从你的 Twilio 帐户的仪表板选项卡上，复制**帐户 SID**和**身份验证令牌**。

**ASPSMS:**  
从你的帐户设置，导航到**用户密钥**并将其连同复制你**密码**。

我们将更高版本将存储在密钥中的密钥管理器工具使用这些值`SMSAccountIdentification`和`SMSAccountPassword`。

#### <a name="specifying-senderid--originator"></a>指定 SenderID / 发起方

**Twilio:**  
从数字选项卡，将复制你的 Twilio**电话号码**。 

**ASPSMS:**  
解锁原始发件人菜单上，在解锁一个或多个发送方或选择 （不支持的所有网络） 的字母数字发起方。 

我们将更高版本存储在项的密钥管理器工具使用此值`SMSAccountFrom`。


### <a name="provide-credentials-for-the-sms-service"></a>SMS 服务提供的凭据

我们将使用[选项模式](xref:fundamentals/configuration/options)访问的用户帐户和密钥设置。 

   * 创建一个类以提取安全 SMS 密钥。 对于此示例，`SMSoptions`中创建类*Services/SMSoptions.cs*文件。

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

设置`SMSAccountIdentification`，`SMSAccountPassword`和`SMSAccountFrom`与[机密管理器工具](xref:security/app-secrets)。 例如:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* SMS 提供程序添加 NuGet 包。 从包管理器控制台 (PMC) 运行：

**Twilio:**  
`Install-Package Twilio`

**ASPSMS:**  
`Install-Package ASPSMS`


* 中添加代码*Services/MessageServices.cs*文件以启用 SMS。 使用 Twilio 或 ASPSMS 部分：


**Twilio:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>配置要使用的启动 `SMSoptions`

添加`SMSoptions`对中的服务容器`ConfigureServices`中的方法*Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>启用双因素身份验证

打开*Views/Manage/Index.cshtml* Razor 视图文件并删除注释字符 （因此没有标记出 commnted）。

## <a name="log-in-with-two-factor-authentication"></a>使用双因素身份验证登录

* 运行应用并注册新的用户

![Web 应用程序视图打开 Microsoft Edge 中的注册](2fa/_static/login2fa1.png)

* 在你激活的用户名称上点击`Index`管理控制器中的操作方法。 然后点击的电话号码**添加**链接。

![管理视图](2fa/_static/login2fa2.png)

* 添加一个电话号码，它将接收验证代码，并点击**发送验证码**。

![添加电话号码页](2fa/_static/login2fa3.png)

* 你将获取验证码短信。 输入它，然后点击**提交**

![验证电话号码页](2fa/_static/login2fa4.png)

如果你不会获得文本消息，请参阅 twilio 日志页。

* 管理视图显示你的电话号码已成功添加。

![管理视图](2fa/_static/login2fa5.png)

* 点击**启用**启用双因素身份验证。

![管理视图](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>测试两个双因素身份验证

* 注销。

* 登录。

* 用户帐户已启用双因素身份验证，因此你需要提供身份验证的第二个因素。 在本教程中已启用电话验证。 内置的模板还可用于将作为第二因素的电子邮件设置。 你可以设置其他的第二个因素，如 QR 代码的身份验证。 点击**提交**。

![发送验证代码视图](2fa/_static/login2fa7.png)

* 输入中的 SMS 消息得到的代码。

* 单击**记住此浏览器**复选框将免除你无需使用 2FA 时使用相同的设备和浏览器登录。 启用 2FA 和单击**记住此浏览器**将为你提供强 2FA 保护免受恶意用户尝试访问你的帐户，只要它们无权访问你的设备。 可以在任何您经常使用的专用设备上执行此操作。 通过设置**记住此浏览器**，设备不经常使用的情况下，从获取 2FA 的附加的安全性以及你可以方便地在无需在你自己的设备通过 2FA 转。

![验证视图](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>针对暴力破解攻击提供保护的帐户锁定

帐户锁定建议的 2FA 中。 一旦用户通过本地帐户或社交帐户登录，则会存储 2FA 在每次失败的尝试。 如果达到最大未成功的访问，则用户锁定 (默认： 5 分钟锁定 5 失败的访问尝试次数后)。 成功的身份验证将失败的访问尝试计数重置并重置时钟。 最大失败的访问尝试次数，可以用来设置的锁定时间[MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts)和[DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan)。 以下 10 分钟后访问尝试失败 10 次配置帐户锁定：

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

确认[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync)设置`lockoutOnFailure`到`true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
