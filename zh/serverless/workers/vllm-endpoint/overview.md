# 概览

使用 `submodel/worker-v1-vllm:stable-cuda12.1.0` image 部署一个 vLLM Worker。\
vLLM Worker 支持大多数 Hugging Face LLM,并通过指定 `MODEL_NAME` 参数兼容 OpenAI 的 API。\
你也可以使用 SubModel 的 [`input` 请求格式](https://github.com/SubModel/docs/tree/main/serverless/endpoints/send-requests/README.md)。

SubModel 的 vLLM Serverless Endpoint Worker 是一套高度优化的方案,用于释放各类 LLM 的能力。

更多信息请参阅 [vLLM Worker](https://github.com/SubModel/worker-vllm) 仓库。

## 主要特性

* **易于使用**:使用预构建的 Docker image 部署任意 LLM,无需构建自定义 Docker image、上传庞大的模型,或等待漫长的下载。
* **OpenAI 兼容**:只需修改两行代码即可无缝对接 OpenAI 的 API。支持 Chat Completions、Completions 和 Models,并同时支持流式和非流式。
* **动态批大小(Dynamic Batch Size)**:兼得无批处理带来的极快首 token 时延,以及更大批大小带来的高吞吐。(与流式输出时的 token 批处理相关)
* **广泛的模型支持**:可部署几乎任意来自 Hugging Face 的 LLM,包括你自己的自定义模型。
* **可定制**:对部署的方方面面拥有完全控制权,从模型设置、tokenizer 选项到系统配置,全部通过环境变量管理。
* **速度**:体验 vLLM Engine 的速度。
* **Serverless 的可扩展性与成本效益**:可扩展部署以处理任意数量的请求,且只为实际使用付费。

## 兼容的模型

你可以部署大多数 [来自 Hugging Face 的模型](https://huggingface.co/models?other=LLM)。\
完整的受支持模型架构列表,请参阅 [兼容的模型架构](https://github.com/SubModel/worker-vllm/blob/main/README.md#compatible-model-architectures)。

## 快速开始

从宏观上看,你可以通过以下方式设置 vLLM Worker:

* 选择你的部署选项
* 配置任何必要的环境变量
* 部署你的模型

有关设置、配置和部署 vLLM Serverless Endpoint Worker 的详细指导,包括兼容性细节、环境变量设置和使用示例,请参阅 [快速开始](https://github.com/SubModel/docs/tree/main/serverless/workers/vllm-endpoint/get-started/README.md)。

### 部署选项

* [**Configurable Endpoints**](https://github.com/SubModel/docs/tree/main/serverless/workers/vllm-endpoint/get-started/README.md#deploy-using-the-web-ui):(推荐)使用 SubModel 的 Web UI 通过 vLLM Worker 快速部署兼容 OpenAI 的 LLM。
* [**预构建的 Docker Image**](https://github.com/SubModel/docs/tree/main/serverless/workers/vllm-endpoint/get-started/README.md#deploy-using-the-worker-image):利用预配置的 Docker image 实现无忧部署。适合追求快速、直接设置流程的用户。
* **自定义 Docker Image**:面向高级用户,可自定义并构建把模型烘焙进去的 Docker image,对部署流程拥有更大的控制权。

更多信息请参阅:

* [vLLM Worker GitHub 仓库](https://github.com/SubModel/worker-vllm)
* [vLLM Worker Docker Hub](https://hub.docker.com/r/submodel/worker-vllm/tags)

有关创建自定义 Docker image 的更多信息,请参阅 [构建内置模型的 Docker Image](https://github.com/SubModel/worker-vllm/blob/main/README.md#option-2-build-docker-image-with-model-inside)。

## 后续步骤

* [快速开始](https://github.com/SubModel/docs/tree/main/serverless/workers/vllm-endpoint/get-started/README.md):了解如何将 vLLM Worker 部署为 Serverless Endpoint,并附有配置和发送请求的详细指南。
* [Configurable Endpoints](https://github.com/SubModel/docs/tree/main/serverless/workers/vllm-endpoint/configurable-endpoints/README.md):选择你的 Hugging Face 模型,让 vLLM 处理模型加载、硬件配置和执行的底层细节。
* [环境变量](https://github.com/SubModel/docs/tree/main/serverless/workers/vllm-endpoint/environment-variables/README.md):了解 vLLM Worker 可用的环境变量,包括详细文档和示例。
* [运行 Gemma 7b](https://github.com/SubModel/docs/tree/main/tutorials/serverless/gpu/run-gemma-7b/README.md):完整演示如何使用 SubModel 的 vLLM Worker 部署 Google 的 Gemma 模型,指导你为一个受限的大语言模型(LLM)设置 Serverless Endpoint。
