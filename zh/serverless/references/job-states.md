# Job 状态

在 SubModel 中使用 Handler 时,理解 job 从发起到完成所经历的各种状态至关重要。每个状态都提供了有关 job 当前状况的宝贵信息,便于有效管理 job 流程。

## Job 状态

以下是 job 可能处于的状态:

* **IN\_QUEUE**:\
  job 当前位于 endpoint 的 queue 中,等待可用的 worker 接手处理。
* **IN\_PROGRESS**:\
  一旦 worker 接手该 job,其状态变为 `IN_PROGRESS`。这表示 job 正在被积极处理,已不再位于 queue 中。
* **COMPLETED**:\
  当 job 成功完成处理并返回结果时,它会转为 `COMPLETED` 状态。这表示 job 执行成功。
* **FAILED**:\
  如果 job 在执行过程中遇到错误并返回错误,它会被标记为 `FAILED`。该状态表示 job 因遇到问题而未能成功完成。
* **CANCELLED**:\
  job 可以通过 `/cancel/job_id` endpoint 手动取消。如果一个 job 在完成或失败之前被取消,它将处于 `CANCELLED` 状态。
* **TIMED\_OUT**:\
  此状态出现在两种情形下:
  1. 当 job 在被 worker 接手之前过期。
  2. 当 worker 在 job 达到超时阈值之前未能报告结果。

理解这些状态有助于在 SubModel 框架内更好地跟踪、排障和管理 job。
