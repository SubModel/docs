# 概述

Submodel.ai 是一个专为 AI、机器学习应用以及通用计算需求打造的云计算平台。

以令人惊叹的价格体验企业级云计算——我们的解决方案能提供与其他领先厂商相当的性能，而成本仅为它们的 20%。

通过我们的 [Pods](pods/overview.md)、[Serverless](serverless/overview.md)、[裸金属服务器](baremetal/overview.md) 等多种计算方式，借助平台上的 GPU 和 CPU 资源来运行你的代码。

立即 [注册账户](https://submodel.ai/#/login?redirect=%2Fdashboard) 开始使用。

## 什么是 Pods?

Pods 让你能够使用容器在 GPU 实例上运行代码。

我们的 GPU 全部托管在 T3/T4 认证的 Datacenter 中，保障了企业级的可靠性与安全基础设施。

## 什么是 Serverless?

Serverless 为你的生产环境提供按秒计费、具备自动扩缩容能力的无服务器计算。

定义一个 Worker，为它创建一个 REST API Endpoint，将任务排入队列，并享受按需自动扩缩容。该服务确保了较低的 cold start（冷启动）耗时和稳健的安全措施。

### 从这里开始：

- [构建你自己的 Worker 镜像](serverless/workers/deploy/package-and-deploy-an-image.md)
- [用 vLLM worker 运行任意 LLM](serverless/workers/vllm-endpoint/get-started.md)

## 什么是网络存储卷?

网络存储卷是独立于 Pod 生命周期的持久化网络存储，可在不同 Pod 之间共享和迁移。适合存储需要长期保留或跨 Pod 复用的模型文件和数据集。

了解更多：[网络存储卷文档](storage/overview.md)

## 什么是裸金属服务器?

裸金属服务器提供专用物理服务器租用，用户独占全部硬件资源，零虚拟化损耗，适合对性能、延迟或硬件控制有极高要求的大规模 AI 训练和高性能计算任务。

了解更多：[裸金属服务器文档](baremetal/overview.md)

## 什么是自定义模板?

自定义模板允许你将常用的 Docker 镜像和启动配置保存为模板，创建 Pod 时一键选用，无需重复填写配置。

了解更多：[自定义模板文档](templates/overview.md)

## 我们的使命

Submodel.ai 致力于让所有人都能负担得起、便捷地使用云计算，同时不在功能、易用性或体验上做出妥协。我们用前沿技术赋能个人和企业，释放 AI 与云计算的潜能。

如有一般咨询，请浏览我们的文档或与我们联系。更多信息请见我们的 [GitHub](https://github.com/SubModel)。

## 接下来去哪里?

进一步了解 Submodel.ai：

- [创建账户](https://submodel.ai/#/login?redirect=%2Finst%2Flist)
- [为账户充值](Get%20started/Billing%20information.md)
- [运行你的第一个教程](Get%20started/Get%20started.md)
- [我们的产品与价格](https://submodel-gpu-cloud-gkztjf0.gamma.site/)
