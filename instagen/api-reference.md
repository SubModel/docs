# API Reference

InstaGen provides OpenAI-compatible API endpoints for seamless integration.

## Base URL

```
https://llm.submodel.ai/v1
```

## Authentication

Include your access key in the Authorization header:

```bash
Authorization: Bearer YOUR_ACCESS_KEY
```

## Supported Endpoints

### List Models
```http
GET /v1/models
```

Returns available models and their capabilities.

### Chat Completions
```http
POST /v1/chat/completions
```

Standard chat completion endpoint for conversational AI.

### Text Completions
```http
POST /v1/completions
```

Text completion endpoint for traditional prompt-based generation.

## Infrastructure

- **Load Balancing**: Automatic load balancing across multiple instances
- **High Availability**: Redundant infrastructure with automatic failover
- **Global CDN**: Optimized routing for worldwide access

## Rate Limits

- **Concurrent Requests**: 1000 per IP
- **No Daily Limits**: Unlimited requests and tokens
- **Higher Limits**: Contact us for enterprise needs

## Quick Start

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

- [Create Access Keys](access-keys/create-key.md)
- [View Available Models](models/available-models.md)
- [Learn About Limits](limits-and-policies.md)
