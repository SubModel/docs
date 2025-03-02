# Use environment variables

Incorporating environment variables into your Handler Functions is a crucial practice for securely managing external resources such as S3 buckets. This section provides a comprehensive guide on utilizing environment variables to facilitate image uploads to an S3 bucket using SubModel Handler Functions. You will learn how to write Python code for uploading images and configure the necessary environment variables through the Web interface.

## Prerequisites

Before proceeding, ensure the following prerequisites are met:

- **SubModel Python Library**: Install the SubModel library using `pip install submodel`.
- **Image File**: Have an image file named `image.png` available in the Docker container's working directory.

## Python Code for S3 Image Uploads

Let's break down the steps to upload an image to an S3 bucket using Python:

### 1. **Handler Function for S3 Upload**

Here’s an example of a handler function that uploads `image.png` to an S3 bucket and returns the image URL:

```python
from submodel.serverless.utils import rp_upload
import submodel

def handler(job):
    image_url = rp_upload.upload_image(job["id"], "./image.png")
    return [image_url]

submodel.serverless.start({"handler": handler})
```

### 2. **Packaging Your Code**

Follow the guidelines in [Worker Image Creation](/serverless/workers/deploy) for packaging and deploying your code.

### Configuring Environment Variables for S3

Environment variables are essential for securely passing credentials and configurations to your serverless function. Here’s how to set them up:

#### 1. **Accessing Environment Variables**

In the template creation/editing interface of your pod, navigate to the section where environment variables can be configured.

#### 2. **Setting S3-Specific Variables**

Configure the following key environment variables for your S3 bucket:

- `BUCKET_ENDPOINT_URL`
- `BUCKET_ACCESS_KEY_ID`
- `BUCKET_SECRET_ACCESS_KEY`

**Note**: Ensure that your `BUCKET_ENDPOINT_URL` includes the bucket name. For example:

```
https://your-bucket-name.nyc3.digitaloceanspaces.com
```

## Testing Your API

Finally, test your serverless function to confirm that it successfully uploads images to your S3 bucket:

### 1. **Making a Request**

Make a POST request to your API endpoint with the necessary headers and input data. The input must be a JSON object:

```python
import requests

endpoint = "https://api.submodel.ai/v2/xxxxxxxxx/run"
headers = {"Content-Type": "application/json", "Authorization": "Bearer XXXXXXXXXXXXX"}
input_data = {"input": {"inp": "this is an example input"}}

response = requests.post(endpoint, json=input_data, headers=headers)
```

### 2. **Checking the Output**

Make a GET request to retrieve the job status and output. Here’s an example:

```python
response = requests.get(
    "https://api.submodel.ai/v2/xxxxxxxxx/status/" + response.json()["id"],
    headers=headers,
)
response.json()
```

The response should include the URL of the uploaded image upon completion:

```json
{
  "delayTime": 86588,
  "executionTime": 1563,
  "id": "e3d2e250-ea81-4074-9838-1c52d006ddcf",
  "output": [
    "https://your-bucket.s3.us-west-004.backblazeb2.com/your-image.png"
  ],
  "status": "COMPLETED"
}
```

## Conclusion

By following these steps, you can effectively use environment variables to manage S3 bucket credentials and operations within your SubModel Handler Functions. This approach ensures secure, scalable, and efficient handling of external resources in your serverless applications.