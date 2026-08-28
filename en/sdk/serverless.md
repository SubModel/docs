# Serverless SDK Reference

`client.serverless` lets you submit jobs to SubModel Serverless endpoints and build worker handlers.

---

## Client API

### `serverless.endpoint(inst_id)` → `ServerlessEndpoint`

Return an endpoint object for a given serverless instance.

```python
ep = client.serverless.endpoint("inst-abc123")
```

Use the endpoint object when you need to call multiple methods against the same instance — it avoids repeating the instance ID.

---

### Shortcut methods

The following methods on `client.serverless` are direct shortcuts that internally create a temporary endpoint:

```python
client.serverless.run(inst_id, input_data)
client.serverless.run_sync(inst_id, input_data, timeout=90)
client.serverless.status(inst_id, job_id)
client.serverless.cancel(inst_id, job_id)
```

---

## ServerlessEndpoint

### `ep.run(input_data, webhook=None)`

Submit an asynchronous job. Returns immediately with a job ID.

```python
job = ep.run({"prompt": "Explain transformers in one sentence."})
job_id = job["data"]["id"]
```

| Parameter | Type | Default | Description |
|---|---|---|---|
| `input_data` | any | required | Payload sent to the worker's `job["input"]` |
| `webhook` | str | `None` | URL to POST the result to when the job completes |

**Returns** `{"code": 20000, "data": {"id": "job-...", "status": "IN_QUEUE"}}`

---

### `ep.run_sync(input_data, timeout=90)`

Submit a job and wait inline for the result.

```python
result = ep.run_sync({"prompt": "Hello"}, timeout=60)
output = result["data"]["output"]
```

| Parameter | Type | Default | Description |
|---|---|---|---|
| `input_data` | any | required | Job payload |
| `timeout` | int | 90 | Max seconds to wait before the server returns a timeout status |

---

### `ep.wait(job_id, poll_interval=1.0, timeout=None)`

Poll the endpoint until the job reaches a terminal state.

```python
job = ep.run({"prompt": "hello"})
final = ep.wait(job["data"]["id"], poll_interval=2.0, timeout=300)
print(final["data"]["output"])
```

Terminal states: `COMPLETED`, `FAILED`, `CANCELLED`, `TIMED_OUT`

| Parameter | Type | Default | Description |
|---|---|---|---|
| `job_id` | str | required | Job ID from `run()` |
| `poll_interval` | float | 1.0 | Seconds between status checks |
| `timeout` | float | `None` | Raise `TimeoutError` after this many seconds |

**Common pattern — submit then wait:**

```python
ep = client.serverless.endpoint("inst-abc123")
job = ep.run({"prompt": "summarize this text …"})
result = ep.wait(job["data"]["id"], timeout=120)
```

---

### `ep.status(job_id)`

Get the current status of a job.

```python
status = ep.status("job-xyz")
print(status["data"]["status"])   # IN_QUEUE | IN_PROGRESS | COMPLETED | FAILED | …
```

---

### `ep.cancel(job_id)`

Cancel a queued or in-progress job.

```python
ep.cancel("job-xyz")
```

---

### `ep.health()`

Check endpoint health and worker availability.

```python
info = ep.health()
print(info["data"]["workers"]["available"])
```

---

### `ep.metrics()`

Get throughput and latency metrics for the endpoint.

```python
metrics = ep.metrics()
```

---

### `ep.requests(pod_id=None)`

List recent requests processed by the endpoint, optionally scoped to one pod.

```python
recent = ep.requests()
per_pod = ep.requests(pod_id="pod-0")
```

---

### `ep.purge_queue()`

Delete all queued (not yet started) jobs.

```python
ep.purge_queue()
```

---

## ServerlessWorker

`ServerlessWorker` is for code that **runs inside** a SubModel Serverless container. It reads jobs from the `SUBMODEL_INPUT` environment variable and writes JSON results to stdout.

### Basic usage

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

Decorator that registers your processing function.

- Receives a `job` dict with at least an `"input"` key.
- Must return a JSON-serialisable value.
- If it raises, the worker outputs `{"error": "<message>"}`.

### `worker.set_max_iterations(n)`

Limit how many jobs the worker processes before exiting (default 100).

```python
worker.set_max_iterations(1)   # single-shot container
```

### `worker.start()`

Enter the processing loop (blocks). Raises `RuntimeError` if no handler has been registered.

### Environment variable

| Variable | Description |
|---|---|
| `SUBMODEL_INPUT` | JSON-encoded job dict injected by the platform. Read automatically by `start()`. |

### Local testing

```python
import os, json

os.environ["SUBMODEL_INPUT"] = json.dumps({"input": {"text": "hello world"}})

worker = ServerlessWorker()

@worker.handler
def process(job):
    return {"result": job["input"]["text"].upper()}

worker.start()   # prints: {"output": {"result": "HELLO WORLD"}}
```
