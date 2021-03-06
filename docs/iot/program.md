# 编程模型


您将使用 AWS Lambda 支持的语言之一为 Lambda 函数编写代码。无论您选择哪种语言，都可以采用一个通用模式，即为包含以下核心概念的 Lambda 函数编写代码：

处理程序 – 处理程序是 AWS Lambda 调用的用于开始执行 Lambda 函数的函数。您在创建 Lambda 函数时标识处理程序。在调用 Lambda 函数时，AWS Lambda 将通过调用处理程序函数来开始执行您的代码。AWS Lambda 将任何事件数据作为第一个参数传递给处理程序。您的处理程序应处理此传入事件数据，并且可调用您的代码中的任何其他函数/方法。

上下文 – AWS Lambda 还会将上下文对象作为第二个参数传递给处理程序函数。通过此 context 对象，您的代码可与 AWS Lambda 交互。例如，您的代码可在 AWS Lambda 终止 Lambda 函数前发现剩余的执行时间。

此外，对于 Node.js 等语言，有一个使用回调的异步平台。AWS Lambda 提供了有关此 context 对象的其他方法。您将使用这些 context 对象方法指示 AWS Lambda 终止 Lambda 函数并将值返回到调用方（可选）。

日志记录 – 您的 Lambda 函数可包含日志记录语句。AWS Lambda 将这些日志写入 CloudWatch Logs。特定语言语句将生成日志条目，具体取决于您编写 Lambda 函数代码所用的语言。

日志记录需要遵循 CloudWatch Logs 限制。日志数据会由于限制而丢失；在某些情况下，当执行上下文终止时也会丢失。

异常 – 您的 Lambda 函数需要将函数执行的结果传递给 AWS Lambda。根据您编写 Lambda 函数代码所用的语言，成功结束请求或向 AWS Lambda 通知执行期间出现了错误有几种不同的方式。如果您同步调用该函数，则 AWS Lambda 会将结果转发回客户端。

并发 – 当您的函数的调用速度快于函数的单个实例可处理事件的速度时，Lambda 会通过运行其他实例来扩展您的函数。您的函数的每个实例每次仅处理一个请求，因此您无需担心同步线程或进程的问题。不过，您可以使用异步语言功能并行处理事件批次，并将数据保存到 /tmp 目录以便在同一实例上的未来调用中使用。

必须以无状态风格编写 Lambda 函数代码，且与底层的计算基础设施不存在关联。您的代码应该要求将本地文件系统访问权限、子流程和类似项目限制为请求的生命周期。持久状态应存储在 Amazon S3、Amazon DynamoDB 或其他云存储服务中。 要求函数无状态可使 AWS Lambda 根据需要启动合适数量的函数副本，以根据传入事件和请求的速率调整规模。对于不同的请求，这些函数可能不会始终运行在同一个计算实例上，且 AWS Lambda 可能会多次使用 Lambda 函数的给定实例。有关更多信息，请参阅 使用 AWS Lambda 函数的最佳实践。
