# <a name="cookie-sharing-sample-app"></a>Cookie 共享示例应用程序

此示例阐释了在使用 cookie 身份验证的三个应用间共享的 cookie:

| 项目                             | 描述 |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | ASP.NET 核心 2.0 Razor 页应用而无需使用 ASP.NET 核心标识 |
| CookieAuthWithIdentity.Core         | 使用 ASP.NET Core 标识的 ASP.NET Core 2.0 MVC 应用程序 |
| CookieAuthWithIdentity.NETFramework | 使用 ASP.NET Identity 的 ASP.NET Framework 4.6.1 MVC 应用程序 |

说明：

1. 运行 CookieAuth.Core 应用。 注册用户。 注册用户时，应用程序对用户进行身份验证。 注销用户。
1. 在同一浏览器会话中，运行 CookieAuthWithIdentity.Core 应用。 将同一个用户注册为与核心应用程序一起使用。 注册用户时，应用程序对用户进行身份验证。 注销用户。
1. 在同一浏览器会话中，运行 CookieAuthWithIdentity.NETFramework 应用。 将同一个用户注册为与其他应用程序使用。 注册用户时，应用程序对用户进行身份验证。 注销用户。
1. 登录到三个应用的任何用户。 在应用间共享身份验证 cookie。 请注意用户自动登录到其他两个应用程序。
1. 注销的任何应用的用户。 请注意用户自动注销其他两个应用程序。

此示例演示中所述的功能[共享在应用之间的 cookie](https://docs.microsoft.com/aspnet/core/security/cookie-sharing)主题。
