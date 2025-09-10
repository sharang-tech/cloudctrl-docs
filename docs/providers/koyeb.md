# Koyeb
Koyeb is a modern, developer-centric platform that enables seamless deployment of full-stack applications, APIs, and background workers directly from Git repositories. With CloudCTL, you can manage web services, workers, persistent storage, and custom domains—all from a single, consistent CLI.

No more juggling dashboards or writing deployment scripts. CloudCTL brings **universal control** to your Koyeb infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following Koyeb services:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `app` | Web Services | Deploy public-facing apps from GitHub, GitLab, or Docker |
| `worker` | Background Workers | Run long-running or scheduled background tasks |
| `storage` | Persistent Volumes | Attach durable storage to services |
| `domain` | Custom Domains | Map custom domains with automatic SSL via Let's Encrypt |

All resources support full lifecycle management—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Koyeb in three steps.

### 1. Generate an API Token

1. Log in to the [Koyeb Console](https://app.koyeb.com)
2. Go to **Settings → API Tokens**
3. Click **Create API Token**
4. Give it a name (e.g., `cloudctl-token`) and click **Create**
5. Copy the token (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets koyeb api_token
```
You’ll be prompted to enter the token. It is stored encrypted in your system keyring—never in plaintext.

---

## Example Commands
### Deploy a Web Service (App)
```bash
cloudctl deploy app my-website \
  --provider koyeb \
  --repo https://github.com/user/website \
  --branch main \
  --env DATABASE_URL=postgres://... \
  --name web-service
```

* Auto-deploys on Git push
* Supports Docker, static sites, and custom build commands
* Returns public URL (e.g., `https://web-service.your-org.koyeb.app`)

### Launch a Background Worker
```bash
cloudctl deploy worker queue-processor \
  --provider koyeb \
  --repo https://github.com/user/worker \
  --branch main \
  --cmd "python worker.py" \
  --name bg-worker
```

* Runs continuously or on a schedule
* Ideal for task queues, cron jobs, or message processing
* Logs are streamed to the Koyeb dashboard

### Create a Persistent Volume
```bash
cloudctl deploy storage data-store \
  --provider koyeb \
  --app web-service \
  --size 10 \
  --mount_path /data \
  --name app-volume
```

* Attached to services for persistent data
* Size in GB (minimum 1GB)
* Mounted at specified path (e.g., `/data`)

### Map a Custom Domain
```bash
cloudctl deploy domain app.example.com \
  --provider koyeb \
  --service_id svc-12345678 \
  --is_www_redirect_enabled true
```

* Automatically provisions SSL certificate
* Supports A, CNAME, and ALIAS records
* Enables HTTPS by default

---

## List Resources
List any deployed resources:

```bash
# List all apps
cloudctl list app --provider koyeb

# List background workers
cloudctl list worker --provider koyeb

# List volumes
cloudctl list storage --provider koyeb

# List domains
cloudctl list domain --provider koyeb
```

Output includes:

* ID
* Name
* Status
* Region
* Git Repository
* Creation Date

---

## Destroy Resources
Safely terminate resources:

```bash
cloudctl destroy svc-12345678 --provider koyeb
cloudctl destroy vol-87654321 --provider koyeb
```
>⚠️ Warning: Deletion is irreversible. Use --dry-run first.

## Dry-Run Mode
Preview changes safely:

```bash
cloudctl destroy svc-12345678 --provider koyeb --dry-run
```
---

## Cost Estimation
CloudCTL estimates monthly costs based on Koyeb’s pricing:

```bash
cloudctl cost --provider koyeb
```

Output:
```bash
Provider: koyeb
Resource: web-service (app)
Plan: free
Estimated Monthly Cost: $0.00 USD

Resource: bg-worker (worker)
Plan: free
Estimated Monthly Cost: $0.00 USD
```
Koyeb offers generous free tier usage. Paid plans start at $5/month for higher compute and storage.

---

## Authentication & Security
CloudCTL uses Koyeb API Tokens with encrypted storage.

* Tokens are account-scoped
* Full audit log in Koyeb dashboard
* Never stored in config files

---

## Best Practices

* Use separate tokens for CI/CD and personal access
* Enable auto-deploy for production branches
* Use `--dry-run` before production changes
* Store secrets in Koyeb’s environment variables (not in code)
* Use custom domains with SSL for production sites