# RunPod

RunPod is a high-performance cloud platform designed for AI, machine learning, and GPU-intensive workloads. With CloudCTL, you can deploy and manage GPU Pods, Serverless Inference Endpoints, Persistent Storage Volumes, and Private Workers—all from a single, consistent CLI.

No more juggling dashboards or writing custom automation scripts. CloudCTL brings **universal control** to your RunPod infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following RunPod services:

| Resource Type  | Maps To              | Description                                                         |
| -------------- | -------------------- | ------------------------------------------------------------------- |
| `ai`         | GPU Pods             | Persistent GPU-powered virtual machines with customizable templates |
| `serverless` | Serverless Functions | On-demand AI inference with auto-scaling and low latency            |
| `storage`    | Volumes              | Persistent storage attached to Pods or used independently           |
| `worker`     | Private Workers      | Dedicated workers for secure, isolated compute                      |

All resources support full lifecycle operations—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with RunPod in three steps.

### 1. Generate an API Key

1. Log in to the [RunPod Dashboard](https://runpod.io)
2. Go to **Account → API Keys**
3. Click **Create API Key**
4. Name it (e.g., `cloudctl-key`) and click **Create**
5. Copy the key (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets runpod api_key
```

You’ll be prompted to enter the key. It is stored encrypted in your system keyring—never in plaintext.

---

## Example Commands

### Deploy a GPU Pod

```bash
cloudctl deploy ai llama3-trainer \
  --provider runpod \
  --gpu_type "NVIDIA RTX A5000" \
  --template pytorch:latest \
  --name gpu-pod-01 \
  --volume_size 50 \
  --region all
```

* Launches a persistent GPU instance
* Supports popular templates: `pytorch`, `tensorflow`, `base`, etc.
* Attaches a 50GB persistent volume
* Returns Pod ID, public IP, and SSH access details

### Launch a Serverless Inference Endpoint

```bash
cloudctl deploy serverless text-generation \
  --provider runpod \
  --model "meta/llama3-8b" \
  --runtime python3.11 \
  --handler "inference.handler"
```

* Auto-scales to zero when idle
* Ideal for API-based AI inference
* Returns endpoint URL and invocation command

### Create a Persistent Volume

```bash
cloudctl deploy storage model-store \
  --provider runpod \
  --name model-data \
  --size 200 \
  --region us-east
```

* Used for storing models, datasets, or checkpoints
* Can be attached to multiple Pods over time
* Returns volume ID and status

### Deploy a Private Worker

```bash
cloudctl deploy worker training-worker \
  --provider runpod \
  --name worker-01 \
  --gpu_type "NVIDIA A100" \
  --worker_type SECURE_WORKER
```

* Runs in your private network
* Isolated from public internet
* Ideal for sensitive or high-priority workloads

---

## List Resources

List any deployed resources:

```bash
# List all GPU Pods
cloudctl list ai --provider runpod

# List serverless endpoints
cloudctl list serverless --provider runpod

# List storage volumes
cloudctl list storage --provider runpod

# List private workers
cloudctl list worker --provider runpod
```

Output includes:

* ID
* Name
* GPU Type
* Status
* Region
* Cost Per Hour

---

## Destroy Resources

Safely terminate resources:

```bash
cloudctl destroy pod-123abc456def --provider runpod
cloudctl destroy vol-789xyz123abc --provider runpod
```

> ⚠️ Warning: Deletion is irreversible and results in data loss. Use --dry-run first.

### Dry-Run Mode

Preview changes safely:

```bash
cloudctl destroy pod-123abc456def --provider runpod --dry-run
```

---

## Cost Estimation

CloudCTL estimates monthly costs based on RunPod’s on-demand pricing:

```bash
cloudctl cost --provider runpod
```

Output:

```bash
Provider: runpod
Resource: gpu-pod-01 (ai)
GPU: NVIDIA RTX A5000
Estimated Hourly Cost: $0.60 USD
Estimated Monthly Cost: $432.00 USD

Resource: model-store (storage)
Size: 200 GB
Estimated Monthly Cost: $20.00 USD
```

Costs are based on GPU type, compute time, and storage size.

---

## Authentication & Security

CloudCTL uses RunPod API Keys with encrypted storage.

* Keys are account-scoped
* Full audit log in RunPod dashboard
* Never stored in config files

---

## Best Practices

* Use separate keys for CI/CD and personal access
* Use persistent volumes for model/data storage
* Use `--dry-run` before production changes
* Monitor GPU utilization to optimize costs
* Use serverless for bursty or low-latency inference workloads
