# 快速开始

本指南帮助你在 5 分钟内完成 SDK 安装并运行第一个示例。

## 1. 安装

```bash
pip install submodel
```

同时安装了 Python SDK 和 `submodelctl` 命令行工具。

## 2. 设置 API Key

前往 [控制台 → Settings → API Keys](https://submodel.ai/console) 创建 API Key，然后写入环境变量：

```bash
# macOS / Linux
export SUBMODEL_API_KEY="sk-your-api-key"

# Windows PowerShell
$env:SUBMODEL_API_KEY = "sk-your-api-key"
```

> 不要将 API Key 直接写入代码或提交到代码仓库。

## 3. 创建客户端

```python
from submodel import SubModelClient

# 自动读取 SUBMODEL_API_KEY 环境变量
client = SubModelClient()
```

## 4. 列出 Pod 实例

```python
result = client.pod.list()
items = result["data"]["items"]

if items:
    for pod in items:
        print(f"{pod['inst_id']}  {pod['status']}")
else:
    print("暂无实例")
```

## 5. 提交 Serverless 任务

```python
# 异步提交（立即返回 job ID）
ep = client.serverless.endpoint("inst-abc123")
job = ep.run({"prompt": "用一句话解释 Transformer 模型"})
job_id = job["data"]["id"]

# 等待结果
result = ep.wait(job_id, timeout=120)
print(result["data"]["output"])
```

同步版本（等待完成后才返回）：

```python
result = ep.run_sync({"prompt": "Hello"}, timeout=60)
print(result["data"]["output"])
```

## 6. 错误处理

```python
from submodel import SubModelClient, AuthenticationError, APIError

client = SubModelClient(api_key="sk-...")

try:
    result = client.pod.list()
except AuthenticationError:
    print("API Key 无效或已过期")
except APIError as e:
    print(f"请求失败，错误码 {e.code}：{e.message}")
```

## 下一步

* [命令行工具](cli.md) — 直接从终端管理实例
* [Pod SDK 参考](pod.md) — 创建和管理 Pod 的完整 API
* [Serverless SDK 参考](serverless.md) — 提交任务和编写 worker
* [Storage SDK 参考](storage.md) — 持久化网络卷管理
