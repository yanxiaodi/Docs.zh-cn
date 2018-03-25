---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: "ASP.NET Web API 中的绑定的参数 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a>ASP.NET Web API 中的绑定的参数
====================
通过[Mike Wasson](https://github.com/MikeWasson)

在 Web API 在控制器上调用方法，它必须设置的参数，调用进程值*绑定*。 本文介绍了如何 Web API 绑定参数，以及如何自定义绑定过程。

默认情况下，Web API 使用以下规则来将参数绑定：

- 如果参数为"简单"的类型，Web API 将尝试从 URI 中获取的值。 简单类型包括.NET[基元类型](https://msdn.microsoft.com/library/system.type.isprimitive.aspx)(**int**， **bool**， **double**，依此类推)，加上**TimeSpan**， **DateTime**， **Guid**，**十进制**，和**字符串**，*加上*任何类型具有可以从字符串转换的类型转换器。 （有关详细信息的类型转换器更高版本。）
- 为复杂类型，Web API 尝试从消息正文中读取值时，使用[媒体类型格式化程序](media-formatters.md)。

例如，以下是典型的 Web API 控制器方法：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Id*参数是&quot;简单&quot;类型，因此 Web API 尝试于请求 URI 中获取的值。 *项*参数是一个复杂类型，因此 Web API 使用的媒体类型格式化程序以从请求正文中读取值。

若要从 URI 中获取一个值，Web API，查找路线数据和 URI 查询字符串中。 路由系统分析 URI 和匹配到某个路由时，填充路线数据。 有关详细信息，请参阅[路由和操作选择](../web-api-routing-and-actions/routing-and-action-selection.md)。

在本文的其余部分，我将展示如何自定义模型绑定过程。 对于复杂类型，但是，考虑使用只要有可能的媒体类型格式化程序。 HTTP 的关键原则在于，资源发送在消息正文中，使用内容协商指定的资源表示形式。 为此，设计媒体类型格式化程序。

## <a name="using-fromuri"></a>使用 [FromUri]

若要强制 Web API 以读取复杂类型的 URI 中，添加**[FromUri]**属性设为参数。 下面的示例定义`GeoPoint`类型，以及获取控制器方法`GeoPoint`从 URI。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

客户端可以将纬度和经度值放在查询字符串和 Web API 将使用它们来构造`GeoPoint`。 例如:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>使用 [FromBody]

若要强制 Web API 以从请求正文读取简单类型，添加**[FromBody]**属性设为参数：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

在此示例中，Web API 将使用媒体类型格式化程序读取的值*名称*从请求正文。 下面是一个示例客户端请求。

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

当参数具有 [FromBody] 时，Web API 使用的内容类型标头选择格式化程序。 在此示例中，内容类型是&quot;应用程序/json&quot;和请求正文是原始的 JSON 字符串 （不是一个 JSON 对象）。

最多一个参数允许从消息正文中读取。 因此这不起作用：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

此规则的原因是，请求正文可能存储在只读取一次的非缓冲的流。

## <a name="type-converters"></a>类型转换器

你可以将类视为简单类型，（以便 Web API 将尝试将其绑定从 URI） 的 Web API 通过创建**TypeConverter**并提供字符串转换。

下面的代码演示`GeoPoint`类表示一个地理点，加上**TypeConverter** ，用于从字符串转换为将转换`GeoPoint`实例。 `GeoPoint`类用修饰**[TypeConverter]**特性以指定的类型转换器。 (此示例已由 Mike 停止的博客文章激发[如何将绑定到 MVC/WebAPI 中的操作签名中的自定义对象](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)。)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

现在 Web API 会将`GeoPoint`为简单类型，这意味着它将尝试将绑定`GeoPoint`URI 中的参数。 无需包括**[FromUri]**的参数。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

客户端可以调用具有如下 URI 的方法：

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>模型联编程序

比类型转换器更灵活的选项是创建自定义模型联编程序。 模型联编程序，与你拥有访问权限等 HTTP 请求、 操作说明和的原始值从路线数据。

若要创建模型联编程序，实现**IModelBinder**接口。 此接口定义单个方法**BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

下面是模型联编程序`GeoPoint`对象。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

模型联编程序获取原始输入的值从*值提供程序*。 这种设计将分隔两个不同的功能：

- 值提供程序获取 HTTP 请求，并填充键 / 值对的字典。
- 模型联编程序使用此字典来填充模型。

Web API 中的默认值提供程序从路由数据和查询字符串中获取值。 例如，如果的 URI 是`http://localhost/api/values/1?location=48,-122`，值提供程序创建以下的键 / 值对：

- id = &quot;1&quot;
- 位置 = &quot;48,122&quot;

(假设默认路由模板，这是&quot;api / {controller} / {id}&quot;。)

要绑定的参数的名称存储在**ModelBindingContext.ModelName**属性。 模型联编程序看起来与此值在字典中的键。 如果值存在，并且可以转换为`GeoPoint`，模型联编程序将该绑定的值赋给**ModelBindingContext.Model**属性。

请注意，模型联编程序不限于简单类型转换。 在此示例中，模型联编程序首先查找已知位置，表中，如果失败，它使用类型转换。

**设置模型联编程序**

有多种方法来设置模型联编程序。 首先，您可以添加**[ModelBinder]**属性设为参数。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

你还可以添加**[ModelBinder]**属性类型。 Web API 将使用指定的模型联编程序为该类型的所有参数。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

最后，你可以添加到模型联编程序提供程序**HttpConfiguration**。 模型联编程序提供程序是只需创建模型联编程序的工厂类。 你可以通过从派生创建提供程序[ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx)类。 但是，如果模型联编程序处理的单一类型，很容易地使用内置**SimpleModelBinderProvider**，为此用途设计的。 下面的代码演示如何执行此操作。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

使用模型绑定提供程序，你仍需要添加**[ModelBinder]**属性设为参数，以告知 Web API，它应使用模型联编程序和不格式化程序媒体类型。 但是，现在你无需在属性中指定的一种模型联编程序：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>值在提供程序

我所述模型联编程序从值提供程序中获取值。 若要编写一个自定义值提供程序，实现**IValueProvider**接口。 下面是在请求中提取 cookie 中的值示例：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

你还需要通过从派生来创建值提供程序工厂**ValueProviderFactory**类。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

添加到值提供程序工厂**HttpConfiguration** ，如下所示。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Web API 编写的所有值提供程序，因此当模型联编程序调用**ValueProvider.GetValue**，模型联编程序从第一个值提供程序能够生成它接收的值。

或者，通过使用，在参数级别设置的值提供程序工厂**ValueProvider**特性，，如下所示：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

这将告知 Web API，以便使用指定的值提供程序工厂，使用模型绑定，并不是使用任何其他已注册的值提供程序。

## <a name="httpparameterbinding"></a>HttpParameterBinding

模型联编程序是更多常规机制的特定实例。 如果你看一下**[ModelBinder]**属性中，你将看到它派生自抽象**ParameterBindingAttribute**类。 此类定义一个方法， **GetBinding**，它将返回**HttpParameterBinding**对象：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding**负责参数绑定到一个值。 情况下**[ModelBinder]**，属性将返回**HttpParameterBinding**使用的实现**IModelBinder**若要执行实际绑定。 你还可以实现你自己**HttpParameterBinding**。

例如，假设你想要获取从 Etag`if-match`和`if-none-match`在请求中的标头。 我们将开始通过定义一个类来表示 Etag。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

我们还将定义一个枚举，以指示是否获取从 ETag`if-match`标头或`if-none-match`标头。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

下面是**HttpParameterBinding** ，从所需标头获取 ETag，并将其绑定到类型 ETag 的参数：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync**方法执行绑定。 在这种方法，添加到绑定的参数值**ActionArgument**字典中的**HttpActionContext**。

> [!NOTE]
> 如果你**ExecuteBindingAsync**方法读取请求消息的正文，请重写**WillReadBody**属性返回 true。 请求正文可能只能读取一次，因此 Web API 会强制实现一个规则，最多其中一个绑定未缓冲的数据流可以读取消息正文。


要应用自定义**HttpParameterBinding**，您可以定义属性派生自**ParameterBindingAttribute**。 有关`ETagParameterBinding`，我们将定义两个属性，一个用于`if-match`标头和一个用于`if-none-match`标头。 都派生自抽象基类。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

下面是使用一个控制器方法`[IfNoneMatch]`属性。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

除了**ParameterBindingAttribute**，没有用于添加自定义的另一个挂钩**HttpParameterBinding**。 上**HttpConfiguration**对象， **ParameterBindingRules**属性是 anomymous 函数的类型的集合 (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**)。 例如，可以添加任何 ETag 参数的 GET 方法使用的规则`ETagParameterBinding`与`if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

该函数应返回`null`参数绑定不适用。

## <a name="iactionvaluebinder"></a>IActionValueBinder

由可插入的服务，控制整个参数绑定过程**IActionValueBinder**。 默认实现**IActionValueBinder**执行下列任务：

1. 查找**ParameterBindingAttribute**的参数。 这包括**[FromBody]**， **[FromUri]**，和**[ModelBinder]**，或自定义属性。
2. 否则，查找**HttpConfiguration.ParameterBindingRules**函数返回非 null **HttpParameterBinding**。
3. 否则，请使用我以前所述的默认规则。 

    - 如果参数类型是"简单"，或者有 URI 中将绑定的类型转换器。 这相当于加上**[FromUri]**参数上的属性。
    - 否则，尝试从消息正文读取参数。 这相当于加上**[FromBody]**的参数。

如果你想要则无法将整个**IActionValueBinder**服务为自定义实现。

## <a name="additional-resources"></a>其他资源

[自定义参数绑定示例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike 停止写下了很好的博客文章有关 Web API 参数绑定：

- [Web API 原理参数绑定](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Web API 的 MVC 样式参数绑定](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [如何将绑定到 MVC/Web API 中的操作签名中的自定义对象](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [如何在 Web API 中创建自定义值提供程序](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [实质上的 web API 参数绑定](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
