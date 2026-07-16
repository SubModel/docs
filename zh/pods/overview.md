# 概述

Pod 是正在运行的容器实例,可以从容器镜像仓库拉取,例如 Docker Hub、GitHub Container Registry、Amazon Elastic Container Registry 或其他任何兼容的仓库。

> **注意:** 在 Mac(Apple Silicon)上为 submodel.ai 构建镜像时,请使用 `--platform linux/amd64` 标志以确保兼容性。这个标志必不可少,因为 submodel.ai 目前仅支持 `linux/amd64` 架构。

## 理解 Pod 的组成与配置

Pod 是为访问硬件而创建的服务器容器,会被分配一个动态生成的标识符。例如,`2s56cp0pof1rmt` 就代表这样一个实例。

一个 Pod 由以下几个关键组件构成:

- **Container Volume(容器卷):** 存储操作系统和临时数据。
  - 这类存储是易失性的,一旦 Pod 停止或重启就会丢失。
- **Disk Volume(磁盘卷):** 提供持久化存储,类似硬盘,在 Pod 租期内一直保留。
  - 即使 Pod 停止或重启,这类存储也会保持完好。
<!--- **Network Storage:** Functions like a volume but can be moved between machines.
  - When using network storage, the Pod can only be deleted.
-->
- **Ubuntu Linux 容器:** 可运行几乎任何与 Ubuntu 兼容的软件。
- **已分配的 vCPU 与系统 RAM:** 供容器及其进程使用的专用资源。
- **可选的 GPU:** 适用于 CUDA 或 AI/ML 等特定工作负载。
- **预配置的 Template:** 在 Pod 创建时自动完成软件安装与设置,让你快速用上各种软件包。
- **用于 Web 访问的代理连接:** 便于连接到容器上任意开放的 port。
  - 示例:`https://[pod-id]-[port number].tun.submodel.ai`,例如 `https://2s56cp0pof1rmt-7860.tun.submodel.ai/`。

开始之前,先了解如何[选择 Pod](/pods/choose-a-pod.md),然后参照[管理 Pod](/pods/manage-pods.md) 指南操作。

## 了解更多

你可以使用 Template 快速启动一个正在运行的 Pod。若需进一步定制,可配置以下设置:

- [GPU 类型](/references/gpu-types.md)与数量
- 系统磁盘大小
- 启动命令
- [环境变量](/pods/references/environment-variables.md)
- [暴露 HTTP/TCP port](/pods/configuration/expose-ports.md)
- [持久化存储选项](/pods/storage/types.md)

开始时,请参阅如何[选择 Pod](/pods/choose-a-pod.md),并参考[管理 Pod](/pods/manage-pods.md) 指南。
