---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: "从 SQL 成员资格的现有网站迁移到 ASP.NET 标识 |Microsoft 文档"
author: Rick-Anderson
description: "本教程说明了将迁移用户和角色数据创建使用 SQL 成员资格为新的 ASP.NET 标识 s 的现有 web 应用程序的步骤..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 3638c6779a0fcedaaa49623126b28ecf09a4954f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>从 SQL 成员资格的现有网站迁移到 ASP.NET 标识
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)， [Suhas Joshi](https://github.com/suhasj)

> 本教程说明了将迁移用户和角色数据到新的 ASP.NET 标识系统中使用 SQL 成员身份创建的现有 web 应用程序的步骤。 此方法涉及将现有的数据库架构更改为一个所需的 ASP.NET 标识和挂钩陷入到它的新旧/类。 迁移你的数据库后，采用此方法后，将可轻松处理标识的未来更新。


对于本教程中，我们将采用使用 Visual Studio 2010 创建用户和角色的数据创建的 web 应用程序模板 （Web 窗体）。 然后，我们将使用 SQL 脚本以将现有数据库迁移到所需的标识系统的表。 接下来，我们将安装所需的 NuGet 包并添加新帐户管理页用于成员资格管理的标识系统使用它们。 为迁移的测试，创建使用 SQL 成员身份的用户应该能够登录和新用户应该能够注册。 你可以找到完整的示例[此处](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/)。 另请参阅[从 ASP.NET 成员资格迁移到 ASP.NET 标识](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html)。

## <a name="getting-started"></a>入门

### <a name="creating-an-application-with-sql-membership"></a>使用 SQL 成员身份创建应用程序

1. 我们需要以开头的现有应用程序使用 SQL 成员资格，并且有用户和角色的数据。 本文中，为了让我们在 Visual Studio 2010 中创建 web 应用程序。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. 使用 ASP.NET 配置工具中，创建 2 个用户： **oldAdminUser**和**oldUser。**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. 创建一个名为管理员角色并为该角色中的用户添加 oldAdminUser。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. 使用 Default.aspx 创建站点的管理部分。 在 web.config 文件以启用访问仅向管理员角色中的用户设置的授权标记。 详细信息可在此处找到[https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. 查看数据库在服务器资源管理器，以了解 SQL 成员资格系统所创建的表。 用户登录名数据存储在 aspnet\_用户和 aspnet\_成员资格表，而角色数据存储在 aspnet\_Roles 表。 用户所在哪些角色存储在 aspnet 信息\_UsersInRoles 表。 对于基本成员资格管理它已足够移植到 ASP.NET 标识系统上述表中的信息。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>迁移到 Visual Studio 2013

1. 安装 Visual Studio Express 2013 for Web 或 Visual Studio 2013 沿[最新的更新](https://www.microsoft.com/download/details.aspx?id=44921)。
2. 在你安装的 Visual Studio 版本中打开上述项目。 如果在计算机上未安装 SQL Server Express，当你打开项目，因为该连接字符串使用 SQL Express 时被显示一条提示。 你可以选择要安装 SQL Express 或作为变通解决更改 LocalDb 的连接字符串。 此文章我们会将其更改为 LocalDb。
3. 打开 web.config 并更改中的连接字符串。到 (LocalDb) v11.0 SQLExpess。 删除用户实例 = true 从连接字符串。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. 打开服务器资源管理器并验证可以观察的表架构和数据。
5. ASP.NET 标识系统适用于 4.5 或更高版本的 framework 版本。 重定目标为 4.5 或更高版本的应用程序。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    生成项目，以验证有任何错误。

### <a name="installing-the-nuget-packages"></a>正在安装 Nuget 包

1. 在解决方案资源管理器，右键单击项目&gt;**管理 NuGet 包**。 在搜索框中，输入"Asp.net 标识"。 在结果列表中选择的包，然后单击安装。 通过单击"我接受"按钮接受许可协议。 请注意，此包将安装依赖项包： EntityFramework 和 Microsoft ASP.NET Identity Core。 同样安装以下包 （如果你不想要启用 OAuth 登录，请跳过最后 4 OWIN 包）：

    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>将数据库迁移到新的标识系统

下一步是将现有数据库迁移到由 ASP.NET 标识系统所需的架构。 若要实现此我们运行 SQL 脚本拥有一组命令以创建新表并将现有的用户信息迁移到新表。 找不到脚本文件[此处](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql)。

此脚本文件是特定于此示例。 如果创建的表的架构使用 SQL 成员资格自定义或修改脚本需要相应地更改。

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>如何生成架构迁移的 SQL 脚本

对于 ASP.NET 标识类的现有用户与数据现成工作，我们需要将数据库架构迁移到 ASP.NET 标识所需的一个。 我们可以通过添加新表并将现有的信息复制到这些表来执行此操作。 默认情况下 ASP.NET 标识使用 EntityFramework 将标识模型类映射到数据库来存储/检索信息。 这些模型类实现核心标识接口定义用户和角色对象。 表和数据库中的列基于这些模型类。 下面定义是标识 v2.1.0 和它们的属性中的 EntityFramework 模型类

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | 字符串 | Id | RoleId | ProviderKey | Id |
| 用户名 | 字符串 | name | UserId | UserId | ClaimType |
| PasswordHash | 字符串 |  |  | LoginProvider | ClaimValue |
| SecurityStamp | 字符串 |  |  |  | 用户\_Id |
| 电子邮件 | 字符串 |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | 字符串 |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

我们需要为每个这些模型的表具有与属性相对应的列。 在中定义类和表之间的映射`OnModelCreating`方法`IdentityDBContext`。 这称为配置的 fluent API 方法和可以找到更多信息[此处](https://msdn.microsoft.com/data/jj591617.aspx)。 类的配置是如下所示

| **类** | **Table** | **主键** | **外键** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | 用户\_Id-&gt;AspnetUsers RoleId-&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | 用户\_Id-&gt;AspnetUsers |

使用此信息中，我们可以创建 SQL 语句以创建新表。 我们可以单独写入每个语句，或生成根据需要使用 EntityFramework PowerShell 命令，然后，我们可以编辑整个脚本。 为此，请在 VS 打开**程序包管理器控制台**从**视图**或**工具**菜单

- 运行命令"Enable-migrations"以启用 EntityFramework 迁移。
- 运行命令"add-migration 初始"，这将创建要在 C# 中创建数据库的初始设置代码 / VB.
- 最后一步是运行"Update-database-脚本"生成 SQL 脚本的命令基于模型类。

此数据库生成脚本可以用作其中我们将会进行其他更改，以添加新列，将数据复制的开始。 这样做的优点是我们生成`_MigrationHistory`EntityFramework 用于修改数据库架构时模型类针对将来版本的标识发布的更改的表。 

SQL 成员资格用户信息具有其他以及标识用户模型类即电子邮件中的属性、 密码尝试次数、 上次登录日期、 上次锁定日期等。这是有用的信息，我们希望它转入标识系统。 这可以通过将附加属性添加到用户模型并将其映射回数据库中表的列。 我们可以通过执行此添加类子类`IdentityUser`模型。 我们可以将属性添加到此自定义类和编辑 SQL 脚本，以创建表时添加相应的列。 此类代码进行了进一步的文章中描述。 创建的 SQL 脚本`AspnetUsers`表后将添加新属性

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

接下来，我们需要将现有信息从 SQL 成员资格数据库复制到新添加的表，标识。 这可以通过 SQL 通过将数据直接从一个表复制到另一个。 若要将数据添加到表中的行，我们使用`INSERT INTO [Table]`构造。 将从另一个表，我们可以使用复制`INSERT INTO`语句连同`SELECT`语句。 若要获取我们需要查询的所有用户信息*aspnet\_用户*和*aspnet\_成员资格*表和复制数据的*AspNetUsers*表。 我们使用`INSERT INTO`和`SELECT`连同`JOIN`和`LEFT OUTER JOIN`语句。 有关查询和表之间复制数据的详细信息，请参阅[这](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx)链接。 此外 AspnetUserLogins 和 AspnetUserClaims 表是空开头因为没有在默认情况下映射到此 SQL 成员资格信息。 复制的唯一信息是用户和角色。 对于前面的步骤中创建的项目，为要将信息复制到用户表的 SQL 查询

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

在上述 SQL 语句中，从每个用户有关的信息*aspnet\_用户*和*aspnet\_成员资格*表复制到的列*AspnetUsers*表。 我们将密码复制时完成此处的唯一修改。 由于 SQL 成员资格中的密码的加密算法使用 PasswordSalt 和 PasswordFormat，我们将此内容复制太以及经过哈希处理密码，以便可以使用来解密密码的标识。 这中进一步说明文章时挂接自定义密码 hasher。 

此脚本文件是特定于此示例。 对于具有其他表的应用程序，开发人员可以遵循类似的方法在用户模型类上添加其他属性，并将它们映射到 AspnetUsers 表中的列。 若要运行脚本，

1. 打开服务器资源管理器。 展开 ApplicationServices 连接，以显示表。 右键单击表节点并选择新建查询选项

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. 在查询窗口中，复制并粘贴来自 Migrations.sql 文件的整个 SQL 脚本。 通过点击执行箭头按钮运行脚本文件。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    刷新服务器资源管理器窗口。 数据库中创建新的五个表。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    下面是 SQL 成员资格表中的信息映射到新的标识系统的方式。

    aspnet\_角色-&gt; AspNetRoles

    asp\_netUsers 和 asp\_netMembership-&gt; AspNetUsers

    aspnet\_UserInRoles-&gt; AspNetUserRoles

    如上面的部分中所述，AspNetUserClaims 和 AspNetUserLogins 表为空。 AspNetUser 表中的鉴别器字段应与匹配的模型类名称，吞吐量被定义为下一步。 PasswordHash 列还采用的形式 ' 加密的密码 | 密码 salt | 密码格式。 这使你可以使用特殊 SQL 成员资格加密逻辑，以便可以重用旧密码。 是本文后面的部分中所述。

### <a name="creating-models-and-membership-pages"></a>创建模型和成员资格页

如前所述，标识功能使用实体框架与默认情况下存储帐户信息的数据库通信。 若要使用表中的现有数据，我们需要创建模型类映射回表和挂钩标识系统中。 作为标识协定的一部分，模型类应实现 Identity.Core dll 中定义的接口，也可以扩展 Microsoft.AspNet.Identity.EntityFramework 中提供这些接口的现有实现。

在我们示例中，AspNetRoles、 AspNetUserClaims、 AspNetLogins 和 AspNetUserRole 表具有相似的标识系统的现有实现的列。 因此，我们可以重复使用现有的类将映射到这些表。 AspNetUser 表具有一些额外的列，可用于存储 SQL 成员资格表中的其他信息。 这可以通过创建模型类来扩展 IdentityUser 的现有实现并添加其他属性映射。

1. 可用来创建模型项目中的文件夹并添加类用户。 类的名称应匹配 AspnetUsers 表的鉴别器列中添加的数据。

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    用户类应扩展中找到的 IdentityUser 类*Microsoft.AspNet.Identity.EntityFramework* dll。 声明映射回 AspNetUser 列中类的属性。 属性 ID、 用户名、 PasswordHash 和 SecurityStamp IdentityUser 中定义，并且省略了。 下面是具有所有属性的用户类的代码

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. 实体框架 DbContext 类需要持久保存回表的模型中的数据并从表来填充模型中检索数据。 *Microsoft.AspNet.Identity.EntityFramework* dll 定义与这些标识表以检索并存储信息交互的 IdentityDbContext 类。 IdentityDbContext&lt;热熔器&gt;采用热熔器类可以是任何扩展 IdentityUser 类的类。

    创建一个新类扩展模型文件夹下，在步骤 1 中创建的 'User' 类传入 IdentityDbContext 的 ApplicationDBContext

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. 完成新的标识系统中的用户管理使用 UserManager&lt;热熔器&gt;中定义的类*Microsoft.AspNet.Identity.EntityFramework* dll。 我们需要创建自定义扩展的类的 UserManager，在步骤 1 中创建的 'User' 类传递。

    在模型文件夹中创建一个新的类扩展 UserManager UserManager&lt;用户&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. 应用程序的用户的密码进行加密和存储在数据库中。 使用 SQL 成员资格中的加密算法是不同的是新的标识系统中。 若要重用旧密码我们需要在为新的用户在标识中使用的加密算法时使用 SQL 成员身份算法的旧用户登录时有选择地解密密码。

    UserManager 类具有一个属性存储实现 IPasswordHasher 接口的类的实例的 PasswordHasher。 使用此密码进行加密/解密用户身份验证事务处理期间。 在步骤 3 中定义的 UserManager 类，创建一个新类 SQLPasswordHasher 和复制下面的代码。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    通过导入 System.Text 和 System.Security.Cryptography 命名空间中解决编译错误。

    EncodePassword 方法对根据默认 SQL 成员资格加密实现的密码进行加密。 这将取自 System.Web dll。 如果旧版应用程序使用的自定义实现则它应反映此处。 我们需要定义两个其他方法*HashPassword*和*VerifyHashedPassword*使用*EncodePassword*方法可以提供的密码执行哈希或验证了纯文本与一个现有数据库中的密码。

    SQL 成员资格系统使用 PasswordHash、 PasswordSalt 和 PasswordFormat 进行哈希在注册或更改其密码时，由用户输入的密码。 在迁移期间所有三个字段存储隔开 AspNetUser 表中的 PasswordHash 列在 | 字符。 当用户登录和密码包含的这些字段时，我们可以使用 SQL 成员资格加密来检查密码，则为否则我们使用的标识系统的默认加密以验证密码。 这种方式旧用户不需要迁移应用程序后更改其密码。
5. 声明 UserManager 类的构造函数，并将此作为 SQLPasswordHasher 传递给构造函数中的属性。

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>创建新的帐户管理页

迁移的下一步是添加将允许用户注册和登录的帐户管理页。 从 SQL 成员资格的旧帐户页使用的新的标识系统不支持的控件。 若要添加新用户管理页，请按照教程在以下链接[https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)从步骤启动为注册到你的应用程序的用户添加 Web 窗体由于我们已经创建该项目并添加 NuGet 包。

我们需要进行一些更改以处理我们在这里有项目的示例。

- Register.aspx.cs 和 Login.aspx.cs 代码隐藏类可以使用该`UserManager`从标识包来创建的用户。 对于此示例使用添加 Models 文件夹中，按照前面所述的步骤 UserManager。
- 使用创建而不是 IdentityUser Register.aspx.cs 和 Login.aspx.cs 代码隐藏类中的用户类。 这挂钩我们自定义用户类中的标识系统。
- 要创建数据库的部件，可以跳过。
- 开发人员需要设置新用户 ApplicationId，以匹配当前的应用程序 id。 这可以通过查询此应用程序 ApplicationId 之前 Register.aspx.cs 类中创建用户对象并将其设置在创建用户之前。 

    示例:

    定义一个方法中 Register.aspx.cs 页后，可以查询 aspnet\_应用程序表，并根据应用程序名称获取应用程序 Id

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    现在获取或设置此用户对象上

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

使用旧的用户名和密码登录现有用户。 使用注册页来创建新用户。 此外验证的用户是在角色中，按预期方式。

迁移到的标识系统，从而帮助用户将开放式身份验证 (OAuth) 添加到应用程序。 此示例，请参阅[此处](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/)具有启用的 OAuth。

## <a name="next-steps"></a>后续步骤

在本教程中，我们介绍了如何移植到 ASP.NET 标识从 SQL 成员资格用户，但我们未端口配置文件数据。 在下一教程中，我们将了解到移植到新的标识系统的配置文件数据从 SQL 成员资格。

你可以将在本文底部的反馈。

*感谢 Tom Dykstra 和 Rick Anderson 对本文的审阅。*
