# Modal

Modal is a serverless platform that lets you run Python functions, AI models, and batch jobs at scale—with zero infrastructure management. With CloudCTL, you can deploy serverless functions, manage persistent volumes, and automate workflows—all from a single, consistent CLI.

Unlike most providers, Modal does not expose a public REST API. Instead, it uses a **Python-native SDK** for deep integration. CloudCTL wraps this SDK to provide a unified interface while preserving Modal’s powerful capabilities.

---

## Supported Resource Types

CloudCTL supports the following Modal services:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `ai` | Functions | Deploy serverless Python functions for AI, inference, or batch processing |
| `storage` | Volumes | Persistent storage for models, datasets, or state |

All resources support `deploy`, `list`, and cost estimation. Destruction is limited due to SDK constraints.

---

## Quick Start

Get started with Modal in three steps.

### 1. Generate API Credentials

Modal uses **token-based authentication** via environment variables:

- `MODAL_TOKEN_ID`
- `MODAL_TOKEN_SECRET`

1. Log in to the [Modal Dashboard](https://modal.com)
2. Go to **Account → Tokens**
3. Click **Create Token**
4. Copy both the **Token ID** and **Token Secret**

### 2. Save Credentials Securely

```bash
cloudctl secrets modal token_id
cloudctl secrets modal token_secret
```
You’ll be prompted to enter each value. They are stored encrypted in your system keyring—never in plaintext.

>✅ CloudCTL automatically sets MODAL_TOKEN_ID and MODAL_TOKEN_SECRET in the environment when needed. 

---

## Example Commands
### Deploy a Serverless AI Function
```bash
cloudctl deploy ai image-generator \
  --provider modal \
  --app ai-app \
  --name generate-image \
  --runtime python311 \
  --handler "main.handler" \
  --image modal/python:3.11
```

* Deploys a function from your local code
* Automatically packages and uploads
* Returns function URL and status

### Create a Persistent Volume
```bash
cloudctl deploy storage model-store \
  --provider modal \
  --name trained-models \
  --size 100
```

* Creates a volume for storing large files (e.g., ML models)
* Can be mounted to multiple functions
* Data persists across function runs

---

## List Resources
List deployed resources:

```bash
# List serverless functions
cloudctl list ai --provider modal

# List persistent volumes
cloudctl list storage --provider modal
```

Output includes:

* ID
* Name
* Status
* App
* Created At

>⚠️ Note: Modal’s SDK does not expose a full list API for all resource types. CloudCTL retrieves what is available. 

---

## Destroy Resources
Modal does not support deletion via API in a way that CloudCTL can safely automate.

---

## Cost Estimation
CloudCTL estimates monthly costs based on Modal’s pricing model:

```bash
cloudctl cost --provider modal
```
Output:
```bash
Provider: modal
Resource: image-generator (ai)
Runtime: vCPU-Hour
Estimated Hourly Cost: $0.013 USD
Estimated Monthly Cost: $9.36 USD

Resource: model-store (storage)
Size: 100 GB
Estimated Monthly Cost: $10.00 USD
```

Costs are based on:

* vCPU and memory usage
* Function duration
* Storage size

Modal bills per gigabyte-second of compute.

---

## Authentication & Security
CloudCTL uses Modal Token ID + Secret with encrypted storage.

* Credentials are securely injected into the Modal SDK
* Full audit log in Modal dashboard
* Never stored in config files

---

## Best Practices

* Use separate tokens for CI/CD and personal access
* Use volumes for large model storage
* Keep functions stateless and idempotent
* Use --dry-run to preview deployments
* Monitor logs and performance in the Modal dashboard