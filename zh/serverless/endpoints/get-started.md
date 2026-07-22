# 快速上手

现在你的 Endpoint 已经部署完成,是时候发送一个测试请求了。这是在将 Endpoint 集成到应用之前,确认其正常运行的绝佳方式。

## 发送请求

1. 进入 Endpoint 页面,选择 **Requests**。
2. 点击 **Run**。
3. 你应当收到类似下面的成功响应:

```json
{
  "id": "8f3a2b4c-5678-1234-9876-54321fedcba9-u2",
  "status": "IN_QUEUE"
}
```

几分钟后,流式输出将显示完整响应。

现在你已经可以从终端或应用向你的 Endpoint 发送请求了。

## 使用 cURL 发送请求

Endpoint 部署完成后,你可以使用 cURL 发送请求。下例演示了如何用 cURL 向 Endpoint 发送请求,但你也可以使用任意 HTTP 客户端。

```curl
curl --request POST \
     --url https://api.submodel.ai/v1/sl/${endpoint_id}/runsync \
     --header "accept: application/json" \
     --header "x-apikey: ${YOUR_API_KEY}" \
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

将 `endpoint_id` 替换为你的 Endpoint 名称,将 `YOUR_API_KEY` 替换为你的实际 API Key。

> **注意** 根据你对 Handler 所做的修改,你可能需要相应地调整请求内容。

## 后续步骤

现在你已经成功使用 Template 启动了一个 Endpoint,你可以:

* [调用 job](https://github.com/SubModel/docs/tree/main/serverless/endpoints/job-operations.md)
