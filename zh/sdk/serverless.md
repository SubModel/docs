# Serverless SDK 参考

`client.serverless` 用于向 SubModel Serverless 端点提交任务，以及在容器内编写 worker handler。

---

## 客户端 API

### `serverless.endpoint(inst_id)` → `ServerlessEndpoint`

返回指定 Serverless 实例的端点对象。

```python
ep = client.serverless.endpoint("inst-abc123")
```

当需要对同一实例多次调用时，使用端点对象可避免重复传入实例 ID。

---

### 快捷方法

以下方法直接挂在 `client.serverless` 上，内部自动创建临时端点：

```python
client.serverless.run(inst_id, input_data)
client.serverless.run_sync(inst_id, input_data, timeout=90)
client.serverless.status(inst_id, job_id)
client.serverless.cancel(inst_id, job_id)
```

---

## ServerlessEndpoint

### `ep.run(input_data, webhook=None)`

提交异步任务，立即返回 job ID。

```python
job = ep.run({"prompt": "用一句话解释 Transformer。"})
job_id = job["data"]["id"]
```

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `input_data` | any | 必填 | 发送给 worker `job["input"]` 的数据 |
| `webhook` | str | `None` | 任务完成后回调的 URL |

**返回** `{"code": 20000, "data": {"id": "job-...", "status": "IN_QUEUE"}}`

---

### `ep.run_sync(input_data, timeout=90)`

提交任务并同步等待结果。

```python
result = ep.run_sync({"prompt": "你好"}, timeout=60)
output = result["data"]["output"]
```

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `input_data` | any | 必填 | 任务数据 |
| `timeout` | int | 90 | 服务端最大等待秒数 |

---

### `ep.wait(job_id, poll_interval=1.0, timeout=None)`

轮询直到任务进入终态。

```python
job = ep.run({"prompt": "hello"})
final = ep.wait(job["data"]["id"], poll_interval=2.0, timeout=300)
print(final["data"]["output"])
```

终态：`COMPLETED`、`FAILED`、`CANCELLED`、`TIMED_OUT`

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `job_id` | str | 必填 | `run()` 返回的任务 ID |
| `poll_interval` | float | 1.0 | 状态查询间隔（秒） |
| `timeout` | float | `None` | 超时后抛出 `TimeoutError` |

**常用模式 — 提交后等待：**

```python
ep = client.serverless.endpoint("inst-abc123")
job = ep.run({"prompt": "请帮我总结以下内容……"})
result = ep.wait(job["data"]["id"], timeout=120)
```

---

### `ep.status(job_id)`

查询任务当前状态。

```python
status = ep.status("job-xyz")
print(status["data"]["status"])   # IN_QUEUE | IN_PROGRESS | COMPLETED | FAILED | …
```

---

### `ep.cancel(job_id)`

取消排队或运行中的任务。

```python
ep.cancel("job-xyz")
```

---

### `ep.health()`

检查端点健康状态和 worker 可用数量。

```python
info = ep.health()
print(info["data"]["workers"]["available"])
```

---

### `ep.metrics()`

获取端点的吞吐量和延迟指标。

```python
metrics = ep.metrics()
```

---

### `ep.requests(pod_id=None)`

列出端点最近处理的请求，可按 pod 过滤。

```python
recent = ep.requests()
per_pod = ep.requests(pod_id="pod-0")
```

---

### `ep.purge_queue()`

删除所有排队中（尚未开始）的任务。

```python
ep.purge_queue()
```

---

## ServerlessWorker

`ServerlessWorker` 用于在 SubModel Serverless 容器内运行的代码。它从 `SUBMODEL_INPUT` 环境变量读取任务，将 JSON 结果输出到 stdout。

### 基本用法

```python
from submodel import ServerlessWorker

worker = ServerlessWorker()

@worker.handler
def process(job):
    text = job["input"]["text"]
    return {"result": text.upper()}

worker.start()
```

### `@worker.handler`

注册任务处理函数的装饰器。

- 接收包含 `"input"` 键的 `job` 字典。
- 必须返回可 JSON 序列化的值。
- 如果抛出异常，worker 输出 `{"error": "<message>"}`。

### `worker.set_max_iterations(n)`

限制 worker 处理的最大任务数后退出（默认 100）。

```python
worker.set_max_iterations(1)   # 单次运行容器
```

### `worker.start()`

进入处理循环（阻塞）。若未注册 handler 则抛出 `RuntimeError`。

### 环境变量

| 变量 | 说明 |
|---|---|
| `SUBMODEL_INPUT` | 平台注入的 JSON 格式任务数据，由 `start()` 自动读取 |

### 本地测试

```python
import os, json

os.environ["SUBMODEL_INPUT"] = json.dumps({"input": {"text": "hello world"}})

worker = ServerlessWorker()

@worker.handler
def process(job):
    return {"result": job["input"]["text"].upper()}

worker.start()   # 输出: {"output": {"result": "HELLO WORLD"}}
```
