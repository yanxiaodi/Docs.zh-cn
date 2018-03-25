---
title: "使用 Swagger 的 ASP.NET Core Web API 帮助页"
author: spboyer
description: "本教程提供添加 Swagger 以生成文档的演练和针对 Web API 应用程序的帮助页。"
manager: wpickett
ms.author: spboyer
ms.date: 09/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 911504d9472ae78a0d1d002f1feb57f3a160d5bf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a>使用 Swagger 的 ASP.NET Core Web API 帮助页

<a name="web-api-help-pages-using-swagger"></a>

作者：[Shayne Boyer](https://twitter.com/spboyer) 和 [Scott Addie](https://twitter.com/Scott_Addie)

对于构建消费应用程序的开发人员来说，了解 API 的各种方法是一个挑战。

使用包含 .NET Core 实现 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) 的 [Swagger](https://swagger.io/) 为 Web API 生成优秀的文档和帮助页与添加多个 NuGet 包并修改 Startup.cs 一样简单。

* [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) 是一个开源项目，用于生成 ASP.NET Core Web API 的 Swagger 文档。

* [Swagger](https://swagger.io/) 是 RESTful API 的一种计算机可读表示形式，为交互式文档、客户端 SDK 生成和可发现性提供支持。

本教程以[使用 ASP.NET Core MVC 和 Visual Studio 构建你的第一个 Web API](xref:tutorials/first-web-api) 为示例进行构建。 如果想要按步骤操作，请在 [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) 中下载该示例。

## <a name="getting-started"></a>入门

Swashbuckle 有三个主要组成部分：

* `Swashbuckle.AspNetCore.Swagger`：将 `SwaggerDocument` 对象公开为 JSON 终结点的 Swagger 对象模型和中间件。

* `Swashbuckle.AspNetCore.SwaggerGen`：Swagger 生成器，从你的路由、控制器和模型直接生成 `SwaggerDocument` 对象。 它通常与 Swagger 终结点中间件结合，以自动公开 Swagger JSON。

* `Swashbuckle.AspNetCore.SwaggerUI`：Swagger UI 工具的嵌入式版本，它解释 Swagger JSON 以构建描述 Web API 功能的可自定义的丰富体验。 它包括针对公共方法的内置测试工具。

## <a name="nuget-packages"></a>NuGet 包

可以使用以下方法来添加 Swashbuckle：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 从“程序包管理器控制台”窗口：

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* 从“管理 NuGet 程序包”对话框中：

     * 右键单击“解决方案资源管理器” > “管理 NuGet 包”中的项目
     * 将“包源”设置为“nuget.org”
     * 在搜索框中输入“Swashbuckle.AspNetCore”
     * 从“浏览”选项卡中选择“Swashbuckle.AspNetCore”包，然后单击“安装”

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 右键单击“Solution Pad” > “添加包...”中的“包”文件夹
* 将“添加包”窗口的“源”下拉列表设置为“nuget.org”
* 在搜索框中输入 Swashbuckle.AspNetCore
* 从结果窗格中选择“Swashbuckle.AspNetCore”包，然后单击“添加包”

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

从“集成终端”中运行以下命令：

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

运行下面的命令：

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a>向中间件添加和配置 Swagger

将 Swagger 生成器添加到 Startup.cs 的 `ConfigureServices` 方法中的服务集合：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

为 `Info` 类添加以下 using 语句：

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

在 Startup.cs 的 `Configure` 方法中，启用中间件为生成的 JSON 文档和 SwaggerUI 提供服务：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

启动应用，并导航到 `http://localhost:<random_port>/swagger/v1/swagger.json`。 随即显示生成的用于描述终结点的文档。

注意：Microsoft Edge、Google Chrome 和 Firefox 在本机显示 JSON 文档。 提供可设置文档格式的部件版式扩展，使文档更易于阅读。 为简洁起见，对下面的示例内容进行了缩减。

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

此文档驱动 Swagger UI，可以通过导航到 `http://localhost:<random_port>/swagger` 进行查看：

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

`TodoController` 中的每个公共操作方法都可以从 UI 中进行测试。 单击方法名称可以展开该部分。 添加所有必要的参数，然后单击“试试看!”。

![示例 Swagger GET 测试](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a>自定义项和扩展性

Swagger 提供了为对象模型进行归档和自定义 UI 以匹配你的主题的选项。

### <a name="api-info-and-description"></a>API 信息和说明

传递给 `AddSwaggerGen` 方法的配置操作可用于添加信息，如作者、许可证和说明：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

下图描绘了显示版本信息的 Swagger UI：

![包含版本信息的 Swagger UI：说明、作者以及查看更多链接](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML 注释

可使用以下方法启用 XML 注释：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 右键单击“解决方案资源管理器”中的项目，然后选择“属性”
* 查看“生成”选项卡的“输出”部分下的“XML 文档文件”框：

![项目属性的“生成”选项卡](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 打开“项目选项”对话框 >“生成” > “编译器”
* 查看“常规选项”部分下的“生成 xml 文档”框：

![项目选项的“常规选项”部分](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

将以下代码片段手动添加到 .csproj 文件：

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

查看 Visual Studio Code。

---

启用 XML 注释，为未记录的公共类型和成员提供调试信息。 未记录的类型和成员由以下警告消息指示：缺少对公共可见类型或成员的 XML 注释。

配置 Swagger 以使用生成的 XML 文件。 对于 Linux 或非 Windows 操作系统，文件名和路径区分大小写。 例如，“ToDoApi.XML”文件可以在 Windows 上找到，但无法在 CentOS 上找到。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

在前面的代码中，`ApplicationBasePath` 获取应用的基路径。 基路径用于查找 XML 注释文件。 “TodoApi.xml”仅适用于本示例，因为生成的 XML 注释文件的名称基于应用程序名称。

通过向节标题添加说明，将三斜杠注释添加到方法增强了 Swagger UI：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![显示 DELETE 方法的 XML 注释“删除特定 TodoItem” 的 Swagger UI](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

UI 由生成的 JSON 文件驱动，还包含以下这些注释：

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

将 [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) 标记添加到 `Create` 操作方法文档。 它可以补充 `<summary>` 标记中指定的信息，并提供更可靠的 Swagger UI。 `<remarks>` 标记内容可包含文本、JSON 或 XML。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

请注意带这些附加注释的 UI 增强。

![显示包含其他注释的 Swagger UI](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>数据注释

使用 `System.ComponentModel.DataAnnotations` 中找到的属性来修饰模型，以帮助驱动 Swagger UI 组件。

将 `[Required]` 属性添加到 `TodoItem` 类的 `Name` 属性：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

此属性的状态更改 UI 行为并更改基础 JSON 架构：

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

将 `[Produces("application/json")]` 属性添加到 API 控制器。 这样做的目的是声明控制器的操作支持返回 application/json 的内容类型：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

“响应内容类型”下拉列表选此内容类型作为控制器的默认 GET 操作：

![包含默认响应内容类型的 Swagger UI](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

随着 Web API 中的数据注释的使用越来越多，UI 和 API 帮助页变得更具说明性和更为有用。

### <a name="describing-response-types"></a>描述响应类型

消费应用程序开发人员最关心的问题是返回的内容和具体的响应类型和错误代码（如果不标准）。 这些问题在 XML 注释和数据批注中进行处理。

`Create` 操作在成功时返回 `201 Created`，或在发布的请求正文为 null 时返回 `400 Bad Request`。 如果 Swagger UI 中没有提供合适的文档，那么使用者会缺少对这些预期结果的了解。 在下面的示例中，通过添加突出显示的行解决了此问题：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

Swagger UI 现在清楚地记录预期的 HTTP 响应代码：

![Swagger UI 针对“响应消息”下的状态代码和原因显示 POST 响应类描述“返回新建的待办事项”和“400 - 如果该项为 null”](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a>自定义 UI

Stock UI 兼具功能性和可演示性，但是，为你的 API 构建文档页时，你希望呈现你的品牌或主题。 使用 Swashbuckle 组件完成该任务需要添加资源以服务静态文件，然后构建文件夹结构以托管这些文件。

如果面向 .NET Framework，则需要向项目添加 `Microsoft.AspNetCore.StaticFiles` NuGet 包：

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

启用静态文件中间件：

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

从 [Swagger UI GitHub 存储库](https://github.com/swagger-api/swagger-ui/tree/2.x/dist)中获取 dist 文件夹的内容。 此文件夹包含 Swagger UI 页必需的资产。

创建 wwwroot/swagger/ui 文件夹，然后将 dist 文件夹的内容复制到其中。

使用下面的 CSS 创建 wwwroot/swagger/ui/css/custom.css 文件以自定义页面标题：

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

引用 index.html 文件中的“custom.css”：

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

浏览到 `http://localhost:<random_port>/swagger/ui/index.html` 中的 index.html 页。 在标题文本框中输入 `http://localhost:<random_port>/swagger/v1/swagger.json`，然后单击“浏览”按钮。 生成的页面如下所示：

![使用自定义标题的 Swagger UI](web-api-help-pages-using-swagger/_static/custom-header.png)

还可以对页面执行更多操作。 在 [Swagger UI GitHub 存储库](https://github.com/swagger-api/swagger-ui)中查看 UI 资源的完整功能。
