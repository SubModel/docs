# Quick Deploys

Quick Deploys enables you to deploy custom endpoints for popular models with minimal configuration.

You can explore [Quick Deploys](https://www.submodel.ai/console/serverless) and their descriptions in the web interface.

## Getting Started with Quick Deploys

To begin using Quick Deploys, follow these steps:

1. Navigate to the [Serverless section](https://www.submodel.ai/console/serverless) in the web interface.
2. Choose your desired model.
3. Assign a name to your endpoint.
4. Select a GPU instance.
   1. (Optional) Further customize your deployment settings.
5. Click **Deploy**.

Once deployed, your Endpoint ID will be generated, and you can integrate it into your application.

## Customizing Your Functions

To tailor AI endpoints to your needs, visit the [SubModel GitHub repositories](https://github.com/submodel). Here, you can fork and modify programming and compute model templates.

Start with the [worker-template](https://github.com/submodel/worker-template) and adjust it as required. These SubModel workers include CI/CD features to simplify your project setup.

For comprehensive instructions on customizing your interaction endpoints, consult the [Handler Functions](/serverless/workers/handlers/overview) documentation.