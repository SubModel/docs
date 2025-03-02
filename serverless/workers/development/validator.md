# Input Validation with SubModel's Validator Utility

SubModel's validator utility ensures robust execution of serverless workers by validating input data against a defined schema. This approach allows for early detection of input errors, preventing issues from unexpected or malformed inputs.

## Getting Started

To use the validator, import the following into your Python file:

```python
from submodel.serverless.utils.rp_validator import validate
```

The `validate` function takes two arguments:

- **Input Data**: The data to be validated.
- **Schema**: The schema to validate the input data against.

## Schema Definition

Define your schema as a nested dictionary with the following possible rules for each input:

- **`required`** (default: `False`): Marks the field as required.
- **`default`** (default: `None`): Default value if the input is not provided.
- **`type`** (required): The expected type of the input.
- **`constraints`** (optional): Additional constraints, such as a lambda function that returns `True` or `False`.

## Example Usage

Here's an example of how to use the validator in a serverless worker:

```python
import submodel
from submodel.serverless.utils.rp_validator import validate

# Define the schema
schema = {
    "text": {
        "type": str,
        "required": True,
    },
    "max_length": {
        "type": int,
        "required": False,
        "default": 100,
        "constraints": lambda x: x > 0,
    },
}

# Handler function
def handler(event):
    try:
        # Validate the input
        validated_input = validate(event["input"], schema)
        if "errors" in validated_input:
            return {"error": validated_input["errors"]}

        # Extract validated inputs
        text = validated_input["validated_input"]["text"]
        max_length = validated_input["validated_input"]["max_length"]

        # Process the input
        result = text[:max_length]
        return {"output": result}
    except Exception as e:
        return {"error": str(e)}

# Start the serverless worker
submodel.serverless.start({"handler": handler})
```

## Testing

Save the above code as `your_handler.py` and test it using the following methods:

### Command Line Testing

Run the handler directly:

```bash
python your_handler.py
```

Or test with inline input:

```bash
python your_handler.py --test_input '{"input": {"text": "Hello, world!", "max_length": 5}}'
```

### JSON Input Testing

Create a `test_input.json` file:

```json
{
  "input": {
    "text": "The quick brown fox jumps over the lazy dog",
    "max_length": 50
  }
}
```

Then run the handler with the JSON file:

```bash
python your_handler.py --test_input @test_input.json
```

This approach ensures that your serverless workers are robust and can handle unexpected or malformed inputs gracefully.