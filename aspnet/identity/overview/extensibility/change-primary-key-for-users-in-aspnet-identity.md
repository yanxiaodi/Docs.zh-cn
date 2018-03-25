---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: "更改为在 ASP.NET Identity 中的用户的主要密钥 |Microsoft 文档"
author: tfitzmac
description: "在 Visual Studio 2013 中，默认 web 应用程序使用用户帐户的密钥的字符串值。 ASP.NET 标识使你能够更改的一种..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>更改主键在 ASP.NET Identity 中的用户
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 在 Visual Studio 2013 中，默认 web 应用程序使用用户帐户的密钥的字符串值。 ASP.NET 标识可以更改键以满足你的数据要求的类型。 例如，可以更改从字符串键的类型为整数。
> 
> 本主题说明如何开始时使用默认 web 应用程序，并将用户帐户密钥更改为整数。 可以使用相同的修改要在你的项目中实现任何类型的密钥。 它演示如何在默认 web 应用程序中进行这些更改，但你无法应用到自定义应用程序类似的修改。 它显示当使用 MVC 或 Web 窗体时需要的更改。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Visual Studio 2013 Update 2 （或更高版本）
> - ASP.NET 标识 2.1 或更高版本


若要执行的步骤在本教程中，必须具有 Visual Studio 2013 Update 2 （或更高版本），并从 ASP.NET Web 应用程序模板创建的 web 应用程序。 在 Update 3 中更改模板。 本主题说明如何更改 Update 2 和 Update 3 中的模板。

本主题包含以下各节：

- [更改标识用户类中的键的类型](#userclass)
- [添加使用键类型的自定义的标识类](#customclass)
- [更改的上下文类和用户管理器使用的键类型](#context)
- [要使用的键类型的更改启动配置](#startup)
- [对于 MVC Update 2 中，更改 AccountController 中传递的键类型](#mvcupdate2)
- [对于 MVC Update 3 中，更改 AccountController 并 ManageController 传递的键类型](#mvcupdate3)
- [对于带 Update 2 的 Web 窗体，更改帐户页面以传递密钥类型](#webformsupdate2)
- [对于带更新 3 的 Web 窗体，更改帐户页面以传递密钥类型](#webformsupdate3)
- [运行应用程序](#run)
- [其他资源](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>更改标识用户类中的键的类型

在 ASP.NET Web 应用程序模板创建的项目中，指定 ApplicationUser 类将用于用户帐户的密钥的一个整数。 在 IdentityModels.cs，更改 ApplicationUser 类继承的类型为 IdentityUser **int**为 TKey 泛型参数。 你还传递的三个自定义类，该类尚未实现的名称。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

您已更改的键的类型，但默认情况下，应用程序的其余部分仍假定密钥是一个字符串。 你必须显式指示假定字符串的代码中的键的类型。

在**ApplicationUser**类中，更改**GenerateUserIdentityAsync**方法以包括 int，如下面的突出显示代码中所示。 此更改不需要对于具有 Update 3 模板的 Web 窗体项目。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>添加使用键类型的自定义的标识类

其他标识类，如 IdentityUserRole、 IdentityUserClaim、 IdentityUserLogin、 IdentityRole、 UserStore、 RoleStore，要仍设置为使用字符串密钥。 创建指定键的一个整数这些类的新的版本。 不需要提供这些类中的多实现代码，你主要只通过以下方式设置 int 作为键。

将以下类添加到你 IdentityModels.cs 文件。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>更改的上下文类和用户管理器使用的键类型

在 IdentityModels.cs，更改的定义**ApplicationDbContext**类以使用新自定义类和**int**的键，如突出显示的代码中所示。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

ThrowIfV1Schema 参数不再在构造函数中有效。 更改构造函数，因此未通过 ThrowIfV1Schema 值。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

打开 IdentityConfig.cs，并将更改**ApplicationUserManger**类以使用你的新用户存储保留数据的类和**int**键。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

在 Update 3 模板中，你必须更改 ApplicationSignInManager 类。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>要使用的键类型的更改启动配置

在 Startup.Auth.cs，替换 OnValidateIdentity 代码，则为以下突出显示部分。 请注意 getUserIdCallback 定义中，将字符串值分析为整数。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

如果你的项目无法识别的通用实现**GetUserId**方法，你可能需要更新到版本 2.1 ASP.NET 标识 NuGet 包

您已经更改了大量使用 ASP.NET 标识的基础结构类。 如果你尝试编译该项目，你将注意到了很多错误。 幸运的是，剩余的错误是所有类似。 标识类需要整数的键，但需要的控制器 （或 Web 窗体） 传递一个字符串值。 在每种情况下，你需要通过调用从一个字符串和整数转换**GetUserId&lt;int&gt;**。 你可以通过编译错误列表处理，或按照下面的更改。

剩余的更改取决于你要创建并且已安装在 Visual Studio 中的哪种更新的项目的类型。 你可以直接转到通过以下链接的相关部分

- [对于 MVC Update 2 中，更改 AccountController 中传递的键类型](#mvcupdate2)
- [对于 MVC Update 3 中，更改 AccountController 并 ManageController 传递的键类型](#mvcupdate3)
- [对于带 Update 2 的 Web 窗体，更改帐户页面以传递密钥类型](#webformsupdate2)
- [对于带更新 3 的 Web 窗体，更改帐户页面以传递密钥类型](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>对于 MVC Update 2 中，更改 AccountController 中传递的键类型

打开 AccountController.cs 文件。 你需要更改以下方法。

**ConfirmEmail**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**取消关联**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

你现在可以[运行该应用程序](#run)并注册新的用户。

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>对于 MVC Update 3 中，更改 AccountController 并 ManageController 传递的键类型

打开 AccountController.cs 文件。 你需要更改以下方法。

**ConfirmEmail**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

打开 ManageController.cs 文件。 你需要更改以下方法。

**索引**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

你现在可以[运行该应用程序](#run)并注册新的用户。

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>对于带 Update 2 的 Web 窗体，更改帐户页面以传递密钥类型

对于带 Update 2 的 Web 窗体，你需要更改的以下页面。

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

你现在可以[运行该应用程序](#run)并注册新的用户。

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>对于带更新 3 的 Web 窗体，更改帐户页面以传递密钥类型

对于带更新 3 的 Web 窗体，你需要更改的以下页面。

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>运行应用程序

你已完成所有到默认 Web 应用程序模板所需的更改。 运行应用程序并注册新的用户。 注册用户之后，你将注意到 AspNetUsers 表具有是一个整数的 Id 列。

![新的主密钥](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

如果你之前已创建 ASP.NET 标识表具有不同的主键值，你需要进行一些附加更改。 如果可能，只需删除现有数据库。 运行 web 应用程序以及添加新用户时，将与正确设计重新创建该数据库。 如果删除不可能，运行代码优先迁移来更改表。 但是，新整数主键将不会将设置为在数据库中的 SQL 标识属性。 作为标识，必须手动设置 Id 列。

<a id="other"></a>
## <a name="other-resources"></a>其他资源

- [自定义存储提供程序 ASP.NET 标识概述](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [从 SQL 成员资格的现有网站迁移到 ASP.NET 标识](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [将通用提供程序的数据迁移的成员身份和到 ASP.NET 标识的用户配置文件](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [示例应用程序](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)具有已更改的主关键字
