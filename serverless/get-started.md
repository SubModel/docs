# Get Started with Endpoints

## Overview

This guide will help you understand how to build a Docker image, deploy a Serverless endpoint, and send requests. You'll also gain a basic understanding of customizing the handler for your specific use case.

## Prerequisites

This section assumes you are familiar with using the terminal and can execute commands from it.

### SubModel

To proceed with this quick start, you'll need the following from SubModel:

- A SubModel account
- A SubModel API Key

### Docker

To build your Docker image, ensure you have the following:

- Docker installed
- A Docker account

### GitHub

To clone the `worker-template` repository, you'll need:

- Git installed
- Permissions to clone GitHub repositories

## Build a Serverless Application on SubModel

Follow these steps to create a handler file, test it locally, and build a Docker image for deployment:

1. **Create the handler file (`rp_handler.py`):**

```python
import submodel
import time

def handler(event):
    input = event['input']
    instruction = input.get('instruction')
    seconds = input.get('seconds', 0)

    # Placeholder for a task; replace with image or text generation logic as needed
    time.sleep(seconds)
    result = instruction.replace(instruction.split()[0], 'created', 1)

    return result

if __name__ == '__main__':
    submodel.serverless.start({'handler': handler})
```

2. **Create a `test_input.json` file in the same folder:**

```json
{
    "input": {
        "instruction": "create a image",
        "seconds": 15
    }
}
```

3. **Test the handler code locally:**

```bash
python3 rp_handler.py

# You should see an output like this:
--- Starting Serverless Worker |  Version 1.7.0 ---
INFO   | Using test_input.json as job input.
DEBUG  | Retrieved local job: {'input': {'instruction': 'create a image', 'seconds': 15}, 'id': 'local_test'}
INFO   | local_test | Started.
DEBUG  | local_test | Handler output: created a image
DEBUG  | local_test | run_job return: {'output': 'created a image'}
INFO   | Job local_test completed successfully.
INFO   | Job result: {'output': 'created a image'}
INFO   | Local testing complete, exiting.
```

4. **Create a Dockerfile:**

```dockerfile
FROM python:3.10-slim

WORKDIR /
RUN pip install --no-cache-dir submodel
COPY rp_handler.py /

# Start the container
CMD ["python3", "-u", "rp_handler.py"]
```

5. **Build and push your Docker image:**

```bash
docker build --platform linux/amd64 --tag <username>/<repo>:<tag> .
```

6. **Push to your container registry:**

```bash
docker push <username>/<repo>:<tag>
```

> **Note**
>
> When building your Docker image, you might need to specify the platform you are building for. This is particularly important if you are building on a machine with a different architecture than the one you are deploying to.
>
> When building for SubModel providers, use `--platform=linux/amd64`.

Alternatively, you can clone our [worker-template](https://github.com/submodel-workers/worker-template) repository to quickly build a Docker image and push it to your container registry for a faster start.

Now that you've pushed your container registry, you're ready to deploy your Serverless Endpoint to SubModel.

## Deploy a Serverless Endpoint

This step will guide you through deploying a Serverless Endpoint to SubModel. You can refer to this walkthrough to deploy your own custom Docker image.

1. Log in to the [SubModel Serverless console](https://www.submodel.io/console/serverless).
2. Select **+ New Endpoint**.
3. Provide the following:
   1. Endpoint name.
   2. Select your GPU configuration.
   3. Configure the number of Workers.
   4. (Optional) Select **FlashBoot**.
   5. (Optional) Select a template.
   6. Enter the name of your Docker image.
      - For example, `<username>/<repo>:<tag>`.
   7. Specify enough memory for your Docker image.
4. Select **Deploy**.

Now, let's send a request to your [Endpoint](/serverless/endpoints/get-started).