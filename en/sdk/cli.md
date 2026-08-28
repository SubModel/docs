# CLI Reference — submodelctl

`submodelctl` is the Submodel.ai command-line tool. It wraps the Python SDK and is installed automatically with the `submodel` package.

## Authentication

Pass credentials via flags or environment variables:

```bash
export SUBMODEL_API_KEY="sk-your-api-key"
# or
submodelctl --api-key sk-your-api-key pod list
```

## Global Flags

| Flag | Env var | Description |
|---|---|---|
| `--api-key KEY` | `SUBMODEL_API_KEY` | API key |
| `--token TOK` | `SUBMODEL_TOKEN` | Session token |
| `--json` | — | Output raw JSON instead of formatted tables |
| `--version` | — | Print version and exit |

---

## pod

### `pod list`

List your Pod instances.

```bash
submodelctl pod list [--page N] [--limit N] [--mode pod|serverless] [--search STR]
```

| Option | Default | Description |
|---|---|---|
| `--page` | 1 | Page number |
| `--limit` | 20 | Results per page |
| `--mode` | — | Filter by `pod` or `serverless` |
| `--search` | — | Search by label |

### `pod get INST_ID`

Get details for a single instance.

```bash
submodelctl pod get inst-abc123
```

### `pod create`

Create a new Pod instance.

```bash
submodelctl pod create --plan gpu-rtx4090-24g-1 --image pytorch/pytorch:2.0.0-cuda11.7-cudnn8-runtime
```

| Option | Default | Description |
|---|---|---|
| `--plan` | required | GPU plan ID |
| `--image` | required | Docker image. Prefix with `[custom]` for private registry |
| `--billing` | `payg` | `payg` \| `day` \| `week` \| `month` \| `spot` \| `spare` |
| `--mode` | `pod` | `pod` or `serverless` |
| `--pods` | 1 | Number of pods (1–128) |
| `--area` | — | Availability zone. Repeatable |
| `--volume-size` | 0 | Persistent volume size in GB |
| `--container-size` | 10 | Container disk size in GB |
| `--mount-path` | `/workspace` | Volume mount path |
| `--label` | — | Display label |

### `pod stop INST_ID`

Stop a running instance.

```bash
submodelctl pod stop inst-abc123
```

### `pod start INST_ID`

Start a stopped instance.

```bash
submodelctl pod start inst-abc123
```

### `pod restart INST_ID`

Restart an instance.

```bash
submodelctl pod restart inst-abc123
```

### `pod release INST_ID`

Permanently delete an instance. Prompts for confirmation.

```bash
submodelctl pod release inst-abc123
submodelctl pod release inst-abc123 --force   # skip confirmation
```

### `pod logs INST_ID POD_ID`

Fetch logs from a specific pod container.

```bash
submodelctl pod logs inst-abc123 pod-0
```

---

## serverless

### `serverless run INST_ID INPUT_JSON`

Submit an async job.

```bash
submodelctl serverless run inst-abc123 '{"prompt": "hello"}'
```

Add `--wait` to block until the job completes:

```bash
submodelctl serverless run inst-abc123 '{"prompt": "hello"}' --wait --timeout 300
```

| Option | Default | Description |
|---|---|---|
| `--wait` | false | Block until terminal state |
| `--timeout` | 300 | Max wait time in seconds |
| `--poll` | 2.0 | Poll interval in seconds |

### `serverless runsync INST_ID INPUT_JSON`

Submit a synchronous job and wait inline.

```bash
submodelctl serverless runsync inst-abc123 '{"prompt": "hello"}' --timeout 90
```

### `serverless status INST_ID JOB_ID`

Get the status of a job.

```bash
submodelctl serverless status inst-abc123 job-xyz
```

### `serverless cancel INST_ID JOB_ID`

Cancel a queued or running job.

```bash
submodelctl serverless cancel inst-abc123 job-xyz
```

### `serverless health INST_ID`

Check endpoint health and active worker count.

```bash
submodelctl serverless health inst-abc123
```

---

## whoami

Show the authenticated user's account information.

```bash
submodelctl whoami
```
