# Endpoint 操作

SubModel 的 Endpoint 支持无缝的 job 提交与输出获取。通过以下地址访问这些 endpoint:`https://api.submodel.ai/api/v1/sl/{inst_id}/{operation}`

## run

* **Method**: `POST`
* **描述**:用于提交 job 的异步 endpoint
* **返回**:唯一的 Job ID
* **载荷上限**:10 MB
* **速率限制**:每 10 秒 1000 次请求,200 并发
* **Job 可用时长**:完成后 30 分钟

## runsync

* **Method**: `POST`
* **描述**:用于较短运行 job 的同步 endpoint
* **返回**:即时结果
* **载荷上限**:20 MB
* **速率限制**:每 10 秒 2000 次请求,400 并发
* **Job 可用时长**:完成后 60 秒

## Queue 限制

* 在以下情况下,请求将收到 `429 (Too Many Requests)` 状态:
  * queue 大小超过 50 个 job,并且
  * queue 大小超过 endpoint.WorkersMax \* 500

## Status 和 Stream 操作

所有 status 和 stream 操作共享每 10 秒 2000 次请求、400 并发的速率限制:

* `/status/{job_id}` - 检查 job 状态并获取输出
  * Methods: `GET` | `POST`
* `/status-sync/{job_id}` - 同步状态检查
  * Methods: `GET` | `POST`
* `/stream/{job_id}` - 从生成器类型的 Handler 流式获取结果
  * Methods: `GET` | `POST`

## 其他操作

* `/cancel/{job_id}`
  * **Method**: `POST`
  * **速率限制**:每 10 秒 100 次请求,20 并发
* `/purge-queue`
  * **Method**: `POST`
  * **描述**:清除所有排队中的 job(不影响运行中的 job)
  * **速率限制**:每 10 秒 2 次请求
* `/health`
  * **Method**: `GET`
  * **描述**:提供 worker 统计信息和 endpoint 健康状况
* `/requests`
  * **Method**: `GET`
  * **速率限制**:每 10 秒 10 次请求,2 并发

有关如何执行这些 Endpoint 操作的详细说明,请参阅[调用 Job](https://github.com/SubModel/docs/tree/main/serverless/endpoints/job-operations.md)。
