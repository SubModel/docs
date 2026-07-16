# OpenAI Compatibility

vLLM Worker 与 OpenAI 的 API 完全兼容,你可以像调用 OpenAI API 一样,用同样的代码与 vLLM Worker 交互。这种兼容性确保了无缝的迁移与集成体验。

## 约定

### Completions Endpoint
completions endpoint 为单个 prompt 提供补全,接受单个字符串作为输入。

### Chat Completions
chat completions endpoint 针对给定的对话提供响应,要求输入采用与消息历史相对应的特定格式。

选择最适合你使用场景的约定即可。

### 模型名称
所有请求都需要 `MODEL_NAME` 环境变量。在向 vLLM Worker 发送请求时使用该值。

模型名称示例包括 `openchat/openchat-3.5-0106`、`mistral:latest` 以及 `llama2:70b`。

要用指定模型为给定 prompt 生成响应,请使用流式(streaming)endpoint。这将产生一系列响应,其中最终的响应对象会包含统计信息以及请求的附加数据。

## 参数

### Chat Completions
在使用 vLLM Serverless Endpoint Worker 的 chat completion 功能时,你可以通过以下参数定制请求:

<details>
<summary>支持的 Chat Completions 输入及说明</summary>

| 参数 | 类型 | 默认值 | 说明 |
|-----------|------|---------------|-------------|
| `messages` | Union[str, List[Dict[str, str]]] |  | 消息列表,其中每条消息是一个包含 `role` 和 `content` 的字典。模型的 chat template 会自动应用到消息上,因此模型必须具备 chat template,或通过 `CUSTOM_CHAT_TEMPLATE` 环境变量指定。 |
| `model` | str |  | 你在 SubModel Serverless Endpoint 上部署的模型仓库。如果你不确定名称,或是将模型直接打包进 image,请参考 **Examples: Using your SubModel endpoint with OpenAI** 章节中的指南获取可用模型列表。 |
| `temperature` | Optional[float] | 0.7 | 控制采样随机性的浮点数。较低的值使模型更确定,较高的值使模型更随机。零表示贪婪采样。 |
| `top_p` | Optional[float] | 1.0 | 控制所考虑的 top token 累计概率的浮点数。必须在 (0, 1] 范围内。设为 1 表示考虑所有 token。 |
| `n` | Optional[int] | 1 | 为给定 prompt 返回的输出序列数量。 |
| `max_tokens` | Optional[int] | None | 每个输出序列生成的最大 token 数。 |
| `seed` | Optional[int] | None | 用于生成的随机种子。 |
| `stop` | Optional[Union[str, List[str]]] | list | 一组字符串,当它们被生成时会停止生成。返回的输出不会包含这些停止字符串。 |
| `stream` | Optional[bool] | False | 是否流式输出。 |
| `presence_penalty` | Optional[float] | 0.0 | 根据新 token 是否已出现在已生成文本中对其进行惩罚的浮点数。值 > 0 鼓励模型使用新 token,值 < 0 鼓励模型重复 token。 |
| `frequency_penalty` | Optional[float] | 0.0 | 根据新 token 在已生成文本中出现的频率对其进行惩罚的浮点数。值 > 0 鼓励模型使用新 token,值 < 0 鼓励模型重复 token。 |
| `logit_bias` | Optional[Dict[str, float]] | None | vLLM 不支持。 |
| `user` | Optional[str] | None | vLLM 不支持。 |

### vLLM 支持的额外参数

| 参数 | 类型 | 默认值 | 说明 |
|-----------|------|---------------|-------------|
| `best_of` | Optional[int] | None | 从 prompt 生成的输出序列数量。从这 `best_of` 个序列中返回最优的 `n` 个序列。`best_of` 必须大于或等于 `n`。当 `use_beam_search` 为 True 时,该值被视为 beam width。默认情况下 `best_of` 等于 `n`。 |
| `top_k` | Optional[int] | -1 | 控制所考虑的 top token 数量的整数。设为 -1 表示考虑所有 token。 |
| `ignore_eos` | Optional[bool] | False | 是否忽略 EOS token 并在其生成后继续生成 token。 |
| `use_beam_search` | Optional[bool] | False | 是否使用 beam search 而非采样。 |
| `stop_token_ids` | Optional[List[int]] | list | 一组 token,当它们被生成时会停止生成。返回的输出会包含这些停止 token,除非它们是特殊 token。 |
| `skip_special_tokens` | Optional[bool] | True | 是否在输出中跳过特殊 token。 |
| `spaces_between_special_tokens` | Optional[bool] | True | 是否在输出的特殊 token 之间添加空格。默认为 True。 |
| `add_generation_prompt` | Optional[bool] | True | 详见[此处](https://huggingface.co/docs/transformers/main/en/chat_templating#what-are-generation-prompts)。 |
| `echo` | Optional[bool] | False | 在补全内容之外回显 prompt。 |
| `repetition_penalty` | Optional[float] | 1.0 | 根据新 token 是否出现在 prompt 及已生成文本中对其进行惩罚的浮点数。值 > 1 鼓励模型使用新 token,值 < 1 鼓励模型重复 token。 |
| `min_p` | Optional[float] | 0.0 | 表示某 token 被考虑所需的最小概率(相对于最可能的 token)的浮点数。必须在 [0, 1] 范围内。设为 0 表示禁用。 |
| `length_penalty` | Optional[float] | 1.0 | 根据序列长度对其进行惩罚的浮点数。用于 beam search。 |
| `include_stop_str_in_output` | Optional[bool] | False | 是否在输出文本中包含停止字符串。默认为 False。 |

</details>

### Completions
在使用 completions 功能时,你可以通过以下参数定制请求:

<details>
<summary>支持的 Completions 输入及说明</summary>

| 参数 | 类型 | 默认值 | 说明 |
|-----------|------|---------------|-------------|
| `model` | str |  | 你在 SubModel Serverless Endpoint 上部署的模型仓库。如果你不确定名称,或是将模型直接打包进 image,请参考 **Examples: Using your SubModel endpoint with OpenAI** 章节中的指南获取可用模型列表。 |
| `prompt` | Union[List[int], List[List[int]], str, List[str]] |  | 作为模型输入的字符串、字符串数组、token 数组或 token 数组的数组。 |
| `suffix` | Optional[str] | None | 追加到所生成文本末尾的字符串。 |
| `max_tokens` | Optional[int] | 16 | 每个输出序列生成的最大 token 数。 |
| `temperature` | Optional[float] | 1.0 | 控制采样随机性的浮点数。较低的值使模型更确定,较高的值使模型更随机。零表示贪婪采样。 |
| `top_p` | Optional[float] | 1.0 | 控制所考虑的 top token 累计概率的浮点数。必须在 (0, 1] 范围内。设为 1 表示考虑所有 token。 |
| `n` | Optional[int] | 1 | 为给定 prompt 返回的输出序列数量。 |
| `stream` | Optional[bool] | False | 是否流式输出。 |
| `logprobs` | Optional[int] | None | 每个输出 token 返回的对数概率数量。 |
| `echo` | Optional[bool] | False | 是否在补全内容之外回显 prompt。 |
| `stop` | Optional[Union[str, List[str]]] | list | 一组字符串,当它们被生成时会停止生成。返回的输出不会包含这些停止字符串。 |
| `seed` | Optional[int] | None | 用于生成的随机种子。 |
| `presence_penalty` | Optional[float] | 0.0 | 根据新 token 是否已出现在已生成文本中对其进行惩罚的浮点数。值 > 0 鼓励模型使用新 token,值 < 0 鼓励模型重复 token。 |
| `frequency_penalty` | Optional[float] | 0.0 | 根据新 token 在已生成文本中出现的频率对其进行惩罚的浮点数。值 > 0 鼓励模型使用新 token,值 < 0 鼓励模型重复 token。 |
| `best_of` | Optional[int] | None | 从 prompt 生成的输出序列数量。从这 `best_of` 个序列中返回最优的 `n` 个序列。`best_of` 必须大于或等于 `n`。该参数会影响输出的多样性。 |
| `logit_bias` | Optional[Dict[str, float]] | None | token ID 到偏置值的字典。 |
| `user` | Optional[str] | None | 用于个性化响应的用户标识。(vLLM 不支持) |

### vLLM 支持的额外参数

| 参数 | 类型 | 默认值 | 说明 |
|-----------|------|---------------|-------------|
| `top_k` | Optional[int] | -1 | 控制所考虑的 top token 数量的整数。设为 -1 表示考虑所有 token。 |
| `ignore_eos` | Optional[bool] | False | 是否忽略 End Of Sentence token 并在其生成后继续生成 token。 |
| `use_beam_search` | Optional[bool] | False | 是否使用 beam search 而非采样来生成输出。 |
| `stop_token_ids` | Optional[List[int]] | list | 一组 token,当它们被生成时会停止生成。返回的输出会包含这些停止 token,除非它们是特殊 token。 |
| `skip_special_tokens` | Optional[bool] | True | 是否在输出中跳过特殊 token。 |
| `spaces_between_special_tokens` | Optional[bool] | True | 是否在输出的特殊 token 之间添加空格。默认为 True。 |
| `repetition_penalty` | Optional[float] | 1.0 | 根据新 token 是否出现在 prompt 及已生成文本中对其进行惩罚的浮点数。值 > 1 鼓励模型使用新 token,值 < 1 鼓励模型重复 token。 |
| `min_p` | Optional[float] | 0.0 | 表示某 token 被考虑所需的最小概率(相对于最可能的 token)的浮点数。必须在 [0, 1] 范围内。设为 0 表示禁用。 |
| `length_penalty` | Optional[float] | 1.0 | 根据序列长度对其进行惩罚的浮点数。用于 beam search。 |
| `include_stop_str_in_output` | Optional[bool] | False | 是否在输出文本中包含停止字符串。默认为 False。 |

</details>

## 初始化你的项目

首先用你的 SubModel API Key 和 Endpoint URL 设置 OpenAI Client。

```python
from openai import OpenAI
import os

# Initialize the OpenAI Client with your SubModel API Key and Endpoint URL
client = OpenAI(
    api_key=os.environ.get("SUBMODEL_API_KEY"),
    base_url=f"https://api.SubModel.ai/v1/sl/{SUBMODEL_ENDPOINT_ID}/openai/v1",
)
```

client 初始化完成后,你就可以开始向 SubModel Serverless Endpoint 发送请求了。

## 生成一个请求

你可以利用 LLM 实现指令跟随和对话能力。这适用于多种开源对话和指令模型,例如:

- `meta-llama/Llama-2-7b-chat-hf`
- `mistralai/Mixtral-8x7B-Instruct-v0.1`
- 以及更多

对于本身并非为对话和指令任务设计的模型,可通过 `CUSTOM_CHAT_TEMPLATE` 环境变量指定自定义 chat template 来进行适配。

更多信息请参见 [OpenAI 文档](https://platform.openai.com/docs/guides/text-generation)。

### 流式响应

为实现与模型的实时交互,创建一个 chat completion 流。此方法非常适合需要即时反馈的应用。

```python
# Create a chat completion stream
response_stream = client.chat.completions.create(
    model=MODEL_NAME,
    messages=[{"role": "user", "content": "Why is SubModel the best platform?"}],
    temperature=0,
    max_tokens=100,
    stream=True,
)

# Stream the response
for response in response_stream:
    print(chunk.choices[0].delta.content or "", end="", flush=True)
```

### 非流式响应

你也可以返回一个同步的非流式响应,用于批处理,或当单个整合后的响应已足够时。

```python
# Create a chat completion
response = client.chat.completions.create(
    model=MODEL_NAME,
    messages=[{"role": "user", "content": "Why is SubModel the best platform?"}],
    temperature=0,
    max_tokens=100,
)

# Print the response
print(response.choices[0].message.content)
```

## 生成一个 Chat Completion

此方法专为支持文本补全的模型定制。它会用一段续写的输出流来补全你的输入,不同于交互式的对话格式。

### 流式响应

启用流式以获得连续、实时的输出。这种方式对动态交互或监控进行中的过程很有帮助。

```python
# Create a completion stream
response_stream = client.completions.create(
    model=MODEL_NAME,
    prompt="SubModel is the best platform because",
    temperature=0,
    max_tokens=100,
    stream=True,
)

# Stream the response
for response in response_stream:
    print(response.choices[0].text or "", end="", flush=True)
```

### 非流式响应

当单个整合后的响应即可满足需求时,选择非流式方法。

```python
# Create a completion
response = client.completions.create(
    model=MODEL_NAME,
    prompt="SubModel is the best platform because",
    temperature=0,
    max_tokens=100,
)

# Print the response
print(response.choices[0].text)
```

## 获取可用模型列表

你可以列出可用的模型。

```python
models_response = client.models.list()
list_of_models = [model.id for model in models_response]
print(list_of_models)
```
