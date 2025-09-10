# Vercel

Vercel is the leading platform for frontend developers, offering instant deployments, serverless functions, and global CDN for static sites and full-stack applications. With CloudCTL, you can deploy and manage frontend projects directly from GitHub—all from a single, consistent CLI.

No more juggling dashboards or writing custom scripts. CloudCTL brings **universal control** to your Vercel infrastructure, with full support for `deploy`, `list`, cost estimation, and `--dry-run` safety.

> ⚠️ **Note**: Vercel does not support resource destruction via its public API. Deletion must be performed through the dashboard.

---

## Supported Resource Types

CloudCTL supports the following Vercel services:

| Resource Type | Maps To                | Description                                                   |
| ------------- | ---------------------- | ------------------------------------------------------------- |
| `frontend`  | Projects & Deployments | Deploy static sites and full-stack apps from Git repositories |

All resources support `deploy` and `list` operations, with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Vercel in three steps.

### 1. Generate an API Token

1. Log in to the [Vercel Dashboard](https://vercel.com)
2. Go to **Settings → Tokens**
3. Click **Create**
4. Name it (e.g., `cloudctl-token`) and click **Create**
5. Copy the token (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets vercel api_token
```

You’ll be prompted to enter the token. It is stored encrypted in your system keyring—never in plaintext.

### 3. Link Your GitHub Account

Before deploying, ensure your GitHub account is linked in the Vercel dashboard. All deployments are Git-triggered.

---

## Example Commands

### Deploy a Frontend Project

```bash
cloudctl deploy frontend docs-site \
  --provider vercel \
  --repo https://github.com/user/docs \
  --branch main \
  --name docs-site
```

* Auto-enables Git synchronization
* Deploys with zero-downtime
* Returns public URL (e.g., `https://docs-site.vercel.app`)

### Deploy a Full-Stack Application

```bash
cloudctl deploy frontend api-app \
  --provider vercel \
  --repo https://github.com/user/api-app \
  --branch main \
  --env DATABASE_URL=postgres://... \
  --env NODE_ENV=production
```

* Supports serverless functions and API routes
* Builds using vercel.json or framework defaults
* Automatically handles environment variables

---

## List Resources

List any deployed frontend projects:

```bash
cloudctl list frontend --provider vercel
```

Output includes:

* ID
* Name
* Git Repository
* Branch
* URL
* State (READY, BUILDING, ERROR)
* Created At

---

## Destroy Resources

Vercel does not support deletion via its public API.
To destroy a project: Go to Vercel Dashboard

---

## Cost Estimation

CloudCTL estimates monthly costs based on Vercel’s pricing:

```bash
cloudctl cost --provider vercel
```

Output:

```bash
Provider: vercel
Resource: docs-site (frontend)
Plan: hobby
Estimated Monthly Cost: $0.00 USD

Resource: api-app (frontend)
Plan: pro
Estimated Monthly Cost: $20.00 USD
```

* Hobby Plan: Free (personal projects)
* Pro Plan: $20/month (custom domains, enhanced analytics, team collaboration)

Costs are based on bandwidth, serverless function execution, and team seats.

---

## Authentication & Security

CloudCTL uses Vercel API Tokens with encrypted storage.

* Tokens are account-scoped
* Full audit log in Vercel dashboard
* Never stored in config files

---

## Best Practices

* Use separate tokens for CI/CD and personal access
* Enable automatic Git deploys for production branches
* Use `--dry-run` to preview changes
* Store secrets in Vercel’s environment variables (not in code)
* Use Preview Deployments for staging
