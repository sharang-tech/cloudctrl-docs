# Render

Render is a modern cloud platform for hosting web services, APIs, background workers, and static sites with built-in CI/CD, SSL, and custom domains. With CloudCTL, you can deploy and manage full-stack applications across frontend, backend, and database services—all from a single, consistent CLI.

No more juggling dashboards or writing deployment scripts. CloudCTL brings **universal control** to your Render infrastructure, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## Supported Resource Types

CloudCTL supports the following Render services:

| Resource Type | Maps To                     | Description                                                        |
| ------------- | --------------------------- | ------------------------------------------------------------------ |
| `frontend`  | Static Sites & Web Services | Deploy static sites or full web apps from GitHub/GitLab            |
| `worker`    | Background Workers          | Run background jobs and queue processors                           |
| `database`  | PostgreSQL, Redis           | Managed databases with automated backups and SSL                   |
| `service`   | Private Services            | Internal APIs and microservices not exposed to the public internet |

All resources support full lifecycle management—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Render in three steps.

### 1. Generate an API Token

1. Log in to the [Render Dashboard](https://dashboard.render.com)
2. Go to **Account → Tokens**
3. Click **Create New Token**
4. Give it a name (e.g., `cloudctl-token`) and click **Create**
5. Copy the token (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets render api_token
```

You’ll be prompted to enter the token. It is stored encrypted in your system keyring—never in plaintext.

---

## Example Commands

### Deploy a Static Site (Frontend)

```bash
cloudctl deploy frontend docs-site \
  --provider render \
  --repo https://github.com/user/docs \
  --branch main \
  --name docs-site
```

* Auto-enables GitHub sync
* Deploys with zero-downtime
* Returns public URL (e.g., `https://docs-site.onrender.com`)

### Deploy a Web Service

```bash
cloudctl deploy frontend api-service \
  --provider render \
  --repo https://github.com/user/api \
  --branch main \
  --env DATABASE_URL=postgres://... \
  --plan standard
```

* Builds from Dockerfile or `render.yaml`
* Supports environment variables
* Auto-configures HTTPS and custom domains

### Launch a Background Worker

```bash
cloudctl deploy worker queue-processor \
  --provider render \
  --repo https://github.com/user/worker \
  --branch main \
  --cmd "python worker.py"
```

* Runs continuously or on schedule
* Ideal for task queues, cron jobs, or message processing
* Logs streamed to Render dashboard

### Create a PostgreSQL Database

```bash
cloudctl deploy database main-db \
  --provider render \
  --engine postgres \
  --plan free \
  --name production-db
```

* Auto-generates connection string with SSL
* Enables automated daily backups
* Supports read replicas (via dashboard)

### Deploy a Redis Instance

```bash
cloudctl deploy database cache-store \
  --provider render \
  --engine redis \
  --plan free \
  --name redis-cache
```

* In-memory store for sessions or caching
* Returns connection endpoint and port
* SSL enabled by default

### Create a Private Service

```bash
cloudctl deploy service internal-api \
  --provider render \
  --repo https://github.com/user/internal-api \
  --branch staging \
  --plan standard
```

* Not exposed to the public internet
* Accessible only from other services in the same account
* Ideal for internal microservices

---

## List Resources

List any deployed resources:

```bash
# List all frontend services
cloudctl list frontend --provider render

# List background workers
cloudctl list worker --provider render

# List databases
cloudctl list database --provider render

# List private services
cloudctl list service --provider render
```

Output includes:

* ID
* Name
* Status
* Plan (free, standard, pro)
* URL (if public)
* Creation Date

---

## Destroy Resources

Safely terminate resources:

```bash
cloudctl destroy srv-cv8abcdefg1234567890 --provider render
cloudctl destroy dbs-xyz123456789 --provider render
```

> ⚠️ Warning: Deletion is irreversible and results in data loss (for databases). Use --dry-run first.

### Dry-Run Mode

Preview changes safely:

```bash
cloudctl destroy srv-cv8abcdefg1234567890 --provider render --dry-run
```

---

## Cost Estimation

CloudCTL estimates monthly costs based on Render’s pricing:

```bash
cloudctl cost --provider render
```

Output:

```bash
Provider: render
Resource: api-service (frontend)
Plan: standard
Estimated Monthly Cost: $7.00 USD

Resource: production-db (database)
Plan: starter
Estimated Monthly Cost: $7.00 USD
```

Estimates include compute, storage, and bandwidth. Free-tier resources show $0.00.

---

## Authentication & Security

CloudCTL uses Render API Tokens with encrypted storage.

* Tokens are account-scoped
* Full audit log in Render dashboard
* Never stored in config files

---

## Best Practices

* Use separate tokens for CI/CD pipelines
* Enable GitHub integration for automatic deploys
* Use `--dry-run` before production changes
* Store secrets in Render’s environment variables (not in code)
* Use private services for internal communication
