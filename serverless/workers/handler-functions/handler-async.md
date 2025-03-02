# Asynchronous Handler in SubModel

SubModel supports the implementation of asynchronous handlers, which are essential for efficiently managing tasks that benefit from non-blocking operations. This feature is particularly advantageous for scenarios such as processing large datasets, interacting with APIs, or handling I/O-bound operations.

## Implementing Asynchronous Handlers

Asynchronous handlers in SubModel are crafted using Python's `async` and `await` syntax. Below is a sample implementation of an asynchronous generator handler. This example illustrates how to yield multiple outputs over time, simulating tasks like processing data streams or generating responses incrementally.

```python
import submodel
import asyncio

async def async_generator_handler(job):
    for i in range(5):
        # Generate an asynchronous output token
        output = f"Generated async token output {i}"
        yield output

        # Simulate an asynchronous task, such as processing time for a large language model
        await asyncio.sleep(1)

# Configure and start the SubModel serverless function
submodel.serverless.start(
    {
        "handler": async_generator_handler,  # Required: Specify the async handler
        "return_aggregate_stream": True,  # Optional: Aggregate results are accessible via /run endpoint
    }
)
```

### Advantages of Asynchronous Handlers

- **Efficiency**: Asynchronous handlers can execute non-blocking operations, enabling more tasks to be handled concurrently.
- **Scalability**: They are ideal for scaling applications, especially when dealing with high-frequency requests or large-scale data processing.
- **Flexibility**: Async handlers offer the flexibility to yield results over time, making them suitable for streaming data and long-running tasks.

### Best Practices

When developing asynchronous handlers:

- Ensure proper use of `async` and `await` to prevent blocking operations.
- Consider employing `yield` for generating multiple outputs over time.
- Thoroughly test your handlers to manage asynchronous exceptions and edge cases effectively.

Utilizing asynchronous handlers in your SubModel applications can significantly boost performance and responsiveness, particularly for applications requiring real-time data processing or handling multiple requests simultaneously.