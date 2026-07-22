# 发送 Job 请求

本指南概述了如何使用 JSON 请求体向 endpoint 发送 job 请求。以下是关键组成部分:

1. **JSON 请求体**:
   *   你的请求必须包含一个 `input` 键。例如:

       ```json
       {
         "input": {
           "prompt": "The lazy brown fox jumps over the"
         }
       }
       ```
2. **可选输入**:
   * 可以包含其他键,如 `webhook`、`execution policies` 和 `S3-compatible storage`。
3. **Webhook**:
   *   若要在 job 完成时收到通知,请在请求体中包含一个 `webhook` URL:

       ```json
       {
         "input": {},
         "webhook": "https://URL.TO.YOUR.WEBHOOK"
       }
       ```
   * 请确保你的 webhook endpoint 返回 `200` 状态码。
4. **执行策略(Execution Policies)**:
   * 通过以下参数控制 job 的执行:
     * `executionTimeout`:job 完成的最长时限。
     * `lowPriority`:为该 job 分配较低的优先级。
     * `ttl`:job 在被终止前可在 queue 中停留的时间。
5. **S3 兼容存储(S3-Compatible Storage)**:
   *   要配置 S3 兼容存储,请在请求中包含凭证:

       ```json
       {
         "input": {},
         "s3Config": {
           "accessId": "key_id_or_username",
           "accessSecret": "key_secret_or_password",
           "bucketName": "storage_location_name",
           "endpointUrl": "storage_location_address"
         }
       }
       ```
   * 请注意,此配置会传递给 worker,但不会包含在 job 输出中。

这种结构可确保 job 高效处理,并有助于对 job 生命周期进行有效的跟踪与管理。
