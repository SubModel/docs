# Python SDK 概览

SubModel Python SDK 让你用代码管理 Pod 实例、Serverless 端点和持久化存储，同时提供 `submodelctl` 命令行工具。

## 功能

| 模块 | 说明 |
|---|---|
| `client.pod` | 创建、查询、启停、释放 Pod 实例 |
| `client.serverless` | 提交任务、查询状态、管理 Serverless 端点 |
| `client.storage` | 创建、挂载、管理持久化网络卷 |
| `submodelctl` | 命令行界面，功能与 Python SDK 对应 |
| `ServerlessWorker` | 在 Serverless 容器内编写 worker handler |

## 安装

```bash
pip install submodel
```

需要 Python 3.8 及以上版本。

## 认证

SDK 支持两种认证方式：

| 方式 | 适用场景 | 如何获取 |
|---|---|---|
| API Key（`sk-...`） | 脚本、CI/CD、长期使用 | 控制台 → Settings → API Keys |
| Session Token | 短期临时访问 | 调用登录接口获取 |

**推荐做法**：将 API Key 写入环境变量，避免硬编码到代码中。

```bash
export SUBMODEL_API_KEY="sk-your-api-key"
```

```python
from submodel import SubModelClient

# 自动从 SUBMODEL_API_KEY 环境变量读取
client = SubModelClient()

# 或显式传入
client = SubModelClient(api_key="sk-your-api-key")
```

## 快速示例

```python
from submodel import SubModelClient

client = SubModelClient()

# 列出所有 Pod 实例
result = client.pod.list()
for pod in result["data"]["items"]:
    print(pod["inst_id"], pod["status"])
```

## 异常

| 异常类 | 触发条件 |
|---|---|
| `AuthenticationError` | API Key 无效或 Token 过期（code 50008 / 50012 / 50014） |
| `ValidationError` | 请求参数错误（code 40000） |
| `APIError` | 其他服务端错误 |

```python
from submodel import SubModelClient, AuthenticationError, APIError

client = SubModelClient(api_key="sk-...")

try:
    result = client.pod.list()
except AuthenticationError:
    print("API Key 无效")
except APIError as e:
    print(f"错误 {e.code}: {e.message}")
```

## 下一步

* [快速开始](get-started.md)
* [命令行工具](cli.md)
* [Pod SDK 参考](pod.md)
* [Serverless SDK 参考](serverless.md)
* [Storage SDK 参考](storage.md)
