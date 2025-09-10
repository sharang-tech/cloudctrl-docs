# Changelog

This document tracks all notable changes to **CloudCTL**, including new features, provider integrations, breaking changes, and improvements. CloudCTL follows semantic versioning and is actively maintained to support the latest infrastructure platforms and developer workflows.

---

## v1.0.0 — 2025-04-05

The first stable release of CloudCTL, marking the culmination of a vision: **universal infrastructure control**.

### ✅ Features

- **25+ Providers Supported** — Full integration with AWS, GCP, DigitalOcean, Linode, Vultr, RunPod, Vast.ai, Modal, Aiven, Render, Fly.io, Koyeb, Railway, PlanetScale, Neon, Alibaba Cloud, Tencent Cloud, Huawei Cloud, Scaleway, Vercel, Netlify, Together.ai, Fireworks.ai, Fal.ai
- **Unified CLI Interface** — Consistent `deploy`, `list`, `destroy`, and `cost` commands across all providers
- **Dry-Run Safety** — Preview changes before applying with `--dry-run`
- **Cost Estimation Engine** — Real-time monthly cost estimates based on provider pricing
- **Secure Credential Management** — Encrypted storage via system keyring (`cloudctl secrets`)
- **Plugin System** — Extend CloudCTL with custom provider plugins
- **Installer Support** — Native `.msi` (Windows) and `.deb` (Linux) installers
- **Comprehensive Documentation** — MkDocs-based site with provider guides and integration references

### 🐞 Fixes

- Fixed Pylance type errors in Huawei Cloud and Alibaba Cloud providers
- Resolved HTTP request body handling across providers using `content=` instead of `data=`
- Improved error handling for providers without deletion APIs (e.g., PlanetScale, Railway)

### 📚 Docs

- Launched full documentation site with provider-specific guides
- Added quickstart, CLI reference, and integration pages

---

## v0.5.0 — 2025-03-15

A major milestone enabling AI/ML workflows and multi-cloud control.

### ✅ Features

- **AI Provider Expansion** — Added support for RunPod, Vast.ai, Modal, Together.ai, Fireworks.ai, and Fal.ai
- **MCP Server Integration** — Enabled AI agent control via Model Context Protocol
- **Global Providers** — Added Alibaba Cloud, Tencent Cloud, Huawei Cloud, and Scaleway
- **Frontend Hosting** — Integrated Vercel and Netlify for full-stack deployments
- **Notification System** — Introduced Slack and Discord integrations for real-time alerts

### 🛠️ Improvements

- Unified authentication model across all providers
- Enhanced cost estimation accuracy with per-engine pricing
- Improved dry-run output for better preview clarity

---

## v0.1.0 — 2025-01-10

Initial public release of CloudCTL with core functionality.

### ✅ Features

- **Core Cloud Support** — AWS, GCP, DigitalOcean, Linode, Vultr
- **Basic CLI Commands** — `deploy`, `list`, `destroy`
- **Secrets Management** — Secure credential storage using keyring
- **HTTP-Only Providers** — No SDKs required; all providers use direct API calls
- **Pylance-Clean Codebase** — Type-safe, no linter errors, full type hints

### 📄 Docs

- Launched initial documentation with quickstart and CLI reference

---

## What’s Next?

CloudCTL is under active development. Upcoming features include:

- **Terraform Bridge** — Import and manage existing Terraform state
- **Policy Engine** — Enforce tagging, region, and cost policies
- **CI/CD Plugins** — GitHub Actions, GitLab CI, and Jenkins integrations
- **Additional Providers** — Oracle Cloud, Azure, Upstash, Supabase
