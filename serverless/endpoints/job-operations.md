# Job Operations Guide

This guide provides detailed instructions on performing job operations using the Runpod Endpoint. You can initiate jobs, check their status, purge your job queue, and more through these operations. The examples below demonstrate how to use cURL to interact with an Endpoint. Additionally, you can use the [Python SDK](/sdks/python/endpoints) for programmatic interaction.

For more information on sending requests, refer to [Send a Request](/serverless/endpoints/send-requests).

## Asynchronous Endpoints

Asynchronous endpoints are designed for long-running tasks. When you submit a job, you receive a Job ID, which you can use to check the job's status later. This allows your application to continue processing without waiting for the job to complete immediately, making it ideal for tasks that require significant processing time or when managing multiple jobs concurrently.

```bash
curl -X POST https://api.submodel.ai/v2/{endpoint_id}/run \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer ${API_KEY}' \
    -d '{"input": {"prompt": "Your prompt"}}'
```

**Output:**

```json
{
  "id": "eaebd6e7-6a92-4bb8-a911-f996ac5ea99d",
  "status": "IN_QUEUE"
}
```

## Synchronous Endpoints

Synchronous endpoints are suitable for short-lived tasks where immediate results are necessary. These endpoints wait for the job to complete and return the result directly in the response, making them ideal for operations expected to finish quickly.

```bash
curl -X POST https://api.submodel.ai/v2/{endpoint_id}/runsync \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer ${API_KEY}' \
    -d '{"input": {"prompt": "Your prompt"}}'
```

**Output:**

```json
{
  "delayTime": 824,
  "executionTime": 3391,
  "id": "sync-79164ff4-d212-44bc-9fe3-389e199a5c15",
  "output": [
    {
      "image": "https://image.url",
      "seed": 46578
    }
  ],
  "status": "COMPLETED"
}
```

## Health Endpoint

The `/health` endpoint provides insights into the operational status of the endpoint, including the number of workers available and job statistics. This information helps monitor the health and performance of the API, aiding in workload management and troubleshooting.

```bash
curl --request GET \
     --url https://api.submodel.ai/v2/{endpoint_id}/health \
     --header 'accept: application/json' \
     --header 'Authorization: Bearer ${API_KEY}'
```

**Output:**

```json
{
  "jobs": {
    "completed": 1,
    "failed": 5,
    "inProgress": 0,
    "inQueue": 2,
    "retried": 0
  },
  "workers": {
    "idle": 0,
    "running": 0
  }
}
```

## Cancel Job

To cancel a job in progress, specify the `cancel` parameter with the endpoint ID and the job ID.

```bash
curl -X POST https://api.submodel.ai/v2/{endpoint_id}/cancel/{job_id} \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer ${API_KEY}'
```

**Output:**

```json
{
  "id": "724907fe-7bcc-4e42-998d-52cb93e1421f-u1",
  "status": "CANCELLED"
}
```

## Purge Queue Endpoint

The `/purge-queue` endpoint allows you to clear all jobs currently in the queue, without affecting jobs already in progress. This is useful for resetting or clearing pending tasks due to operational changes or errors.

```bash
curl -X POST https://api.submodel.ai/v2/{endpoint_id}/purge-queue \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer ${API_KEY}'
```

**Output:**

```json
{
  "removed": 2,
  "status": "completed"
}
```

## Check Job Status

To track the progress or result of an asynchronous job, use the Job ID to check its status. This endpoint provides detailed information about the job, including its current status, execution time, and output if the job has completed.

```bash
curl -X POST https://api.submodel.ai/v2/{endpoint_id}/status/{job_id} \
    -H 'Authorization: Bearer ${API_KEY}'
```

**Output:**

```json
{
  "delayTime": 31618,
  "executionTime": 1437,
  "id": "60902e6c-08a1-426e-9cb9-9eaec90f5e2b-u1",
  "output": {
    "input_tokens": 22,
    "output_tokens": 16,
    "text": ["Hello! How can I assist you today?\nUSER: I'm having"]
  },
  "status": "COMPLETED"
}
```

## Stream Results

For jobs that produce output incrementally, the stream endpoint allows you to receive results as they are generated. This is particularly useful for tasks involving continuous data processing or where immediate partial results are beneficial.

```bash
curl -X POST https://api.submodel.ai/v2/{endpoint_id}/stream/{job_id} \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer ${API_KEY}'
```

**Output:**

```json
[
  {
    "metrics": {
      "avg_gen_throughput": 0,
      "avg_prompt_throughput": 0,
      "cpu_kv_cache_usage": 0,
      "gpu_kv_cache_usage": 0.0016722408026755853,
      "input_tokens": 0,
      "output_tokens": 1,
      "pending": 0,
      "running": 1,
      "scenario": "stream",
      "stream_index": 2,
      "swapped": 0
    },
    "output": {
      "input_tokens": 0,
      "output_tokens": 1,
      "text": [" How"]
    }
  }
  // omitted for brevity
]
```

> **Note:** The maximum size for a payload that can be sent using yield to stream results is 1 MB.

## Rate Limits

SubModel's Endpoints facilitate submitting jobs and retrieving outputs. Access these endpoints at: `https://api.submodel.ai/v2/{endpoint_id}/{operation}`

- `/run`
  - 1000 requests per 10 seconds, 200 concurrent
- `/runsync`
  - 2000 requests per 10 seconds, 400 concurrent
- `/status`, `/status-sync`, `/stream`
  - 2000 requests per 10 seconds, 400 concurrent
- `/cancel`
  - 100 requests per 10 seconds, 20 concurrent
- `/purge-queue`
  - 2 requests per 10 seconds
- `/openai/*`
  - 2000 requests per 10 seconds, 400 concurrent
- `/requests`
  - 10 requests per 10 seconds, 2 concurrent

> **Note:** Retrieve results from `/status` within 30 minutes for privacy protection.

For reference information on Endpoints, see [Endpoint Operations](/serverless/references/operations).