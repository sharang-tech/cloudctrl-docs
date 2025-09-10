# Fireworks.ai

Fireworks.ai delivers blazing-fast, low-latency inference for large language models (LLMs) and vision models—optimized for scale, reliability, and developer experience. With CloudCTL, you can deploy and manage AI inference jobs across state-of-the-art models like Llama, Mixtral, and custom fine-tuned variants—all from a single, consistent CLI.

No more writing custom HTTP clients or managing API endpoints. CloudCTL brings **universal control** to your Fireworks.ai usage, with full support for `deploy`, cost estimation, and `--dry-run` safety.

> ⚠️ **Note**: Fireworks.ai is an inference-only platform. It does not support persistent infrastructure or deletion via CLI. All actions are stateless and API-driven.

---

## Supported Resource Types

CloudCTL supports the following Fireworks.ai services:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `ai` | Inference API | Run real-time or batch inference on hosted LLMs and vision models |

All operations are ephemeral and executed via API calls. No persistent resources are created.

---

## Quick Start

Get started with Fireworks.ai in three steps.

### 1. Generate an API Key

1. Log in to the [Fireworks.ai Dashboard](https://app.fireworks.ai)
2. Go to **Settings → API Keys**
3. Click **Create API Key**
4. Copy the key (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets fireworks api_key
```
You’ll be prompted to enter the key. It is stored encrypted in your system keyring—never in plaintext.

---

## Example Commands
### Deploy an AI Inference Job
```bash
cloudctl deploy ai llama3-response \
  --provider fireworks \
  --model accounts/fireworks/models/llama-v3-8b-instruct \
  --prompt "Explain the theory of relativity in simple terms." \
  --max_tokens 512 \
  --temperature 0.7
```

* Uses Fireworks.ai’s optimized Llama 3 model
* Returns generated text and metadata
* Supports streaming output for real-time UIs

### Run a Mixtral Inference
```bash
cloudctl deploy ai mixtral-summary \
  --provider fireworks \
  --model accounts/fireworks/models/mixtral-8x7b-instruct \
  --prompt "Summarize the key points of this article: ..." \
  --top_p 0.9 \
  --repetition_penalty 1.1
```
* Leverages Mixtral’s sparse Mixture-of-Experts architecture
* Ideal for high-quality summarization, reasoning, and code generation
* Ultra-low latency compared to open-source self-hosted options

### Deploy a Vision Model
```bash
cloudctl deploy ai image-understanding \
  --provider fireworks \
  --model accounts/fireworks/models/firellava-13b \
  --image "https://example.com/chart.png" \
  --prompt "Describe the data shown in this chart."
```
* Supports multimodal (image + text) input
* Returns natural language descriptions
* Great for accessibility, analytics, and automation

---

## List Resources
Fireworks.ai does not expose a list API for inference jobs.

You can only:

* Deploy new inference requests
* View cost estimates
* Use --dry-run to preview

There is no list output—jobs are stateless and immediate.

---

## Destroy Resources
Fireworks.ai does not support deletion of inference jobs—they are transient by design.
Each call is a one-time request. No cleanup is required.

---

## Cost Estimation
CloudCTL estimates inference costs based on Fireworks.ai’s token-based pricing:

```bash
cloudctl cost --provider fireworks
```

Output:
```bash
Provider: fireworks
Resource: llama3-response (ai)
Model: llama-v3-8b-instruct
Estimated Cost: $0.0004 / 1K Tokens (Input), $0.0006 / 1K Tokens (Output)
Estimated Monthly Cost: Based on usage
```

Costs are calculated per input and output token, billed in increments of 1,000 tokens.

Example:
* 1024 input tokens + 512 output tokens ≈ $0.0007

Fireworks.ai offers some of the lowest inference costs for high-quality models.

---

## Authentication & Security
CloudCTL uses Fireworks.ai API Keys with encrypted storage.

* Keys are account-scoped
* Full audit log in Fireworks.ai dashboard
* Never stored in config files

---

## Best Practices

* Use separate keys for CI/CD and personal access
* Cache responses to reduce cost and latency
* Use `--dry-run` to estimate token usage
* Monitor throughput and latency via Fireworks.ai dashboard
* Prefer smaller models for high-volume use cases