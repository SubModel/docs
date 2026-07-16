# Get Started

Get up and running with InstaGen in 3 steps.

## Prerequisites

- SubModel.ai account
- Sufficient balance

## Step 1: Create Access Key

1. Go to **Account Settings** → **InstaGen Access Key**
2. Click **"Add New Access Key"**
3. Enter name and description
4. Copy your key

## Step 2: Choose Model

Browse available models:
- **Qwen**: Coding, reasoning, instruction-following
- **DeepSeek**: Advanced reasoning and analysis
- **ZAI**: Performance-optimized models
- **OpenAI**: General-purpose AI

## Step 3: Make API Call

```bash
curl -X POST https://llm.submodel.ai/v1/chat/completions \
  -H "Authorization: Bearer YOUR_ACCESS_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen/Qwen3-235B-A22B-Instruct-2507",
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

## Next Steps

- [API Reference](api-reference.md)
- [Available Models](models/available-models.md)
- [Usage Analytics](usage/overview.md)
