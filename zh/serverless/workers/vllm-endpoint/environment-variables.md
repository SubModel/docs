# 环境变量

环境变量对于配置你的 vLLM Worker 至关重要,它让你能够控制模型选择、访问凭证和运行参数,以确保最佳性能。

## CUDA 版本

为你的 vLLM Worker 使用不同的 CUDA 版本可以增强在各种硬件配置下的兼容性和性能。请在部署时根据你的具体需求选择合适的 CUDA 版本。

| CUDA 版本 | 稳定版 Image Tag | 开发版 Image Tag | 备注 |
|--------------|------------------|-----------------------|------|
| 12.1.0       | `submodel/worker-v1-vllm:stable-cuda12.1.0` | `submodel/worker-v1-vllm:dev-cuda12.1.0` | 创建 Endpoint 时,在 GPU 筛选器中选择 CUDA 版本 12.2 和 12.1。 |

该表提供了一份参考,帮助你根据所需的 CUDA 版本和 image 稳定性(稳定版或开发版)选择应使用的 image tag。请遵循 CUDA 12.1.0 兼容性的选择备注。

## 环境变量

> **注意**  
> 对于布尔值,`0` 等同于 `False`,`1` 等同于 `True`。

| 名称 | 默认值 | 类型/可选值 | 说明 |
|------|---------|--------------|-------------|
| `MODEL_NAME` | 'facebook/opt-125m' | `str` | 要使用的 Hugging Face 模型的名称或路径。 |
| `TOKENIZER` | None | `str` | 要使用的 Hugging Face tokenizer 的名称或路径。 |
| `SKIP_TOKENIZER_INIT` | False | `bool` | 跳过 tokenizer 和 detokenizer 的初始化。 |
| `TOKENIZER_MODE` | 'auto' | ['auto', 'slow'] | tokenizer 模式。 |
| `TRUST_REMOTE_CODE` | `False` | `bool` | 信任来自 Hugging Face 的远程代码。 |
| `DOWNLOAD_DIR` | None | `str` | 下载和加载权重的目录。 |
| `LOAD_FORMAT` | 'auto' | `str` | 要加载的模型权重的格式。 |
| `HF_TOKEN` | - | `str` | 用于私有和受限模型的 Hugging Face token。 |
| `DTYPE` | 'auto' | ['auto', 'half', 'float16', 'bfloat16', 'float', 'float32'] | 模型权重和激活值的数据类型。 |
| `KV_CACHE_DTYPE` | 'auto' | ['auto', 'fp8'] | KV cache 存储的数据类型。 |
| `QUANTIZATION_PARAM_PATH` | None | `str` | 包含 KV cache 缩放因子的 JSON 文件路径。 |
| `MAX_MODEL_LEN` | None | `int` | 模型上下文长度。 |
| `GUIDED_DECODING_BACKEND` | 'outlines' | ['outlines', 'lm-format-enforcer'] | 默认用于引导式解码的引擎。 |
| `DISTRIBUTED_EXECUTOR_BACKEND` | None | ['ray', 'mp'] | 用于分布式服务的后端。 |
| `WORKER_USE_RAY` | False | `bool` | 已弃用,请使用 --distributed-executor-backend=ray。 |
| `PIPELINE_PARALLEL_SIZE` | 1 | `int` | 流水线阶段数量。 |
| `TENSOR_PARALLEL_SIZE` | 1 | `int` | 张量并行副本数量。 |
| `MAX_PARALLEL_LOADING_WORKERS` | None | `int` | 分多个批次顺序加载模型。 |
| `RAY_WORKERS_USE_NSIGHT` | False | `bool` | 若指定,使用 nsight 对 Ray workers 进行性能分析。 |
| `ENABLE_PREFIX_CACHING` | False | `bool` | 启用自动前缀缓存。 |
| `DISABLE_SLIDING_WINDOW` | False | `bool` | 禁用滑动窗口,将其限制在滑动窗口大小之内。 |
| `USE_V2_BLOCK_MANAGER` | False | `bool` | 使用 BlockSpaceMangerV2。 |
| `NUM_LOOKAHEAD_SLOTS` | 0 | `int` | 推测解码所需的实验性调度配置。 |
| `SEED` | 0 | `int` | 操作的随机种子。 |
| `NUM_GPU_BLOCKS_OVERRIDE` | None | `int` | 若指定,忽略 GPU 性能分析结果并使用此数量的 GPU 块。 |
| `MAX_NUM_BATCHED_TOKENS` | None | `int` | 每次迭代批处理的最大 token 数量。 |
| `MAX_NUM_SEQS` | 256 | `int` | 每次迭代的最大序列数量。 |
| `MAX_LOGPROBS` | 20 | `int` | 当在 SamplingParams 中指定 logprobs 时返回的最大对数概率数量。 |
| `DISABLE_LOG_STATS` | False | `bool` | 禁用统计信息日志。 |
| `QUANTIZATION` | None | ['awq', 'squeezellm', 'gptq'] | 用于量化权重的方法。 |
| `ROPE_SCALING` | None | `dict` | JSON 格式的 RoPE 缩放配置。 |
| `ROPE_THETA` | None | `float` | RoPE theta。与 rope_scaling 配合使用。 |
| `TOKENIZER_POOL_SIZE` | 0 | `int` | 用于异步分词的 tokenizer 池大小。 |
| `TOKENIZER_POOL_TYPE` | 'ray' | `str` | 用于异步分词的 tokenizer 池类型。 |
| `TOKENIZER_POOL_EXTRA_CONFIG` | None | `dict` | tokenizer 池的额外配置。 |
| `ENABLE_LORA` | False | `bool` | 若为 True,启用对 LoRA adapter 的处理。 |
| `MAX_LORAS` | 1 | `int` | 单个批次中 LoRA 的最大数量。 |
| `MAX_LORA_RANK` | 16 | `int` | 最大 LoRA rank。 |
| `LORA_EXTRA_VOCAB_SIZE` | 256 | `int` | LoRA adapter 额外词表的最大大小。 |
| `LORA_DTYPE` | 'auto' | ['auto', 'float16', 'bfloat16', 'float32'] | LoRA 的数据类型。 |
| `LONG_LORA_SCALING_FACTORS` | None | `tuple` | 为 LoRA adapter 指定多个缩放因子。 |
| `MAX_CPU_LORAS` | None | `int` | 存储在 CPU 内存中的 LoRA 最大数量。 |
| `FULLY_SHARDED_LORAS` | False | `bool` | 启用完全分片的 LoRA 层。 |
| `SCHEDULER_DELAY_FACTOR` | 0.0 | `float` | 在调度下一个 prompt 之前应用一个延迟。 |
| `ENABLE_CHUNKED_PREFILL` | False | `bool` | 启用分块 prefill 请求。 |
| `SPECULATIVE_MODEL` | None | `str` | 推测解码中要使用的草稿模型的名称。 |
| `NUM_SPECULATIVE_TOKENS` | None | `int` | 从草稿模型中采样的推测 token 数量。 |
| `SPECULATIVE_DRAFT_TENSOR_PARALLEL_SIZE` | None | `int` | 草稿模型的张量并行副本数量。 |
| `SPECULATIVE_MAX_MODEL_LEN` | None | `int` | 草稿模型支持的最大序列长度。 |
| `SPECULATIVE_DISABLE_BY_BATCH_SIZE` | None | `int` | 若入队请求数量大于此值,则禁用推测解码。 |
| `NGRAM_PROMPT_LOOKUP_MAX` | None | `int` | 推测解码中 ngram prompt 查找窗口的最大大小。 |
| `NGRAM_PROMPT_LOOKUP_MIN` | None | `int` | 推测解码中 ngram prompt 查找窗口的最小大小。 |
| `SPEC_DECODING_ACCEPTANCE_METHOD` | 'rejection_sampler' | ['rejection_sampler', 'typical_acceptance_sampler'] | 指定推测解码中草稿 token 验证的接受方法。 |
| `TYPICAL_ACCEPTANCE_SAMPLER_POSTERIOR_THRESHOLD` | None | `float` | 设置 token 被接受的后验概率下界阈值。 |
| `TYPICAL_ACCEPTANCE_SAMPLER_POSTERIOR_ALPHA` | None | `float` | 用于 token 接受的基于熵的阈值的缩放因子。 |
| `MODEL_LOADER_EXTRA_CONFIG` | None | `dict` | 模型加载器的额外配置。 |
| `PREEMPTION_MODE` | None | `str` | 若为 'recompute',引擎执行感知抢占的重新计算。若为 'save',引擎在发生抢占时将激活值保存到 CPU 内存中。 |
| `PREEMPTION_CHECK_PERIOD` | 1.0 | `float` | 引擎检查是否发生抢占的频率。 |
| `PREEMPTION_CPU_CAPACITY` | 2 | `float` | 用于保存激活值的 CPU 内存百分比。 |
| `DISABLE_LOGGING_REQUEST` | False | `bool` | 禁用请求日志。 |
| `MAX_LOG_LEN` | None | `int` | 日志中打印的 prompt 字符或 prompt ID 数字的最大数量。 |

### Tokenizer 设置

| 名称 | 默认值 | 类型/可选值 | 说明 |
|------|---------|--------------|-------------|
| `TOKENIZER_NAME` | `None` | `str` | 用于使用不同于模型默认 tokenizer 的 tokenizer 仓库。 |
| `TOKENIZER_REVISION` | `None` | `str` | 要加载的 tokenizer 版本。 |
| `CUSTOM_CHAT_TEMPLATE` | `None` | 单行 jinja template 的 `str` | 自定义 chat jinja template。[更多信息](https://huggingface.co/docs/transformers/chat_templating) |

### 系统、GPU 和张量并行(多 GPU)设置

| 名称 | 默认值 | 类型/可选值 | 说明 |
|------|---------|--------------|-------------|
| `GPU_MEMORY_UTILIZATION` | `0.95` | `float` | 设置 GPU VRAM 利用率。 |
| `MAX_PARALLEL_LOADING_WORKERS` | `None` | `int` | 分多个批次顺序加载模型,以避免在使用张量并行和大模型时发生 RAM OOM。 |
| `BLOCK_SIZE` | `16` | `8`、`16`、`32` | 连续 token 块的 token 块大小。 |
| `SWAP_SPACE` | `4` | `int` | 每个 GPU 的 CPU 交换空间大小(GiB)。 |
| `ENFORCE_EAGER` | False | `bool` | 始终使用 eager 模式 PyTorch。若为 False(`0`),将混合使用 eager 模式和 CUDA graph 以获得最大的性能和灵活性。 |
| `MAX_SEQ_LEN_TO_CAPTURE` | `8192` | `int` | CUDA graph 覆盖的最大上下文长度。当序列的上下文长度大于此值时,我们回退到 eager 模式。 |
| `DISABLE_CUSTOM_ALL_REDUCE` | `0` | `int` | 启用或禁用自定义 all reduce。 |

### 流式批大小设置

| 名称 | 默认值 | 类型/可选值 | 说明 |
|------|---------|--------------|-------------|
| `DEFAULT_BATCH_SIZE` | `50` | `int` | token 流式传输的默认和最大批大小,用于减少 HTTP 调用。 |
| `DEFAULT_MIN_BATCH_SIZE` | `1` | `int` | 第一次请求的批大小,后续每次请求都会将其乘以增长因子。 |
| `DEFAULT_BATCH_SIZE_GROWTH_FACTOR` | `3` | `float` | 动态批大小的增长因子。 |

> **注意**  
> 第一次请求的批大小为 `DEFAULT_MIN_BATCH_SIZE`,后续每次请求的批大小为 `previous_batch_size * DEFAULT_BATCH_SIZE_GROWTH_FACTOR`。此过程持续进行,直到批大小达到 `DEFAULT_BATCH_SIZE`。例如,对于默认值,批大小将是 `1, 3, 9, 27, 50, 50, 50, ...`。你也可以按请求指定这些值,输入为 `max_batch_size`、`min_batch_size` 和 `batch_size_growth_factor`。这与 vLLM 的内部批处理无关,而是指 worker 每次 HTTP 请求发送的 token 数量。

### OpenAI 设置

| 名称 | 默认值 | 类型/可选值 | 说明 |
|------|---------|--------------|-------------|
| `RAW_OPENAI_OUTPUT` | `1` | 以 `int` 表示的布尔值 | 在流式传输时启用原始 OpenAI SSE 格式字符串输出。为兼容 OpenAI **必须**启用(默认即已启用)。 |
| `OPENAI_SERVED_MODEL_NAME_OVERRIDE` | `None` | `str` | 将所服务模型的名称从模型仓库/路径覆盖为指定名称,随后你便可在发起 OpenAI 请求时将该值用作 `model` 参数。 |
| `OPENAI_RESPONSE_ROLE` | `assistant` | `str` | OpenAI Chat Completions 中 LLM 响应的角色。 |

### Serverless 设置

| 名称 | 默认值 | 类型/可选值 | 说明 |
|------|---------|--------------|-------------|
| `MAX_CONCURRENCY` | `300` | `int` | 每个 worker 的最大并发请求数。vLLM 有内部队列,因此你无需担心按 VRAM 限制,此项用于提升扩展/负载均衡效率。 |
| `DISABLE_LOG_STATS` | False | `bool` | 启用或禁用 vLLM 统计信息日志。 |
| `DISABLE_LOG_REQUESTS` | False | `bool` | 启用或禁用 vLLM 请求日志。 |

> **注意**  
> 如果你在使用 Mixtral 8x7B、量化模型或处理不寻常的模型/架构时遇到问题,请尝试将 `TRUST_REMOTE_CODE` 设置为 `1`。
