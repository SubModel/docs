# API keys

API key 用于对发往 Submodel.ai 的请求进行身份验证。你可以生成具有读/写(Read/Write)权限、受限(Restricted)权限或只读(Read Only)权限的 API key。

## 生成

创建 API key:

1. 在控制台中,选择 **Account→Settings**。
2. 在 **API Keys** 下,选择 **+ Create API Key**。
3. 选择权限。如果你选择 Restricted 权限,可以为每个 API 自定义访问权限:
   - **None**:无访问权限
   - **(仅 AI API) Restricted**:对特定 endpoint 的自定义访问权限。默认无访问权限。
   - **Read/Write**:完全访问权限
   - **Read Only**:只读访问权限,无写入权限

   > **警告**
   > 请为你的使用场景选择所需的最小权限。仅在为 Serverless endpoint 之外的自动化操作(例如创建或管理 Submodel.ai 资源)确有必要时,才授予 GraphQL 的完全访问权限。

4. 选择 **Create**。

> **注意**
> 生成 API key 后,请妥善保管。像对待密码一样对待它,避免在不安全的环境中共享。

## 编辑权限

编辑 API key:

1. 在控制台中,选择 **Account→Settings**。
2. 在 **API Keys** 下,选择铅笔图标并选择权限。
3. 选择 **Update**。

## 禁用

禁用 API key:

1. 在控制台中,选择 **Account→Settings**。
2. 在 **API Keys** 下,选择开关并选择 **Yes**。

## 启用

启用 API key:

1. 在控制台中,选择 **Account→Settings**。
2. 在 **API Keys** 下,选择开关并选择 **Yes**。

## 吊销

删除 API key:

1. 在控制台中,选择 **Account→Settings**。
2. 在 **API Keys** 下,选择垃圾桶图标并选择 **Revoke Key**。
