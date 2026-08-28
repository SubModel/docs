# CLI 参考 — submodelctl

`submodelctl` 是 SubModel 的命令行工具，随 `submodel` 包一起安装。

## 认证

通过命令行参数或环境变量传入凭证：

```bash
export SUBMODEL_API_KEY="sk-your-api-key"
# 或
submodelctl --api-key sk-your-api-key pod list
```

## 全局参数

| 参数 | 环境变量 | 说明 |
|---|---|---|
| `--api-key KEY` | `SUBMODEL_API_KEY` | API Key |
| `--token TOK` | `SUBMODEL_TOKEN` | Session Token |
| `--json` | — | 输出原始 JSON，而非格式化表格 |
| `--version` | — | 打印版本并退出 |

---

## pod

### `pod list`

列出你的 Pod 实例。

```bash
submodelctl pod list [--page N] [--limit N] [--mode pod|serverless] [--search STR]
```

| 选项 | 默认值 | 说明 |
|---|---|---|
| `--page` | 1 | 页码 |
| `--limit` | 20 | 每页数量 |
| `--mode` | — | 按 `pod` 或 `serverless` 过滤 |
| `--search` | — | 按标签搜索 |

### `pod get INST_ID`

获取单个实例的详细信息。

```bash
submodelctl pod get inst-abc123
```

### `pod create`

创建新的 Pod 实例。

```bash
submodelctl pod create --plan gpu-rtx4090-24g-1 --image pytorch/pytorch:2.0.0-cuda11.7-cudnn8-runtime
```

| 选项 | 默认值 | 说明 |
|---|---|---|
| `--plan` | 必填 | GPU 规格 ID |
| `--image` | 必填 | Docker 镜像，私有镜像加 `[custom]` 前缀 |
| `--billing` | `payg` | `payg` \| `day` \| `week` \| `month` \| `spot` \| `spare` |
| `--mode` | `pod` | `pod` 或 `serverless` |
| `--pods` | 1 | Pod 数量（1–128） |
| `--area` | — | 可用区，可重复指定 |
| `--volume-size` | 0 | 持久卷大小（GB） |
| `--container-size` | 10 | 容器磁盘大小（GB） |
| `--mount-path` | `/workspace` | 卷挂载路径 |
| `--label` | — | 显示标签 |

### `pod stop INST_ID`

停止运行中的实例。

```bash
submodelctl pod stop inst-abc123
```

### `pod start INST_ID`

启动已停止的实例。

```bash
submodelctl pod start inst-abc123
```

### `pod restart INST_ID`

重启实例。

```bash
submodelctl pod restart inst-abc123
```

### `pod release INST_ID`

永久删除实例。操作不可逆，默认会提示确认。

```bash
submodelctl pod release inst-abc123
submodelctl pod release inst-abc123 --force   # 跳过确认
```

### `pod logs INST_ID POD_ID`

获取指定 Pod 容器的日志。

```bash
submodelctl pod logs inst-abc123 pod-0
```

---

## serverless

### `serverless run INST_ID INPUT_JSON`

提交异步任务。

```bash
submodelctl serverless run inst-abc123 '{"prompt": "你好"}'
```

加 `--wait` 参数可阻塞等待任务完成：

```bash
submodelctl serverless run inst-abc123 '{"prompt": "你好"}' --wait --timeout 300
```

| 选项 | 默认值 | 说明 |
|---|---|---|
| `--wait` | false | 阻塞直到任务进入终态 |
| `--timeout` | 300 | 最大等待秒数 |
| `--poll` | 2.0 | 轮询间隔秒数 |

### `serverless runsync INST_ID INPUT_JSON`

提交同步任务并等待结果。

```bash
submodelctl serverless runsync inst-abc123 '{"prompt": "你好"}' --timeout 90
```

### `serverless status INST_ID JOB_ID`

查询任务状态。

```bash
submodelctl serverless status inst-abc123 job-xyz
```

### `serverless cancel INST_ID JOB_ID`

取消排队或运行中的任务。

```bash
submodelctl serverless cancel inst-abc123 job-xyz
```

### `serverless health INST_ID`

检查 Endpoint 健康状态和活跃 worker 数量。

```bash
submodelctl serverless health inst-abc123
```

---

## whoami

查看当前认证用户的账户信息。

```bash
submodelctl whoami
```
