# Additional Controls

## Update Progress

Progress updates can be sent from your worker while a job is in progress. These updates will be available when the job status is polled. To send an update, call the `submodel.serverless.progress_update` function with your job and the context of your update.

```python
import submodel

def handler(job):
    for update_number in range(0, 3):
        submodel.serverless.progress_update(job, f"Update {update_number}/3")

    return "done"

submodel.serverless.start({"handler": handler})
```

## Refresh Worker

When handling long-running job requests or complex tasks involving extensive file operations, it can be beneficial to start with a fresh worker each time. This can be achieved by returning a flag with the job output to stop and refresh the used worker.

This behavior is implemented as follows within your worker:

### Synchronous

```python
# Requires submodel python version 0.9.0+
import submodel
import time

def sync_handler(job):
    job_input = job["input"]  # Access the input from the request.

    results = []
    for i in range(5):
        # Generate a synchronous output token
        output = f"Generated sync token output {i}"
        results.append(output)

        # Simulate a synchronous task, such as processing time for a large language model
        time.sleep(1)

    # Return the results and indicate the worker should be refreshed
    return {"refresh_worker": True, "job_results": results}

# Configure and start the SubModel serverless function
submodel.serverless.start(
    {
        "handler": sync_handler,  # Required: Specify the sync handler
        "return_aggregate_stream": True,  # Optional: Aggregate results are accessible via /run endpoint
    }
)
```

### Asynchronous

```python
import submodel
import asyncio

async def async_generator_handler(job):
    results = []
    for i in range(5):
        # Generate an asynchronous output token
        output = f"Generated async token output {i}"
        results.append(output)

        # Simulate an asynchronous task, such as processing time for a large language model
        await asyncio.sleep(1)

    # Return the results and indicate the worker should be refreshed
    return {"refresh_worker": True, "job_results": results}

# Configure and start the SubModel serverless function
submodel.serverless.start(
    {
        "handler": async_generator_handler,  # Required: Specify the async handler
        "return_aggregate_stream": True,  # Optional: Aggregate results are accessible via /run endpoint
    }
)
```

Your handler must return a dictionary containing the `refresh_worker` flag. This flag will be removed before the remaining job output is returned.

:::note
Refreshing a worker does not impact billing or count for/against your min, max, and warmed workers. It simply "resets" that worker at the end of a job.
:::