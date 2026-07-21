# 打包并部署 image

有了 Handler 函数之后,下一步就是把它打包成一个可以作为可扩展 Serverless Worker 部署的 Docker image。这一步通过定义一个 Dockerfile 来完成,把运行 handler 所需的一切导入进来。示例 Dockerfile 可在 GitHub 上的 [submodel](https://github.com/orgs/submodel/repositories) 仓库中找到。

> **注意**  
> 对于部署大语言模型(LLM),你可以使用 [Configurable Endpoints](/serverless/workers/vllm/configurable-endpoints.md) 功能,而无需直接与 Docker 打交道。  
> Configurable Endpoints 通过让你选择一个预配置的 template 并按需自定义,简化了部署流程。

*不熟悉 Docker?可查看 Docker 的[概览页面](https://docs.docker.com/get-started/overview/)。*

## Dockerfile

假设我们有一个如下所示的目录:

```
project_directory
├── Dockerfile
├── src
│   └── handler.py
└── builder
    └── requirements.txt
```

你的 Dockerfile 大致会是这样:

```dockerfile
FROM python:3.11.1-buster

WORKDIR /

COPY builder/requirements.txt .
RUN pip install -r requirements.txt

ADD handler.py .

CMD [ "python", "-u", "/handler.py" ]
```

要构建并推送该 image,请参考 [快速开始](/serverless/workers/overview.md) 中的步骤。

> 🚧 如果你的 handler 需要外部文件(例如模型权重),务必把它们缓存进你的 Docker image。你的目标是打造一个完全自包含的 worker,运行时无需下载或获取外部文件。

## 持续集成(Continuous Integration)

通过持续集成来整合你的 Handler 函数。

[Test Runner](https://github.com/submodel-workers/test-runner) GitHub Action 用于测试你的 Handler 函数并将其集成到你的应用中。

> **注意**  
> 运行任何向 SubModel 发送请求的 Action 都会产生费用。

你可以在工作流文件中添加以下内容:

```yaml
- uses: actions/checkout@v3
- name: Run Tests
  uses: submodel/submodel-test-runner@v1
  with:
    image-tag: [tag of image to test]
    submodel-api-key: [a valid SubModel API key]
    test-filename: [path for a JSON file containing a list of tests, defaults to .github/tests.json]
    request-timeout: [number of seconds to wait on each request before timing out, defaults to 300]
```

如果省略 `test-filename`,Test Runner Action 会尝试在 `.github/tests.json` 处查找测试文件。

你可以在 [Worker Template 仓库](https://github.com/submodel/worker-template/tree/main/.github) 中找到一个可用的示例。

## 使用 Docker 标签

我们也强烈建议为 Docker image 使用标签,而不要依赖默认的 `:latest` 标签。这会让版本追踪和发布更新变得显著更容易。

### Docker image 版本管理

为确保 Docker image 版本一致且可靠,我们强烈建议使用 SHA 标签,而不是依赖默认的 `:latest` 标签。

使用 SHA 标签有以下几个好处:

- **版本控制:** SHA 标签为每个 image 版本提供唯一标识符,便于追踪变更和更新。
- **可复现性:** 通过使用 SHA 标签,你可以确保在不同环境中使用同一个 image 版本,降低不一致的风险。
- **安全性:** SHA 标签有助于防止意外覆盖,并确保你使用的是预期的 image 版本。

### 使用 SHA 标签

要通过 SHA 标签拉取 Docker image,使用以下命令:

```bash
docker pull <image_name>@<sha256:hash>
```

例如:

```bash
docker pull myapp@sha256:4d3d4b3c5a5c2b3a5a5c3b2a5a4d2b3a2b3c5a3b2a5d2b3a3b4c3d3b5c3d4a3
```

### 最佳实践

- 避免使用 `:latest` 标签,因为它可能导致不可预测的行为,并使得难以追踪正在使用的是哪个 image 版本。
- 结合语义化版本(例如 `v1.0.0`、`v1.1.0`)和 SHA 标签使用,以提供清晰且有意义的版本标识。
- 记录每次部署所用的 SHA 标签,以便于回滚和版本管理。

## 其他考虑

虽然我们不对 Docker image 大小设限,但你的容器镜像仓库可能有限制。请务必查看它们可能存在的任何限制。理想情况下,你希望让最终的 Docker image 尽可能小,只包含运行 handler 所绝对必需的最少内容。
