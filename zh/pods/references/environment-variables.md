# Pod 环境变量

你可以在 Pod 中存储以下环境变量。

| 变量                      | 说明                                          |
| ----------------------- | ------------------------------------------- |
| `SUBMODEL_POD_ID`       | 你 Pod 的唯一标识符。                               |
| `SUBMODEL_API_KEY`      | 用于对特定 Pod 发起 SubModel API 调用。其作用范围仅限于该 Pod。 |
| `SUBMODEL_POD_HOSTNAME` | Pod 所运行的宿主服务器名称。                            |
| `SUBMODEL_GPU_COUNT`    | 该 Pod 可用的 GPU 数量。                           |
| `SUBMODEL_CPU_COUNT`    | 该 Pod 可用的 CPU 数量。                           |
| `SUBMODEL_PUBLIC_IP`    | 如果可用,该 Pod 的公网可访问 IP。                       |
| `SUBMODEL_TCP_PORT_22`  | SSH port 22 对应的公网 port。                     |
| `SUBMODEL_DC_ID`        | Pod 所在的 Datacenter。                         |
| `SUBMODEL_VOLUME_ID`    | 连接到该 Pod 的 volume 的 ID。                     |
| `CUDA_VERSION`          | 已安装的 CUDA 版本。                               |
| `PWD`                   | 当前工作目录。                                     |
| `PYTORCH_VERSION`       | 已安装的 PyTorch 版本。                            |
| `PUBLIC_KEY`            | 用于通过 SSH 访问 Pod 的 SSH 公钥。                   |
