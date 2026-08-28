# Pod SDK Reference

`client.pod` gives you programmatic access to SubModel Pod instances.

## Methods

### `pod.list()`

List all Pod instances owned by the authenticated user.

```python
result = client.pod.list(page=1, limit=10, mode="pod", search="my-lab")
items = result["data"]["items"]
```

| Parameter | Type | Default | Description |
|---|---|---|---|
| `page` | int | 1 | Page number |
| `limit` | int | 10 | Results per page |
| `mode` | str | `None` | `"pod"` or `"serverless"` |
| `search` | str | `None` | Search by label |

**Returns** `{"code": 20000, "data": {"items": [...], "total": N}}`

---

### `pod.get(inst_id)`

Get full details for a single instance.

```python
inst = client.pod.get("inst-abc123")
print(inst["data"]["status"])
```

---

### `pod.get_pods(inst_id)`

List the running pod containers inside an instance.

```python
pods = client.pod.get_pods("inst-abc123")
```

---

### `pod.get_pod_logs(inst_id, pod_id, **kwargs)`

Fetch logs from a specific container.

```python
logs = client.pod.get_pod_logs("inst-abc123", "pod-0")
print(logs)
```

---

### `pod.create()`

Create a new Pod instance.

```python
result = client.pod.create(
    plan="gpu-rtx4090-24g-1",
    image="pytorch/pytorch:2.0.0-cuda11.7-cudnn8-runtime",
    billing_method="payg",
    mode="pod",
    pod_num=1,
    area=["us-east-1"],
    conf={
        "volume_size": 20,
        "container_size": 10,
        "mount_path": "/workspace",
        "label": "my-training-pod",
    },
)
inst_id = result["data"]["inst_id"]
```

| Parameter | Type | Default | Description |
|---|---|---|---|
| `plan` | str | required | GPU plan ID (e.g. `"gpu-rtx4090-24g-1"`) |
| `image` | str | required | Template name or `"[custom]registry/image:tag"` |
| `billing_method` | str | `"payg"` | `"payg"` \| `"day"` \| `"week"` \| `"month"` \| `"spot"` \| `"spare"` |
| `mode` | str | `"pod"` | `"pod"` or `"serverless"` |
| `pod_num` | int | 1 | Number of pods (1–128) |
| `area` | list[str] | `None` | Preferred availability zones |
| `conf` | dict | `None` | Extra config (ports, env, volume size, etc.) |

**Returns** `{"code": 20000, "data": {"inst_id": "inst-..."}}`

---

### `pod.start(inst_id)`

Start a stopped instance.

```python
client.pod.start("inst-abc123")
```

---

### `pod.stop(inst_id)`

Stop a running instance (instance keeps its disk).

```python
client.pod.stop("inst-abc123")
```

---

### `pod.restart(inst_id)`

Restart an instance.

```python
client.pod.restart("inst-abc123")
```

---

### `pod.release(inst_id, force=False)`

Permanently delete an instance and free its resources.

```python
client.pod.release("inst-abc123")
# skip any server-side confirmation:
client.pod.release("inst-abc123", force=True)
```

> ⚠️ This action is **irreversible**. The instance and its local disk are permanently deleted.

---

### `pod.set_label(inst_id, label)`

Update the display label of an instance.

```python
client.pod.set_label("inst-abc123", "fine-tuning-run-42")
```

---

### `pod.renewal(inst_id)`

Renew a fixed-billing (day / week / month) instance.

```python
client.pod.renewal("inst-abc123")
```

---

### `pod.terminate_pod(inst_id, pod_id)`

Forcibly terminate a specific pod container within an instance.

```python
client.pod.terminate_pod("inst-abc123", "pod-0")
```

---

## Error Handling

```python
from submodel import SubModelClient, AuthenticationError, APIError

client = SubModelClient(api_key="sk-...")

try:
    result = client.pod.create(plan="gpu-rtx4090-24g-1", image="pytorch/pytorch:latest")
except AuthenticationError:
    print("Invalid API key")
except APIError as e:
    print(f"API error {e.code}: {e.message}")
```

See [Exceptions](overview.md#exceptions) for a full list.
