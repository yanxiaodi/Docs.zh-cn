# <a name="custom-model-binding-demo"></a>自定义模型绑定演示

可以通过运行应用程序并将 base64 编码字符串发布到 ImageController 终结点 (/api/image/) 来测试 `ByteArrayModelBinder`。 应在请求正文中将文件和文件名属性指定为表单数据（使用 Postman 或类似的工具）。 可以使用[本示例字符串](Base64String.txt)。 结果将使用指定的文件名保存在 wwwroot/images/upload 文件夹中。

若要测试自定义绑定示例，请尝试以下终结点：/api/authors/1、/api/authors/2 (NOT FOUND)、/api/boundauthors/1、/api/boundauthors/2 (NOT FOUND)、/api/boundauthors/get/1、/api/boundauthors/get/2 (NO CONTENT)；此操作不会检查 NULL，并返回 Not Found
