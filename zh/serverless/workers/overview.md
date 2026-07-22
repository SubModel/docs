# 概览

Worker 在云端运行你的代码。

## 核心特性

* **全托管执行(Fully Managed Execution)**:SubModel 负责底层基础设施,你的代码在被触发时即可运行,无需任何服务器搭建或维护。
* **自动扩缩(Automatic Scaling)**:平台会根据负载自动对你的函数进行扩容或缩容,确保资源的高效利用。
* **多语言 API 支持**:SubModel 的 REST API 遵循行业标准(OpenAPI 3.0),为 10 多种语言提供自动生成的客户端库,同时保留直接 HTTP 访问能力。
* **无缝集成**:代码上传后,SubModel 会提供一个 Endpoint,让你能轻松地将 Handler 函数(Handler Functions)集成到应用的任意环节。

## 快速开始

开始使用 SubModel Worker:

1. **编写你的函数**:用受支持的语言编写你的 Handler 函数。
2. **部署到 SubModel**:将你的 Handler 函数上传到 SubModel。
3. **集成并执行**:使用所提供的 Endpoint 集成到你的应用中。
