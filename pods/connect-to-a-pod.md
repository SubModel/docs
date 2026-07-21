# 连接到 Pod

你可以根据自己的具体需求、偏好以及所用的 Template,使用不同的方法访问 Pod。

## SSH 终端

使用 SSH 终端连接 Pod 是一种安全可靠的方式,非常适合长时间运行的任务和关键操作。

每个 Pod 都已配置为允许 SSH 访问。

开始之前,请确保本地机器上已安装 SSH 客户端。

1. 打开本地设备上的终端。
2. 从以下命令中选择一条,输入到终端中:

    ```bash
    # No support for SCP & SFTP
    ssh <username>@<pod-ssh-hostname> -i <path-to-ssh-key>

    # Supports SCP & SFTP
    ssh <username>@<pod-ip-address> -p <ssh-port> -i <path-to-ssh-key>
    ```

用以下信息填写占位符:

- `<username>`:分配给你的 Pod 用户名
- `<pod-ssh-hostname>`:为你的 Pod 提供的 SSH 主机名
- `<pod-ip-address>`:你的 Pod 的 IP 地址
- `<ssh-port>`:为你的 Pod 指定的 SSH port
- `<path-to-ssh-key>`:你的 SSH 私钥文件所在位置

完成后,你就建立起了到 Pod 的安全 SSH 连接。

## Web 终端

> 能否连接 Web 终端取决于你的 Pod 所用的 Template。

Web 终端提供了一种快速便捷的方式来连接 Pod 并执行命令。不过,它不适合训练 LLM 或其他高优先级作业等长时间运行的任务。它是为快速登录和简单任务设计的。

1. 在你的 Pod 页面上,点击 **Connect**。
2. 选择 **Start Web Terminal**,然后在新窗口中选择 **Connect to Web Terminal**。
3. 输入 **Username** 和 **Password**。

> 点击 **Start Web Terminal** 后,你就能找到 Web 终端的 **Username** 和 **Password**。
