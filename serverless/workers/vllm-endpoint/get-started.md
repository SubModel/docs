# Get Started with SubModel and vLLM

SubModel offers a streamlined approach to deploying large language models (LLMs) as Serverless Endpoints using pre-configured vLLM Docker images. This guide will walk you through deploying an OpenAI-compatible endpoint with a vLLM inference engine on SubModel.

## Prerequisites

Before you begin, ensure you have the following:

- A SubModel account
- A Hugging Face token (required for accessing gated models)
- OpenAI or other necessary libraries installed for code examples

## Deploying via the Web UI

You can deploy a vLLM Worker with a model directly from Hugging Face using SubModel's Web UI.

1. Log in to your SubModel account and navigate to the [Serverless page](https://submodel.ai/#/serverless/list).
2. Under **Quick Deploy**, locate **Serverless vLLM** and click **Start**.

You will now enter the vLLM module. Follow the on-screen instructions to configure your LLM as a Serverless Endpoint:

1. Select a vLLM version.
2. Add a Hugging Face model (e.g., `openchat/openchat-3.5-0106`).
3. (Optional) Provide a Hugging Face token for gated models.
4. Review your settings and click **Next**.

On the **vLLM parameters** page, configure additional parameters for your model:

1. In **LLM Settings**, set **Max Model Length** to **8192**.
2. Review your settings and click **Next**.

On the **Endpoint parameters** page, configure your deployment:

1. Specify your GPU configuration for the Worker.
2. Configure your Worker deployment:
   - Ensure the **Container Image** uses the desired CUDA version.
   - Adjust the **Container Disk** size if necessary.
3. Click **Deploy**.

Once the Endpoint initializes, you can start sending requests to your [Endpoint](/serverless/endpoints/get-started). Proceed to the [Send a Request](#sending-a-request) section for further instructions.

## Deploying via the Worker Image

Deploying your model with the vLLM Worker requires minimal configuration. For most models, you only need to specify the pre-built vLLM Worker image name and the LLM model name.

Follow these steps to deploy the vLLM Worker on a Serverless Endpoint:

1. Log in to the [SubModel Serverless console](https://www.submodel.ai/console/serverless).
2. Click **+ New Endpoint**.
3. Provide the following details:
   - Endpoint name
   - Select a GPU (ensure it supports CUDA 12.1.0+ under the **Advanced** tab if needed)
   - Configure the number of Workers
   - (Optional) Enable **FlashBoot** to accelerate Worker startup times
   - Enter the vLLM SubModel Worker image name with the compatible CUDA version:
     - `submodel/worker-vllm:stable-cuda11.8.0`
     - `submodel/worker-v1-vllm:stable-cuda12.1.0`
   - (Optional) Attach a [network storage volume](/serverless/endpoints/manage-endpoints#add-a-network-volume)
   - Configure the environment variables:
     - `MODEL_NAME`: (Required) Specify the large language model (e.g., `openchat/openchat-3.5-0106`)
     - `HF_TOKEN`: (Optional) Provide your Hugging Face API token for private models
4. Click **Deploy**.

Once the Endpoint initializes, you can start sending requests to your [Endpoint](/serverless/endpoints/get-started). Proceed to the [Send a Request](#sending-a-request) section for further instructions.

For a complete list of available environment variables, refer to the [vLLM Worker variables](/serverless/workers/vllm/environment-variables) documentation.

## Sending a Request

This section guides you through sending a request to your Serverless Endpoint. The vLLM Worker supports any Hugging Face model and is compatible with OpenAI's API. If you have the OpenAI library installed, you can continue using it with the vLLM Worker. For more details, refer to the [OpenAI documentation](https://platform.openai.com/docs/libraries/).

### Environment Setup

Set the `SUBMODEL_ENDPOINT_ID` and `SUBMODEL_API_KEY` environment variables with your Endpoint ID and API Key.

```bash
export SUBMODEL_ENDPOINT_ID=<YOUR_SUBMODEL_ENDPOINT_ID>
export SUBMODEL_API_KEY=<YOUR_SUBMODEL_API_KEY>
```

### Code Implementation

## Using the SubModel API

The SubModel API provides an efficient way to interact with the vLLM endpoint by sending requests with either a prompt or a list of messages. The API automatically applies chat templates to messages, enabling seamless interaction.

---

## API Endpoints

### Synchronous Request

```http
POST https://api.submodel.ai/v2/{endpoint_id}/run_sync
```

### Asynchronous Request

```http
POST https://api.submodel.ai/v2/{endpoint_id}/run
```

---

## Request Parameters

### Main Input Parameters

<details>
<summary>Click to expand</summary>

| Argument | Type | Default | Description |
| --- | --- | --- | --- |
| `prompt` | `str` | `None` | The input string for text generation. |
| `messages` | `list[dict[str, str]]` | `None` | A list of messages with roles (`system`, `user`, `assistant`). Overrides `prompt`. |
| `apply_chat_template` | `bool` | `False` | Whether to apply the model's chat template to the `prompt`. |
| `sampling_params` | `dict` | `{}` | Sampling parameters to control generation (see below). |
| `stream` | `bool` | `False` | Whether to enable streaming of the output. |
| `max_batch_size` | `int` | env var `DEFAULT_BATCH_SIZE` | The maximum number of tokens to stream every HTTP POST call. |
| `min_batch_size` | `int` | env var `DEFAULT_MIN_BATCH_SIZE` | The minimum number of tokens to stream every HTTP POST call. |
| `batch_size_growth_factor` | `int` | env var `DEFAULT_BATCH_SIZE_GROWTH_FACTOR` | The growth factor by which min_batch_size will be multiplied for each call until max_batch_size is reached. |

</details>

### Sampling Parameters

Below are all available sampling parameters that you can specify in the `sampling_params` dictionary. If you do not specify any of these parameters, the default values will be used.

<details>
<summary>Click to expand</summary>

| Argument | Type | Default | Description |
| --- | --- | --- | --- |
| `n` | `int` | `1` | Number of output sequences generated. |
| `best_of` | `Optional[int]` | `n` | Number of sequences generated before selecting the best `n`. |
| `presence_penalty` | `float` | `0.0` | Penalizes tokens based on their presence. |
| `frequency_penalty` | `float` | `0.0` | Penalizes tokens based on frequency. |
| `repetition_penalty` | `float` | `1.0` | Penalizes tokens based on repetition. |
| `temperature` | `float` | `1.0` | Controls randomness; lower values make it deterministic. |
| `top_p` | `float` | `1.0` | Limits probability mass of top tokens. |
| `top_k` | `int` | `-1` | Limits number of top tokens considered. |
| `min_p` | `float` | `0.0` | Minimum probability relative to most likely token. |
| `use_beam_search` | `bool` | `False` | Enables beam search. |
| `length_penalty` | `float` | `1.0` | Penalizes sequences based on length. |
| `early_stopping` | `Union[bool, str]` | `False` | Controls stopping conditions. |
| `stop` | `Union[None, str, List[str]]` | `None` | Stop generation on specified strings. |
| `stop_token_ids` | `Optional[List[int]]` | `None` | Stop generation on specified token IDs. |
| `ignore_eos` | `bool` | `False` | Ignore End-Of-Sequence token. |
| `max_tokens` | `int` | `16` | Maximum tokens generated. |
| `skip_special_tokens` | `bool` | `True` | Whether to skip special tokens in the output. |
| `spaces_between_special_tokens` | `bool` | `True` | Whether to add spaces between special tokens. |

</details>

---

## Text Input Formats

### Using `prompt`

The prompt string can be any string, and the model's chat template will not be applied to it unless `apply_chat_template` is set to `true`, in which case it will be treated as a user message.

```json
{
  "prompt": "Translate the following text to French: 'Hello, how are you?'"
}
```

### Using `messages`

Your list can contain any number of messages, and each message usually can have any role from the following list:

- `user`
- `assistant`
- `system`

However, some models may have different roles, so you should check the model's chat template to see which roles are required. The model's chat template will be applied to the messages automatically, so the model must have one.

```json
{
  "messages": [
    { "role": "system", "content": "You are a helpful assistant." },
    { "role": "user", "content": "Tell me a joke." }
  ]
}
```

---

## Sample API Requests

### Python Example

```python
import requests

url = "https://api.submodel.ai/v2/<endpoint_id>/run"
headers = {"Authorization": "Bearer YOUR_API_KEY", "Content-Type": "application/json"}

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

### cURL Example

```sh
curl -X POST "https://api.submodel.ai/v2/yf2k4t0vl3ciaf/run" \
     -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{"input":
               {
                 "prompt": "Write a haiku about nature.",
                 "sampling_params": {"temperature": 0.8, "max_tokens": 50}
         }}'
```

---

## Troubleshooting

If you encounter issues deploying or using vLLM Workers, check the following:

- Ensure your SubModel API Key has the necessary permissions to deploy and access Serverless Endpoints.
- Double-check that you have set the correct environment variables for your Endpoint ID and API Key.
- Verify that you are using the correct CUDA version for your selected GPU.
- If using a gated model, ensure your Hugging Face token is valid and has access to the model.

For more information on managing your Serverless Endpoints, refer to the [Manage Endpoints](/serverless/endpoints/manage-endpoints) guide. For a complete reference of the vLLM Worker environment variables, see the [vLLM Worker variables](/serverless/workers/vllm/environment-variables) documentation.