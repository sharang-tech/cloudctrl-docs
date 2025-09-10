# Netlify

Netlify is a powerful platform for deploying and hosting static websites, Jamstack applications, and serverless functions with instant cache invalidation, global CDN, and continuous deployment from Git. With CloudCTL, you can deploy and manage frontend sites directly from GitHub, GitLab, or Bitbucket—all from a single, consistent CLI.

No more juggling dashboards or writing custom scripts. CloudCTL brings **universal control** to your Netlify infrastructure, with full support for `deploy`, `list`, cost estimation, and `--dry-run` safety.

> ⚠️ **Note**: Netlify does not support site deletion via its public API. Destructive actions must be performed through the dashboard.

---

## Supported Resource Types

CloudCTL supports the following Netlify services:

| Resource Type | Maps To | Description |
|---------------|--------|-------------|
| `frontend` | Sites & Deployments | Deploy static sites and Jamstack apps from Git repositories |

All resources support `deploy` and `list` operations, with real-time cost tracking and dry-run previews.

---

## Quick Start

Get started with Netlify in three steps.

### 1. Generate a Personal Access Token

1. Log in to the [Netlify Dashboard](https://app.netlify.com)
2. Go to **User Settings → Applications**
3. Under **Personal Access Tokens**, click **New access token**
4. Name it (e.g., `cloudctl-token`) and click **Generate**
5. Copy the token (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets netlify api_token
```
You’ll be prompted to enter the token. It is stored encrypted in your system keyring—never in plaintext.

### 3. Link Your Git Provider
Ensure your GitHub, GitLab, or Bitbucket account is linked in the Netlify dashboard. All deployments are Git-triggered.

---

## Example Commands
### Deploy a Frontend Site
```bash
cloudctl deploy frontend docs-site \
  --provider netlify \
  --repo https://github.com/user/docs \
  --branch main \
  --name docs-site
```

* Auto-enables continuous deployment
* Deploys with zero-downtime and instant cache purge
* Returns public URL (e.g., `https://docs-site.netlify.app`)

### Deploy a Jamstack Application
```bash
cloudctl deploy frontend marketing-app \
  --provider netlify \
  --repo https://github.com/user/marketing \
  --branch main \
  --env API_URL=https://api.example.com \
  --cmd "npm run build"
```
* Runs custom build command
* Supports environment variables
* Automatically handles redirects and headers via `_redirects` or `netlify.toml`

---
## List Resources
List any deployed frontend sites:

```bash
cloudctl list frontend --provider netlify
```
Output includes:

* ID
* Name
* Git Repository
* Branch
* URL
* State (active, locked, error)
* Created At

---

## Destroy Resources
Netlify does not support site deletion via its public API.

---

## Cost Estimation
CloudCTL estimates monthly costs based on Netlify’s pricing:

```bash
cloudctl cost --provider netlify
```

Output:
```bash
Provider: netlify
Resource: docs-site (frontend)
Plan: pro
Estimated Monthly Cost: $19.00 USD
```

* Free Plan: $0/month (personal use, limited builds)
* Pro Plan: $19/month (team collaboration, enhanced analytics, form handling)
* Enterprise: Custom pricing

Costs are based on bandwidth, build minutes, and team seats.

---

## Authentication & Security
CloudCTL uses Netlify Personal Access Tokens with encrypted storage.

* Tokens are account-scoped
* Full audit log in Netlify dashboard
* Never stored in config files

---

## Best Practices

* Use separate tokens for CI/CD and personal access
* Enable branch deploys for staging environments
* Use `--dry-run` to preview changes
* Store secrets in Netlify’s environment variables (not in code)
* Use `netlify.toml` for advanced configuration (redirects, headers, functions)