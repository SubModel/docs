# Get Started

Now that your Endpoint is deployed, it's time to send a test request. This is an excellent way to ensure your Endpoint is functioning correctly before integrating it into your application.

## Send a Request

1. Navigate to the Endpoint's page and select **Requests**.
2. Click on **Run**.
3. You should receive a successful response similar to the following:

```json
{
  "id": "8f3a2b4c-5678-1234-9876-54321fedcba9-u2",
  "status": "IN_QUEUE"
}
```

After a few minutes, the stream will display the complete response.

You are now ready to start sending requests to your Endpoint from your terminal or application.

## Send a Request Using cURL

Once your Endpoint is deployed, you can send a request using cURL. This example demonstrates how to send a request to the Endpoint using cURL, but you can use any HTTP client.

```curl
curl --request POST \
     --url https://api.submodel.ai/v2/${endpoint_id}/runsync \
     --header "accept: application/json" \
     --header "authorization: ${YOUR_API_KEY}" \
     --header "content-type: application/json" \
     --data '
{
  "input": {
    "prompt": "A coffee cup.",
    "height": 512,
    "width": 512,
    "num_outputs": 1,
    "num_inference_steps": 50,
    "guidance_scale": 7.5,
    "scheduler": "KLMS"
  }
}
'
```

Replace `endpoint_id` with the name of your Endpoint and `YOUR_API_KEY` with your actual API Key.

> **Note**
> Depending on any modifications you made to your Handler Function, you may need to adjust the request accordingly.

## Next Steps

Now that you have successfully launched an endpoint using a template, you can:

- [Invoke jobs](/serverless/endpoints/job-operations)

If the provided models do not meet your needs, you can customize the Function Handler:

- [Customize the Handler Function](/serverless/workers/handlers/overview)