# 术语表

## Serverless
Serverless 是一种按秒计费的计算解决方案,专为生产环境中的动态自动扩缩容而设计。它会根据你的请求流量自动调整计算资源,确保经济高效地使用。

我们提供 GPU 和 CPU 两种 Serverless 选项:
- **GPU Serverless**:每个 worker 都配备一块专用 GPU,非常适合 AI/ML 工作负载。
- **CPU Serverless**:worker 配备高主频 CPU 核心,适合通用型工作负载。

## Worker
处理请求的单个计算资源。每个 endpoint 可以拥有多个 worker,从而实现多个请求的并行处理。

## Total Workers(worker 总数)
指你的账户可用的最大 worker 数量。你所有 endpoint 分配的 max workers 之和不能超过此上限。如果你的 total workers 用尽,请通过 [创建支持工单]() 与我们联系。

## Max Workers(最大 worker 数)
设置你的 endpoint 可同时运行的 worker 数量上限。

**默认值**:3

## Active (Min) Workers(活跃 / 最小 worker 数)
"常开"的 worker。将 active workers 设为 1 或更多,可确保始终有一个 worker 准备好响应任务请求,而不会有 cold start(冷启动)延迟。

**默认值**:0  
**注意**:active workers 一旦启用(设为 >0)即开始计费,但它们享有最高 30% 的常规价格折扣。

## Flex Workers(弹性 worker)
"按需开启"的 worker,用于在流量激增时帮助扩展你的 endpoint。它们通常被称为 idle worker(空闲 worker),因为大部分时间都处于空闲状态。一旦某个 flex worker 完成任务,它就会转入空闲或休眠模式以节省成本。

**默认值**:Max Workers(3) - Active Workers(0) = 3

## Extra Workers(额外 worker)
SubModel 会在我们的宿主服务器上缓存你的 worker Docker 镜像,以确保更快的扩展能力。如果你遇到流量高峰,可以增加 max workers 数量,extra workers 将立即作为 flex workers 的一部分被加入,以应对增长的需求。

**默认值**:2

## Worker States(worker 状态)
- **Initializing(初始化中)**:当你创建新的 endpoint 或发布更新时,SubModel 需要为你的 worker 下载并准备 Docker 镜像。在此过程中,worker 会保持初始化状态,直到完全就绪、可处理请求。
- **Idle(空闲)**:worker 已准备好处理新请求,但当前并未主动处理任何请求。worker 处于空闲状态时不计费。
- **Running(运行中)**:运行中的 worker 正在主动处理请求,其运行的每一秒都会计费。如果 worker 运行时间不足一整秒,将向上取整到下一整秒。例如,如果 worker 运行了 2.5 秒,将按 3 秒计费。
- **Throttled(受限)**:有时,缓存该 worker 的机器可能被其他工作负载完全占用。此时,worker 将显示为受限状态,直到资源可用为止。
- **Outdated(已过时)**:当你更新 endpoint 配置或部署新的 Docker 镜像时,现有 worker 会被标记为已过时。这些 worker 会继续处理其当前任务,但会通过滚动更新逐步被替换。
- **Unhealthy(不健康)**:当你的容器崩溃时,通常是由于 Docker 镜像有问题、启动命令不正确,偶尔也可能是机器问题。系统会自动重试不健康的 worker。

## Endpoint
指 SubModel 提供的特定 REST API(URL),你的应用程序或服务可以与之交互。

## Handler
你创建的一个函数,它接收提交的输入,对其进行处理(例如生成图像、文本或音频),并返回最终输出。

## Serverless SDK
创建 handler 函数时使用的一个 Python 包。该包帮助你的代码接收来自我们 serverless 系统的请求,触发你的 handler 函数执行,并将函数结果返回给 serverless 系统。

## Endpoint Settings(endpoint 设置)
- **Idle Timeout(空闲超时)**:worker 在完成当前请求后继续保持运行的时长。
  **默认值**:5 秒
- **Execution Timeout(执行超时)**:系统终止 worker 之前,一个任务可运行的最长时间。
  **默认值**:600 秒(10 分钟)
- **Job TTL(任务存活时间)**:定义一个任务在被自动终止之前可以在队列中停留的最长时间。
  **默认值**:86,400,000 毫秒(24 小时)

## Flashboot
SubModel 用于降低 endpoint 平均 cold start(冷启动)耗时的解决方案。

## Scale Type(扩缩容类型)
- **Queue Delay(队列延迟)**:根据请求的等待时间调整 worker 数量。
- **Request Count(请求数量)**:根据队列中和处理中的请求总数调整 worker 数量。

## Expose HTTP/TCP Ports(暴露 HTTP/TCP 端口)
允许通过 worker 的公共 IP 和端口直接与其通信,适用于对延迟要求极低的实时应用。

## Endpoint Metrics(endpoint 指标)
- **Requests(请求)**:显示你的 endpoint 接收到的请求总数及其状态。
- **Execution Time(执行时间)**:显示你的 endpoint 上请求的 P70、P90 和 P98 执行时间。
- **Delay Time(延迟时间)**:显示请求在队列中等待所花费的时间。
- **Cold Start Time(冷启动时间)**:衡量唤醒一个 worker 所需的时间。
- **Cold Start Count(冷启动次数)**:显示给定时间段内 cold start 的次数。
- **WebhookRequest Responses(Webhook 请求响应)**:显示已发送的 webhook 请求数量及其响应。

## Pod
- **Secure Cloud**:运行在 T3/T4 Datacenter 中的 GPU 实例,提供高可靠性和安全性。
- **Community Cloud**:通过安全的点对点系统将各个独立算力提供方与消费者连接起来的 GPU 实例。

## Datacenter
一个安全的场所,用于托管 SubModel 的云计算服务(例如 GPU 实例)。

## GPU Instance(GPU 实例)
一种基于容器的 GPU 实例,你可以在数秒内完成部署。

## Template
SubModel 的 Template 是一个 Docker 容器镜像与一份配置的组合。
