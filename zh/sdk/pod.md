# Pod SDK 参考

`client.pod` 提供对 SubModel Pod 实例的完整编程访问。

## 方法

### `pod.list()`

列出当前认证用户的所有 Pod 实例。

```python
result = client.pod.list(page=1, limit=10, mode="pod", search="训练")
items = result["data"]["items"]
```

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `page` | int | 1 | 页码 |
| `limit` | int | 10 | 每页数量 |
| `mode` | str | `None` | `"pod"` 或 `"serverless"` |
| `search` | str | `None` | 按标签搜索 |

**返回** `{"code": 20000, "data": {"items": [...], "total": N}}`

---

### `pod.get(inst_id)`

获取单个实例的详细信息。

```python
inst = client.pod.get("inst-abc123")
print(inst["data"]["status"])
```

---

### `pod.get_pods(inst_id)`

列出实例内正在运行的 Pod 容器。

```python
pods = client.pod.get_pods("inst-abc123")
```

---

### `pod.get_pod_logs(inst_id, pod_id, **kwargs)`

获取指定容器的日志。

```python
logs = client.pod.get_pod_logs("inst-abc123", "pod-0")
print(logs)
```

---

### `pod.create()`

创建新的 Pod 实例。

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
        "label": "我的训练 Pod",
    },
)
inst_id = result["data"]["inst_id"]
```

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `plan` | str | 必填 | GPU 规格 ID，如 `"gpu-rtx4090-24g-1"` |
| `image` | str | 必填 | 模板名称，或 `"[custom]registry/image:tag"` 格式的私有镜像 |
| `billing_method` | str | `"payg"` | `"payg"` \| `"day"` \| `"week"` \| `"month"` \| `"spot"` \| `"spare"` |
| `mode` | str | `"pod"` | `"pod"` 或 `"serverless"` |
| `pod_num` | int | 1 | Pod 数量（1–128） |
| `area` | list[str] | `None` | 首选可用区列表 |
| `conf` | dict | `None` | 附加配置（端口、环境变量、存储卷等） |

**返回** `{"code": 20000, "data": {"inst_id": "inst-..."}}`

---

### `pod.start(inst_id)`

启动已停止的实例。

```python
client.pod.start("inst-abc123")
```

---

### `pod.stop(inst_id)`

停止运行中的实例（保留磁盘数据）。

```python
client.pod.stop("inst-abc123")
```

---

### `pod.restart(inst_id)`

重启实例。

```python
client.pod.restart("inst-abc123")
```

---

### `pod.release(inst_id, force=False)`

永久删除实例并释放资源。

```python
client.pod.release("inst-abc123")
# 跳过服务端确认：
client.pod.release("inst-abc123", force=True)
```

> ⚠️ 该操作**不可逆**。实例及其本地磁盘将被永久删除。

---

### `pod.set_label(inst_id, label)`

更新实例的显示标签。

```python
client.pod.set_label("inst-abc123", "微调训练-第42次")
```

---

### `pod.renewal(inst_id)`

为按固定周期计费（天/周/月）的实例续费。

```python
client.pod.renewal("inst-abc123")
```

---

### `pod.terminate_pod(inst_id, pod_id)`

强制终止实例内的指定 Pod 容器。

```python
client.pod.terminate_pod("inst-abc123", "pod-0")
```

---

## 错误处理

```python
from submodel import SubModelClient, AuthenticationError, APIError

client = SubModelClient(api_key="sk-...")

try:
    result = client.pod.create(plan="gpu-rtx4090-24g-1", image="pytorch/pytorch:latest")
except AuthenticationError:
    print("API Key 无效")
except APIError as e:
    print(f"API 错误 {e.code}：{e.message}")
```

完整异常列表见 [概览](overview.md#异常)。
