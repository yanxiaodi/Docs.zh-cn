---
title: "将文件上传至 ASP.NET Core 中的 Razor 页面"
author: guardrex
description: "了解如何将文件上传至 Razor 页面。"
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 4a2c6da6ed698d1a65ee51bd00a557e607f012da
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a>将文件上传至 ASP.NET Core 中的 Razor 页面

作者：[Luke Latham](https://github.com/guardrex)

本部分演示使用 Razor 页面上传文件。

本教程中的 [Razor 页面 Movie 示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)使用简单的模型绑定上传文件，非常适合上传小型文件。 有关流式传输大文件的信息，请参阅[通过流式传输上传大文件](xref:mvc/models/file-uploads#uploading-large-files-with-streaming)。

在下列步骤中，向示例应用添加电影计划文件上传功能。 每个电影计划由一个 `Schedule` 类表示。 该类包括两个版本的计划。 其中一个版本 (`PublicSchedule`) 提供给客户。 另一个版本 (`PrivateSchedule`) 用于公司员工。 每个版本作为单独的文件进行上传。 本教程演示如何通过单个 POST 将两个文件上传至服务器。

## <a name="security-considerations"></a>安全注意事项

向用户提供向服务器上传文件的功能时，必须格外小心。 攻击者可能对系统执行[拒绝服务](/windows-hardware/drivers/ifs/denial-of-service)和其他攻击。 一些降低成功攻击可能性的安全措施如下：

* 将文件上传到系统上的专用文件上传区域，这样可以更轻松地对上传内容实施安全措施。 如果允许文件上传，请确保在上传位置禁用执行权限。
* 使用由应用确定的安全文件名，而不是采用用户输入或已上传文件的文件名。
* 仅允许使用一组特定的已批准文件扩展名。
* 验证是否在服务器上执行了客户端检查。 客户端检查很容易规避。
* 检查上传文件大小，防止上传比预期大的文件。
* 对上传的内容运行病毒/恶意软件扫描程序。

> [!WARNING]
> 将恶意代码上传到系统通常是执行代码的第一步，这些代码可以：
> * 完全接管系统。
> * 重载系统，导致系统完全崩溃。
> * 泄露用户或系统数据。
> * 将涂鸦应用于公共接口。

## <a name="add-a-fileupload-class"></a>添加 FileUpload 类

创建 Razor 页以处理一对文件上传。 添加 `FileUpload` 类（此类与页面绑定以获取计划数据）。 右键单击“Models”文件夹。 选择“添加” > “类”。 将类命名为“FileUpload”，并添加以下属性：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

此类有一个属性对应计划标题，另各有一个属性对应计划的两个版本。 3 个属性皆为必需属性，标题长度必须为 3-60 个字符。

## <a name="add-a-helper-method-to-upload-files"></a>添加用于上传文件的 helper 方法

为避免处理未上传计划文件时出现代码重复，请首先上传一个静态 helper 方法。 在此应用中创建一个“Utilities”文件夹，然后在“FileHelpers.cs”文件中添加以下内容。 helper 方法 `ProcessFormFile` 接受 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 和 [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary)，并返回包含文件大小和内容的字符串。 检查内容类型和长度。 如果文件未通过验证检查，将向 `ModelState` 添加一个错误。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a>将文件保存到磁盘

示例应用将文件内容保存到数据库字段。 若要将文件内容保存到磁盘，请使用[文件流](/dotnet/api/system.io.filestream)：

```csharp
using (var fileStream = new FileStream(filePath, FileMode.Create))
{
    await formFile.CopyToAsync(fileStream);
}
```

工作进程必须对 `filePath` 指定的位置具有写入权限。

### <a name="save-the-file-to-azure-blob-storage"></a>将文件保存到 Azure Blob 存储

若要将文件内容上传到 Azure Blob 存储，请参阅[使用 .NET 的 Azure Blob 存储入门](/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。 本主题演示如何使用 [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) 将[文件流](/dotnet/api/system.io.filestream)保存到 blob 存储。

## <a name="add-the-schedule-class"></a>添加 Schedule 类

右键单击“Models”文件夹。 选择“添加” > “类”。 将类命名为“Schedule”，并添加以下属性：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

此类使用 `Display` 和 `DisplayFormat` 特性，呈现计划数据时，这些特性会生成友好型的标题和格式。

## <a name="update-the-moviecontext"></a>更新 MovieContext

在 `MovieContext` (*Models/MovieContext.cs*) 中为计划指定 `DbSet`：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a>将 Schedule 表添加到数据库

打开包管理器控制台 (PMC)：“工具” > “NuGet 包管理器” > “包管理器控制台”。

![PMC 菜单](../first-mvc-app/adding-model/_static/pmc.png)

在 PMC 中执行以下命令。 这些命令将向数据库添加 `Schedule` 表：

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>添加文件上传 Razor 页面

在“Pages”文件夹中创建“Schedules”文件夹。 在“Schedules”文件夹中，创建名为“Index.cshtml”的页面，用于上传具有如下内容的计划：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

每个窗体组包含一个 \<label>，它显示每个类属性的名称。 `FileUpload` 模型中的 `Display` 特性提供这些标签的显示值。 例如，`UploadPublicSchedule` 特性的显示名称通过 `[Display(Name="Public Schedule")]` 进行设置，因此呈现窗体时会在此标签中显示“Public Schedule”。

每个窗体组包含一个验证 \<span>。 如果用户输入未能满足 `FileUpload` 类中设置的属性特性，或者任何 `ProcessFormFile` 方法文件检查失败，则模型验证会失败。 模型验证失败时，会向用户呈现有用的验证消息。 例如，`Title` 属性带有 `[Required]` 和 `[StringLength(60, MinimumLength = 3)]` 注释。 用户若未提供标题，会接收到一条指示需要提供值的消息。 如果用户输入的值少于 3 个字符或多于 60 个字符，则会接收到一条指示值长度不正确的消息。 如果提供不含内容的文件，则会显示一条指示文件为空的消息。

## <a name="add-the-page-model"></a>添加页面模型

将页面模型 (Index.cshtml.cs) 添加到“Schedules”文件夹中：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

页面模型（Index.cshtml.cs 中的 `IndexModel`）绑定 `FileUpload` 类：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

此模型还使用计划列表 (`IList<Schedule>`) 在页面上显示数据库中存储的计划：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

页面加载 `OnGetAsync` 时，会从数据库填充 `Schedules`，用于生成已加载计划的 HTML 表：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

将窗体发布到服务器时，会检查 `ModelState`。 如果无效，会重新生成 `Schedule`，且页面会呈现一个或多个验证消息，陈述页面验证失败的原因。 如果有效，`FileUpload` 属性将用于“OnPostAsync”中，以完成两个计划版本的文件上传，并创建一个用于存储数据的新 `Schedule` 对象。 然后会将此计划保存到数据库：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>链接文件上传 Razor 页面

打开“_Layout.cshtml”，然后向导航栏添加一个链接以访问文件上传页面：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>添加计划删除确认页面

用户单击删除计划时，为其提供取消此操作的机会。 向“Schedules”文件夹添加删除确认页面 (Delete.cshtml)：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

页面模型 (Delete.cshtml.cs) 在请求的路由数据中加载由 `id` 标识的单个计划。 将“Delete.cshtml.cs”文件添加到“Schedules”文件夹：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

`OnPostAsync` 方法按 `id` 处理计划删除：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

成功删除计划后，`RedirectToPage` 将返回到计划的“Index.cshtml”页面。

## <a name="the-working-schedules-razor-page"></a>有效的 Schedules Razor 页面

页面加载时，计划标题、公用计划和专用计划的标签和输入将呈现提交按钮：

![按初始加载所示计划 Razor 页面，其中不含验证错误和空字段](uploading-files/_static/browser1.png)

在不填充任何字段的情况下选择“上传”按钮会违反此模型上的 `[Required]` 特性。 `ModelState` 无效。 会向用户显示验证错误消息：

![验证错误消息显示在每个输入控件旁边](uploading-files/_static/browser2.png)

在“标题”字段中键入两个字母。 验证消息改为指示标题长度必须介于 3-60 个字符之间：

![标题验证消息已更改](uploading-files/_static/browser3.png)

上传一个或多个计划时，“已加载计划”部分会显示已加载计划：

![已加载计划表，显示每个计划的标题、UTC 格式的上传日期、公用版本文件大小和专用版本文件大小](uploading-files/_static/browser4.png)

用户可单击该表中的“删除”链接以访问删除确认视图，并在其中选择确认或取消删除操作。

## <a name="troubleshooting"></a>疑难解答

有关上传 `IFormFile` 的疑难解答信息，请参阅 [ASP.NET Core 中的文件上传：疑难解答](xref:mvc/models/file-uploads#troubleshooting)。

感谢读完这篇 Razor 页面简介。 我们非常感谢你的反馈。 完成本教程后，大力推荐了解 [MVC 和 EF Core 入门](xref:data/ef-mvc/intro)。

## <a name="additional-resources"></a>其他资源

* [ASP.NET Core 中的文件上传](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[上一篇：验证](xref:tutorials/razor-pages/validation)
