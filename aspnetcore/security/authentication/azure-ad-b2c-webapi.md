---
title: "中的 web Api 与 Azure Active Directory B2C 中 ASP.NET 核心云身份验证"
author: camsoper
description: "了解如何设置与 ASP.NET 核心 Web API 的 Azure Active Directory B2C 身份验证。 测试已通过身份验证的 web API 与 Postman。"
ms.author: casoper
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 1213f7eb25fb6525f98d83dff0956a841ae686a7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>中的 web Api 与 Azure Active Directory B2C 中 ASP.NET 核心云身份验证

作者：[Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是为 web 和移动应用的云标识管理解决方案。 服务提供用于在云中和本地托管的应用的身份验证。 身份验证类型包括个人帐户，社交网络帐户，以及联合企业帐户。 此外，Azure AD B2C 可以提供以最小配置多因素身份验证。

> [!TIP]
> Azure Active Directory (Azure AD) Azure AD B2C 包括单独的产品。 Azure AD 租户表示是一个组织，而 Azure AD B2C 租户表示标识要用于信赖方应用程序的集合。 若要了解详细信息，请参阅[Azure AD B2C： 常见问题 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)。

由于 web Api 具有没有用户界面，因此它们无法将用户重定向到 Azure AD B2C 等安全令牌服务。 相反，API 是具有已用户进行身份验证与 Azure AD B2C 将调用应用程序中传递的持有者令牌。 API 然后验证该令牌而无需直接用户交互。

在本教程中，学习如何：

> [!div class="checklist"]
> * 创建 Azure Active Directory B2C 租户。
> * 在 Azure AD B2C 中注册 Web API。
> * 使用 Visual Studio 创建 Web API 配置为使用 Azure AD B2C 租户进行身份验证。
> * 配置策略控制的 Azure AD B2C 租户的行为。
> * 使用 Postman 以模拟 web 应用将其提供一个登录对话框中，检索令牌，并使用它来对 web API 发出请求。

## <a name="prerequisites"></a>系统必备

对于本演练具备以下条件：

* [Microsoft Azure 订阅](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) （任意版本）
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>创建 Azure Active Directory B2C 租户

创建 Azure AD B2C 租户[文档中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。 出现提示时，将租户与 Azure 订阅相关联是可选的出于本教程。

## <a name="configure-a-sign-up-or-sign-in-policy"></a>配置注册或登录策略

使用到的 Azure AD B2C 文档中的步骤[创建注册或登录策略](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)。 命名策略时**SiUpIn**。  使用的文档中提供的示例值**标识提供程序**，**注册属性**，和**应用程序声明**。 使用**立即运行**是可选的按钮以测试策略，如文档中所述。

## <a name="register-the-api-in-azure-ad-b2c"></a>在 Azure AD B2C 注册 API

新创建的 Azure AD B2C 租户中注册你的 API 使用[文档中的步骤](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api)下**注册 web API**部分。

使用以下值：

| 设置                       | “值”               | 说明                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **名称**                      | *&lt;API 名称&gt;*  | 输入**名称**描述你的应用对使用者的应用。                     |
| **包括 web 应用程序/web API** | 是                 |                                                                                        |
| **允许隐式流**       | 是                 |                                                                                        |
| **回复 URL**                 | `https://localhost` | 答复 Url 是 Azure AD B2C 其中返回您的应用程序请求的任何令牌的终结点。 |
| **应用程序 ID URI**                | *api*               | URI 不需要解析为物理地址。 它只需是唯一的。     |
| **包括本机客户端**     | 否                  |                                                                                        |

注册 API 后，将显示在租户中的应用和 Api 的列表。 选择刚刚注册的 API。 选择**复制**右侧的图标**应用程序 ID**字段可将它复制到剪贴板。 选择**发布作用域**和验证默认*user_impersonation*作用域是存在。

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>在 Visual Studio 2017 年 1 中创建 ASP.NET Core 应用

Visual Studio Web 应用程序模板可以配置为使用 Azure AD B2C 租户进行身份验证。

在 Visual Studio 中：

1. 创建新的 ASP.NET Core Web 应用程序。 
2. 选择**Web API**从模板列表中。
3. 选择**更改身份验证**按钮。
    
    ![更改身份验证按钮](./azure-ad-b2c-webapi/change-auth-button.png)

4. 在**更改身份验证**对话框中，选择**单个用户帐户**，然后选择**连接到现有用户存储在云中**下拉列表中。 
    
    ![更改身份验证对话框](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. 完成窗体具有以下值：
    
    | 设置                       | “值”                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **域名**               | *&lt;B2C 租户的域名&gt;*          |
    | **应用程序 ID**            | *&lt;粘贴剪贴板中的应用程序 ID&gt;* |
    | **注册或登录策略** | `B2C_1_SiUpIn`                                        |
    
    选择**确定**关闭**更改身份验证**对话框。 选择**确定**创建 web 应用。

Visual Studio 使用名为的控制器中创建 web API *ValuesController.cs*返回 GET 请求的硬编码值。 类用修饰[Authorize 属性](xref:security/authorization/simple)，因此所有请求都需要身份验证。

## <a name="run-the-web-api"></a>运行 web API

在 Visual Studio 中，运行 API。 Visual Studio 将启动浏览器指向的 API 根 URL。 请注意地址栏中中的 URL 并将在后台运行的 API。

> [!NOTE]
> 由于没有为根 URL 定义无控制器，浏览器将显示 404 （找不到页） 错误。 这是预期行为。

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>使用 Postman 来获取令牌和测试 API

[Postman](https://getpostman.com/postman)是用于测试的工具 web Api。 对于本教程，Postman 模拟的 web 应用程序访问 web API 代表该用户。

### <a name="register-postman-as-a-web-app"></a>为 web 应用注册 Postman

由于 Postman 模拟的 web 应用程序可以从 Azure AD B2C 租户获取令牌，则必须注册在租户中为 web 应用。 注册使用 Postman[文档中的步骤](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下**注册 web 应用**部分。 在停止**创建 web 应用程序客户端密钥**部分。 出于本教程中，无需客户端密钥。 

使用以下值：

| 设置                       | “值”                            | 说明                           |
|-------------------------------|----------------------------------|---------------------------------|
| **名称**                      | Postman                          |                                 |
| **包括 web 应用程序/web API** | 是                              |                                 |
| **允许隐式流**       | 是                              |                                 |
| **回复 URL**                 | `https://getpostman.com/postman` |                                 |
| **应用程序 ID URI**                | *&lt;将保留为空&gt;*            | 无需进行本教程。 |
| **包括本机客户端**     | 否                               |                                 |

新注册的 web 应用程序需要访问 web API 代表该用户的权限。  

1. 选择**Postman**的应用，然后选择列表中**API 访问**从左侧菜单。
2. 选择**+ 添加**。
3. 在**选择 API**下拉列表中，选择 web API 的名称。
4. 在**选择作用域**下拉列表中，确保选中所有作用域。
5. 选择**确定**。

请注意 Postman 应用的应用程序 ID，因为它需获取的持有者令牌。

### <a name="create-a-postman-request"></a>创建 Postman 请求

启动 Postman。 默认情况下，显示 Postman**新建**时启动的对话框。 如果未显示对话框中，选择**+ 新建**在左上角的按钮。

从**新建**对话框：

1. 选择**请求**。
    
    ![请求按钮](./azure-ad-b2c-webapi/postman-create-new.png)

2. 输入*获取值*中**请求名称**框。
3. 选择**+ 创建集合**以创建用于存储请求新的集合。 命名集合*ASP.NET Core 教程*，然后选择复选标记。
    
    ![创建新集合](./azure-ad-b2c-webapi/postman-create-collection.png)

4. 选择**将保存到 ASP.NET 核心教程**按钮。

### <a name="test-the-web-api-withoutauthentication"></a>测试 web API withoutauthentication

若要验证 web API 需要身份验证，首先请无身份验证的请求。

1. 在**输入请求 URL**框中，输入的 URL `ValuesController`。 URL 是显示在浏览器中使用的相同**api/值**追加。 示例将`https://localhost:44375/api/values`。
2. 选择**发送**按钮。
3. 请注意响应的状态是*401 未授权*。

    ![401 未授权的响应](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a>获取的持有者令牌

要对 web API 进行身份验证的请求，持有者令牌是必需的。 Postman 使得容易登录到 Azure AD B2C 租户并获取的令牌。

1. 上**授权**选项卡上，在**类型**下拉列表中，选择**OAuth 2.0**。 在**授权将数据添加到**下拉列表中，选择**请求标头**。 选择**获取新访问令牌**。
    
    ![使用设置的授权选项卡](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. 完成**获取新访问令牌**，如下所示的对话框：
    
    | 设置                   | “值”                                                                                         | 说明                                                                                      |
    |---------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
    | **标记名称**            | *&lt;标记名称&gt;*                                                                          | 输入该令牌的描述性名称。                                                    |
    | **授予类型构建的**            | 隐式                                                                                      |                                                                                            |
    | **回调 URL**          | `https://getpostman.com/postman`                                                              |                                                                                            |
    | **身份验证 URL**              | `https://login.microsoftonline.com/<tenant domain name>/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` | 替换*&lt;租户域名&gt;*与租户的域名，而无需命令的尖括号。 |
    | **客户端 ID**             | *&lt;输入 Postman 应用<b>应用程序 ID</b>&gt;*                                       |                                                                                            |
    | **客户端密钥**         | *&lt;将保留为空&gt;*                                                                         |                                                                                            |
    | **范围**                 | `https://<tenant domain name>/api/user_impersonation openid offline_access`                   | 替换*&lt;租户域名&gt;*与租户的域名，而无需命令的尖括号。 |
    | **客户端身份验证** | 在正文中发送客户端凭据                                                               |                                                                                            |
    
3. 选择**令牌请求**按钮。

4. Postman 打开包含 Azure AD B2C 租户的登录对话框中的新窗口中。 使用现有帐户登录 （如果已创建一个测试策略） 或选择**立即注册**创建新帐户。 **忘记了密码？**链接用于重置忘记了的密码。

5. 已成功登录后，该窗口将关闭与**管理访问令牌**此时将显示对话框。 向下的滚动到底部并选择**使用令牌**按钮。
    
    ![在哪里可以找到"使用令牌"按钮](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>测试身份验证与 web API

选择**发送**按钮以重新发送请求。 此时，响应状态是*200 确定*JSON 负载且在响应上可见**正文**选项卡。
    
![负载和成功状态](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：

> [!div class="checklist"]
> * 创建 Azure Active Directory B2C 租户。
> * 在 Azure AD B2C 中注册 Web API。
> * 使用 Visual Studio 创建 Web API 配置为使用 Azure AD B2C 租户进行身份验证。
> * 配置策略控制的 Azure AD B2C 租户的行为。
> * 使用 Postman 以模拟 web 应用将其提供一个登录对话框中，检索令牌，并使用它来对 web API 发出请求。

在继续学习到开发你的 API:

* [保护 web 应用使用 Azure AD B2C 的 ASP.NET Core](xref:security/authentication/azure-ad-b2c)。
* [从使用 Azure AD B2C 的.NET web 应用程序调用.NET web API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。
* [自定义 Azure AD B2C 用户界面](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。
* [配置密码复杂性要求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。
* [启用多因素身份验证](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。
* 配置其他标识提供程序，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)， [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)， [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)， [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)， [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)，和其他人。
* [使用 Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)从 Azure AD B2C 租户中检索其他用户信息，例如组成员身份。
