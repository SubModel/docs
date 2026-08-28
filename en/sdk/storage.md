# Storage SDK Reference

`client.storage` manages SubModel persistent network volumes (PVCs). Volumes can be attached to Pod or Serverless instances and survive instance restarts and deletions.

---

## Methods

### `storage.list()`

List all volumes owned by the authenticated user.

```python
result = client.storage.list(page=1, limit=10)
volumes = result["data"]["items"]
```

| Parameter | Type | Default | Description |
|---|---|---|---|
| `page` | int | 1 | Page number |
| `limit` | int | 10 | Results per page |
| `type` | str | `None` | Filter by volume type |
| `area` | str | `None` | Filter by availability zone |

**Returns** `{"code": 20000, "data": {"items": [...], "total": N}}`

---

### `storage.create(name, size, area)`

Create a new custom network volume.

```python
result = client.storage.create(
    name="my-dataset",
    size=100,
    area="us-east-1",
)
```

| Parameter | Type | Description |
|---|---|---|
| `name` | str | Volume name — lowercase alphanumeric, minimum 4 characters |
| `size` | int | Size in GB |
| `area` | str | Availability zone ID where the volume will be created |

**Returns** `{"code": 20000, "data": {...}}`

---

### `storage.delete(volume_name)`

Permanently delete a volume.

```python
client.storage.delete("my-dataset")
```

> ⚠️ This action is **irreversible**. All data on the volume is permanently lost. Make sure no instances are bound to it before deleting.

---

### `storage.resize(volume_name, size_gb)`

Expand a volume. Volumes can only be enlarged, not shrunk.

```python
client.storage.resize("my-dataset", size_gb=200)
```

---

### `storage.bind(volume_name, inst_id, mount_path)`

Attach a volume to a running instance at the specified mount path.

```python
client.storage.bind("my-dataset", "inst-abc123", "/workspace/data")
```

The mount path must begin with `/`. The instance must be in a state that accepts storage changes (usually stopped or running, depending on the volume type).

---

### `storage.unbind(volume_name, inst_id)`

Detach a volume from an instance.

```python
client.storage.unbind("my-dataset", "inst-abc123")
```

---

## Full Workflow Example

```python
from submodel import SubModelClient

client = SubModelClient(api_key="sk-...")

# 1. Create a 50 GB volume in us-east-1
vol = client.storage.create("training-data", size=50, area="us-east-1")

# 2. Attach it to a pod
client.storage.bind("training-data", "inst-abc123", "/workspace/data")

# 3. Later, list all volumes
result = client.storage.list()
for v in result["data"]["items"]:
    print(v["name"], v["size_gb"], v["status"])

# 4. Detach and clean up
client.storage.unbind("training-data", "inst-abc123")
client.storage.delete("training-data")
```

---

## Error Handling

```python
from submodel import AuthenticationError, APIError

try:
    client.storage.create("x", size=10, area="us-east-1")
except APIError as e:
    # e.g. code 40000 if name is too short
    print(f"Error {e.code}: {e.message}")
```
