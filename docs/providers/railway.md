# Railway

Railway is a modern developer platform that enables you to deploy full-stack applications, databases, and services from GitHub with zero configuration. With CloudCTL, you can manage projects, services, databases, and domains—all from a single, consistent CLI.

No more switching between the dashboard and CLI. CloudCTL brings **universal control** to your Railway infrastructure, with full support for `deploy`, `list`, cost estimation, and `--dry-run` safety.

> ⚠️ **Note**: Railway does not support resource deletion via its public API. Destructive actions must be performed through the dashboard.

---

## Supported Resource Types

CloudCTL supports the following Railway services:

| Resource Type | Maps To                    | Description                               |
| ------------- | -------------------------- | ----------------------------------------- |
| `app`       | Projects & Services        | Deploy apps from GitHub repositories      |
| `database`  | PostgreSQL, MongoDB, Redis | Managed databases with connection strings |
| `domain`    | Custom Domains             | Map custom domains to services with SSL   |

All resources support `deploy` and `list` operations, with real-time cost estimation and dry-run previews.

---

## Quick Start

Get started with Railway in three steps.

### 1. Generate an API Token

1. Log in to the [Railway Dashboard](https://railway.app)
2. Go to **Settings → API Tokens**
3. Click **Create Token**
4. Name it (e.g., `cloudctl-token`) and click **Create**
5. Copy the token (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets railway api_token
```

You’ll be prompted to enter the token. It is stored encrypted in your system keyring—never in plaintext.

---

## Example Commands

### Deploy a Project (App)

```bash
cloudctl deploy app my-api \
  --provider railway \
  --repo https://github.com/user/api-service \
  --branch main \
  --env DATABASE_URL=postgres://... \
  --name api-service
```

* Auto-deploys on Git push
* Supports Node.js, Python, Go, Rust, and more
* Returns service URL (e.g., `https://api-service.up.railway.app`)

### Launch a PostgreSQL Database

```bash
cloudctl deploy database production-db \
  --provider railway \
  --engine postgresql \
  --plan starter \
  --name main-db
```

* Auto-generates connection string with credentials
* Enables SSL by default
* Supports environment variable injection into apps

### Create a MongoDB Instance

```bash
cloudctl deploy database analytics-store \
  --provider railway \
  --engine mongodb \
  --plan starter \
  --name mongo-db
```

* No manual setup required
* Connection string available in dashboard and via API
* Ideal for document-based data

### Deploy a Redis Cache

```bash
cloudctl deploy database cache-store \
  --provider railway \
  --engine redis \
  --plan starter \
  --name redis-cache
```

* In-memory store for sessions, queues, or caching
* Returns Redis URL
* Automatically connects to linked services

### Map a Custom Domain

```bash
cloudctl deploy domain app.example.com \
  --provider railway \
  --service_id srv-12345678 \
  --name custom-domain
```

* Automatically provisions SSL via Let's Encrypt
* Supports A and CNAME records
* Instant propagation via Railway’s global CDN

---

## List Resources

List any deployed resources:

```bash
# List all apps (services)
cloudctl list app --provider railway

# List databases
cloudctl list database --provider railway

# List domains
cloudctl list domain --provider railway
```

Output includes:

* ID
* Name
* Status
* Engine (for databases)
* URL
* Updated At

---

## Destroy Resources

Railway does not support deletion via its public API.

---

## Cost Estimation

CloudCTL estimates monthly costs based on Railway’s pricing:

```bash
cloudctl cost --provider railway
```

Output:

```bash
Provider: railway
Resource: api-service (app)
Plan: starter
Estimated Monthly Cost: $5.00 USD

Resource: main-db (database)
Engine: postgresql
Plan: starter
Estimated Monthly Cost: $7.00 USD
```

Railway offers a generous free tier. Paid plans start at $5/month per service.

---

## Authentication & Security

CloudCTL uses Railway API Tokens with encrypted storage.

* Tokens are account-scoped
* Full audit log in Railway dashboard
* Never stored in config files

---

## Best Practices

* Use separate tokens for CI/CD and personal access
* Enable GitHub sync for automatic deploys
* Use `--dry-run` to preview changes
* Store secrets in Railway’s environment variables (not in code)
* Link databases to services using Railway’s UI or API
