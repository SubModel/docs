# Get Started

This guide walks you through installing the Submodel.ai Python SDK and sending your first request.

## Install

```bash
pip install submodel
```

## Set Your API Key

Export your API key as an environment variable so you never hardcode it in source files:

```bash
export SUBMODEL_API_KEY="sk-your-api-key"
```

Get your API key from [API Keys](https://submodel.ai/#/apikey).

## Create a Client

```python
from submodel.sdk.client import SubModelClient

client = SubModelClient(api_key="sk-your-api-key")
# or let the SDK pick it up from the environment:
client = SubModelClient()  # reads SUBMODEL_API_KEY automatically... 
```

> Actually, pass the key explicitly or use `os.environ`:
> ```python
> import os
> from submodel.sdk.client import SubModelClient
> client = SubModelClient(api_key=os.environ["SUBMODEL_API_KEY"])
> ```

## List Your Pods

```python
result = client.pod.list()
items = result["data"]["items"]

if not items:
    print("No instances found.")
else:
    for inst in items:
        print(f"{inst['inst_id']}  {inst['status']}  {inst['plan']}")
```

## Submit a Serverless Job

```python
# Submit an async job to a serverless endpoint
ep = client.serverless.endpoint("your-inst-id")
job = ep.run({"prompt": "A beautiful mountain landscape"})
print("Job submitted:", job["data"]["id"])

# Wait for completion
result = ep.wait(job["data"]["id"], timeout=120)
print("Output:", result["data"])
```

## Handle Errors

```python
from submodel.sdk.exceptions import AuthenticationError, APIError

try:
    result = client.pod.list()
except AuthenticationError:
    print("Invalid API key.")
except APIError as e:
    print(f"API error {e.code}: {e.message}")
```

## Next Steps

- [CLI Reference](cli.md) — use `submodelctl` from the terminal without writing Python
- [Pod SDK](pod.md) — create and manage Pod instances
- [Serverless SDK](serverless.md) — run async and sync jobs on serverless endpoints
- [Storage SDK](storage.md) — manage persistent storage volumes
