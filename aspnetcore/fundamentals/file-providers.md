---
title: "ASP.NET Core 中的文件提供程序"
author: ardalis
description: "了解 ASP.NET Core 如何通过文件提供程序来抽象化文件系统访问。"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/file-providers
ms.openlocfilehash: 06197f967e111d75531e9c3bcbcbdb971cb9f99b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET Core 中的文件提供程序

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core 通过文件提供程序来抽象化文件系统访问。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="file-provider-abstractions"></a>文件提供程序抽象

文件提供程序是对文件系统的抽象。 主接口是`IFileProvider`。 `IFileProvider` 公开一些方法以获取文件信息 (`IFileInfo`)、目录信息 (`IDirectoryContents`)，以及设置更改通知（使用 `IChangeToken`）。

`IFileInfo` 提供有关单个文件或目录的方法和属性。 它有两个布尔属性（`Exists` 和`IsDirectory`），以及描述文件 `Name``Length` （以字节为单位）、`LastModified` 日期的属性。 可使用其 `CreateReadStream` 方法从文件中读取。

## <a name="file-provider-implementations"></a>文件提供程序实现

`IFileProvider` 的三种实现可用：物理、嵌入和复合。 物理提供程序用于访问实际系统的文件。 嵌入式提供程序用于访问嵌入在程序集中的文件。 复合式提供程序提供对一个或多个其他提供程序中的文件和目录的合并访问。

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider` 提供对物理文件系统的访问。 它包装 `System.IO.File` 类型（针对物理提供程序），将所有路径范围限定在一个目录及其子目录中。 此范围限制对特定目录及其子目录的访问，从而阻止对该边界外的文件系统的访问。 实例化此提供程序时，用户必须向其提供一个目录路径，它可作为对该提供程序的所有请求的基本路径（并且限制此路径之外的访问）。 在 ASP.NET Core 应用中，可以直接实例化 `PhysicalFileProvider` 提供程序，也可以通过 [依赖关系注入](dependency-injection.md)在控制器或服务的构造函数中请求 `IFileProvider`。 后一种方法生成的解决方案通常更具灵活性和可测试性。

以下示例演示如何创建 `PhysicalFileProvider`。


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

你可以循环访问其目录内容，或通过提供子路径来获取特定文件的信息。

若要从控制器请求提供程序，请在控制器的构造函数中指定该提供程序，并将其分配给本地字段。 使用操作方法中的本地实例：

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

然后，在应用的 `Startup` 类中创建提供程序：

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

在 Index.cshtml 视图中，循环访问提供的 `IDirectoryContents`：

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

结果：

![列出物理文件和文件夹的文件提供程序示例应用程序](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider` 用于访问嵌入在程序集中的文件。 在 .NET Core 中，通过 .csproj 文件中的 `<EmbeddedResource>` 元素将各个文件嵌入程序集中：

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

在指定要嵌入到程序集中的文件时，你可以使用[通配模式](#globbing-patterns)。 这些模式可用于匹配一个或多个文件。

> [!NOTE]
> 实际上，用户不太可能会将项目中所有的 .js 文件都嵌入其程序集中；以上示例仅供演示。

创建 `EmbeddedFileProvider` 时，将它要读取的程序集传递给其构造函数。

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

以上片段演示如何创建可访问当前执行的程序集的 `EmbeddedFileProvider`。

更新要使用 `EmbeddedFileProvider` 的示例应用会导致以下输出：

![列出嵌入的文件的文件提供程序示例应用程序](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> 嵌入的资源不会公开目录。 而是使用 `.` 分隔符将指向资源的路径（通过其命名空间）嵌入其文件名中。

> [!TIP]
> `EmbeddedFileProvider` 构造函数接受一个可选的 `baseNamespace` 参数。 指定此操作会将到 `GetDirectoryContents` 的调用范围限制为提供的名称空间下的那些资源。

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider` 合并 `IFileProvider` 实例，以便公开一个接口来处理多个提供程序中的文件。 创建 `CompositeFileProvider` 时，将一个或多个 `IFileProvider` 实例传递给其构造函数：

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

更新要使用 `CompositeFileProvider` 的示例应用（该应用包括之前配置的物理和嵌入式提供程序）会导致以下输出：

![列出物理文件/文件夹和嵌入的文件的文件提供程序示例应用程序](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>监视更改

`IFileProvider` `Watch` 方法提供一种方法来监视一个或多个文件或目录的更改。 此方法接受一个路径字符串，该字符串可使用[通配模式](#globbing-patterns)来指定多个文件，并返回一个 `IChangeToken`。 此令牌公开可被检查的 `HasChanged` 属性，以及在检测到对指定路径字符串进行更改时调用的 `RegisterChangeCallback` 方法。 请注意，每个更改令牌仅调用其关联的回叫来响应单个更改。 若要启用持续监视，可以使用如下所示的 `TaskCompletionSource`，或者重新创建 `IChangeToken` 实例来响应更改。

在本文的示例中，控制台应用程序被配置为每当在修改文本文件时显示消息：

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

多次保存文件之后的结果：

![执行 dotnet run 后的命令窗口（显示应用程序正在监视 quotes.txt 文件的更改，该文件已更改五次）。](file-providers/_static/watch-console.png)

> [!NOTE]
> 某些文件系统（例如 Docker 容器和网络共享）可能无法可靠地发送更改通知。 将 `DOTNET_USE_POLLINGFILEWATCHER` 环境变量设置为 `1` 或 `true`，以便每 4 秒对文件系统的更改进行轮询。

## <a name="globbing-patterns"></a>通配模式

文件系统路径使用称作“通配模式”的通配符模式。 这些简单的模式可以用来指定文件组。 这两个通配符为 `*` 和 `**`。

**`*`**

   匹配位于当前文件夹级别的任何内容、任何文件名或任何文件扩展名。 匹配由文件路径中的 `/` 和 `.` 字符终止。

<strong><code>**</code></strong>

   匹配多个目录级别的任何内容。 可用于以递归方式匹配目录层次结构中的许多文件。

### <a name="globbing-pattern-examples"></a>通配模式示例

**`directory/file.txt`**

   匹配特定目录中的特定文件。

**<code>directory/*.txt</code>**

   匹配特定目录中带有 `.txt` 扩展名的所有文件。

**`directory/*/bower.json`**

   匹配 `directory` 目录的下一级目录中的所有 `bower.json` 文件。

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   匹配在 `directory` 目录下带有 `.txt` 扩展名所有文件。

## <a name="file-provider-usage-in-aspnet-core"></a>ASP.NET Core 中的文件提供程序使用情况

ASP.NET Core 的多个部分使用文件提供程序。 `IHostingEnvironment` 将应用的内容根和 Web 根作为 `IFileProvider` 类型公开。 静态文件中间件使用文件提供程序来查找静态文件。 Razor 在查找视图时大量使用 `IFileProvider`。 Dotnet 的发布功能使用文件提供程序和通配模式来指定应该发布哪些文件。

## <a name="recommendations-for-use-in-apps"></a>在应用中使用的建议

如果 ASP.NET Core 应用需要文件系统访问权限，那么你可以通过依赖关系注入请求 `IFileProvider` 的实例，然后使用其方法执行访问操作（如此示例中所示）。 这样在应用启动时配置一次提供程序即可，而且可以减少应用实例化的实现类型数量。
