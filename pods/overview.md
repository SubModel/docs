# Overview

Pods are running container instances that can be pulled from a container registry such as Docker Hub, GitHub Container Registry, Amazon Elastic Container Registry, or any other compatible registry.

> **Note:** When building an image for submodel.ai on a Mac (Apple Silicon), use the flag `--platform linux/amd64` to ensure compatibility. This flag is essential because submodel.ai currently supports only the `linux/amd64` architecture.

## Understanding Pod Components and Configuration

A Pod is a server container created to access hardware, assigned a dynamically generated identifier. For example, `2s56cp0pof1rmt` represents one such instance.

A Pod consists of several key components:

- **Container Volume:** Stores the operating system and temporary data.
  - This storage is volatile and will be lost if the Pod is halted or rebooted.
- **Disk Volume:** Provides persistent storage, similar to a hard disk, retained for the duration of the Pod's lease.
  - This storage remains intact even if the Pod is halted or rebooted.
- **Network Storage:** Functions like a volume but can be moved between machines.
  - When using network storage, the Pod can only be deleted.
- **Ubuntu Linux Container:** Runs almost any software compatible with Ubuntu.
- **Assigned vCPU and System RAM:** Dedicated resources for the container and its processes.
- **Optional GPUs or Additional CPUs:** Available for specific workloads such as CUDA or AI/ML tasks, though not mandatory.
- **Pre-configured Template:** Automates software installation and settings upon Pod creation, enabling quick access to various packages.
- **Proxy Connection for Web Access:** Facilitates connectivity to any open port on the container.
  - Example: `https://[pod-id]-[port number].proxy.submodel.ai`, such as `https://2s56cp0pof1rmt-7860.proxy.submodel.ai/`.

To get started, check out how to [Choose a Pod](/pods/choose-a-pod), then follow the guide on [Managing Pods](/pods/manage-pods).

## Learn More

You can quickly launch a running Pod using a template. For further customization, you can configure the following settings:

- [GPU Type](/references/gpu-types.md) and quantity
- System Disk Size
- Start Command
- [Environment Variables](/pods/references/environment-variables)
- [Exposing HTTP/TCP Ports](/pods/configuration/expose-ports)
- [Persistent Storage Options](/pods/storage/types.md)

To begin, see how to [Choose a Pod](/pods/choose-a-pod) and refer to the guide on [Managing Pods](/pods/manage-pods).
