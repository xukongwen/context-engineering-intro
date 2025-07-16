## 功能：

- 一个 Pydantic AI 代理，它有另一个 Pydantic AI 代理作为工具。
- 主代理为研究代理，子代理为电子邮件草稿代理。
- 用于与代理交互的 CLI。
- 电子邮件草稿代理使用 Gmail，研究代理使用 Brave API。

## 示例：

在 `examples/` 文件夹中，有一个 README 供你阅读以了解示例的全部内容，以及在创建上述功能的文档时如何构建你自己的 README。

- `examples/cli.py` - 使用它作为创建 CLI 的模板
- `examples/agent/` - 阅读这里的所有文件，了解创建支持不同提供商和 LLM 的 Pydantic AI 代理的最佳实践、处理代理依赖项以及向代理添加工具。

不要直接复制这些示例中的任何一个，它们是针对完全不同的项目的。但可以将其作为灵感和最佳实践的参考。

## 文档：

Pydantic AI 文档：https://ai.pydantic.dev/

## 其他注意事项：

- 包含 .env.example、带有设置说明的 README，包括如何配置 Gmail 和 Brave。
- 在 README 中包含项目结构。
- 虚拟环境已经设置好了必要的依赖项。
- 使用 python_dotenv 和 load_env() 管理环境变量