# Concurrent Handlers in SubModel

SubModel empowers developers with asynchronous request handling capabilities, enabling a single worker to manage multiple tasks concurrently through non-blocking operations. This architectural approach significantly enhances task switching efficiency and resource utilization.

In serverless architectures, each worker can process multiple requests simultaneously, with the degree of concurrency determined by the runtime's capacity and available resources.

## Configuring the Concurrency Modifier

The `concurrency_modifier` is a pivotal configuration option within `submodel.serverless.start` that dynamically adjusts a worker's concurrency level. This feature enables optimal resource consumption and performance by regulating the number of concurrent tasks a worker can handle.

### Step 1: Implementing an Asynchronous Handler Function

Develop an asynchronous function specifically designed to process incoming requests. This function should efficiently yield results, preferably in batches, to maximize throughput.

```python
async def process_request(job):
    # Simulates processing delay
    await asyncio.sleep(1)
    return f"Processed: {job['input']}"
```

### Step 2: Establishing the `concurrency_modifier` Function

Create a function to dynamically adjust the worker's concurrency level based on the current request load. This function should account for both maximum and minimum concurrency levels, adapting to fluctuations in request volume.

```python
def adjust_concurrency(current_concurrency):
    """
    Dynamically adjusts the concurrency level based on the observed request rate.
    """
    global request_rate
    update_request_rate()  # Placeholder for request rate updates

    max_concurrency = 10  # Maximum allowable concurrency
    min_concurrency = 1  # Minimum concurrency to maintain
    high_request_rate_threshold = 50  # Threshold for high request volume

    # Increase concurrency if under max limit and request rate is high
    if (
        request_rate > high_request_rate_threshold
        and current_concurrency < max_concurrency
    ):
        return current_concurrency + 1
    # Decrease concurrency if above min limit and request rate is low
    elif (
        request_rate <= high_request_rate_threshold
        and current_concurrency > min_concurrency
    ):
        return current_concurrency - 1

    return current_concurrency
```

### Step 3: Initializing the Serverless Function

Launch the serverless function with the defined handler and `concurrency_modifier` to enable dynamic concurrency adjustment.

```python
submodel.serverless.start(
    {
        "handler": process_request,
        "concurrency_modifier": adjust_concurrency,
    }
)
```

---

## Example Implementation

Below is a comprehensive example demonstrating the setup for a SubModel serverless function capable of handling multiple concurrent requests.

```python
import submodel
import asyncio
import random

# Simulated Metrics
request_rate = 0

async def process_request(job):
    await asyncio.sleep(1)  # Simulate processing time
    return f"Processed: { job['input'] }"

def adjust_concurrency(current_concurrency):
    """
    Adjusts the concurrency level based on the current request rate.
    """
    global request_rate
    update_request_rate()  # Simulate changes in request rate

    max_concurrency = 10
    min_concurrency = 1
    high_request_rate_threshold = 50

    if (
        request_rate > high_request_rate_threshold
        and current_concurrency < max_concurrency
    ):
        return current_concurrency + 1
    elif (
        request_rate <= high_request_rate_threshold
        and current_concurrency > min_concurrency
    ):
        return current_concurrency - 1
    return current_concurrency

def update_request_rate():
    """
    Simulates changes in the request rate to mimic real-world scenarios.
    """
    global request_rate
    request_rate = random.randint(20, 100)

# Start the serverless function with the handler and concurrency modifier
submodel.serverless.start(
    {"handler": process_request, "concurrency_modifier": adjust_concurrency}
)
```

By leveraging the `concurrency_modifier` in SubModel, serverless functions can efficiently handle multiple requests concurrently, optimizing resource usage and enhancing performance. This methodology facilitates the development of scalable and responsive serverless applications.