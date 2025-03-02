# Generator Handler

SubModel offers a powerful streaming capability that allows users to receive real-time updates on job outputs, particularly useful for Language Model tasks. We support two types of streaming generator functions: regular generators and asynchronous generators.

```python
import submodel

def generator_handler(job):
    for count in range(3):
        result = f"This is the {count} generated output."
        yield result

submodel.serverless.start(
    {
        "handler": generator_handler,  # Required
        "return_aggregate_stream": True,  # Optional, results available via /run
    }
)
```

## Return Aggregate Stream

By default, when a generator handler is running, the partial outputs are only accessible through the `/stream` endpoint. If you also want these outputs to be available from the `/run` and `/runsync` endpoints, you need to set the `return_aggregate_stream` parameter to `True` when initializing your handler.