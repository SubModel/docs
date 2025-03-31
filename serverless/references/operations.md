# Endpoint Operations

SubModel's Endpoints enable seamless job submission and output retrieval. Access these endpoints at: `https://api.submodel.ai/api/v1/sl/{inst_id}/{operation}`

## run

- **Method**: `POST`
- **Description**: Asynchronous endpoint for submitting jobs
- **Returns**: Unique Job ID
- **Payload Limit**: 10 MB
- **Rate Limit**: 1000 requests per 10 seconds, 200 concurrent
- **Job Availability**: 30 minutes after completion

### Example Request

**URL**: `https://api.submodel.ai/api/v1/sl/:inst_id/run`

**Request Body**:
```json
{
    "input": {
        "prompt": "Your prompt"
    }
}
```

## runsync

- **Method**: `POST`
- **Description**: Synchronous endpoint for shorter running jobs
- **Returns**: Immediate results
- **Payload Limit**: 20 MB
- **Rate Limit**: 2000 requests per 10 seconds, 400 concurrent
- **Job Availability**: 60 seconds after completion

### Example Request

**URL**: `https://api.submodel.ai/api/v1/sl/:inst_id/runsync`

**Request Body**:
```json
{
    "input": {
        "prompt": "Your prompt"
    }
}
```

## Queue Limits

- Requests will receive a `429 (Too Many Requests)` status if:
  - Queue size exceeds 50 jobs AND
  - Queue size exceeds endpoint.WorkersMax * 500

## Status and Stream Operations

All status and stream operations share a rate limit of 2000 requests per 10 seconds, 400 concurrent:

- `/status/{job_id}` - Check job status and retrieve outputs
  - **Methods**: `GET` | `POST`
  - **URL**: `https://api.submodel.ai/api/v1/sl/:inst_id/status/:job_id`

- `/status-sync/{job_id}` - Synchronous status check
  - **Methods**: `GET` | `POST`

- `/stream/{job_id}` - Stream results from generator-type handlers
  - **Methods**: `GET` | `POST`

## Additional Operations

- `/cancel/{job_id}`
  - **Method**: `POST`
  - **Description**: Cancel a job prematurely
  - **Rate Limit**: 100 requests per 10 seconds, 20 concurrent
  - **URL**: `https://api.submodel.ai/api/v1/sl/:inst_id/cancel/:job_id`

- `/purge-queue`
  - **Method**: `POST`
  - **Description**: Clears all queued jobs (does not affect running jobs)
  - **Rate Limit**: 2 requests per 10 seconds

- `/health`
  - **Method**: `GET`
  - **Description**: Provides worker statistics and endpoint health
  - **URL**: `https://api.submodel.ai/api/v1/sl/:inst_id/health`

- `/metrics`
  - **Method**: `GET`
  - **Description**: Retrieve endpoint metrics
  - **URL**: `https://api.submodel.ai/api/v1/sl/:inst_id/metrics`

- `/requests`
  - **Method**: `GET`
  - **Description**: Retrieve request logs
  - **Rate Limit**: 10 requests per 10 seconds, 2 concurrent
  - **URL**: `https://api.submodel.ai/api/v1/sl/:inst_id/_requests`

For detailed instructions on how to execute these Endpoint Operations, refer to [Invoke a Job](/serverless/endpoints/job-operations.md).