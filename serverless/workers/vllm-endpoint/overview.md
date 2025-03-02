# Overview

Deploy a vLLM Worker using the `submodel/worker-v1-vllm:stable-cuda12.1.0` image.  
The vLLM Worker supports most Hugging Face LLMs and is compatible with OpenAI's API by specifying the `MODEL_NAME` parameter.  
You can also utilize SubModel's [`input` request format](/serverless/endpoints/send-requests).

SubModel's vLLM Serverless Endpoint Worker is a highly optimized solution for leveraging the power of various LLMs.

For more information, refer to the [vLLM Worker](https://github.com/SubModel/worker-vllm) repository.

## Key Features

- **Ease of Use**: Deploy any LLM using the pre-built Docker image without the need to build custom Docker images, upload heavy models, or wait for lengthy downloads.
- **OpenAI Compatibility**: Seamlessly integrate with OpenAI's API by modifying just 2 lines of code. Supports Chat Completions, Completions, and Models, with both streaming and non-streaming capabilities.
- **Dynamic Batch Size**: Enjoy the rapid time-to-first-token of no batching combined with the high throughput of larger batch sizes. (Related to batching tokens when streaming output)
- **Extensive Model Support**: Deploy almost any LLM from Hugging Face, including your own custom models.
- **Customization**: Gain full control over every aspect of your deployment, from model settings and tokenizer options to system configurations, all managed through environment variables.
- **Speed**: Experience the speed of the vLLM Engine.
- **Serverless Scalability and Cost-Effectiveness**: Scale your deployment to handle any number of requests and only pay for active usage.

## Compatible Models

You can deploy most [models from Hugging Face](https://huggingface.co/models?other=LLM).  
For a full list of supported model architectures, see [Compatible Model Architectures](https://github.com/SubModel/worker-vllm/blob/main/README.md#compatible-model-architectures).

## Getting Started

At a high level, you can set up the vLLM Worker by:

- Selecting your deployment options
- Configuring any necessary environment variables
- Deploying your model

For detailed guidance on setting up, configuring, and deploying your vLLM Serverless Endpoint Worker, including compatibility details, environment variable settings, and usage examples, see [Get Started](/serverless/workers/vllm/get-started).

### Deployment Options

- **[Configurable Endpoints](/serverless/workers/vllm/get-started#deploy-using-the-web-ui)**: (Recommended) Use SubModel's Web UI to quickly deploy the OpenAI-compatible LLM with the vLLM Worker.
- **[Pre-Built Docker Image](/serverless/workers/vllm/get-started#deploy-using-the-worker-image)**: Leverage a pre-configured Docker image for hassle-free deployment. Ideal for users seeking a quick and straightforward setup process.
- **Custom Docker Image**: For advanced users, customize and build your Docker image with the model baked in, offering greater control over the deployment process.

For more information, see:

- [vLLM Worker GitHub Repository](https://github.com/SubModel/worker-vllm)
- [vLLM Worker Docker Hub](https://hub.docker.com/r/submodel/worker-vllm/tags)

For more information on creating a custom Docker image, see [Build Docker Image with Model Inside](https://github.com/SubModel/worker-vllm/blob/main/README.md#option-2-build-docker-image-with-model-inside).

## Next Steps

- [Get Started](/serverless/workers/vllm/get-started): Learn how to deploy a vLLM Worker as a Serverless Endpoint, with detailed guides on configuration and sending requests.
- [Configurable Endpoints](/serverless/workers/vllm/configurable-endpoints): Select your Hugging Face model and let vLLM handle the low-level details of model loading, hardware configuration, and execution.
- [Environment Variables](/serverless/workers/vllm/environment-variables): Explore the environment variables available for the vLLM Worker, including detailed documentation and examples.
- [Run Gemma 7b](/tutorials/serverless/gpu/run-gemma-7b): Walk through deploying Google's Gemma model using SubModel's vLLM Worker, guiding you to set up a Serverless Endpoint with a gated large language model (LLM).