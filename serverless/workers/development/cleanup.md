# Efficient Resource Management with SubModel's `clean()` Function

When developing serverless applications with SubModel, efficient resource management is essential. The SubModel SDK provides a powerful `clean()` utility function to help you maintain a clean environment by removing temporary files and folders after processing. This guide will demonstrate how to effectively utilize this cleanup feature in your serverless workflows.

## Overview of the `clean()` Function

The `clean()` function is a key component of SubModel's serverless utilities, designed to automatically remove specified directories and files after job completion.

To use it, import the function from the SubModel serverless utilities module:

```python
from submodel.serverless.utils.rp_cleanup import clean
```

## Default Cleanup Behavior

By default, the `clean()` function removes the following resources:

- `input_objects` directory
- `output_objects` directory
- `job_files` directory
- `output.zip` file

## Implementation in Your Handler Function

Here's a comprehensive example demonstrating how to integrate the `clean()` function in your AI model handler:

```python
import submodel
from submodel.serverless.utils.rp_cleanup import clean
import requests
import os


def download_image(url, save_path):
    """Download an image from a given URL and save it to the specified path."""
    response = requests.get(url)
    if response.status_code == 200:
        with open(save_path, "wb") as file:
            file.write(response.content)
        return True
    return False


def handler(event):
    """
    AI model handler function that:
    1. Downloads an image
    2. Processes it
    3. Cleans up temporary resources
    """
    try:
        # Extract the image URL from the input
        image_url = event["input"]["image_url"]

        # Create a temporary directory for the image
        os.makedirs("temp_images", exist_ok=True)
        image_path = "temp_images/downloaded_image.jpg"

        # Download the image
        if not download_image(image_url, image_path):
            raise Exception("Failed to download image")

        # Your AI model processing logic here
        # For demonstration, we're simulating processing
        result = f"Processed image from: {image_url}"

        # Perform cleanup after processing
        clean(folder_list=["temp_images"])

        # Return the processing result
        return {"output": result}
    except Exception as e:
        # Ensure cleanup in case of errors
        clean(folder_list=["temp_images"])
        return {"error": str(e)}


# Initialize the serverless function
submodel.serverless.start({"handler": handler})
```

In this implementation, the `clean()` function is strategically placed to ensure temporary files and folders are removed after processing, regardless of whether the operation succeeds or fails.

## Custom Cleanup Configuration

You can specify additional directories for cleanup by passing a list to the `clean()` function:

```python
clean(["custom_folder1", "custom_folder2"])
```

## Testing Your Handler with Cleanup

To test your handler with the cleanup functionality:

### CLI Testing

```bash
python ai_model_handler.py \
  --test_input '{
    "input": {
        "image_url": "https://avatars.githubusercontent.com/u/95939477?s=200&v=4"
    }
}'
```

### JSON File Testing

1. Create a `test_input.json` file:

```json
{
  "input": {
    "image_url": "https://avatars.githubusercontent.com/u/95939477?s=200&v=4"
  }
}
```

2. Execute the handler:

```bash
python ai_model_handler.py
```

## Best Practices for Effective Cleanup

1. **Consistent Cleanup**: Always call `clean()` at the end of your handler function to ensure proper resource cleanup.
2. **Error Handling**: Implement try-except blocks to handle potential errors during cleanup operations.
3. **Custom Cleanup**: Exercise caution when adding custom folders to the cleanup list to avoid accidental deletion of important files.
4. **Logging**: Consider implementing logging mechanisms to track cleanup activities for debugging and monitoring purposes.

By incorporating the `clean()` function into your serverless handlers, you ensure that each job execution starts with a clean environment, preventing potential issues caused by residual files from previous executions. This practice enhances the reliability and efficiency of your SubModel serverless applications.