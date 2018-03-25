# <a name="adding-validation"></a>添加验证

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本部分中，将向 `Movie` 模型添加验证逻辑，并确保每当用户创建或编辑电影时，都会强制执行验证规则。

## <a name="keeping-things-dry"></a>坚持 DRY 原则

MVC 的设计原则之一是 [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself)（“不要自我重复”）。 ASP.NET MVC 支持你仅指定一次功能或行为，然后使它应用到整个应用中。 这可以减少所需编写的代码量，并使编写的代码更少出错，更易于测试和维护。

MVC 和 Entity Framework Core Code First 提供的验证支持是 DRY 原则在实际操作中的极佳示例。 可以在一个位置（模型类中）以声明方式指定验证规则，并且在应用中的所有位置强制执行。

## <a name="adding-validation-rules-to-the-movie-model"></a>将验证规则添加到电影模型

打开 Movie.cs 文件。 DataAnnotations 提供一组内置验证特性，可通过声明方式应用于任何类或属性。 （它还包含 `DataType` 等格式特性，这些特性可帮助进行格式设置，但不提供任何验证。）

更新 `Movie` 类以使用内置的 `Required`、`StringLength`、`RegularExpression` 和 `Range` 验证特性。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

验证特性指定要对应用这些特性的模型属性强制执行的行为。 `Required` 和 `MinimumLength` 特性表示属性必须有值；但用户可输入空格来满足此验证。 `RegularExpression` 特性用于限制可输入的字符。 在上述代码中，`Genre` 和 `Rating` 仅可使用字母（禁用空格、数字和特殊字符）。 `Range` 特性将值限制在指定范围内。 `StringLength` 特性使你能够设置字符串属性的最大长度，以及可选的最小长度。 从本质上来说，需要值类型（如 `decimal`、`int`、`float`、`DateTime`），但不需要 `[Required]` 特性。

让 ASP.NET 强制自动执行验证规则有助于提升应用的可靠性。 同时它能确保你无法忘记验证某些内容，并防止你无意中将错误数据导入数据库。

## <a name="validation-error-ui-in-mvc"></a>MVC 中的验证错误 UI

运行应用并导航到电影控制器。

点击“新建”连接添加新电影的链接。 使用无效值填写表单。 当 jQuery 客户端验证检测到错误时，会显示一条错误消息。

![带有多个 jQuery 客户端验证错误的电影视图表单](../../tutorials/first-mvc-app/validation/_static/val.png)

> [!NOTE]
> 可能无法在 `Price` 字段中输入十进制逗号。 若要使 [jQuery 验证](https://jqueryvalidation.org/)支持使用逗号（“,”）表示小数点的的非英语区域设置，以及支持非美国英语日期格式，必须执行使应用全球化的步骤。 有关添加十进制逗号的说明，请参阅 [GitHub 问题 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420)。 

请注意表单如何自动呈现每个包含无效值的字段中相应的验证错误消息。 客户端（使用 JavaScript 和 jQuery）和服务器端（若用户禁用 JavaScript）都必定会遇到这些错误。

明显的好处在于不需要在 `MoviesController` 类或 Create.cshtml 视图中更改单个代码行来启用此验证 UI。 在本教程前面创建的控制器和视图会自动选取验证规则，这些规则是通过在 `Movie` 模型类的属性上使用验证特性所指定的。 使用 `Edit` 操作方法测试验证后，即已应用相同的验证。

存在客户端验证错误时，不会将表单数据发送到服务器。 可通过使用 [Fiddler 工具](http://www.telerik.com/fiddler)或 [F12 开发人员工具](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)在 `HTTP Post` 方法中设置断点来对此进行验证。

## <a name="how-validation-works"></a>验证工作原理

你可能想知道在不对控制器或视图中的代码进行任何更新的情况下，验证 UI 是如何生成的。 下列代码显示两种 `Create` 方法。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

第一个 (HTTP GET) `Create` 操作方法显示初始的“创建”表单。 第二个 (`[HttpPost]`) 版本处理表单发布。 第二个 `Create` 方法（`[HttpPost]` 版本）调用 `ModelState.IsValid` 以检查电影是否有任何验证错误。 调用此方法将评估已应用于对象的任何验证特性。 如果对象有验证错误，则 `Create` 方法会重新显示此表单。 如果没有错误，此方法则将新电影保存在数据库中。 在我们的电影示例中，在检测到客户端上存在验证错误时，表单不会发布到服务器。当存在客户端验证错误时，第二个 `Create` 方法永远不会被调用。 如果在浏览器中禁用 JavaScript，客户端验证将被禁用，而你可以测试 HTTP POST `Create` 方法 `ModelState.IsValid` 检测任何验证错误。

可以在 `[HttpPost] Create` 方法中设置断点，并验证方法从未被调用，客户端验证在检测到存在验证错误时不会提交表单数据。 如果在浏览器中禁用 JavaScript，然后提交错误的表单，将触发断点。 在没有 JavaScript 的情况下仍然可以进行完整的验证。 

以下图片显示如何在 FireFox 浏览器中禁用 JavaScript。

![Firefox：在“选项”的“内容”选项卡上，取消选中“启用 Javascript”复选框。](../../tutorials/first-mvc-app/validation/_static/ff.png)

以下图片显示如何在 Chrome 浏览器中禁用 JavaScript。

![Google Chrome：在“内容”设置的 Javascript 部分中，选择“不允许任何网站运行 JavaScript”。](../../tutorials/first-mvc-app/validation/_static/chrome.png)

禁用 JavaScript 后，发布无效数据并单步执行调试程序。

![在对无效数据进行调试时，ModelState.IsValid 上的 Intellisense 显示值为 false。](../../tutorials/first-mvc-app/validation/_static/ms.png)

以下是之前在本教程中已搭建基架的 Create.cshtml 视图模板的一部分。 以上所示的操作方法使用它来显示初始表单，并在发生错误时重新显示此表单。

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]

[输入标记帮助程序](xref:mvc/views/working-with-forms)使用 [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 特性，并在客户端上生成 jQuery 验证所需的 HTML 特性。 [验证标记帮助程序](xref:mvc/views/working-with-forms#the-validation-tag-helpers)用于显示验证错误。 有关详细信息，请参阅[验证](xref:mvc/models/validation)。

此方法真正好的一点是：无论是控制器还是 `Create` 视图模板都不知道强制实施的实际验证规则或显示的特定错误消息。 仅可在 `Movie` 类中指定验证规则和错误字符串。 这些相同的验证规则自动应用于 `Edit` 视图和可能创建用于编辑模型的任何其他视图模板。

需要更改验证逻辑时，可以通过将验证特性添加到模型在同一个位置实现此操作。（在此示例中为 `Movie` 类）。 无需担心对应用程序的不同部分所强制执行规则的方式不一致 - 所有验证逻辑都将定义在一个位置并用于整个应用程序。 这使代码非常简洁，并且更易于维护和改进。 这意味着对 DRY 原则的完全遵守。

## <a name="using-datatype-attributes"></a>使用 DataType 特性

打开 Movie.cs 文件并检查 `Movie` 类。 除了一组内置的验证特性，`System.ComponentModel.DataAnnotations` 命名空间还提供格式特性。 我们已经在发布日期和价格字段中应用了 `DataType` 枚举值。 以下代码显示具有适当 `DataType` 特性的 `ReleaseDate` 和 `Price` 属性。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` 属性仅提供相关提示来帮助视图引擎设置数据格式（并提供元素/属性，例如向 URL 提供 `<a>` 和向电子邮件提供 `<a href="mailto:EmailAddress.com">`）。 可以使用 `RegularExpression` 特性验证数据的格式。 `DataType` 属性用于指定比数据库内部类型更具体的数据类型，它们不是验证属性。 在此示例中，我们只想跟踪日期，而不是时间。 `DataType` 枚举提供了多种数据类型，例如日期、时间、电话号码、货币、电子邮件地址等。 应用程序还可通过 `DataType` 特性自动提供类型特定的功能。 例如，可以为 `DataType.EmailAddress` 创建 `mailto:` 链接，并且可以在支持 HTML5 的浏览器中为 `DataType.Date` 提供日期选择器。 `DataType` 特性发出 HTML 5 `data-`（读作 data dash）特性供 HTML 5 浏览器理解。 `DataType` 特性不提供任何验证。

`DataType.Date` 不指定显示日期的格式。 默认情况下，数据字段根据基于服务器的 `CultureInfo` 的默认格式进行显示。

`DisplayFormat` 特性用于显式指定日期格式：

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` 设置指定在文本框中显示值以进行编辑时也应用格式。 （你可能不想为某些字段执行此操作 — 例如对于货币值，你可能不希望文本框中的货币符号可编辑。）

可以单独使用 `DisplayFormat` 特性，但通常建议使用 `DataType` 特性。 `DataType` 特性传达数据的语义而不是传达如何在屏幕上呈现数据，并提供 DisplayFormat 不具备的以下优势：

* 浏览器可启用 HTML5 功能（例如显示日历控件、区域设置适用的货币符号、电子邮件链接等）

* 默认情况下，浏览器将根据区域设置采用正确的格式呈现数据。

* `DataType` 特性使 MVC 能够选择正确的字段模板来呈现数据（如果 `DisplayFormat` 由自身使用，则使用的是字符串模板）。

> [!NOTE]
> jQuery 验证不适用于 `Range` 属性和 `DateTime`。 例如，以下代码将始终显示客户端验证错误，即便日期在指定的范围内：

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

需要禁用 jQuery 日期验证才能使用具有 `DateTime` 的 `Range` 特性。 通常，在模型中编译固定日期是不恰当的，因此不推荐使用 `Range` 特性和 `DateTime`。

以下代码显示组合在一行上的特性：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

在本系列的下一部分中，我们将回顾应用程序，并对自动生成的 `Details` 和 `Delete` 方法进行一些改进。

## <a name="additional-resources"></a>其他资源

* [使用表单](xref:mvc/views/working-with-forms)
* [全球化和本地化](xref:fundamentals/localization)
* [标记帮助程序简介](xref:mvc/views/tag-helpers/intro)
* [创作标记帮助程序](xref:mvc/views/tag-helpers/authoring)
