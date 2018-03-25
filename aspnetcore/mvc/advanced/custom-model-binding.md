---
title: "自定义模型绑定"
author: ardalis
description: "自定义 ASP.NET Core MVC 中的模型绑定。"
manager: wpickett
ms.author: riande
ms.date: 04/10/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 313bc586a1c313f0bf5d8f413a4b082ffc2b7f0c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="custom-model-binding"></a>自定义模型绑定

作者：[Steve Smith](https://ardalis.com/)

通过模型绑定，控制器操作可直接使用模型类型（作为方法参数传入）而不是 HTTP 请求。 由模型绑定器处理传入的请求数据和应用程序模型之间的映射。 开发人员可以通过实现自定义模型绑定器来扩展内置的模型绑定功能（尽管通常不需要编写自己的提供程序）。

[查看或下载 GitHub 中的示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>默认模型绑定器限制

默认模型绑定器支持大多数常见的 .NET Core 数据类型，能够满足大部分开发人员的需求。 他们希望将基于文本的输入从请求直接绑定到模型类型。 绑定输入之前，可能需要对其进行转换。 例如，当拥有某个可以用来查找模型数据的键时。 基于该键，用户可以使用自定义模型绑定器来获取数据。

## <a name="model-binding-review"></a>模型绑定查看

模型绑定为其操作对象的类型使用特定定义。 简单类型转换自输入中的单个字符串。 复杂类型转换自多个输入值。 框架基于是否存在 `TypeConverter` 来确定差异。 如果简单 `string` -> `SomeType` 映射不需要外部资源，建议创建类型转换器。

创建自己的自定义模型绑定器之前，有必要查看现有模型绑定器的实现方式。 考虑使用 [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder)，它可将 base64 编码的字符串转换为字节数组。 字节数组通常存储为文件或数据库 BLOB 字段。

### <a name="working-with-the-bytearraymodelbinder"></a>使用 ByteArrayModelBinder

Base64 编码的字符串可用来表示二进制数据。 例如，下图可编码为一个字符串。

![dotnet 机器人](custom-model-binding/images/bot.png "dotnet 机器人")

如下显示了编码字符串的一小部分：

![编码的 dotnet 机器人](custom-model-binding/images/encoded-bot.png "编码的 dotnet 机器人")

按照[示例的自述文件](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md)中的说明将 base64 编码的字符串转换为文件。

ASP.NET Core MVC 可以采用 base64 编码的字符串，并使用 `ByteArrayModelBinder` 将其转换为字节数组。 实现 [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) 的 [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) 将 `byte[]` 参数映射到 `ByteArrayModelBinder`：

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

创建自己的自定义模型绑定器时，可实现自己的 `IModelBinderProvider` 类型，或使用 [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute)。

以下示例显示如何使用 `ByteArrayModelBinder` 将 base64 编码的字符串转换为 `byte[]`，并将结果保存到文件中：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

可以使用 [Postman](https://www.getpostman.com/) 等工具将 base64 编码的字符串发布到此 api 方法：

![postman](custom-model-binding/images/postman.png "postman")

只要绑定器可以将请求数据绑定到相应命名的属性或参数，模型绑定就会成功。 以下示例演示如何将 `ByteArrayModelBinder` 与 视图模型结合使用：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>自定义模型绑定器示例

在本部分中，我们将实现具有以下功能的自定义模型绑定器：

- 将传入的请求数据转换为强类型键参数。
- 使用 Entity Framework Core 来提取关联的实体。
- 将关联的实体作为自变量传递给操作方法。

以下示例在 `Author` 模型上使用 `ModelBinder` 属性：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

在前面的代码中，`ModelBinder` 属性指定应当用于绑定 `Author` 操作参数的 `IModelBinder` 的类型。 

通过 Entity Framework Core 和 `authorId` 提取数据源中的实体，可使用 `AuthorEntityBinder` 来绑定 `Author` 参数：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

以下代码显示如何在操作方法中使用 `AuthorEntityBinder`：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

可使用 `ModelBinder` 属性将 `AuthorEntityBinder` 应用于不使用默认约定的参数：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

在此示例中，由于参数的名称不是默认的 `authorId`，因此使用 `ModelBinder` 属性在参数上指定该名称。 请注意，比起在操作方法中查找实体，控制器和操作方法都得到了简化。 使用 Entity Framework Core 获取创建者的逻辑会移动到模型绑定器。 如果有几种方法绑定到创建者模型，就能得到很大程度的简化，并且有助于遵循 [DRY 原则](http://deviq.com/don-t-repeat-yourself/)。

可以将 `ModelBinder` 属性应用到各个模型属性（例如视图模型上）或操作方法参数，以便为该类型或操作指定某一模型绑定器或模型名称。

### <a name="implementing-a-modelbinderprovider"></a>实现 ModelBinderProvider

可以实现 `IModelBinderProvider`，而不是应用属性。 这就是内置框架绑定器的实现方式。 指定绑定器所操作的类型时，指定它生成的参数的类型，而不是绑定器接受的输入。 以下绑定器提供程序适用于 `AuthorEntityBinder`。 将其添加到 MVC 提供程序的集合中时，无需在 `Author` 或 `Author` 类型参数上使用 `ModelBinder` 属性。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> 注意：上述代码返回 `BinderTypeModelBinder`。 `BinderTypeModelBinder` 充当模型绑定器中心，并提供依赖关系注入 (DI)。 `AuthorEntityBinder` 需要 DI 来访问 EF Core。 如果模型绑定器需要 DI 中的服务，请使用 `BinderTypeModelBinder`。

若要使用自定义模型绑定器提供程序，请将其添加到 `ConfigureServices` 中：

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

评估模型绑定器时，按顺序检查提供程序的集合。 使用返回绑定器的第一个提供程序。

下图显示调试程序中的默认模型绑定器。

![默认模型绑定器](custom-model-binding/images/default-model-binders.png "默认模型绑定器")

向集合的末尾添加提供程序，可能会导致在调用自定义绑定器之前调用内置模型绑定器。 在此示例中，向集合的开头添加自定义提供程序，确保它用于 `Author` 操作参数。

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>建议和最佳做法

自定义模型绑定：
- 不应尝试设置状态代码或返回结果（例如 404 Not Found）。 如果模型绑定失败，那么该操作方法本身的[操作筛选器](xref:mvc/controllers/filters)或逻辑会处理失败。
- 对于消除操作方法中的重复代码和跨领域问题最为有用。
- 通常不应用其将字符串转换为自定义类型，而应选择用 [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) 来完成此操作。
