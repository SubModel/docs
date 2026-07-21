# SSH 与 API Key 管理文档

## 1. SSH 公钥配置
### 概述
使用 SSH 密钥对认证来安全访问实例。

### 操作步骤
1. 生成密钥对(本地终端):
   ```bash
   ssh-keygen -t ecdsa -b 521 -f ~/.ssh/instance_key
   ```
2. 通过管理控制台上传公钥(`instance_key.pub`)
3. 连接到实例:
   ```bash
   ssh -i ~/.ssh/instance_key user@instance-ip
   ```

### 安全要求
- 将私钥权限设置为 600
- 必须使用 ECDSA 或 EdDSA 算法
- 每季度轮换密钥

## 2. API Key 管理
### 概述
用于通过程序访问 API 的凭据。

### 密钥处理
1. 通过控制台界面生成密钥
2. 在请求中带上:
   ```http
   x-apikey: YOUR_API_KEY
   ```

### 安全策略
- 视为敏感凭据对待
- 一旦泄露立即吊销
- 强制每 90 天轮换一次

## 3. 镜像仓库认证
### 支持的仓库
- Docker Hub
- AWS ECR
- GCR

### 配置
1. 按以下格式提供凭据:
   ```
   Username: [registry_user]
   Password: [registry_token]
   Endpoint: [registry_url]
   ```
2. 创建 Template 时选择相应凭据

### 故障排查
- 出现 403 错误时,检查凭据权限
- 服务器地址中要包含协议(https://)

## 最佳实践
- 每个环境使用独立的 SSH 密钥
- 按服务层级隔离 API Key
- 优先使用临时的仓库 token
