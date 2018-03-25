---
title: "ASP.NET 核心标识的自定义的存储提供程序"
author: ardalis
description: "了解如何配置 ASP.NET 核心标识的自定义存储提供程序。"
manager: wpickett
ms.author: riande
ms.date: 05/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 559c5c58b416912b1caef1964ff8d7532bd98439
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>ASP.NET 核心标识的自定义的存储提供程序

作者：[Steve Smith](https://ardalis.com/)

ASP.NET 核心标识是一种可扩展系统，可用于创建自定义存储提供程序并将其连接到你的应用。 本主题介绍如何创建 ASP.NET 核心标识的自定义的存储提供程序。 它介绍如何创建你自己的存储提供程序的重要概念，但不的分步演练。

[查看或下载 GitHub 中的示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample)。

## <a name="introduction"></a>介绍

默认情况下，ASP.NET 核心标识系统中使用实体框架核心的 SQL Server 数据库存储用户信息。 对于许多应用程序，此方法也适用。 但是，你可能希望使用不同的持久性机制或数据架构。 例如:

* 你使用[Azure 表存储](https://docs.microsoft.com/azure/storage/)或其他数据存储。
* 数据库表具有不同的结构。 
* 你可能想要使用不同的数据访问方法，如[Dapper](https://github.com/StackExchange/Dapper)。 

在每个这种情况下，你可以为你的存储机制编写自定义提供程序并将该提供程序插入您的应用程序。

ASP.NET 核心标识包含在 Visual Studio 中使用"单个用户帐户"选项的项目模板中。

在使用.NET 核心 CLI 时，添加`-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>ASP.NET 核心标识体系结构

ASP.NET 核心标识包含类称为管理器和存储区。 *管理器*是高级类应用程序开发人员用于执行操作，如创建标识用户。 *存储*是用于指定如何保持实体，如用户和角色，较低级别类。 存储按照[存储库模式](http://deviq.com/repository-pattern/)和紧密耦合持久性的机制。 管理器是相互脱耦存储区，这意味着你可以将持久性机制，而无需更改应用程序代码 （除外配置）。

下图显示的 web 应用程序的交互方式具有管理器中，当与数据访问层的存储区交互时。

![ASP.NET Core 应用适用于管理器 （例如，UserManager、 RoleManager）。 管理器使用存储 (例如，UserStore) 用于传达与数据源使用如实体框架核心库。](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

若要创建自定义的存储提供程序，请使用此数据访问层 （绿色和灰色框在上图中） 创建数据源、 数据访问层和交互的存储类。 你不需要自定义管理器或与它们 （蓝色上面的框） 进行交互的应用程序代码。

创建的新实例时`UserManager`或`RoleManager`提供用户类的类型和作为参数传递存储类的实例。 此方法使您可以插入到 ASP.NET 核心的自定义的类。 

[重新配置应用程序以使用新存储提供程序](#reconfigure-app-to-use-new-storage-provider)演示如何实例化`UserManager`和`RoleManager`使用自定义存储。

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET 核心标识将存储的数据类型

[ASP.NET 核心标识](https://github.com/aspnet/identity)数据类型进行详细介绍下列各节：

### <a name="users"></a>用户

注册用户的网站的用户。 [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)可能扩展类型，或将其用作自定义类型的示例。 你不必继承来实现自定义标识的存储解决方案的特定类型。

### <a name="user-claims"></a>用户声明

一组语句 (或[声明](https://docs.microsoft.com//dotnet/api/system.security.claims.claim)) 有关的用户的表示用户的标识。 可以启用的用户的标识不是可以通过角色实现更大的表达式。

### <a name="user-logins"></a>用户登录名

有关外部身份验证提供程序 （如 Facebook 或 Microsoft 帐户） 的信息在用户日志记录时使用。 [示例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>角色

你的站点的授权组。 包括 （如"Admin"或"Employee"） 的角色 Id 和角色名称。 [示例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>数据访问层

本主题假定你熟悉要使用的持久性机制以及如何创建该机制的实体。 本主题不提供有关如何创建存储库或数据访问类; 详细信息使用 ASP.NET 核心标识时，它提供了有关设计决策的一些建议。

为自定义的存储提供程序设计的数据访问层时，你有大量的自由。 只需创建想要使用它在你的应用程序的功能的持久性机制。 例如，如果不使用你的应用程序中的角色，你不必创建角色或用户角色关联的存储。 你的技术和现有基础结构可能需要的 ASP.NET 核心标识的默认实现不同的结构。 你的数据访问层，可以提供要使用的存储实现结构的逻辑。

数据访问层提供逻辑来将数据从 ASP.NET 核心标识保存到数据源。 自定义的存储提供程序的数据访问层可能包括以下类用于存储用户和角色信息。

### <a name="context-class"></a>Context 类

封装用于连接到持久性机制和执行查询的信息。 多个数据类需要通过依赖关系注入通常提供此类的实例。 [示例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)。

### <a name="user-storage"></a>用户存储

存储和检索用户信息 （如用户名称和密码哈希）。 [示例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>角色存储

存储和检索角色信息 （如角色名称中）。 [示例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>UserClaims 存储

存储和检索用户声明信息 （如的声明类型和值）。 [示例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>UserLogins 存储

存储和检索用户登录信息 （例如外部身份验证提供程序）。 [示例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>UserRole 存储

存储和检索哪些角色分配给哪些用户。 [示例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**提示：**仅实现你要在应用程序中使用的类。

在数据访问类中，提供的代码来执行持久性机制的数据操作。 例如，在自定义提供程序，你可能具有以下代码以创建中的新用户*存储*类：

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

实现逻辑用于创建用户处于``_usersTable.CreateAsync``方法，如下所示。

## <a name="customize-the-user-class"></a>自定义用户类

时实现存储提供程序，创建一个用户类，这等效于[`IdentityUser`类](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)。

您的用户类必须包含至少`Id`和`UserName`属性。

`IdentityUser`类定义的属性，``UserManager``调用时执行请求的操作。 默认类型`Id`属性是一个字符串，但可以继承`IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>`并指定不同的类型。 框架需要要处理的数据类型转换的存储实现。

## <a name="customize-the-user-store"></a>自定义用户存储区

创建`UserStore`提供用户的所有数据操作的方法的类。 此类是等效于[UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1)类。 在你`UserStore`类中，实现`IUserStore<TUser>`和所需的可选接口。 你选择的可选接口，以实现基于你的应用程序中提供的功能。

### <a name="optional-interfaces"></a>可选接口

- IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1
- IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1
- IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1
- IUserSecurityStampStore <!-- make these all links and remove / -->
- IUserEmailStore
- IPhoneNumberStore
- IQueryableUserStore
- IUserLoginStore
- IUserTwoFactorStore
- IUserLockoutStore

可选接口继承自`IUserStore`。 你可以看到存储部分实现的示例用户[此处](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)。

在`UserStore`类，你将使用你创建用于执行操作的数据访问类。 它们将传递中使用依赖关系注入。 例如，在 SQL Server 使用 Dapper 实现，`UserStore`类具有`CreateAsync`使用的实例方法`DapperUsersTable`以插入新记录：

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>要实现自定义用户存储时的接口

- **IUserStore**  
 [IUserStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1)接口是在用户存储中必须实现的唯一接口。 它定义了用于创建、 更新、 删除和检索用户的方法。
- **IUserClaimStore**  
 [IUserClaimStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1)接口定义实现以启用用户声明的方法。 它包含用于添加、 删除和检索用户声明的方法。
- **IUserLoginStore**  
 [IUserLoginStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1)定义实现以启用外部身份验证提供程序的方法。 它包含用于添加、 删除和检索用户登录名和用于检索用户的登录信息基于方法的方法。
- **IUserRoleStore**  
 [IUserRoleStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1)接口定义实现的用户映射到角色的方法。 它包含要添加、 删除和检索用户的角色和方法来检查是否将用户分配到角色的方法。
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)接口定义实现以保持经过哈希处理的密码的方法。 它包含用于获取和设置工作经过哈希处理的密码，以及用于指示用户是否已设置密码的方法的方法。
- **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)接口定义的方法实现用于安全戳，该值指示是否已更改用户的帐户信息。 当用户更改密码，或添加或删除登录名，将更新此 stamp。 它包含用于获取和设置安全戳的方法。
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)接口定义实现以支持两个因素身份验证的方法。 它包含用于获取和设置是否为用户启用双重身份验证的方法。
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)接口定义实现以存储用户电话号码的方法。 它包含用于获取和设置的电话号码和是否确认的电话号码的方法。
- **IUserEmailStore**  
 [IUserEmailStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1)接口定义实现以存储用户电子邮件地址的方法。 它包含用于获取和设置的电子邮件地址和是否确认电子邮件的方法。
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)接口定义为存储帐户的锁定信息而实现的方法。 它包含用于跟踪未成功的访问和锁定方法。
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;热熔器&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)接口定义成员实现此方法可以提供可查询的用户存储区。

应用程序中实现所需的接口。 例如:

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim、 IdentityUserLogin 和 IdentityUserRole

``Microsoft.AspNet.Identity.EntityFramework``命名空间包含的实现[IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1)， [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)，和[IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1)类。 如果你正使用这些功能，你可能想要创建你自己版本的这些类并定义你的应用程序的属性。 但是，有时它会更加高效，若要执行基本操作 （如添加或删除用户的声明） 时不将这些实体加载到内存。 相反后, 端存储类可以执行这些操作直接在数据源上。 例如，``UserStore.GetClaimsAsync``方法可以调用``userClaimTable.FindByUserId(user.Id)``方法上执行查询，它直接表并返回的声明列表。

## <a name="customize-the-role-class"></a>自定义角色类

在实现角色存储提供程序时，你可以创建自定义角色类型。 它不需要实现特定接口，但是它必须具有`Id`，其中包含通常`Name`属性。

以下是一个示例角色类：

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>自定义角色存储

你可以创建``RoleStore``提供在角色上的所有数据操作的方法的类。 此类是等效于[RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)类。 在`RoleStore`类时，实现``IRoleStore<TRole>``和 （可选）``IQueryableRoleStore<TRole>``接口。

- **IRoleStore&lt;TRole&gt;**  
 [IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1)接口定义角色存储类中实现的方法。 它包含用于创建、 更新、 删除和检索角色的方法。
- **RoleStore&lt;TRole&gt;**  
 若要自定义`RoleStore`，创建一个类以实现`IRoleStore`接口。 

## <a name="reconfigure-app-to-use-new-storage-provider"></a>重新配置应用程序以使用新存储提供程序

一旦你已实现的存储提供程序，你将配置您的应用程序使用它。 如果你的应用程序使用默认的提供程序，可将其替换你自定义提供程序上。

1. 删除`Microsoft.AspNetCore.EntityFramework.Identity`NuGet 包。
1. 如果存储提供程序驻留在单独的项目或包，添加对它的引用。
1. 将对所有引用`Microsoft.AspNetCore.EntityFramework.Identity`使用的命名空间的存储提供程序的语句。
1. 在``ConfigureServices``方法，更改`AddIdentity`要使用自定义类型的方法。 你可以创建你自己的扩展方法实现此目的。 请参阅[IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs)有关示例。
1. 如果你使用的角色，更新`RoleManager`用于你`RoleStore`类。
1. 到你的应用的配置更新的连接字符串和凭据。

示例:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>参考资料

- [ASP.NET 标识的自定义的存储提供程序](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET 核心标识](https://github.com/aspnet/identity)-此存储库包含指向社区维护存储提供程序。
