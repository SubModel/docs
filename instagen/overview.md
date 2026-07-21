# InstaGen 概述

通过兼容 OpenAI 的 API 即刻访问高性能 AI model。

## 什么是 InstaGen?

免部署的 AI model 访问方式,内置自动负载均衡与高可用能力。

## 核心特性

- **即刻访问(Instant Access)**:无需部署
- **兼容 OpenAI**:可直接替换现有集成
- **高可用**:自动负载均衡与故障转移
- **按用量计费(Pay-per-Use)**:定价透明
- **全球 CDN**:面向全球的访问优化

## API Endpoint

```
https://llm.submodel.ai/v1
```

## 支持的 Endpoint

- `GET /v1/models` - 列出可用 model
- `POST /v1/chat/completions` - 聊天补全
- `POST /v1/completions` - 文本补全

## 快速开始

1. [创建 Access Key](access-keys/create-key.md)
2. [查看 model](models/available-models.md)
3. [开始使用 API](api-reference.md)

## 后续步骤

- [API 参考](api-reference.md)
- [可用 model](models/available-models.md)
- [创建 Access Key](access-keys/create-key.md)
