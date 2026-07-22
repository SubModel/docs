# 管理 Pod

了解如何使用 SubModel 启动、停止和管理 Pod。本指南涵盖 Pod 的创建与终止。

如果你不确定哪种 Pod 最适合自己的需求,请参阅[选择 Pod](https://github.com/SubModel/docs/tree/main/pods/choose-a-pod.md)。

## 创建 Pod

1. 进入 [Pods](https://submodel.ai/#/inst/list),点击 **+ Deploy**。
2. 在 **GPU** 和 **CPU** 之间做选择。
3. 通过配置以下项来定制你的实例:
   1. (可选)指定 network volume。
   2. 选择实例类型,例如 **A40**。
   3. (可选)提供一个 Template,例如 **SubModel Pytorch**。
   4. (仅 GPU)指定计算卡数量。
4. 检查你的配置,然后点击 **Deploy On-Demand**。

## 停止 Pod

1. 进入 **Pods** 页面。
2. 点击你要操作的 Pod 右侧的 **View** 按钮。
3. 点击 **Stop Pod** 按钮确认。

### 在指定时间后停止 Pod

你也可以在经过指定时间后停止 Pod。例如,以下命令会在 2 小时后停止 Pod。

#### SSH

使用以下命令在 2 小时后停止 Pod:

```bash
sleep 2h; submodelctl stop pod $SUBMODEL_POD_ID &
```

该命令使用 `sleep` 等待 2 小时,然后执行 `submodelctl stop pod` 命令来停止 Pod。末尾的 `&` 让命令在后台运行,使你能够继续使用 SSH 会话。

#### Web 终端

要通过 Web 终端在 2 小时后停止 Pod,请输入:

```bash
nohup bash -c "sleep 2h; submodelctl stop pod $SUBMODEL_POD_ID" &
```

`nohup` 命令确保即使你关闭 Web 终端窗口,进程也会继续运行。

> **警告**
>
> 存储闲置的 Pod 也会产生费用。如果你不需要保留 Pod,请务必将其终止。

## 启动 Pod

你可以恢复一个已停止的 Pod。

1. 进入 **Pods** 页面。
2. 点击你要操作的 Pod 右侧的 **View** 按钮。
3. 点击 **Run**。

你的 Pod 将恢复运行。

## 终止 Pod

> **危险**
>
> 终止 Pod 会永久删除所有数据。请确保你已保存所有以后还想访问的数据。

1. 进入 **Pods** 页面。
2. 点击你要操作的 Pod 右侧的 **View** 按钮。
3. 选择 **More→Terminate**。
4. 点击 **Yes** 按钮确认。
