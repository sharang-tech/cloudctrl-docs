# Vast.ai
Vast.ai is a decentralized GPU marketplace that lets you rent high-performance instances at spot-market prices for AI training, inference, and rendering workloads. With CloudCTL, you can deploy GPU instances, persistent storage volumes, and place rent bids—all from a single, consistent CLI.

No more juggling dashboards or writing custom scripts. CloudCTL brings **universal control** to your Vast.ai infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following Vast.ai services:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `ai` | GPU Instances | Rent powerful GPUs (A100, H100, RTX 4090, etc.) by the hour |
| `storage` | Persistent Volumes | Attach durable storage to instances |
| `bid` | Rent Bids | Place competitive bids for lower-cost GPU access |

All resources support full lifecycle operations—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Vast.ai in three steps.

### 1. Generate an API Key

1. Log in to the [Vast.ai Dashboard](https://vast.ai)
2. Go to **Account → API Keys**
3. Click **Create API Key**
4. Copy the key (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets vast api_key
```

You’ll be prompted to enter the key. It is stored encrypted in your system keyring—never in plaintext.

---

## Example Commands
### Deploy a GPU Instance
```bash
cloudctl deploy ai llama3-trainer \
  --provider vast \
  --machine_id 123456 \
  --model pytorch:latest \
  --onstart "pip install transformers && python train.py" \
  --disk 50
```

* Targets a specific machine from the marketplace
* Runs a startup script on boot
* Attaches 50GB of disk space
* Returns instance ID and SSH connection details

### Place a Rent Bid (Spot-like Pricing)
```bash
cloudctl deploy bid gpu-bid-01 \
  --provider vast \
  --machine_id 789012 \
  --price 0.50 \
  --image pytorch/pytorch:latest
```

* Bids $0.50/hour for a GPU machine
* If accepted, automatically provisions the instance
* Ideal for cost-sensitive batch jobs

### Create a Persistent Volume
```bash
cloudctl deploy storage model-store \
  --provider vast \
  --name trained-models \
  --size 200 \
  --region all
```

* Used for storing models, datasets, or checkpoints
* Can be attached to multiple instances over time
* Returns volume ID and mount instructions

---

## List Resources
List any deployed resources:

```bash
# List all GPU instances
cloudctl list ai --provider vast

# List rent bids
cloudctl list bid --provider vast

# List storage volumes
cloudctl list storage --provider vast
```

Output includes:

* ID
* Name
* GPU Type
* Cost Per Hour
* Status
* Machine ID
* Region

---

## Destroy Resources
Safely terminate instances and volumes:

```bash
cloudctl destroy 123456 --provider vast
cloudctl destroy vol-789012 --provider vast
```
>⚠️ Warning: Deletion is irreversible and results in data loss. Use --dry-run first.

### Dry-Run Mode
Preview changes safely:

```bash
cloudctl destroy 123456 --provider vast --dry-run
```

---

## Cost Estimation
CloudCTL estimates monthly costs based on Vast.ai’s hourly pricing:

```bash
cloudctl cost --provider vast
```

Output:
```bash
Provider: vast
Resource: llama3-trainer (ai)
Hourly Rate: $0.80 USD
Estimated Monthly Cost: $576.00 USD

Resource: model-store (storage)
Size: 200 GB
Estimated Monthly Cost: $20.00 USD
```

Costs are based on bid price, instance runtime, and storage size.

---

## Authentication & Security
CloudCTL uses Vast.ai API Keys with encrypted storage.

* Keys are account-scoped
* Full audit log in Vast.ai dashboard
* Never stored in config files

---

## Best Practices

* Use `bid` for cost-efficient batch training
* Use persistent volumes for model checkpointing
* Use `--dry-run` before production changes
* Monitor machine availability and pricing trends
* Automate deployments with scripts and CI/CD
