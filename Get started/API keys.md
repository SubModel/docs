# API keys

API keys authenticate requests to Submodel.ai. You can generate an API key with Read/Write permission, Restricted permission, or Read Only permission.

## Generate

To create an API key:

1. From the console, select **Settings**.
2. Under **API Keys**, choose **+ Create API Key**.
3. Select the permission. If you choose Restricted permission, you can customize access for each API:
   - **None**: No access
   - **(AI API only) Restricted**: Custom access to specific endpoints. No access is default.
   - **Read/Write**: Full access
   - **Read Only**: Read access without write access

   > **Warning**
   > Select the minimum permission needed for your use case. Only allow full access to GraphQL when absolutely necessary for automations like creating or managing Submodel.ai resources outside of Serverless endpoints.

4. Choose **Create**.

> **Note**
> Once your API key is generated, keep it secure. Treat it like a password and avoid sharing it in insecure environments.

## Edit permission

To edit an API key:

1. From the console, select **Settings**.
2. Under **API Keys**, select the pencil icon and select the permission.
3. Choose **Update**.

## Disable

To disable an API key:

1. From the console, select **Settings**.
2. Under **API Keys**, select the toggle and select **Yes**.

## Enable

To enable an API key:

1. From the console, select **Settings**.
2. Under **API Keys**, select the toggle and select **Yes**.

## Revoke

To delete an API key:

1. From the console, select **Settings**.
2. Under **API Keys**, select the trash can icon and select **Revoke Key**.