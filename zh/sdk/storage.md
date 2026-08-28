# Storage SDK 参考

`client.storage` 管理 SubModel 持久化网络卷（PVC）。存储卷可挂载到 Pod 或 Serverless 实例，实例重启或删除后数据仍然保留。

---

## 方法

### `storage.list()`

列出当前认证用户的所有存储卷。

```python
result = client.storage.list(page=1, limit=10)
volumes = result["data"]["items"]
```

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `page` | int | 1 | 页码 |
| `limit` | int | 10 | 每页数量 |
| `type` | str | `None` | 按卷类型过滤 |
| `area` | str | `None` | 按可用区过滤 |

**返回** `{"code": 20000, "data": {"items": [...], "total": N}}`

---

### `storage.create(name, size, area)`

创建新的自定义网络卷。

```python
result = client.storage.create(
    name="my-dataset",
    size=100,
    area="us-east-1",
)
```

| 参数 | 类型 | 说明 |
|---|---|---|
| `name` | str | 卷名称，小写字母和数字，最少 4 个字符 |
| `size` | int | 大小（GB） |
| `area` | str | 卷所在可用区 ID |

**返回** `{"code": 20000, "data": {...}}`

---

### `storage.delete(volume_name)`

永久删除存储卷。

```python
client.storage.delete("my-dataset")
```

> ⚠️ 该操作**不可逆**，卷上的所有数据将永久丢失。删除前请确保没有实例挂载该卷。

---

### `storage.resize(volume_name, size_gb)`

扩容存储卷。存储卷只能扩大，不能缩小。

```python
client.storage.resize("my-dataset", size_gb=200)
```

---

### `storage.bind(volume_name, inst_id, mount_path)`

将存储卷挂载到指定实例的路径。

```python
client.storage.bind("my-dataset", "inst-abc123", "/workspace/data")
```

挂载路径必须以 `/` 开头。实例需处于可接受存储变更的状态。

---

### `storage.unbind(volume_name, inst_id)`

从实例卸载存储卷。

```python
client.storage.unbind("my-dataset", "inst-abc123")
```

---

## 完整示例

```python
from submodel import SubModelClient

client = SubModelClient(api_key="sk-...")

# 1. 在 us-east-1 创建 50 GB 存储卷
vol = client.storage.create("training-data", size=50, area="us-east-1")

# 2. 挂载到 Pod 实例
client.storage.bind("training-data", "inst-abc123", "/workspace/data")

# 3. 列出所有存储卷
result = client.storage.list()
for v in result["data"]["items"]:
    print(v["name"], v["size_gb"], v["status"])

# 4. 卸载并删除
client.storage.unbind("training-data", "inst-abc123")
client.storage.delete("training-data")
```

---

## 错误处理

```python
from submodel import APIError

try:
    client.storage.create("x", size=10, area="us-east-1")
except APIError as e:
    # 例：名称过短时返回 code 40000
    print(f"错误 {e.code}：{e.message}")
```
