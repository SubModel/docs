# Configurable Endpoints

SubModel 的 Configurable Endpoints 功能让用户能够通过集成 vLLM(一个高性能推理引擎)来无缝部署任意大语言模型(LLM)。

当选择 **Serverless vLLM** 部署选项时,SubModel 会借助 vLLM 的高级能力,高效地加载并执行你指定的 Hugging Face 模型。此集成通过屏蔽复杂的技术细节来简化部署流程,让你可以专注于模型选择和参数定制,而由 vLLM 处理模型加载、硬件优化和执行等繁琐环节。

## LLM 部署分步指南

1. **发起部署**\
   进入 **Explore** 区域并选择 **Serverless vLLM**,开始部署你所选择的大语言模型。
2. **配置 vLLM 部署**\
   在 vLLM 部署弹窗中,提供以下信息:
   * 选择你偏好的 vLLM 版本
   * 指定 Hugging Face LLM 仓库名称
   * (可选)提供你的 Hugging Face 认证 token
3. **审查 vLLM 参数**\
   进入下一步,检查并确认 vLLM 配置设置。
4. **自定义 Endpoint 参数**\
   在 Endpoint 配置页面:
   * **Worker 配置**:通过指定你偏好的 GPU 选择顺序来确定 GPU 分配优先级
   * **Worker 规格**:定义 Active Workers、Max Workers 数量以及每个 Worker 的 GPU 数量
   * **容器配置**:
     * (可选)选择一个预配置的 Template
     * 确认 Container Image 使用了你所需的 CUDA 版本
     * 指定所需的 Container Disk 大小
     * 审查并确认环境变量
5. **完成部署**\
   通过选择 **Deploy** 来完成整个流程,启动模型部署。

部署成功后,你的 LLM 将通过指定的 Endpoint 访问,可用于基于 API 的交互和推理任务。

> **重要提示**\
> SubModel 的 Configurable Endpoints 支持任何与 [vLLM](https://github.com/vllm-project/vllm) 兼容的模型架构,在保持最佳性能的同时提供灵活的模型部署能力。

此增强版本在保留原始技术内容的同时,提升了可读性、结构和专业性。信息现以更有条理、更易理解的格式呈现,章节划分清晰,步骤之间衔接流畅。技术术语得以保留,同时对说明略作扩展以便更好地理解。
