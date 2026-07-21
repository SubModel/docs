# Overview

SubModel provides Serverless GPU and CPU computing solutions tailored for AI inference, training, and general-purpose computation. With a pay-per-second pricing model, our platform offers unparalleled flexibility, dynamically scaling to meet the computational demands of AI workloads, from small-scale projects to enterprise-level operations.

## Usage Methods

- **Quick Deploy**: Utilize pre-configured custom endpoints for the most popular AI models.
- **Handler Functions**: Deploy and execute your custom functions in the cloud.
- **vLLM Endpoint**: Deploy any Hugging Face model directly to the cloud.

## Why Choose SubModel Serverless?

SubModel Serverless instances stand out for several compelling reasons:

- **AI Inference**: Capable of handling millions of inference requests daily, with scalability to billions, making it an optimal solution for machine learning inference tasks. This ensures cost-effective scaling for your machine learning inference needs.
- **AI Training**: Efficiently manage machine learning training tasks that can last up to 12 hours. GPUs are provisioned on-demand and scaled down post-task completion, offering a flexible and efficient training environment.
- **Autoscale**: Dynamically scale workers from 0 to 100 on the Secure Cloud platform, which is highly available and distributed globally. This provides users with the computational resources exactly when needed.
- **Container Support**: Deploy any Docker container to SubModel. Support for both public and private image repositories allows for complete environment customization.
- **3s Cold-Start**: SubModel proactively pre-warms workers to minimize cold-start times. While start times vary by runtime, for instance, stable diffusion experiences a 3-second cold-start plus a 5-second runtime.
- **Metrics and Debugging**: Gain transparency into your computational workloads with access to GPU, CPU, Memory, and other metrics. Comprehensive debugging capabilities, including logs, SSH access, and a web terminal, simplify troubleshooting.
- **Webhooks**: Utilize webhooks for immediate data output upon request completion. Results are pushed directly to your Webhook API, facilitating instant access.

SubModel Serverless is not limited to AI Inference and Training. It's also ideal for a variety of other computational tasks such as rendering, molecular dynamics, and more.

## Interacting with SubModel Serverless

SubModel assigns an Endpoint Id for interacting with your Serverless Pod. Incorporate your Endpoint Id into the Endpoint URL and specify an operation.

The Endpoint URL format is as follows:

```
https://api.submodel.ai/api/v1/sl/{inst_id}/{operation}
```

- `api.submodel.ai`: The base URL for accessing SubModel.
- `v1`: The API version.
- `inst_id`: The unique ID of your Serverless Instance.
- `operation`: The operation to execute on the Serverless Endpoint.
  - Valid operations include: <code>cancel</code> | <code>health</code> | <code>metrics</code> | <code>requests</code> | <code>status</code> | <code>run</code> | <code>runsync</code> | <code>purge-queue</code>
