# 快速上手 Endpoint

## 概述

本指南将帮助你了解如何构建 Docker 镜像、部署 Serverless Endpoint 并发送请求。你还将初步了解如何针对自身的具体用例定制 Handler。

## 前置条件

本节假设你已熟悉终端的使用,并能从中执行命令。

### SubModel

要完成本快速上手指南,你需要从 SubModel 获取以下内容:

* 一个 SubModel 账号
* 一个 SubModel API Key

### Docker

要构建 Docker 镜像,请确保你具备以下条件:

* 已安装 Docker
* 一个 Docker 账号

### GitHub

要克隆 `worker-template` 仓库,你需要:

* 已安装 Git
* 拥有克隆 GitHub 仓库的权限

## 在 SubModel 上构建 Serverless 应用

按照以下步骤创建 Handler 文件、在本地测试并构建可部署的 Docker 镜像:

1. **创建 Handler 文件(`rp_handler.py`):**

```python
import submodel
import time

def handler(event):
    input = event['input']
    instruction = input.get('instruction')
    seconds = input.get('seconds', 0)

    # Placeholder for a task; replace with image or text generation logic as needed
    time.sleep(seconds)
    result = instruction.replace(instruction.split()[0], 'created', 1)

    return result

if __name__ == '__main__':
    submodel.serverless.start({'handler': handler})
```

2. **在同一文件夹中创建 `test_input.json` 文件:**

```json
{
    "input": {
        "instruction": "create a image",
        "seconds": 15
    }
}
```

3. **在本地测试 Handler 代码:**

```bash
python3 rp_handler.py

# You should see an output like this:
--- Starting Serverless Worker |  Version 1.7.0 ---
INFO   | Using test_input.json as job input.
DEBUG  | Retrieved local job: {'input': {'instruction': 'create a image', 'seconds': 15}, 'id': 'local_test'}
INFO   | local_test | Started.
DEBUG  | local_test | Handler output: created a image
DEBUG  | local_test | run_job return: {'output': 'created a image'}
INFO   | Job local_test completed successfully.
INFO   | Job result: {'output': 'created a image'}
INFO   | Local testing complete, exiting.
```

4. **创建 Dockerfile:**

```dockerfile
FROM python:3.10-slim

WORKDIR /
RUN pip install --no-cache-dir submodel
COPY rp_handler.py /

# Start the container
CMD ["python3", "-u", "rp_handler.py"]
```

5. **构建并推送你的 Docker 镜像:**

```bash
docker build --platform linux/amd64 --tag <username>/<repo>:<tag> .
```

6. **推送到你的容器镜像仓库:**

```bash
docker push <username>/<repo>:<tag>
```

> **注意**
>
> 在构建 Docker 镜像时,你可能需要指定构建目标平台。如果你的构建机器架构与部署目标架构不同,这一点尤为重要。
>
> 为 SubModel 服务商构建时,请使用 `--platform=linux/amd64`。

或者,你也可以克隆我们的 [worker-template](https://github.com/submodel-workers/worker-template) 仓库,以便快速构建 Docker 镜像并推送到你的容器镜像仓库,从而更快上手。

现在你已经推送了容器镜像仓库,可以将 Serverless Endpoint 部署到 SubModel 了。

## 部署 Serverless Endpoint

本步骤将引导你把 Serverless Endpoint 部署到 SubModel。你可以参照此流程部署自己的自定义 Docker 镜像。

1. 登录 [SubModel Serverless 控制台](https://submodel.ai/#/serverless/list)。
2. 选择 **+ New Endpoint**。
3. 提供以下信息:
   1. Endpoint 名称。
   2. 选择你的 GPU 配置。
   3. 配置 Worker 数量。
   4. 选择一个 Template。
   5. 输入你的 Docker 镜像名称。
      * 例如 `<username>/<repo>:<tag>`。
   6. 为你的 Docker 镜像指定足够的内存。
4. 选择 **Deploy**。

现在,让我们向你的 [Endpoint](https://github.com/SubModel/docs/tree/main/serverless/endpoints/get-started.md) 发送一个请求。
