# SubModel 与 vLLM 快速开始

SubModel 提供了一种简化的方式,使用预配置的 vLLM Docker image 将大语言模型(LLM)部署为 Serverless Endpoint。本指南将带你在 SubModel 上部署一个搭载 vLLM 推理引擎、兼容 OpenAI 的 endpoint。

## 前置条件

在开始之前,请确保你已具备以下条件:

* 一个 SubModel 账户
* 一个 Hugging Face token(访问受限模型时必需)
* 已安装 OpenAI 或其他代码示例所需的库

## 通过 Web UI 部署

你可以使用 SubModel 的 Web UI 直接从 Hugging Face 部署一个带模型的 vLLM Worker。

1. 登录你的 SubModel 账户并进入 [Serverless 页面](https://submodel.ai/#/serverless/list)。
2. 在 **Quick Deploy** 下找到 **Serverless vLLM** 并点击 **Start**。

你现在会进入 vLLM 模块。按照屏幕上的说明将你的 LLM 配置为 Serverless Endpoint:

1. 选择一个 vLLM 版本。
2. 添加一个 Hugging Face 模型(例如 `openchat/openchat-3.5-0106`)。
3. (可选)为受限模型提供一个 Hugging Face token。
4. 检查你的设置并点击 **Next**。

在 **vLLM parameters** 页面,为你的模型配置更多参数:

1. 在 **LLM Settings** 中,将 **Max Model Length** 设置为 **8192**。
2. 检查你的设置并点击 **Next**。

在 **Endpoint parameters** 页面,配置你的部署:

1. 为 Worker 指定你的 GPU 配置。
2. 配置你的 Worker 部署:
   * 确保 **Container Image** 使用所需的 CUDA 版本。
   * 如有必要,调整 **Container Disk** 大小。
3. 点击 **Deploy**。

一旦 Endpoint 初始化完成,你就可以开始向你的 [Endpoint](https://github.com/SubModel/docs/tree/main/serverless/endpoints/get-started.md) 发送请求。请继续阅读 [发送请求](get-started.md#sending-a-request) 部分获取进一步说明。

## 通过 Worker Image 部署

使用 vLLM Worker 部署你的模型只需极少的配置。对大多数模型而言,你只需指定预构建的 vLLM Worker image 名称和 LLM 模型名称。

按照以下步骤在 Serverless Endpoint 上部署 vLLM Worker:

1. 登录 [SubModel Serverless 控制台](https://submodel.ai/#/serverless/list)。
2. 点击 **+ New Endpoint**。
3. 提供以下详细信息:
   * Endpoint 名称
   * 选择一个 GPU(如有需要,在 **Advanced** 标签下确保它支持 CUDA 12.1.0+)
   * 配置 Worker 的数量
   * 输入带兼容 CUDA 版本的 vLLM SubModel Worker image 名称:
     * `submodel/worker-vllm:stable-cuda11.8.0`
     * `submodel/worker-v1-vllm:stable-cuda12.1.0`
   * (可选)挂载一个 [网络存储卷](https://github.com/SubModel/docs/tree/main/serverless/endpoints/manage-endpoints/README.md#add-a-network-volume)
   * 配置环境变量:
     * `MODEL_NAME`:(必需)指定大语言模型(例如 `openchat/openchat-3.5-0106`)
     * `HF_TOKEN`:(可选)为私有模型提供你的 Hugging Face API token
4. 点击 **Deploy**。

一旦 Endpoint 初始化完成,你就可以开始向你的 [Endpoint](https://github.com/SubModel/docs/tree/main/serverless/endpoints/get-started/README.md) 发送请求。请继续阅读 [发送请求](get-started.md#sending-a-request) 部分获取进一步说明。

有关可用环境变量的完整列表,请参阅 [vLLM Worker 变量](https://github.com/SubModel/docs/tree/main/serverless/workers/vllm-endpoint/environment-variables/README.md) 文档。

## 发送请求

本节将指导你如何向你的 Serverless Endpoint 发送请求。vLLM Worker 支持任意 Hugging Face 模型,并兼容 OpenAI 的 API。如果你已安装 OpenAI 库,你可以继续用它来配合 vLLM Worker 使用。更多细节请参阅 [OpenAI 文档](https://platform.openai.com/docs/libraries/)。

### 环境设置

使用你的 Endpoint ID 和 API Key 设置 `SUBMODEL_ENDPOINT_ID` 和 `SUBMODEL_API_KEY` 环境变量。

```bash
export SUBMODEL_ENDPOINT_ID=<YOUR_SUBMODEL_ENDPOINT_ID>
export SUBMODEL_API_KEY=<YOUR_SUBMODEL_API_KEY>
```

### 代码实现

## 使用 SubModel API

SubModel API 提供了一种高效的方式来与 vLLM endpoint 交互,你可以发送带 prompt 或消息列表的请求。该 API 会自动对消息应用 chat template,实现无缝交互。

***

## API Endpoints

### 同步请求

```http
POST https://api.submodel.ai/v1/sl/{endpoint_id}/run_sync
```

### 异步请求

```http
POST https://api.submodel.ai/v1/sl/{endpoint_id}/run
```

***

## 请求参数

### 主要输入参数

<details>

<summary>点击展开</summary>

| 参数                         | 类型                     | 默认值                                     | 说明                                                    |
| -------------------------- | ---------------------- | --------------------------------------- | ----------------------------------------------------- |
| `prompt`                   | `str`                  | `None`                                  | 用于文本生成的输入字符串。                                         |
| `messages`                 | `list[dict[str, str]]` | `None`                                  | 带角色(`system`、`user`、`assistant`)的消息列表。会覆盖 `prompt`。   |
| `apply_chat_template`      | `bool`                 | `False`                                 | 是否对 `prompt` 应用模型的 chat template。                     |
| `sampling_params`          | `dict`                 | `{}`                                    | 控制生成的采样参数(见下文)。                                       |
| `stream`                   | `bool`                 | `False`                                 | 是否启用输出的流式传输。                                          |
| `max_batch_size`           | `int`                  | 环境变量 `DEFAULT_BATCH_SIZE`               | 每次 HTTP POST 调用要流式传输的最大 token 数量。                     |
| `min_batch_size`           | `int`                  | 环境变量 `DEFAULT_MIN_BATCH_SIZE`           | 每次 HTTP POST 调用要流式传输的最小 token 数量。                     |
| `batch_size_growth_factor` | `int`                  | 环境变量 `DEFAULT_BATCH_SIZE_GROWTH_FACTOR` | 每次调用时 min\_batch\_size 乘以的增长因子,直到达到 max\_batch\_size。 |

</details>

### 采样参数

以下是你可以在 `sampling_params` 字典中指定的所有可用采样参数。如果你不指定其中任何一个,将使用默认值。

<details>

<summary>点击展开</summary>

| 参数                              | 类型                            | 默认值     | 说明                          |
| ------------------------------- | ----------------------------- | ------- | --------------------------- |
| `n`                             | `int`                         | `1`     | 生成的输出序列数量。                  |
| `best_of`                       | `Optional[int]`               | `n`     | 在选出最佳的 `n` 个之前生成的序列数量。      |
| `presence_penalty`              | `float`                       | `0.0`   | 基于 token 是否出现对其进行惩罚。        |
| `frequency_penalty`             | `float`                       | `0.0`   | 基于 token 频率对其进行惩罚。          |
| `repetition_penalty`            | `float`                       | `1.0`   | 基于 token 重复对其进行惩罚。          |
| `temperature`                   | `float`                       | `1.0`   | 控制随机性;值越低越确定。               |
| `top_p`                         | `float`                       | `1.0`   | 限制排名靠前的 token 的概率质量。        |
| `top_k`                         | `int`                         | `-1`    | 限制考虑的排名靠前的 token 数量。        |
| `min_p`                         | `float`                       | `0.0`   | 相对于最可能 token 的最小概率。         |
| `use_beam_search`               | `bool`                        | `False` | 启用束搜索(beam search)。         |
| `length_penalty`                | `float`                       | `1.0`   | 基于长度对序列进行惩罚。                |
| `early_stopping`                | `Union[bool, str]`            | `False` | 控制停止条件。                     |
| `stop`                          | `Union[None, str, List[str]]` | `None`  | 在指定字符串处停止生成。                |
| `stop_token_ids`                | `Optional[List[int]]`         | `None`  | 在指定 token ID 处停止生成。         |
| `ignore_eos`                    | `bool`                        | `False` | 忽略结束(End-Of-Sequence)token。 |
| `max_tokens`                    | `int`                         | `16`    | 生成的最大 token 数量。             |
| `skip_special_tokens`           | `bool`                        | `True`  | 是否在输出中跳过特殊 token。           |
| `spaces_between_special_tokens` | `bool`                        | `True`  | 是否在特殊 token 之间添加空格。         |

</details>

***

## 文本输入格式

### 使用 `prompt`

prompt 字符串可以是任意字符串,除非将 `apply_chat_template` 设置为 `true`,否则不会对其应用模型的 chat template;设置为 `true` 时,它将被当作一条 user 消息处理。

```json
{
  "prompt": "Translate the following text to French: 'Hello, how are you?'"
}
```

### 使用 `messages`

你的列表可以包含任意数量的消息,每条消息通常可以拥有以下列表中的任一角色:

* `user`
* `assistant`
* `system`

不过,有些模型可能有不同的角色,因此你应该查看模型的 chat template 来了解需要哪些角色。模型的 chat template 会被自动应用到消息上,所以模型必须具备 chat template。

```json
{
  "messages": [
    { "role": "system", "content": "You are a helpful assistant." },
    { "role": "user", "content": "Tell me a joke." }
  ]
}
```

***

## API 请求示例

### Python 示例

```python
import requests

url = "https://api.submodel.ai/v1/sl/<endpoint_id>/run"
headers = {"x-apikey": "YOUR_API_KEY", "Content-Type": "application/json"}

data = {"input": {
    "messages": [
        { "role": "system", "content": "You are a helpful assistant." },
        { "role": "user", "content": "Write a short poem." }
    ],
    "sampling_params": {"temperature": 0.7, "max_tokens": 100}
}}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

### cURL 示例

```sh
curl -X POST "https://api.submodel.ai/v1/sl/<endpoint_id>/run" \
     -H "x-apikey: YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{"input":
               {
                 "prompt": "Write a haiku about nature.",
                 "sampling_params": {"temperature": 0.8, "max_tokens": 50}
         }}'
```

***

## 故障排查

如果你在部署或使用 vLLM Worker 时遇到问题,请检查以下几点:

* 确保你的 SubModel API Key 拥有部署和访问 Serverless Endpoint 所需的权限。
* 仔细核对你为 Endpoint ID 和 API Key 设置了正确的环境变量。
* 确认你为所选 GPU 使用了正确的 CUDA 版本。
* 如果使用受限模型,请确保你的 Hugging Face token 有效且有权访问该模型。

有关管理 Serverless Endpoint 的更多信息,请参阅 [管理 Endpoints](https://github.com/SubModel/docs/tree/main/serverless/endpoints/manage-endpoints/README.md) 指南。有关 vLLM Worker 环境变量的完整参考,请参阅 [vLLM Worker 变量](https://github.com/SubModel/docs/tree/main/serverless/workers/vllm-endpoint/environment-variables/README.md) 文档。
