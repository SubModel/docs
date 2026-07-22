# 概述

SubModel 提供专为 AI 推理、训练及通用计算打造的 Serverless GPU 与 CPU 计算方案。凭借按秒计费的定价模式,我们的平台具备无与伦比的灵活性,可动态扩展以满足 AI 工作负载的计算需求,从小型项目到企业级运营皆可胜任。

## 使用方式

* **快速部署(Quick Deploy)**:针对最热门的 AI 模型,使用预配置的自定义 Endpoint。
* **Handler 函数(Handler Functions)**:在云端部署并执行你的自定义函数。
* **vLLM Endpoint**:将任意 Hugging Face 模型直接部署到云端。

## 为什么选择 SubModel Serverless?

SubModel Serverless 实例出众之处体现在以下几个方面:

* **AI 推理**:每天可处理数百万次推理请求,并可扩展至数十亿次,是机器学习推理任务的理想方案。这确保了你的机器学习推理需求能以高性价比进行扩展。
* **AI 训练**:高效管理最长可持续 12 小时的机器学习训练任务。GPU 按需分配,任务完成后自动缩减,提供灵活高效的训练环境。
* **自动扩缩(Autoscale)**:在高可用、全球分布的 Secure Cloud 平台上,将 Worker 从 0 动态扩展到 100。这让用户能在需要时恰好获得所需的计算资源。
* **容器支持**:可将任意 Docker 容器部署到 SubModel。支持公有和私有镜像仓库,便于完整定制运行环境。
* **3 秒 cold start(冷启动)**:SubModel 会主动预热 Worker,以尽量缩短 cold start 时间。启动时间因运行时而异,例如 stable diffusion 的 cold start 为 3 秒,加上 5 秒的运行时间。
* **指标与调试**:通过访问 GPU、CPU、内存等指标,让你的计算工作负载更透明。完善的调试能力(包括日志、SSH 访问和 Web 终端)简化了故障排查。
* **Webhook**:利用 webhook 在请求完成时即时输出数据。结果会直接推送到你的 Webhook API,便于即时访问。

SubModel Serverless 不仅限于 AI 推理和训练,同样适用于渲染、分子动力学等多种其他计算任务。

## 与 SubModel Serverless 交互

SubModel 会为与你的 Serverless Pod 交互分配一个 Endpoint Id。将你的 Endpoint Id 加入 Endpoint URL 并指定一个操作(operation)。

Endpoint URL 格式如下:

```
https://api.submodel.ai/api/v1/sl/{inst_id}/{operation}
```

* `api.submodel.ai`:访问 SubModel 的基础 URL。
* `v1`:API 版本。
* `inst_id`:你的 Serverless 实例的唯一 ID。
* `operation`:在 Serverless Endpoint 上执行的操作。
  * 有效操作包括:`cancel` | `health` | `metrics` | `requests` | `status` | `run` | `runsync` | `purge-queue`
