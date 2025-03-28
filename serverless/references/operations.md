# Endpoint Operations

RunPod's Endpoints enable seamless job submission and output retrieval. Access these endpoints at: `https://api.submodel.ai/v2/{endpoint_id}/{operation}`

## run

- **Method**: `POST`
- **Description**: Asynchronous endpoint for submitting jobs
- **Returns**: Unique Job ID
- **Payload Limit**: 10 MB
- **Rate Limit**: 1000 requests per 10 seconds, 200 concurrent
- **Job Availability**: 30 minutes after completion

## runsync

- **Method**: `POST`
- **Description**: Synchronous endpoint for shorter running jobs
- **Returns**: Immediate results
- **Payload Limit**: 20 MB
- **Rate Limit**: 2000 requests per 10 seconds, 400 concurrent
- **Job Availability**: 60 seconds after completion

## Queue Limits

- Requests will receive a `429 (Too Many Requests)` status if:
  - Queue size exceeds 50 jobs AND
  - Queue size exceeds endpoint.WorkersMax * 500

## Status and Stream Operations

All status and stream operations share a rate limit of 2000 requests per 10 seconds, 400 concurrent:

- `/status/{job_id}` - Check job status and retrieve outputs
  - Methods: `GET` | `POST`
- `/status-sync/{job_id}` - Synchronous status check
  - Methods: `GET` | `POST`
- `/stream/{job_id}` - Stream results from generator-type handlers
  - Methods: `GET` | `POST`

## Additional Operations

- `/cancel/{job_id}`
  - **Method**: `POST`
  - **Rate Limit**: 100 requests per 10 seconds, 20 concurrent
- `/purge-queue`
  - **Method**: `POST`
  - **Description**: Clears all queued jobs (does not affect running jobs)
  - **Rate Limit**: 2 requests per 10 seconds
- `/health`
  - **Method**: `GET`
  - **Description**: Provides worker statistics and endpoint health
- `/requests`
  - **Method**: `GET`
  - **Rate Limit**: 10 requests per 10 seconds, 2 concurrent

For detailed instructions on how to execute these Endpoint Operations, refer to [Invoke a Job](/serverless/endpoints/job-operations).