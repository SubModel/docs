# 管理 Serverless Endpoint

了解如何高效地管理 Serverless Endpoint。

## 创建 Endpoint

你可以通过 Web 界面创建 Endpoint。

1. 前往 [Serverless Endpoints](https://submodel.ai/#/serverless/list)。
2. 点击 **+ New Endpoint** 并提供以下详细信息:
   1. **Endpoint 名称**。
   2. **选择你的 GPU**。
   3. **配置你的 Worker**。
   4. **添加容器镜像**。
   5. 点击 **Deploy**。

## 删除 Endpoint

你可以通过 Web 界面删除 Endpoint。
在删除 Endpoint 前,请确保所有 Worker 都已移除。

1. 访问 [Serverless Endpoints](https://submodel.ai/#/serverless/list)。
2. 选择你要删除的 Endpoint。
3. 点击 **Edit Endpoint**,将 **Max Workers** 设为 `0`。
4. 点击 **Update**,然后点击 **Delete Endpoint**。

## 编辑 Endpoint

部署完成后,你可以通过 Web 界面修改正在运行的 Endpoint。

1. 进入 [Serverless Endpoints](https://submodel.ai/#/serverless/list)。
2. 选择你要编辑的 Endpoint。
3. 点击 **Edit Endpoint**,进行必要的更改。
4. 点击 **Update**。

## 为 Endpoint 设置 GPU 优先级

在创建或修改 Worker Endpoint 时,可按优先级顺序指定你的 GPU 偏好。
此配置允许你为 Worker Endpoint 选择首选的 GPU 型号。

SubModel 会在可用时尝试分配你的首选项。
如果首选 GPU 不可用,系统将自动切换到你列表中下一个可用的 GPU。

1. 前往 [Serverless Endpoints](https://submodel.ai/#/serverless/list)。
2. 选择你要更新的 Endpoint。
3. 为你偏好的 GPU 设置优先级。
4. 点击 **Update**。



**强制更新配置:**

- 将 **Max Workers** 设为 `0`。
- 点击 **Update**。
- 再将 **Max Workers** 调回所需的数值。
