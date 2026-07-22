# Endpoint 配置

以下是 Endpoint 内可配置的设置项。

## Endpoint 名称

为 Endpoint 配置创建一个你想使用的名称。 创建后的 Endpoint 会被分配一个随机 ID,用于发起调用。

该名称仅你自己可见。

## GPU 选择

选择一个或多个你希望 Endpoint 运行所在的 GPU。SubModel 会按你选择的顺序为你匹配 GPU,所以你选择的第一个 GPU 类型优先级最高,然后是第二个,以此类推。选择多种 GPU 类型有助于你更快获得 worker,尤其当你的首选是热门 GPU 时。

## 活跃(最小)Worker 数

将活跃 worker 数设为 1 或以上可确保你拥有「常驻」的 worker,随时响应 job 请求,而无需承担 cold start 延迟。

默认值:0

> **note**
>
> 活跃 worker 在你启用它们(设为 >0)时即开始计费,但它们享有最高 30% 的常规价格折扣。

## 最大 Worker 数

最大 worker 数为你的 endpoint 可同时运行的 worker 数量设定上限。如果最大 worker 数设得过低,你可能会遇到 worker 被限流(throttled)的情况。为避免这种情况,如果你经常看到限流现象,可考虑将最大 worker 数提升到 5 或更高。

默认值:3

<details>

<summary>如何配置最大 Worker 数</summary>

你还可以配置最大 worker 数量。这是 SubModel 为你自动扩缩容时尝试达到的上限。用它来限制你的并发请求数,同时也限制你的成本上限。

> **note**
>
> 我们目前根据这个数字来设定你的缓存系数,因此最大 worker 数越高的 endpoint 在缓存 worker 时也会获得越高的优先级。
>
> 这也部分解释了为什么我们在账户层面将新账户限制在相对较低的最大并发数。 如果你想提高这个数字,通常需要有更高的历史消费记录,或承诺相对较高的月度消费。
>
> 通常你应将最大 worker 数设为比你预期的最大并发数高出 20%。

</details>

## GPU / Worker

你希望分配给每个 worker 的 GPU 数量。

> **note**
>
> 目前仅对 48 GB GPU 可用。

## 空闲超时(Idle Timeout)

worker 在完成当前请求后继续运行的时长。在此期间,worker 保持活跃,持续检查 queue 中是否有新 job,并继续计费。如果在此时间内没有新请求到达,worker 将进入休眠。

默认值:5 秒

## 高级设置

用于帮助你控制 endpoint 部署位置以及如何响应传入请求的额外控制项。

### 数据中心(Data Centers)

控制哪些 Datacenter 可以部署和缓存你的 worker。允许多个 Datacenter 有助于你更快获得 worker。

默认值:所有 Datacenter

### 扩缩容类型(Scale Type)

* **Queue Delay(队列延迟)** 扩缩容策略根据请求等待时间调整 worker 数量。初始为零 worker 时,第一个请求会增加一个 worker。后续请求只有在 queue 中等待达到设定的延迟秒数后才会增加 worker。
* **Request Count(请求数)** 扩缩容策略根据 queue 中和进行中的请求总数来调整 worker 数量。它会随着请求数量增加而自动增加 worker,确保任务被高效处理。

```
_Total Workers Formula: Math.ceil((requestsInQueue + requestsInProgress) / <set request count)_
```

### GPU 类型

在所选的 GPU 尺寸类别内,你可以进一步选择希望 endpoint worker 运行所在的 GPU 型号。 默认值:`4090` | `A4000` | `A4500`

<details>

<summary>不同 GPU 型号之间有什么区别。</summary>

A100 比 A5000 快大约 2-3 倍,并且允许两倍的 VRAM 以及全程非常高的带宽吞吐。3090 和 A5000 比 A4000 快 1.5-2 倍。有时候,即使你不需要,使用 24 GB 相比 16 GB 也可能更合理,因为响应速度更快。根据任务的性质,执行速度也可能存在瓶颈,单纯使用更高端的显卡未必能带来显著提升。请自行计算和实验,以确定对你的工作负载和任务类型而言最具性价比的方案。

想要不同的规格?[告诉我们](https://github.com/SubModel/docs/tree/main/references/contact-us.md),我们可以考虑扩展我们的产品选择!

</details>

## CUDA 版本选择

你可以选择工作负载所允许的 CUDA 版本。 CUDA 版本选择决定了用于执行你的 Serverless 任务的兼容 GPU 类型。

具体来说,CUDA 版本选择的工作方式如下:

* 你可以选择一个或多个你的工作负载兼容或所需的 CUDA 版本。
* SubModel 随后会将你的工作负载匹配到安装了所选 CUDA 版本的可用 GPU 实例。
* 这确保你的 Serverless 任务在满足 CUDA 版本要求的 GPU 硬件上运行。

例如,如果你选择 CUDA 11.6,你的 Serverless 任务将被调度到安装了 CUDA 11.6 或兼容版本的 GPU 实例上运行。这让你可以根据工作负载的依赖或性能要求来针对特定的 CUDA 版本。
