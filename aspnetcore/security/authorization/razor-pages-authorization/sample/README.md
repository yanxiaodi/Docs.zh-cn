# <a name="aspnet-core-authorization-sample"></a>ASP.NET 核心授权示例

此示例演示了约定的 Razor 页授权的使用。 此示例演示中所述的功能[Razor 页授权约定](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization)主题。

在此示例中的用户授权使用 cookie 身份验证功能中所述[使用 Cookie 身份验证，无 ASP.NET 核心标识](https://docs.microsoft.com/aspnet/core/security/authentication/cookie)主题。 有关使用 ASP.NET 核心标识的信息，请参阅中的主题[身份验证](https://docs.microsoft.com/aspnet/core/security/authentication/index)文档的部分。

运行示例时，使用电子邮件地址 **maria.rodriguez@contoso.com** 用户进行身份验证。

## <a name="examples-in-this-sample"></a>在此示例中的示例

| 功能 | 描述 |
| ------- | ----------- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | 将添加[AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)到与指定路径的页。 |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | 将添加[AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)到所有具有指定的路径的文件夹中的页。 |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | 将添加[AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)到具有指定的路径页。 |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | 将添加[AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)到所有具有指定的路径的文件夹中的页。 |
