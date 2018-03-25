---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: "ASP.NET 标识的自定义的存储提供程序的概述 |Microsoft 文档"
author: tfitzmac
description: "ASP.NET 标识是一种可扩展系统，以便您可以创建自己的存储提供程序并将其插入你的应用程序而无需重新使用应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: bbc1f6ef291eddd7488531943b146bb67ae7ee02
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>自定义存储提供程序 ASP.NET 标识概述
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET 标识是一种可扩展系统，以便您可以创建自己的存储提供程序并将其插入你的应用程序而无需重新使用该应用程序。 本主题介绍如何创建 ASP.NET 标识的自定义的存储提供程序。 它涵盖了创建你自己的存储提供程序的重要概念，但它不是实现自定义存储提供程序的分步操作实例。
> 
> 实现自定义存储提供程序的示例，请参阅[实现自定义 MySQL ASP.NET 标识存储提供程序](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)。
> 
> 本主题已更新为使用 ASP.NET 标识 2.0。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Visual Studio 2013 Update 2
> - ASP.NET 标识 2


## <a name="introduction"></a>介绍

默认情况下，ASP.NET 标识系统将用户信息存储在 SQL Server 数据库中，并使用 Entity Framework Code First 创建数据库。 对于许多应用程序，此方法也适用。 但是，你可能希望使用不同类型的持久性机制，例如 Azure 表存储，或者你可能已具有与默认实现与非常不同的结构的数据库表。 在任一情况下，你可以为你的存储机制编写自定义提供程序并将该提供程序插入你的应用程序。

默认情况下，在 Visual Studio 2013 模板中的许多情况下，包含 ASP.NET Identity。 你可以更新到通过 ASP.NET 标识[Microsoft AspNet 标识 EntityFramework NuGet 包](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)。

本主题包括以下部分：

- [了解的体系结构](#architecture)
- [了解存储的数据](#data)
- [创建数据访问层](#dal)
- [自定义用户类](#user)
- [自定义用户存储区](#userstore)
- [自定义角色类](#role)
- [自定义角色存储](#rolestore)
- [重新配置应用程序以使用新存储提供程序](#reconfigure)
- [自定义的存储提供程序的其他实现](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>了解的体系结构

ASP.NET 标识包含类称为管理器和存储区。 管理器是高级类用于执行操作，如 ASP.NET 标识系统中创建用户，应用程序开发人员。 存储是用于指定如何保持实体，如用户和角色，较低级别类。 存储紧密结合持久性机制，但管理器将会分离从存储区这意味着你可以将持久性机制，而不会中断整个应用程序。

下图显示了 web 应用程序如何与管理器交互并存储进行交互的数据访问层。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

若要创建的自定义的存储提供程序 ASP.NET 标识，必须使用此数据访问层中创建数据源、 数据访问层和交互的存储类。 你可以继续使用相同的管理器 Api 来执行数据操作，对该用户，但现在，数据保存到不同的存储系统。

不需要自定义管理器类，因为创建 UserManager 或 RoleManager 的新实例时提供的用户类的类型和作为参数传递存储类的实例。 此方法使您可以插入到现有结构的您自定义的类。 你将了解如何使用您的部分中的自定义的存储类中实例化 UserManager 并 RoleManager[重新配置应用程序以使用新存储提供程序](#reconfigure)。

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>了解存储的数据

若要实现自定义的存储提供程序，你必须了解 ASP.NET 标识，与所使用的数据类型，并确定哪些功能是与你的应用程序。

| 数据 | 描述 |
| --- | --- |
| 用户 | 注册用户的网站的用户。 包括用户 Id 和用户名称。 如果使用特定于你的站点的凭据登录的用户可能会包括经过哈希处理的密码 （而不是使用凭据从 Facebook 之类的外部站点），和安全戳，以指示是否任何内容已更改的用户凭据。 可能还包含电子邮件地址、 电话号码，是否启用了双重身份验证，登录名，失败的当前数量和帐户已被锁定。 |
| 用户声明 | 一组语句 （或声明） 有关的用户的表示用户的标识。 可以启用的用户的标识不是可以通过角色实现更大的表达式。 |
| 用户登录名 | 有关外部身份验证提供程序 （如 Facebook) 的信息在用户日志记录时使用。 |
| 角色 | 你的站点的授权组。 包括 （如"Admin"或"Employee"） 的角色 Id 和角色名称。 |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>创建数据访问层

本主题假定你熟悉要使用的持久性机制以及如何创建该机制的实体。 本主题不提供有关如何创建存储库或数据访问类; 详细信息相反，它提供有关的设计决策的一些建议，你需要进行处理 ASP.NET 标识时。

有很多自由设计的自定义的存储库时的存储提供程序。 只需创建想要使用它在你的应用程序的功能的存储库。 例如，如果不使用你的应用程序中的角色，你不必创建角色或用户角色的存储。 你的技术和现有基础结构可能需要的 ASP.NET Identity 的默认实现不同的结构。 在你的数据访问层，你提供逻辑，以使用您的存储库的结构。

有关适用于 ASP.NET 标识 2.0 的数据存储库 MySQL 而实现，请参阅[MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql)。

数据访问层，可以提供用于将数据从 ASP.NET 标识保存到您的数据源的逻辑。 自定义的存储提供程序的数据访问层可能包括以下类用于存储用户和角色信息。

| 类 | 描述 | 示例 |
| --- | --- | --- |
| 上下文 | 封装用于连接到持久性机制和执行查询的信息。 此类是你的数据访问层的核心。 其他数据类将需要此类来执行其操作的实例。 你还将初始化你的存储类与此类的实例。 | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| 用户存储 | 存储和检索用户信息 （如用户名称和密码哈希）。 | [数据库 (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| 角色存储 | 存储和检索角色信息 （如角色名称中）。 | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| UserClaims 存储 | 存储和检索用户声明信息 （如的声明类型和值）。 | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| UserLogins 存储 | 存储和检索用户登录信息 （例如外部身份验证提供程序）。 | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| UserRole 存储 | 存储和检索用户分配给哪些角色。 | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

同样，你只需实现你要在你的应用程序中使用的类。

在数据访问类中，提供的代码以执行特定的永久性机制的数据操作。 例如中的 MySQL 实现中，数据库类包含用户数据库表中插入一条新记录的方法。 命名的变量`_database`MySQLDatabase 类的实例。

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

在创建后你的数据访问类，必须创建的特定方法调用数据访问层中的存储类。

<a id="user"></a>
## <a name="customize-the-user-class"></a>自定义用户类

在实现自己的存储提供程序时，你必须创建一个用户类，这等效于[IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx)类[Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)命名空间：

下图显示了你必须创建 IdentityUser 类和要在此类中实现的接口。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx)接口定义 UserManager 尝试调用时执行请求的操作的属性。 该接口包含两个属性的 Id 和用户名。 [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx)接口，可指定的密钥，用户通过泛型类型**TKey**参数。 Id 属性的类型匹配 TKey 参数的值。

标识框架还提供了[IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx)接口 （如果不使用泛型参数） 时，你想要用于键的字符串值。

IdentityUser 类实现 IUser，并包含任何其他属性或构造函数为您的网站上的用户。 下面的示例演示用于密钥使用整数 IdentityUser 类。 Id 字段设置为**int**以匹配的泛型参数的值。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 有关完整实现，请参阅[IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs)。 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>自定义用户存储区

你还创建一个 UserStore 类，提供用户的所有数据操作的方法。 此类是等效于[UserStore&lt;热熔器&gt;](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx)类[Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)命名空间。 在 UserStore 类中，你实现[IUserStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx)和任何可选接口。 你选择的可选接口，以实现基于你希望应用程序中提供的功能。

下图显示您必须创建 UserStore 类和相关的接口。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Visual Studio 中的默认项目模板包含假定已在用户存储区将许多可选接口实现的代码。 如果你正在使用自定义的用户存储使用默认模板，你必须在用户存储区实现可选接口，或更改不再调用方法调用中不具有实现的接口的模板代码。

 下面的示例演示一个简单的用户存储类。 **热熔器**泛型参数采用你的用户类的通常是你定义的 IdentityUser 类的类型。 **TKey**泛型参数使用用户密钥的类型。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 在此示例中，采用一个参数的构造函数名为*数据库*ExampleDatabase 是仅举例说明了如何在数据访问类传递的类型。 例如，在 MySQL 实现中，此构造函数采用类型 MySQLDatabase 的参数。 

在你 UserStore 类中，你可以使用你创建用于执行操作的数据访问类。 例如，在 MySQL 实现中，UserStore 类具有 CreateAsync 方法的数据库实例用于插入新记录。 **插入**方法**数据库**对象是上一节中所示的相同方法。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>要实现自定义用户存储时的接口

下一步的映像显示在每个接口中定义的功能的更多详细信息。 从 IUserStore 继承的所有可选的接口。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
 [IUserStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx)接口是在用户存储区必须实现的唯一接口。 它定义了用于创建、 更新、 删除和检索用户的方法。
- **IUserClaimStore**  
 [IUserClaimStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx)接口定义的方法必须在你的用户存储，从而启用用户声明实现。 它包含方法或添加、 删除和检索用户声明。
- **IUserLoginStore**  
 [IUserLoginStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx)定义的方法必须在你的用户存储区，以启用外部身份验证提供程序实现。 它包含用于添加、 删除和检索用户登录名和用于检索用户的登录信息基于方法的方法。
- **IUserRoleStore**  
 [IUserRoleStore&lt;，热熔器&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx)接口定义的方法必须在你的用户存储，从而将用户映射到角色实现。 它包含要添加、 删除和检索用户的角色和方法来检查是否将用户分配到角色的方法。
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx)接口定义必须在你的用户存储区，以保留实现的方法进行密码哈希处理。 它包含用于获取和设置工作经过哈希处理的密码，以及用于指示用户是否已设置密码的方法的方法。
- **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx)接口定义必须在你的用户存储区，用于指示是否已更改用户的帐户信息安全戳实现的方法. 当用户更改密码，或添加或删除登录名，将更新此 stamp。 它包含用于获取和设置安全戳的方法。
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx)接口定义必须实现到实现两个因素身份验证的方法。 它包含用于获取和设置是否为用户启用双重身份验证的方法。
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx)接口定义必须实现以存储用户电话号码的方法。 它包含用于获取和设置的电话号码和是否确认的电话号码的方法。
- **IUserEmailStore**  
 [IUserEmailStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx)接口定义必须实现以存储用户电子邮件地址的方法。 它包含用于获取和设置的电子邮件地址和是否确认电子邮件的方法。
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx)接口定义为存储帐户的锁定信息而必须实现的方法。 它包含用于获取当前的失败的访问尝试次数、 获取和设置是否可以锁定该帐户，获取和设置在锁住结束日期，递增的数字失败的尝试，和重置失败尝试次数的方法。
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;热熔器、 TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx)接口定义必须实现以提供可查询的用户存储的成员。 它包含一个属性，保存查询的用户。

 在你的应用程序; 实现所需接口例如，IUserClaimStore、 IUserLoginStore、 IUserRoleStore、 IUserPasswordStore 和 IUserSecurityStampStore 接口如下所示。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

有关完整的实现 （包括所有接口），请参阅[UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs)。

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim、 IdentityUserLogin 和 IdentityUserRole

Microsoft.AspNet.Identity.EntityFramework 命名空间包含的实现[IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx)， [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)，和[IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx)类。 如果你正使用这些功能，你可能想要创建你自己版本的这些类并定义你的应用程序的属性。 但是，有时它会更加高效，若要执行基本操作 （如添加或删除用户的声明） 时不将这些实体加载到内存。 相反后, 端存储类可以执行这些操作直接在数据源上。 例如，UserStore.GetClaimsAsync() 方法可以调用 userClaimTable.FindByUserId(user.Id) 方法来执行查询上直接表并且返回的声明列表。

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>自定义角色类

在实现自己的存储提供程序时，你必须创建一个角色类，这等效于[IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx)类[Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)命名空间：

下图显示了你必须创建 IdentityRole 类和要在此类中实现的接口。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx)接口定义 RoleManager 尝试调用时执行请求的操作的属性。 该接口包含两个属性的 Id 和名称。 [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx)接口，可指定的类型的密钥，通过泛型角色**TKey**参数。 Id 属性的类型匹配 TKey 参数的值。

标识框架还提供了[IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx)接口 （如果不使用泛型参数） 时，你想要用于键的字符串值。

下面的示例演示用于密钥使用整数 IdentityRole 类。 Id 字段设置为 int，以匹配的泛型参数的值。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 有关完整实现，请参阅[IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs)。 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>自定义角色存储

你还创建一个 RoleStore 类，提供对角色的所有数据操作的方法。 此类是等效于[RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) Microsoft.ASP.NET.Identity.EntityFramework 命名空间中的类。 在 RoleStore 类中，你实现[IRoleStore&lt;TRole、 TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx)和 （可选） [IQueryableRoleStore&lt;TRole、 TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx)接口。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

下面的示例演示角色存储类。 TRole 泛型参数采用你的角色类的通常是你定义的 IdentityRole 类的类型。 TKey 泛型参数的取值角色键的类型。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
 [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx)接口定义角色存储类中实现的方法。 它包含用于创建、 更新、 删除和检索角色的方法。
- **RoleStore&lt;TRole&gt;**  
 若要自定义 RoleStore，请创建实现 IRoleStore 接口的类。 只需实现此类，如果想要使用你的系统上的角色。 采用名为的参数的构造函数*数据库*ExampleDatabase 是仅举例说明了如何在数据访问类传递的类型。 例如，在 MySQL 实现中，此构造函数采用类型 MySQLDatabase 的参数。  
  
 有关完整实现，请参阅[RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) 。

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>重新配置应用程序以使用新存储提供程序

你已实现新的存储提供程序。 现在，你必须配置应用程序使用此存储提供程序。 如果默认的存储提供程序已包含在你的项目，你必须删除默认的提供程序，并将其替换为你提供程序。

### <a name="replace-default-storage-provider-in-mvc-project"></a>将 MVC 项目中的默认存储提供程序

1. 在**管理 NuGet 包**窗口中，卸载**Microsoft ASP.NET Identity EntityFramework**包。 你可以通过 Identity.EntityFramework 搜索已安装的包中找到此包。  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png)系统将要求你还希望卸载实体框架。 如果你不需要它在你的应用程序的其他部分，则可以卸载它。
2. 在 IdentityModels.cs 文件中的 Models 文件夹中，删除或注释掉**ApplicationUser**和**ApplicationDbContext**类。 在 MVC 应用程序，你可以删除整个 IdentityModels.cs 文件。 在 Web 窗体应用程序中，删除两个类，但请确保你保留也位于 IdentityModels.cs 文件中的帮助器类。
3. 如果你的存储提供程序驻留在一个单独的项目，请在 web 应用程序中添加对它的引用。
4. 将对所有引用`using Microsoft.AspNet.Identity.EntityFramework;`使用的命名空间的存储提供程序的语句。
5. 在**Startup.Auth.cs**类中，更改**ConfigureAuth**方法来使用合适的上下文中的单个实例。 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. 在应用程序\_开始文件夹中，打开**IdentityConfig.cs**。 在 ApplicationUserManager 类中，更改**创建**方法以返回使用自定义的用户存储区的用户管理器。 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. 将对所有引用**ApplicationUser**与**IdentityUser**。
8. 默认项目包括在 IUser 接口中; 中未定义用户类中的某些成员例如，电子邮件、 PasswordHash 和 GenerateUserIdentityAsync。 如果您的用户类不具有这些成员，你必须实现它们，或更改使用这些成员的代码。
9. 如果你已创建的 RoleManager 任何实例，则更改该代码以使用新 RoleStore 类。  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. 默认项目专为具有键的字符串值的用户类。 如果您的用户类具有不同类型 （如整数） 的密钥，则必须更改项目以使用你的类型。 请参阅[更改为在 ASP.NET Identity 中的用户的主要密钥](change-primary-key-for-users-in-aspnet-identity.md)。
11. 如果需要将连接字符串添加到 Web.config 文件中。

<a id="other"></a>
## <a name="other-resources"></a>其他资源

- 博客：[实现 ASP.NET 标识](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- 教程和 GIT 的代码： [Simple.Data Asp.Net 标识提供程序](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- 教程：[设置了基本的标识帐户，并指向外部 DB](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)。 通过[ @xivSolutions ](https://twitter.com/xivSolutions)。
- 教程[： 实现自定义 MySQL ASP.NET 标识存储提供程序](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent 实体](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/)通过[SoftFluent](http://www.softfluent.com/)
- [Azure 表存储](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/)James 美丽通过。
- Azure 表存储： [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage)通过[ @stuartleeks ](https://twitter.com/stuartleeks)。
- [CouchDB / 通过 Daniel Wertheim Cloudant。](https://github.com/danielwertheim/mycouch.aspnet.identity)
- 弹性搜索[h： 弹性标识](https://github.com/bmbsqd/elastic-identity)通过 Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) Jonathan Sheely Jonathan Sheely 通过。
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) Antônio Milesi Bastos 通过。
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0)通过[ @tourismgeek ](https://twitter.com/tourismgeek)。
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity)通过[ILMServices](http://www.ilmservice.com/)。
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- T4 模板生成 EF 的代码"第一个数据库"的用户存储区： [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
