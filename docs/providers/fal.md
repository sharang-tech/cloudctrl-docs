# Fal.ai

Fal.ai is a serverless platform optimized for running AI models with zero infrastructure management, instant scaling, and low-latency inference. With CloudCTL, you can deploy and manage AI functions across popular models like Llama, Stable Diffusion, Whisper, and custom fine-tuned variants—all from a single, consistent CLI.

No more juggling API endpoints or writing custom clients. CloudCTL brings **universal control** to your Fal.ai usage, with full support for `deploy`, cost estimation, and `--dry-run` safety.

> ⚠️ **Note**: Fal.ai is a serverless inference platform. It does not support persistent resources or deletion via CLI. All actions are stateless and API-driven.

---

## Supported Resource Types

CloudCTL supports the following Fal.ai services:

| Resource Type | Maps To              | Description                                              |
| ------------- | -------------------- | -------------------------------------------------------- |
| `ai`        | Serverless Functions | Run inference on hosted AI models with automatic scaling |

All operations are ephemeral and executed via API calls. No persistent infrastructure is created.

---

## Quick Start

Get started with Fal.ai in three steps.

### 1. Generate an API Key

1. Log in to the [Fal.ai Dashboard](https://fal.ai)
2. Go to **Settings → API Keys**
3. Click **Create API Key**
4. Copy the key (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets fal api_key
```

You’ll be prompted to enter the key. It is stored encrypted in your system keyring—never in plaintext.

---

## Example Commands

### Deploy a Text Generation Job (Llama)

```bash
cloudctl deploy ai llama3-chat \
  --provider fal \
  --model fal-ai/llama3-8b \
  --prompt "Explain how photosynthesis works." \
  --max_new_tokens 512 \
  --temperature 0.7
```

* Uses Fal.ai’s optimized Llama 3 model
* Returns generated text and metadata
* Supports streaming for real-time applications

### Generate an Image with Stable Diffusion

```bash
cloudctl deploy ai image-gen \
  --provider fal \
  --model fal-ai/stable-diffusion-xl \
  --prompt "A futuristic city under a purple sky, digital art" \
  --image_size "1024x1024" \
  --steps 30
```

* Generates high-quality images from text prompts
* Returns image URL and generation metadata
* Ideal for creative workflows and content generation

### Transcribe Audio with Whisper

```bash
cloudctl deploy ai transcribe \
  --provider fal \
  --model fal-ai/whisper \
  --audio "https://example.com/audio.mp3" \
  --language en
```

* Automatically transcribes speech to text
* Supports multiple languages
* Great for accessibility, note-taking, and voice assistants

---

## List Resources

Fal.ai does not expose a list API for inference jobs.

You can only:

* Deploy new inference requests
* View cost estimates
* Use --dry-run to preview

There is no list output—jobs are stateless and immediate.

---

## Destroy Resources

Fal.ai does not support deletion of inference jobs—they are transient by design.
Each call is a one-time request. No cleanup is required.

---

## Cost Estimation

CloudCTL estimates inference costs based on Fal.ai’s usage-based pricing:

```bash
cloudctl cost --provider fal
```

Output:

```bash
Provider: fal
Resource: llama3-chat (ai)
Model: fal-ai/llama3-8b
Estimated Cost: $0.0012 per run (approx)
Estimated Monthly Cost: Based on usage
```

Costs are calculated per inference call or compute second, depending on model size and duration.

* Text Models: Billed per token or per second
* Image Models: Billed per generation
* Audio Models: Billed per minute of audio processed

Fal.ai offers predictable pricing with transparent metering.

---

## Authentication & Security

CloudCTL uses Fal.ai API Keys with encrypted storage.

* Keys are account-scoped
* Full audit log in Fal.ai dashboard
* Never stored in config files

---

## Best Practices

* Use separate keys for CI/CD and personal access
* Cache responses to reduce cost and latency
* Use `--dry-run` to estimate job cost before execution
* Monitor usage via Fal.ai dashboard
* Prefer smaller models for high-throughput use cases
