# Python SDK Overview

The Submodel.ai Python SDK (`submodel`) lets you manage Pods, Serverless endpoints, and storage volumes programmatically from Python code or the `submodelctl` command-line tool.

## What You Can Do

- **Pods** — list, create, start, stop, restart, and release container instances
- **Serverless** — submit jobs to endpoints, poll for results, and manage endpoint health
- **Storage** — manage persistent volume claims (PVC)
- **CLI** — the `submodelctl` command-line tool wraps the SDK for shell scripting and one-off tasks

## Installation

```bash
pip install submodel
```

For development extras (includes pytest):

```bash
pip install "submodel[dev]"
```

## Authentication

The SDK accepts two credential types. Pass them directly or export as environment variables:

| Credential | Direct param | Environment variable |
|---|---|---|
| API Key | `api_key="sk-..."` | `SUBMODEL_API_KEY` |
| Session token | `token="tok-..."` | `SUBMODEL_TOKEN` |

```python
from submodel.sdk.client import SubModelClient

client = SubModelClient(api_key="sk-your-api-key")
```

> **Note:** Never hardcode credentials in source files. Use environment variables or a secrets manager.

## Quick Example

```python
import os
from submodel.sdk.client import SubModelClient

client = SubModelClient(api_key=os.environ["SUBMODEL_API_KEY"])

# List your running pods
result = client.pod.list(limit=5)
for inst in result["data"]["items"]:
    print(inst["inst_id"], inst["status"])
```

## Next Steps

- [Get Started](get-started.md) — install the SDK and run your first request
- [CLI Reference](cli.md) — use `submodelctl` from the terminal
- [Pod SDK](pod.md) — full Pod API reference
- [Serverless SDK](serverless.md) — submit and manage serverless jobs
- [Storage SDK](storage.md) — manage persistent volumes
