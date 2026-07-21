# 快速上手

通过 3 个步骤即可开始使用 InstaGen。

## 前置条件

- SubModel.ai 账户
- 充足的余额

## 第 1 步:创建 Access Key

1. 进入 **Account Settings** → **InstaGen Access Key**
2. 点击 **"Add New Access Key"**
3. 填写名称和描述
4. 复制你的 key

## 第 2 步:选择 model

浏览可用的 model:
- **Qwen**:编程、推理、指令遵循
- **DeepSeek**:高级推理与分析
- **ZAI**:面向性能优化的 model
- **OpenAI**:通用型 AI

## 第 3 步:发起 API 调用

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

- [API 参考](api-reference.md)
- [可用 model](models/available-models.md)
- [用量分析](usage/overview.md)
