下表详细说明了 ASP.NET Core 代码生成器的参数：

| 参数               | 描述|
| ----------------- | ------------ |
| -m  | 模型的名称。 |
| -dc  | 数据上下文。 |
| -udl | 使用默认布局。 |
| -outDir | 用于创建视图的相对输出文件夹路径。 |
| --referenceScriptLibraries | 向“编辑”和“创建”页面添加 `_ValidationScriptsPartial` |

使用 `h` 开关获取 `aspnet-codegenerator razorpage` 命令方面的帮助：

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>测试应用

* 运行应用并将 `/Movies` 追加到浏览器中的 URL (`http://localhost:port/movies`)。
* 测试“创建”链接。

 ![创建页面](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* 测试“编辑”、“详细信息”和“删除”链接。

如果收到类似如下的错误，则验证是否已运行迁移并更新数据库：

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
