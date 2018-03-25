## <a name="overview"></a>概述

本教程将创建以下 API：

|API | 描述 | 请求正文 | 响应正文 |
|--- | ---- | ---- | ---- |
|GET /api/todo | 获取所有待办事项 | 无 | 待办事项的数组|
|GET /api/todo/{id} | 按 ID 获取项 | 无 | 待办事项|
|POST /api/todo | 添加新项 | 待办事项 | 待办事项 |
|PUT /api/todo/{id} | 更新现有项 &nbsp; | 待办事项 | 无 |
|DELETE /api/todo/{id} &nbsp; &nbsp; | 删除项&nbsp; &nbsp; | 无 | 无|

<br>

下图显示了应用的基本设计。

![客户端提交请求并从应用程序接收响应，客户端由左侧的框表示，应用程序则由右侧的框表示。 在应用程序框内，三个框分别代表控制器、模型和数据访问层。 请求进入应用程序的控制器，读/写操作是在控制器和数据访问层之间进行的。 模型被序列化并在响应中被返回给客户端。](../../tutorials/first-web-api/_static/architecture.png)

* 该客户端是使用 Web API（移动应用、浏览器等）的对象。 本教程不会创建客户端。 [Postman](https://www.getpostman.com/) 或 [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) 是用作测试应用的客户端。

* 模型是表示应用程序中的数据的对象。 在此示例中，唯一的模型是待办事项。 模型表示为 C# 类，也称为 Plain Old C#  Object (POCO)。

* 控制器是处理 HTTP 请求并创建 HTTP 响应的对象。 此应用程序具有单个控制器。

* 为了简化教程，应用不会使用永久数据库。 示例应用将待办事项存储在内存数据库中。
