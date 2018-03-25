---
title: "ASP.NET Core 中的文件上传"
author: ardalis
description: "如何在 ASP.NET Core MVC 中使用模型绑定和流式处理上传文件。"
manager: wpickett
ms.author: riande
ms.date: 07/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/file-uploads
ms.openlocfilehash: 314d585c7bf7f8c95f763babe6cdf93e514ff656
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="file-uploads-in-aspnet-core"></a>ASP.NET Core 中的文件上传

作者：[Steve Smith](https://ardalis.com/)

ASP.NET MVC 操作支持使用简单的模型绑定（针对较小文件）或流式处理（针对较大文件）上传一个或多个文件。

[查看或下载 GitHub 中的示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>使用模型绑定上传小文件

要上传小文件，可使用多部分 HTML 窗体或使用 JavaScript 构造 POST 请求。 下方显示使用 Razor（支持上传多个文件）的示例窗体：

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

为支持文件上传，HTML 窗体必须指定 `multipart/form-data` 的 `enctype`。 上面显示的 `files` 输入元素支持上传多个文件。 忽略此输入元素上的 `multiple` 属性，只允许上传单个文件。 上述标记在浏览器中呈现为：

![文件上传窗体](file-uploads/_static/upload-form.png)

上传到服务器的单个文件可使用 [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) 接口通过[模型绑定](xref:mvc/models/model-binding)进行访问。 `IFormFile` 具有以下结构：

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> 切勿依赖或信任未经验证的 `FileName` 属性。 `FileName` 属性应仅用于显示目的。

使用模型绑定和 `IFormFile` 接口上传文件时，操作方法可接受单个 `IFormFile` 或代表多个文件的 `IEnumerable<IFormFile>`（或 `List<IFormFile>`）。 以下示例循环访问一个或多个上传的文件、将其保存到本地文件系统，并返回上传的文件总数和大小。

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

使用 `IFormFile` 技术上传的文件在处理之前会缓存在内存中或 Web 服务器的磁盘中。 在操作方法中，`IFormFile` 内容可作为流访问。 除了本地文件系统之外，还可将文件流式传输到 [Azure Blob 存储](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)或[实体框架](https://docs.microsoft.com/ef/core/index)。

要使用实体框架将二进制文件数据存储在数据库中，请在实体上定义类型为 `byte[]` 属性：

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

指定类型为 `IFormFile` 的 viewmodel 属性：

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile` 可直接用作操作方法参数或 viewmodel 属性，如上所示。

将 `IFormFile` 复制到流并将其保存到字节数组中：

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> 在关系数据库中存储二进制数据时要格外小心，因为它可能对性能产生不利影响。

## <a name="uploading-large-files-with-streaming"></a>使用流式处理上传大文件

如果文件上传的大小或频率会导致应用出现资源问题，请考虑使用流式处理上传文件，而不是像如上所示的模型绑定方法那样全部缓冲文件。 尽管使用 `IFormFile` 和模型绑定是更为简单的一种解决方案，但流式处理需要大量步骤才能正确实现。

> [!NOTE]
> 任何超过 64KB 的单个缓冲文件会从 RAM 移动到服务器磁盘上的临时文件中。 文件上传所用的资源（磁盘、RAM）取决于并发文件上传的数量和大小。 流式处理与性能没有太大的关系，而是与规模有关。 如果尝试缓冲过多上传，站点就会在内存或磁盘空间不足时崩溃。

以下示例演示如何通过 JavaScript/Angular 来流式传输到控制器操作。 使用自定义筛选器属性生成文件的防伪令牌，并在 HTTP 头中（而不是在请求正文中）传递该令牌。 由于操作方法直接处理上传的数据，所以其他筛选器会禁用模型绑定。 在该操作中，使用 `MultipartReader` 读取窗体的内容，它会读取每个单独的 `MultipartSection`，从而根据需要处理文件或存储内容。 读取所有节之后，该操作会执行自己的模型绑定。

初始操作加载窗体并将防伪令牌保存在 Cookie 中（通过 `GenerateAntiforgeryTokenCookieForAjax` 属性）：

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

该属性使用 ASP.NET Core 的内置[防伪](xref:security/anti-request-forgery)支持来设置包含请求令牌的 Cookie：

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Angular 会在名为 `X-XSRF-TOKEN` 的请求标头中自动传递防伪令牌。 ASP.NET Core MVC 应用配置为在 Startup.cs 的配置中引用此标头：

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

如下所示的 `DisableFormValueModelBinding` 属性用于禁用针对 `Upload` 操作方法的模型绑定。

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

由于已禁用模型绑定，因此 `Upload` 操作方法不接受参数。 它直接使用 `ControllerBase` 的 `Request` 属性。 `MultipartReader` 用于读取每个节。 该文件以 GUID 文件名保存，并且键/值数据存储在 `KeyValueAccumulator` 中。 读取所有节之后，系统会使用 `KeyValueAccumulator` 的内容将窗体数据绑定到模型类型。

完整的 `Upload` 方法如下所示：

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>疑难解答

以下是上传文件时遇到的一些常见问题及其可能的解决方案。

### <a name="unexpected-not-found-error-with-iis"></a>IIS 发生意外错误“找不到”

以下错误表示文件上传超过服务器配置的 `maxAllowedContentLength`：

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

默认设置为 `30000000`（大约 28.6MB）。 可通过编辑 web.config 自定义该值：

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

此设置仅适用于 IIS。 在 Kestrel 上托管时，默认情况下不会出现此行为。 有关详细信息，请参阅 [Request Limits \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)（请求限制 <requestLimits>）。

### <a name="null-reference-exception-with-iformfile"></a>IFormFile 的空引用异常

如果控制器正在接受使用 `IFormFile` 上传的文件，但你发现该值始终为 NULL，请确认 HTML 窗体指定的 `enctype` 值是否为 `multipart/form-data`。 如果未在 `<form>` 元素上设置此属性，则不会发生文件上传，并且任何绑定的 `IFormFile` 参数都将为 NULL。
