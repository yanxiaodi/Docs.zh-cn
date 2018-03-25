我们将在下一教程中介绍 [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6)。 [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) 特性指定要显示的字段名称的内容（本例中应为“Release Date”，而不是“ReleaseDate”）。 [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) 属性指定数据的类型（日期），使字段中存储的时间信息不会显示。

浏览到 Pages/Movies，并将鼠标悬停在“编辑”链接上以查看目标 URL。

![鼠标悬停在“编辑”链接上的浏览器窗口，显示了 http://localhost:1234/Movies/Edit/5 的链接 URL](../../tutorials/razor-pages/da1/edit7.png)

“编辑”、“详细信息”和“删除”链接是在 Pages/Movies/Index.cshtml 文件中由[定位标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)生成的。

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[标记帮助程序](xref:mvc/views/tag-helpers/intro)使服务器端代码可以在 Razor 文件中参与创建和呈现 HTML 元素。 在前面的代码中，`AnchorTagHelper` 从 Razor 页面（路由是相对的）、`asp-page` 和路由 ID (`asp-route-id`) 动态生成 HTML `href` 特性值。 有关详细信息，请参阅[页面的 URL 生成](xref:mvc/razor-pages/index#url-generation-for-pages)。

在最喜欢的浏览器中使用“查看源”来检查生成的标记。 生成的 HTML 的一部分如下所示：

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

动态生成的链接通过查询字符串传递电影 ID（例如 `http://localhost:5000/Movies/Details?id=2`）。 

更新“编辑”、“详细信息”和“删除”Razor 页面以使用“{id:int?}”路由模板。 将上述每个页面的页面指令从 `@page` 更改为 `@page "{id:int}"`。 运行应用，然后查看源。 生成的 HTML 会将 ID 添加到 URL 的路径部分：

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

如果对具有“{id: int}” 路由模板的页面进行的请求中不包含整数，则将返回 HTTP 404（未找到）错误。 例如，`http://localhost:5000/Movies/Details` 将返回 404 错误。 若要使 ID 可选，请将 `?` 追加到路由约束：

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a>更新并发异常处理

在 Pages/Movies/Edit.cshtml.cs 文件中更新 `OnPostAsync` 方法。 下列突出显示的代码显示了更改：

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

之前的代码仅检测当第一个并发客户端删除电影以及第二个并发客户端对电影发布更改时的并发异常。

测试 `catch` 块：

* 在 `catch (DbUpdateConcurrencyException)` 上设置断点
* 编辑电影。
* 在其他浏览器窗口中，选择同一电影的“删除”链接，然后删除此电影。
* 在之前的浏览器窗口中，将更改发布到电影。

当两个或更多客户端同时更新记录时，生产代码通常将检测到并发冲突。 有关详细信息，请参阅[处理并发冲突](xref:data/ef-rp/concurrency)。

### <a name="posting-and-binding-review"></a>发布和绑定审阅

检查 *Pages/Movies/Edit.cshtml.cs* 文件：[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]

当对 Movies/Edit 页面进行 HTTP GET 请求时（例如 `http://localhost:5000/Movies/Edit/2`）：

* `OnGetAsync` 方法从数据库提取电影并返回 `Page` 方法。 
* `Page` 方法呈现“Pages/Movies/Edit.cshtml”Razor 页面。 Pages/Movies/Edit.cshtml 文件包含模型指令 (`@model RazorPagesMovie.Pages.Movies.EditModel`)，这使电影模型在页面上可用。
* “编辑”表单中会显示电影的值。

当发布 Movies/Edit 页面时：

* 此页面上的表单值将绑定到 `Movie` 属性。 `[BindProperty]` 特性会启用[模型绑定](xref:mvc/models/model-binding)。

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* 如果模型状态中存在错误（例如，`ReleaseDate` 无法被转换为日期），则会使用已提交的值再次发布表单。
* 如果没有模型错误，则电影已保存。

“索引”、“创建”和“删除”Razor 页面中的 HTTP GET 方法遵循一个类似的模式。 “创建”Razor 页面中的 HTTP POST `OnPostAsync` 方法遵循的模式类似于“编辑”Razor 页面中的 `OnPostAsync` 方法所遵循的模式。

在下一教程中将添加搜索。