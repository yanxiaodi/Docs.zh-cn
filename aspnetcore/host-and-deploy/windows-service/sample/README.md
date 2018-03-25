# <a name="custom-webhost-service-sample"></a>自定义 WebHost 服务示例

此示例演示而无需使用作为 Windows 服务的 IIS 承载 ASP.NET Core 应用在 Windows 上的推荐的方式。 此示例演示中所述的功能[ASP.NET Core 应用托管在 Windows 服务](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)。

## <a name="instructions"></a>说明

示例应用程序是一个简单的 MVC web 应用，根据中的说明修改[ASP.NET Core 应用托管在 Windows 服务](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)。

若要在服务中运行应用，请执行以下步骤：

1. 创建在一个文件夹*c:\svc*。

1. 将应用发布到的文件夹`dotnet publish --configuration Release --output c:\\svc`。 该命令会将应用的资产移动到的文件夹，包括所需`appsettings.json`文件和`wwwroot`文件夹及其内容。

1. 打开**管理员**命令行界面。

1. 执行以下命令：

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. 在浏览器中，转到`http://localhost:5000`以确认服务正在运行。

1. 若要停止服务，使用该命令：

   ```console
   sc stop MyService
   ```

如果不按预期运行时服务中运行启动应用程序，以使错误消息可访问的捷径是添加日志记录提供程序，如[Windows 事件日志提供程序](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog)。 另一种方法是检查系统上使用事件查看器应用程序事件日志。 例如，下面是 FileNotFound 错误应用程序事件日志中的未经处理的异常：

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
