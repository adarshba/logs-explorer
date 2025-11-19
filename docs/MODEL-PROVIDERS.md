# AI Model Providers Configuration Guide

Zeteo supports multiple AI providers for maximum flexibility, cost optimization, and reliability. This guide covers how to configure each supported provider in both the TypeScript API (Vercel deployment) and Rust backend (local/self-hosted).

## 📋 Quick Reference

| Provider | TypeScript API | Rust Backend | Models Available |
|----------|---------------|--------------|------------------|
| OpenAI | ✅ Full Support | ✅ Full Support | GPT-4o, GPT-4o-mini, GPT-4, GPT-3.5 |
| Azure OpenAI | ✅ Full Support | ✅ Full Support | Same as OpenAI |
| Google Gemini | ✅ Via Neurolink | 🔄 Planned | Gemini Pro, Gemini Ultra |
| Google Vertex AI | ✅ Via Neurolink | 🔄 Planned | PaLM 2, Gemini |
| Anthropic Claude | ✅ Via Neurolink | 🔄 Planned | Claude 3 Opus, Sonnet, Haiku |
| Other Providers | ✅ Via Neurolink | ⚠️ Limited | Various |

## 🚀 Quick Start

### Option 1: OpenAI (Simplest)

**TypeScript API (.env):**
```bash
AI_PROVIDER=openai
OPENAI_API_KEY=sk-your-openai-key-here
AI_MODEL=gpt-4o-mini
```

**Rust Backend (.env):**
```bash
OPENAI_API_KEY=sk-your-openai-key-here
OPENAI_MODEL=gpt-4o-mini
AI_PROVIDER=openai
```

### Option 2: Azure OpenAI (Enterprise)

**TypeScript API (.env):**
```bash
AI_PROVIDER=azure-openai
AZURE_OPENAI_API_KEY=your-azure-api-key
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com
AZURE_OPENAI_DEPLOYMENT=gpt-4o-mini
AZURE_OPENAI_API_VERSION=2024-02-15-preview
AI_MODEL=gpt-4o-mini
```

**Rust Backend (.env):**
```bash
AI_PROVIDER=azure-openai
AZURE_OPENAI_API_KEY=your-azure-api-key
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com
AZURE_OPENAI_DEPLOYMENT=gpt-4o-mini
AZURE_OPENAI_API_VERSION=2024-02-15-preview
AI_MODEL=gpt-4o-mini
```

---

## 📖 Detailed Provider Configuration

### 1. OpenAI

**Overview:**
- Direct access to OpenAI's models
- Best for: Quick setup, latest models
- Pricing: Pay-per-token
- Region: Global

**Setup Steps:**

1. **Get API Key:**
   - Visit https://platform.openai.com/api-keys
   - Create new API key
   - Copy the key (starts with `sk-`)

2. **Configure Environment:**

   **TypeScript API:**
   ```bash
   AI_PROVIDER=openai
   OPENAI_API_KEY=sk-proj-xxx...
   AI_MODEL=gpt-4o-mini
   ```

   **Rust Backend:**
   ```bash
   AI_PROVIDER=openai
   OPENAI_API_KEY=sk-proj-xxx...
   OPENAI_MODEL=gpt-4o-mini
   ```

3. **Recommended Models:**
   - `gpt-4o-mini` - Fast, cost-effective (recommended)
   - `gpt-4o` - Most capable, balanced cost
   - `gpt-4-turbo` - Previous generation
   - `gpt-3.5-turbo` - Fastest, cheapest

**Cost Estimates (per 1M tokens):**
- `gpt-4o-mini`: $0.15 input / $0.60 output
- `gpt-4o`: $5.00 input / $15.00 output

---

### 2. Azure OpenAI

**Overview:**
- Enterprise-grade OpenAI access via Microsoft Azure
- Best for: Enterprise customers, compliance requirements, data residency
- Pricing: Commitment-based or pay-as-you-go
- Region: Multiple Azure regions

**Setup Steps:**

1. **Create Azure OpenAI Resource:**
   - Go to Azure Portal (https://portal.azure.com)
   - Create "Azure OpenAI" resource
   - Note your endpoint URL (e.g., `https://your-resource.openai.azure.com`)

2. **Deploy a Model:**
   - In your Azure OpenAI resource, go to "Model deployments"
   - Click "Create new deployment"
   - Choose a model (e.g., gpt-4o-mini)
   - Give it a deployment name (e.g., `gpt-4o-mini-deployment`)

3. **Get API Key:**
   - In Azure OpenAI resource, go to "Keys and Endpoint"
   - Copy KEY 1 or KEY 2

4. **Configure Environment:**

   **TypeScript API:**
   ```bash
   AI_PROVIDER=azure-openai
   AZURE_OPENAI_API_KEY=abc123...
   AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com
   AZURE_OPENAI_DEPLOYMENT=gpt-4o-mini-deployment
   AZURE_OPENAI_API_VERSION=2024-02-15-preview
   AI_MODEL=gpt-4o-mini
   ```

   **Rust Backend:**
   ```bash
   AI_PROVIDER=azure-openai
   AZURE_OPENAI_API_KEY=abc123...
   AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com
   AZURE_OPENAI_DEPLOYMENT=gpt-4o-mini-deployment
   AZURE_OPENAI_API_VERSION=2024-02-15-preview
   AI_MODEL=gpt-4o-mini
   ```

**API Versions:**
- `2024-02-15-preview` - Latest (recommended)
- `2023-12-01-preview` - Stable
- `2023-05-15` - Legacy

**Benefits:**
- ✅ Enterprise SLA
- ✅ Data residency control
- ✅ VNET/Private endpoint support
- ✅ Azure Active Directory authentication
- ✅ Commitment discounts available

---

### 3. Google Gemini (TypeScript API Only)

**Overview:**
- Google's latest AI models
- Best for: Google Cloud customers, cost optimization
- Pricing: Competitive with OpenAI
- Region: Global

**Setup Steps:**

1. **Get API Key:**
   - Visit https://makersuite.google.com/app/apikey
   - Create API key

2. **Configure Environment:**

   **TypeScript API:**
   ```bash
   AI_PROVIDER=google-ai
   AI_API_KEY=AIzaSy...
   AI_MODEL=gemini-pro
   ```

   **Rust Backend:**
   ```bash
   # Not yet supported - use TypeScript API
   ```

3. **Available Models:**
   - `gemini-pro` - Balanced performance
   - `gemini-pro-vision` - Multimodal (text + images)
   - `gemini-ultra` - Most capable (waitlist)

**Cost Estimates:**
- `gemini-pro`: Free tier available, then competitive pricing

---

### 4. Google Vertex AI (TypeScript API Only)

**Overview:**
- Enterprise Google AI via Google Cloud Platform
- Best for: GCP customers, production deployments
- Pricing: GCP billing
- Region: Multiple GCP regions

**Setup Steps:**

1. **Setup GCP Project:**
   - Create/select project in Google Cloud Console
   - Enable Vertex AI API

2. **Create Service Account:**
   - Create service account with Vertex AI permissions
   - Download JSON key file

3. **Configure Environment:**

   **TypeScript API:**
   ```bash
   AI_PROVIDER=vertex-ai
   GOOGLE_APPLICATION_CREDENTIALS=/path/to/service-account.json
   VERTEX_AI_PROJECT=your-gcp-project-id
   VERTEX_AI_LOCATION=us-central1
   AI_MODEL=gemini-pro
   ```

   **Rust Backend:**
   ```bash
   # Not yet supported - use TypeScript API
   ```

---

### 5. Anthropic Claude (TypeScript API Only)

**Overview:**
- Claude 3 family of models
- Best for: Long context, complex reasoning
- Pricing: Competitive
- Region: Global

**Setup Steps:**

1. **Get API Key:**
   - Visit https://console.anthropic.com/
   - Generate API key

2. **Configure Environment:**

   **TypeScript API:**
   ```bash
   AI_PROVIDER=anthropic
   AI_API_KEY=sk-ant-...
   AI_MODEL=claude-3-sonnet-20240229
   ```

   **Rust Backend:**
   ```bash
   # Not yet supported - use TypeScript API
   ```

3. **Available Models:**
   - `claude-3-opus-20240229` - Most capable
   - `claude-3-sonnet-20240229` - Balanced (recommended)
   - `claude-3-haiku-20240307` - Fast, cost-effective

**Unique Features:**
- 200K token context window
- Strong safety features
- Constitutional AI approach

---

## 🎯 Provider Selection Guide

### When to Use OpenAI
✅ **Use OpenAI when:**
- Quick setup needed
- Latest models required
- No compliance constraints
- Global access needed

### When to Use Azure OpenAI
✅ **Use Azure OpenAI when:**
- Enterprise compliance required
- Data residency matters
- Already using Azure
- Need SLA guarantees
- Want commitment discounts

### When to Use Google Gemini
✅ **Use Google Gemini when:**
- Cost optimization priority
- Using Google Cloud
- Need multimodal capabilities
- Want competitive pricing

### When to Use Anthropic Claude
✅ **Use Anthropic Claude when:**
- Need long context (200K tokens)
- Complex reasoning required
- Safety is critical
- Alternative to OpenAI needed

---

## 🔧 Settings UI Configuration

The frontend Settings UI supports provider configuration:

1. Open Settings (⚙️ icon)
2. Configure Log Source (Elasticsearch/OpenObserve)
3. AI Provider settings are configured via environment variables
4. Future enhancement: UI-based provider selection

**Note:** AI provider configuration is currently environment-variable based for security. API keys should not be stored in browser localStorage.

---

## 🛠️ Advanced Configuration

### Multiple Providers (TypeScript API)

You can configure fallback providers by setting multiple API keys:

```bash
# Primary provider
AI_PROVIDER=openai
OPENAI_API_KEY=sk-...

# Fallback (if primary fails)
AI_API_KEY=your-fallback-key
```

### Cost Optimization Tips

1. **Use appropriate models:**
   - Development: `gpt-4o-mini` or `claude-3-haiku`
   - Production: `gpt-4o` or `claude-3-sonnet`

2. **Set token limits:**
   ```bash
   # Limit response length to control costs
   MAX_TOKENS=1500
   ```

3. **Cache common queries:**
   - Implement caching layer for repeated queries
   - Reduce API calls significantly

4. **Monitor usage:**
   - Track API usage via provider dashboards
   - Set up billing alerts

---

## 🔒 Security Best Practices

### 1. API Key Management

**DO:**
- ✅ Use environment variables
- ✅ Rotate keys regularly
- ✅ Use separate keys for dev/staging/prod
- ✅ Set up billing alerts

**DON'T:**
- ❌ Commit keys to git
- ❌ Share keys via email/chat
- ❌ Use same key across environments
- ❌ Store keys in frontend code

### 2. Access Control

**Azure OpenAI:**
- Use Azure RBAC for access control
- Implement managed identities where possible
- Use private endpoints for production

**Vertex AI:**
- Use service accounts with minimal permissions
- Enable audit logging
- Implement VPC Service Controls

### 3. Rate Limiting

Implement rate limiting to prevent:
- Accidental cost overruns
- API quota exhaustion
- Abuse/attacks

```bash
# Example rate limit configuration
RATE_LIMIT_PER_MINUTE=60
RATE_LIMIT_PER_HOUR=1000
```

---

## 🐛 Troubleshooting

### OpenAI Errors

**"Invalid API key"**
- Check key starts with `sk-`
- Verify key is active in OpenAI dashboard
- Ensure no extra spaces in .env file

**"Rate limit exceeded"**
- Upgrade OpenAI tier
- Implement request throttling
- Consider multiple API keys

### Azure OpenAI Errors

**"Deployment not found"**
- Verify deployment name matches exactly
- Check deployment is in same region as endpoint
- Ensure model is deployed and active

**"Invalid endpoint"**
- Check endpoint format: `https://NAME.openai.azure.com`
- Don't include `/openai` in endpoint URL
- Verify resource exists in Azure

### Neurolink Errors

**"Provider not supported"**
- Check `AI_PROVIDER` value
- Verify Neurolink version supports provider
- Update to latest Neurolink: `npm update @juspay/neurolink`

---

## 📊 Model Comparison

| Model | Speed | Cost | Context | Best For |
|-------|-------|------|---------|----------|
| gpt-4o-mini | ⚡⚡⚡ | 💰 | 128K | Log parsing, quick queries |
| gpt-4o | ⚡⚡ | 💰💰💰 | 128K | Complex analysis |
| claude-3-haiku | ⚡⚡⚡ | 💰 | 200K | Fast responses |
| claude-3-sonnet | ⚡⚡ | 💰💰 | 200K | Balanced analysis |
| claude-3-opus | ⚡ | 💰💰💰💰 | 200K | Deep reasoning |
| gemini-pro | ⚡⚡ | 💰 | 32K | Cost-effective |

---

## 🔄 Migration Guide

### From OpenAI to Azure OpenAI

1. Create Azure OpenAI resource
2. Deploy same model to Azure
3. Update environment variables:
   ```bash
   # Old
   AI_PROVIDER=openai
   OPENAI_API_KEY=sk-...
   
   # New
   AI_PROVIDER=azure-openai
   AZURE_OPENAI_API_KEY=...
   AZURE_OPENAI_ENDPOINT=...
   AZURE_OPENAI_DEPLOYMENT=...
   ```
4. Test thoroughly before production switch

### From Single Provider to Multi-Provider

1. Add Neurolink-based TypeScript API
2. Deploy to Vercel
3. Configure multiple providers via environment variables
4. Implement provider selection logic
5. Monitor costs and performance

---

## 📚 Additional Resources

### OpenAI
- [OpenAI Platform](https://platform.openai.com/)
- [API Documentation](https://platform.openai.com/docs/api-reference)
- [Pricing](https://openai.com/pricing)

### Azure OpenAI
- [Azure OpenAI Service](https://azure.microsoft.com/en-us/products/ai-services/openai-service)
- [Documentation](https://learn.microsoft.com/en-us/azure/ai-services/openai/)
- [Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/)

### Google AI
- [Google AI Studio](https://makersuite.google.com/)
- [Vertex AI](https://cloud.google.com/vertex-ai)
- [Pricing](https://ai.google.dev/pricing)

### Anthropic
- [Anthropic Console](https://console.anthropic.com/)
- [API Documentation](https://docs.anthropic.com/)
- [Model Card](https://www.anthropic.com/claude)

### Neurolink SDK
- [Neurolink NPM Package](https://www.npmjs.com/package/@juspay/neurolink)
- [GitHub Repository](https://github.com/juspay/neurolink)

---

## 🆘 Support

For provider-specific issues:
- OpenAI: https://help.openai.com/
- Azure: Azure Support Portal
- Google Cloud: GCP Support
- Anthropic: support@anthropic.com

For Zeteo-specific issues:
- GitHub Issues: https://github.com/adarshba/zeteo/issues
- Documentation: https://github.com/adarshba/zeteo/tree/main/docs
