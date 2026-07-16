# Job 操作指南

本指南详细介绍如何使用 SubModel Endpoint 执行 job 操作。你可以通过这些操作发起 job、检查其状态、清空 job queue 等。下面的示例演示了如何使用 cURL 与 Endpoint 交互。

有关发送请求的更多信息,请参阅 [发送请求](/serverless/endpoints/send-requests.md)。

## 异步 Endpoint

异步 Endpoint 专为长时间运行的任务设计。提交 job 后,你会收到一个 Job ID,之后可用它来检查 job 状态。这样你的应用无需立即等待 job 完成即可继续处理,非常适合需要大量处理时间的任务,或需要并发管理多个 job 的场景。

```bash
curl -X POST https://api.submodel.ai/v1/sl/{endpoint_id}/run \
    -H 'Content-Type: application/json' \
    -H 'x-apikey: ${API_KEY}' \
    -d '{"input": {"prompt": "Your prompt"}}'
```

**输出:**

```json
{
  "id": "eaebd6e7-6a92-4bb8-a911-f996ac5ea99d",
  "status": "IN_QUEUE"
}
```

## 同步 Endpoint

同步 Endpoint 适用于需要即时结果的短时任务。这类 Endpoint 会等待 job 完成,并直接在响应中返回结果,非常适合预期能快速完成的操作。

```bash
curl -X POST https://api.submodel.ai/v1/sl/{endpoint_id}/runsync \
    -H 'Content-Type: application/json' \
    -H 'x-apikey: ${API_KEY}' \
    -d '{"input": {"prompt": "Your prompt"}}'
```

**输出:**

```json
{
  "delayTime": 824,
  "executionTime": 3391,
  "id": "sync-79164ff4-d212-44bc-9fe3-389e199a5c15",
  "output": [
    {
      "image": "https://image.url",
      "seed": 46578
    }
  ],
  "status": "COMPLETED"
}
```

## Health Endpoint

`/health` endpoint 提供 Endpoint 运行状态的洞察,包括可用 Worker 数量和 job 统计信息。这些信息有助于监控 API 的健康状况与性能,便于工作负载管理和故障排查。

```bash
curl --request GET \
     --url https://api.submodel.ai/v1/sl/{endpoint_id}/health \
     --header 'accept: application/json' \
     --header 'x-apikey: ${API_KEY}'
```

**输出:**

```json
{
  "jobs": {
    "completed": 1,
    "failed": 5,
    "inProgress": 0,
    "inQueue": 2,
    "retried": 0
  },
  "workers": {
    "idle": 0,
    "running": 0
  }
}
```

## 取消 Job

要取消正在进行的 job,请指定 `cancel` 参数以及 endpoint ID 和 job ID。

```bash
curl -X POST https://api.submodel.ai/v1/sl/{endpoint_id}/cancel/{job_id} \
    -H 'Content-Type: application/json' \
    -H 'x-apikey: ${API_KEY}'
```

**输出:**

```json
{
  "id": "724907fe-7bcc-4e42-998d-52cb93e1421f-u1",
  "status": "CANCELLED"
}
```

## Purge Queue Endpoint

`/purge-queue` endpoint 允许你清空当前 queue 中的所有 job,而不影响已在进行中的 job。这在因运营变更或错误而需要重置或清理待处理任务时非常有用。

```bash
curl -X POST https://api.submodel.ai/v1/sl/{endpoint_id}/purge-queue \
    -H 'Content-Type: application/json' \
    -H 'x-apikey: ${API_KEY}'
```

**输出:**

```json
{
  "removed": 2,
  "status": "completed"
}
```

## 检查 Job 状态

要跟踪异步 job 的进度或结果,请使用 Job ID 检查其状态。该 endpoint 提供 job 的详细信息,包括当前状态、执行时间,以及 job 完成后的输出。

```bash
curl -X POST https://api.submodel.ai/v1/sl/{endpoint_id}/status/{job_id} \
    -H 'x-apikey: ${API_KEY}'
```

**输出:**

```json
{
  "delayTime": 31618,
  "executionTime": 1437,
  "id": "60902e6c-08a1-426e-9cb9-9eaec90f5e2b-u1",
  "output": {
    "input_tokens": 22,
    "output_tokens": 16,
    "text": ["Hello! How can I assist you today?\nUSER: I'm having"]
  },
  "status": "COMPLETED"
}
```

## 流式获取结果

对于增量产出结果的 job,stream endpoint 允许你在结果生成的同时逐步接收它们。这对于涉及连续数据处理的任务,或需要即时获取部分结果的场景尤为有用。

```bash
curl -X POST https://api.submodel.ai/v1/sl/{endpoint_id}/stream/{job_id} \
    -H 'Content-Type: application/json' \
    -H 'x-apikey: ${API_KEY}'
```

**输出:**

```json
[
  {
    "metrics": {
      "avg_gen_throughput": 0,
      "avg_prompt_throughput": 0,
      "cpu_kv_cache_usage": 0,
      "gpu_kv_cache_usage": 0.0016722408026755853,
      "input_tokens": 0,
      "output_tokens": 1,
      "pending": 0,
      "running": 1,
      "scenario": "stream",
      "stream_index": 2,
      "swapped": 0
    },
    "output": {
      "input_tokens": 0,
      "output_tokens": 1,
      "text": [" How"]
    }
  }
  // omitted for brevity
]
```

> **注意:** 使用 yield 流式返回结果时,单次 payload 的最大尺寸为 1 MB。

## 速率限制

SubModel 的 Endpoint 便于提交 job 并获取输出。可通过以下地址访问这些 endpoint:`https://api.submodel.ai/v2/{endpoint_id}/{operation}`

- `/run`
  - 每 10 秒 1000 次请求,并发 200
- `/runsync`
  - 每 10 秒 2000 次请求,并发 400
- `/status`、`/status-sync`、`/stream`
  - 每 10 秒 2000 次请求,并发 400
- `/cancel`
  - 每 10 秒 100 次请求,并发 20
- `/purge-queue`
  - 每 10 秒 2 次请求
- `/openai/*`
  - 每 10 秒 2000 次请求,并发 400
- `/requests`
  - 每 10 秒 10 次请求,并发 2

> **注意:** 出于隐私保护考虑,请在 30 分钟内从 `/status` 获取结果。

有关 Endpoint 的参考信息,请参阅 [Endpoint 操作](/serverless/references/operations.md)。
