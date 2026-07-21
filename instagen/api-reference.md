# API 参考

InstaGen 提供兼容 OpenAI 的 API Endpoint,便于无缝集成。

## Base URL

```
https://llm.submodel.ai/v1
```

## 身份认证

在 Authorization 请求头中携带你的 access key:

```bash
Authorization: Bearer YOUR_ACCESS_KEY
```

## 支持的 Endpoint

### 列出 model
```http
GET /v1/models
```

返回可用的 model 及其能力。

### 聊天补全(Chat Completions)
```http
POST /v1/chat/completions
```

用于对话式 AI 的标准聊天补全 Endpoint。

### 文本补全(Text Completions)
```http
POST /v1/completions
```

用于传统基于 prompt 生成的文本补全 Endpoint。

## 基础设施

- **负载均衡(Load Balancing)**:在多个实例间自动负载均衡
- **高可用**:冗余基础设施,支持自动故障转移
- **全球 CDN**:面向全球访问的优化路由

## 速率限制

- **并发请求**:每个 IP 1000
- **无每日上限**:请求和 token 数量不受限
- **更高限额**:企业需求请联系我们

## 快速开始

```bash
curl -X POST https://llm.submodel.ai/v1/chat/completions \
  -H "Authorization: Bearer YOUR_ACCESS_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen/Qwen3-235B-A22B-Instruct-2507",
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

## 后续步骤

- [创建 Access Key](access-keys/create-key.md)
- [查看可用 model](models/available-models.md)
- [了解限额](limits-and-policies.md)
