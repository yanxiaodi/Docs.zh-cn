---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: "JSON 和 ASP.NET Web API 中的 XML 序列化 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON 和 ASP.NET Web API 中的 XML 序列化
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本指南介绍了 ASP.NET Web API 中的 JSON 和 XML 格式化程序。

在 ASP.NET Web API 中，*媒体类型格式化程序*是一个对象，可以：

- 读取 CLR 对象从 HTTP 消息正文
- CLR 对象写入 HTTP 消息正文

Web API 提供对 JSON 和 XML 的媒体类型格式化程序。 默认情况下，框架将这些格式化程序插入到管道。 客户端可以在 HTTP 请求的 Accept 标头中请求 JSON 或 XML。

## <a name="contents"></a>内容

- [JSON 媒体类型格式化程序](#json_media_type_formatter)

    - [只读属性](#json_readonly)
    - [日期](#json_dates)
    - [缩进](#json_indenting)
    - [Camel 大小写形式](#json_camelcasing)
    - [匿名和弱类型化对象](#json_anon)
- [XML 媒体类型格式化程序](#xml_media_type_formatter)

    - [只读属性](#xml_readonly)
    - [日期](#xml_dates)
    - [缩进](#xml_indenting)
    - [设置每个类型 XML 序列化程序](#xml_pertype)
- [删除 JSON 或 XML 格式化程序](#removing_the_json_or_xml_formatter)
- [处理循环对象引用](#handling_circular_object_references)
- [测试对象序列化](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>JSON 媒体类型格式化程序

JSON 格式由**JsonMediaTypeFormatter**类。 默认情况下， **JsonMediaTypeFormatter**使用[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)库来执行序列化。 Json.NET 是一个第三方开源项目。

如果你愿意，你可以配置**JsonMediaTypeFormatter**类以使用**DataContractJsonSerializer**而不是 Json.NET。 为此，请设置**UseDataContractJsonSerializer**属性**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>JSON 序列化

本部分介绍一些特定行为的 JSON 格式化程序，使用默认[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)序列化程序。 这并不意味着要 Json.NET library; 的综合文档有关详细信息，请参阅[Json.NET 文档](http://james.newtonking.com/projects/json/help/)。

#### <a name="what-gets-serialized"></a>什么获取序列化？

默认情况下，所有公共属性和字段的序列化 JSON 中包括。 若要忽略的属性或字段，来修饰它与**JsonIgnore**属性。

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

如果你愿意&quot;参加&quot;方法，请修饰的类具有**DataContract**属性。 如果存在此属性，则将忽略成员，除非它们具有**DataMember**。 你还可以使用**DataMember**序列化私有成员。

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>只读属性

默认情况下，只读属性进行序列化。

<a id="json_dates"></a>
### <a name="dates"></a>日期

默认情况下，Json.NET 日期中写入[ISO 8601](http://www.w3.org/TR/NOTE-datetime)格式。 UTC （协调世界时） 中的日期是编写使用"Z"后缀的。 本地时间的日期包括时区偏移量。 例如:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

默认情况下，Json.NET 保留时区。 您可以替代此设置 DateTimeZoneHandling 属性：

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

如果你希望使用[Microsoft JSON 日期格式](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb)(`"\/Date(ticks)\/"`) 而不是 ISO 8601 设置**DateFormatHandling**上序列化程序设置的属性：

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>缩进

若要编写缩进的 JSON，设置**格式**将设置为**当**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Camel 大小写形式

若要编写 JSON 属性名以 camel 大小写形式，而无需更改你的数据模型，设置**CamelCasePropertyNamesContractResolver**上序列化程序：

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>匿名和弱类型化对象

操作方法可以返回匿名对象，其序列化为 JSON。 例如:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

响应消息正文将包含以下 JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

如果你的 web API 接收松散结构从客户端的 JSON 对象，你可以反序列化请求正文为**Newtonsoft.Json.Linq.JObject**类型。

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

但是，则通常最好使用强类型化的数据对象。 然后，你无需自行，分析数据，获得模型验证的优点。

XML 序列化程序不支持匿名类型或**JObject**实例。 如果你将这些功能用于将 JSON 数据，您应该从管道中，删除 XML 格式化程序，如本文后面所述。

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>XML 媒体类型格式化程序

XML 格式由**XmlMediaTypeFormatter**类。 默认情况下， **XmlMediaTypeFormatter**使用**DataContractSerializer**类来执行序列化。

如果你愿意，你可以配置**XmlMediaTypeFormatter**使用**XmlSerializer**而不是**DataContractSerializer**。 为此，请设置**UseXmlSerializer**属性**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer**类支持的类型，而不集较小**DataContractSerializer**，但提供更好地控制生成的 XML。 请考虑使用**XmlSerializer**如果你需要以匹配现有 XML 架构。

### <a name="xml-serialization"></a>XML 序列化

本部分介绍一些特定行为的 XML 格式化程序，使用默认**DataContractSerializer**。

默认情况下，DataContractSerializer 的行为，如下所示：

- 进行序列化所有公共读/写属性和字段。 若要忽略的属性或字段，来修饰它与**IgnoreDataMember**属性。
- 不序列化私有和受保护成员。
- 只读属性不会序列化。 （但是，只读集合属性的内容进行序列化。）
- 类声明中显示的完全相同，将用 XML 编写类和成员名称。
- 使用默认 XML 命名空间。

如果你需要更好地控制序列化，可以按照与类声明**DataContract**属性。 当存在此属性时，类序列，如下所示：

- &quot;选择加入&quot;方法： 属性和字段，默认情况下不序列化。 要序列化的属性或字段，来修饰它与**DataMember**属性。
- 要序列化的私有或受保护的成员，来修饰它与**DataMember**属性。
- 只读属性不会序列化。
- 若要更改的类名在 XML 中的显示方式，设置*名称*中的参数**DataContract**属性。
- 若要更改成员名称在 XML 中的显示方式，设置*名称*中的参数**DataMember**属性。
- 若要更改的 XML 命名空间，设置*Namespace*中的参数**DataContract**类。

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>只读属性

只读属性不会序列化。 只读属性包含支持私有字段，你可以将标记具有的私有字段**DataMember**属性。 此方法要求**DataContract**类中的属性。

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>日期

日期是采用 ISO 8601 格式编写的。 例如， &quot;2012年-05-23T20:21:37.9116538Z&quot;。

<a id="xml_indenting"></a>
### <a name="indenting"></a>缩进

若要编写缩进的 XML，设置**缩进**属性**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>设置每个类型 XML 序列化程序

你可以为不同的 CLR 类型设置不同的 XML 序列化程序。 例如，你可能需要的特定数据对象**XmlSerializer**为了向后兼容。 你可以使用**XmlSerializer**此对象，并继续使用**DataContractSerializer**对于其他类型。

若要设置为特定类型的 XML 序列化程序，调用**SetSerializer**。

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

你可以指定**XmlSerializer**或派生自的任何对象**XmlObjectSerializer**。

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>删除 JSON 或 XML 格式化程序

如果不想要使用它们，可以从列表中的格式化程序，删除 JSON 格式化程序或 XML 格式化程序。 若要这样做的主要原因如下：

- 若要限制你对某个特定媒体类型的 web API 响应。 例如，你可能决定支持仅将 JSON 响应，并且删除 XML 格式化程序。
- 若要将自定义格式化程序替换默认的格式化程序。 例如，你无法将 JSON 格式化程序替换自己的 JSON 格式化程序的自定义实现。

下面的代码演示如何删除默认格式化程序。 调用此成员，从你**应用程序\_启动**Global.asax 中定义的方法。

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>处理循环对象引用

默认情况下，JSON 和 XML 格式化程序编写的所有对象作为值。 如果两个属性引用同一对象，或格式化程序两次在集合中显示相同的对象，如果将序列化对象两次。 如果这是特定问题对象图包含循环，因为它检测到循环关系图中时，序列化程序将引发异常。

请考虑下列对象模型和控制器。

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

调用此操作将导致格式化程序引发了异常，都会转换为状态代码 500 （内部服务器错误） 响应客户端。

若要保留 JSON 中的对象引用，以下代码添加到**应用程序\_启动**Global.asax 文件中的方法：

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

现在的控制器操作将返回类似如下所示的 JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

请注意，序列化程序将添加&quot;$id&quot;到这两个对象的属性。 此外，检测到 Employee.Department 属性创建一个循环，因此它将该值替换为一个对象引用: {&quot;$ref&quot;:&quot;1&quot;}。

> [!NOTE]
> 对象引用不是标准 json 格式的。 使用此功能之前，请考虑你的客户端是否将能够分析结果。 它可能是更好的做法只需从关系图删除周期。 例如，在此示例中不真正需要返回到部门员工的链接。


若要保留在 XML 中的对象引用，必须有两个选项。 更简单的选项是添加`[DataContract(IsReference=true)]`到模型类。 *IsReference*参数使对象引用。 请记住， **DataContract**选择加入，使序列化，因此你还需要添加**DataMember**属性的属性：

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

现在格式化程序将生成类似的 XML 如下：

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

如果你想要避免在模型类上的属性，则另一个选项： 创建新的类型的特定**DataContractSerializer**实例并设置*preserveObjectReferences*到**true**构造函数中。 然后将设置此实例为每个类型序列化程序在 XML 媒体类型格式化程序上。 以下代码演示如何执行此操作：

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>测试对象序列化

设计你的 web API 时，它可用于测试你的数据对象将序列化。 你可以执行此操作而无需创建控制器或调用的控制器操作。

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
