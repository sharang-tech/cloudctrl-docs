# Together.ai

Together.ai delivers high-performance, low-latency inference for large language models (LLMs) like Llama, Mistral, and Falcon—optimized for production workloads. With CloudCTL, you can deploy and manage AI models via a unified CLI, enabling seamless integration into your AI pipelines.

No more juggling API keys or writing custom HTTP clients. CloudCTL brings **universal control** to your Together.ai usage, with full support for `deploy`, cost estimation, and `--dry-run` safety.

> ⚠️ **Note**: Together.ai is an inference-only platform. It does not support persistent resources, deletion via CLI, or long-running services. All actions are stateless and API-driven.

---

## Supported Resource Types

CloudCTL supports the following Together.ai services:

| Resource Type | Maps To       | Description                            |
| ------------- | ------------- | -------------------------------------- |
| `ai`        | Inference API | Run real-time inference on hosted LLMs |

All operations are ephemeral and executed via API calls. No persistent infrastructure is managed.

---

## Quick Start

Get started with Together.ai in three steps.

### 1. Generate an API Key

1. Log in to the [Together.ai Dashboard](https://api.together.ai)
2. Go to **Settings → API Keys**
3. Click **Create API Key**
4. Copy the key (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets together api_key
```

You’ll be prompted to enter the key. It is stored encrypted in your system keyring—never in plaintext.

---

## Example Commands

### Deploy an AI Inference Job

```bash
cloudctl deploy ai llama3-chat \
  --provider together \
  --model meta-llama/Llama-3-8b-chat-hf \
  --prompt "Explain quantum computing in simple terms." \
  --max_tokens 512 \
  --temperature 0.7
```

* Runs inference using the specified model
* Returns generated text and metadata
* Supports streaming (when enabled in API)

### Run a Mistral Inference

```bash
cloudctl deploy ai mistral-query \
  --provider together \
  --model mistralai/Mistral-7B-v0.1 \
  --prompt "Summarize the benefits of renewable energy." \
  --top_p 0.9 \
  --repetition_penalty 1.1
```

* Uses Mistral-7B for fast, efficient responses
* Configurable sampling parameters
* Ideal for chatbots, summarization, and classification

---

## List Resources

Together.ai does not expose a list API for inference jobs.

You can only:

* Deploy (inference) new requests
* View costs
* Use --dry-run to preview

There is no list output for historical jobs.

---

## Destroy Resources

Together.ai does not support deletion of inference jobs—they are stateless and ephemeral by design.
Each inference call is a one-time request. No cleanup is required.

---

## Cost Estimation

CloudCTL estimates inference costs based on Together.ai’s token-based pricing:

```bash
cloudctl cost --provider together
```

Output:

```bash
Provider: together
Resource: llama3-chat (ai)
Model: meta-llama/Llama-3-8b-chat-hf
Estimated Cost: $0.0006 / 1K Tokens (Input), $0.0008 / 1K Tokens (Output)
Estimated Monthly Cost: Based on usage
```

Costs are calculated per input and output token, billed in increments of 1,000 tokens.

Example:

* 512 input tokens + 256 output tokens ≈ $0.0005

---

## Authentication & Security

CloudCTL uses Together.ai API Keys with encrypted storage.

* Keys are account-scoped
* Full audit log in Together.ai dashboard
* Never stored in config files

---

## Best Practices

* Use separate keys for CI/CD and personal access
* Cache responses when possible to reduce cost
* Use `--dry-run` to estimate prompt costs
* Monitor token usage to avoid budget overruns
* Prefer smaller models for low-latency use cases
