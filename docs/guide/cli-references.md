
CloudCTL provides a consistent, intuitive interface across all providers. Every command follows the same pattern, whether you're deploying a VM on AWS, launching a GPU pod on RunPod, or spinning up a PostgreSQL database on Neon.

This reference documents every command, option, and workflow.

---

## `cloudctl deploy`

The `deploy` command creates new resources across cloud, AI, database, and frontend providers.

```bash
cloudctl deploy <resource-type> <model> [OPTIONS]
```

Arguments:

| Argument      | Description                                                                                                          |
| ------------- | -------------------------------------------------------------------------------------------------------------------- |
| resource-type | Type of resource to deploy. Supported: vm, ai, database, k8s, frontend, storage, worker, cdn, load-balancer, domain. |
| model         | Image, template, or model name (e.g., ubuntu-22.04, llama3, postgres:14, vercel/node-template).                      |

Options:

| Option       | Description                             | Example                                           |
| ------------ | --------------------------------------- | ------------------------------------------------- |
| --provider   | Target provider (required)              | --provider digitalocean                           |
| --region     | Geographic region                       | --region us-east-1                                |
| --zone       | Availability zone                       | --zone us-east-1a                                 |
| --name       | Custom name for the resource            | --name my-api-server                              |
| --type       | Instance or plan type                   | --type t3.micro                                   |
| --env        | Environment variables (can be repeated) | --env DATABASE_URL=... --env LOG_LEVEL=debug      |
| --repo       | GitHub/GitLab repository URL            | --repo https://github.com/user/app                |
| --branch     | Git branch to deploy                    | --branch main                                     |
| --plan       | Service plan (where applicable)         | --plan free                                       |
| --size       | Storage size in GB                      | --size 100                                        |
| --node_pools | JSON for Kubernetes node pools          | '--node_pools [{"type":"s-1vcpu-2gb","count":2}]' |
| --dry-run    | Preview only — no real changes         | --dry-run                                         |
| --verbose    | Verbose output                          | --verbose                                         |

Examples:

```bash
# Deploy a VM on AWS
cloudctl deploy vm ubuntu \
  --provider aws \
  --region us-west-2 \
  --type t3.micro \
  --name web-server

# Deploy a GPU pod on RunPod
cloudctl deploy ai llama3 \
  --provider runpod \
  --gpu_type "NVIDIA RTX A5000" \
  --template pytorch:latest

# Deploy a PostgreSQL database on Neon
cloudctl deploy database analytics-db \
  --provider neon \
  --engine postgres \
  --region aws-us-east-2

# Deploy a frontend on Vercel from GitHub
cloudctl deploy frontend docs-site \
  --provider vercel \
  --repo https://github.com/user/docs \
  --branch main
```

---

## `cloudctl list`

List existing resources of a given type.

```bash
cloudctl list <resource-type> --provider <name> [FILTERS]
```

Arguments:

| Argument      | Description                                   |
| ------------- | --------------------------------------------- |
| resource-type | Type of resource to list (same as ``deploy``) |
| --provider    | Provider to query                             |

Filters:
Use ``--filter key=value`` to filter results (provider-specific).

```bash
cloudctl list vm --provider digitalocean --filter region=nyc1
cloudctl list database --provider aiven --filter engine=postgresql
```

Output
Returns a table with:

* ID
* Name
* Status
* Region
* Type/Plan
* Additional metadata

---

## `cloudctl destroy`

Safely delete a resource.

```bash
cloudctl destroy <resource-id> --provider <name>
```

Arguments:

| Argument    | Description                               |
| ----------- | ----------------------------------------- |
| resource-id | Unique ID of the resource (from ``list``) |
| --provider  | Provider where the resource lives         |

Safety:

* Supports `--dry-run` to preview deletion
* Irreversible — use with care

Example:

```bash
cloudctl destroy 123456789 --provider digitalocean
```

## `cloudctl cost` (Experimental)

Get real-time cost estimates for deployed resources.

```bash
cloudctl cost
```

Output:

| Provider     | Resource   | Type        | Monthly Cost (USD) |
| ------------ | ---------- | ----------- | ------------------ |
| digitalocean | web-vm     | s-1vcpu-1gb | $5.00              |
| runpod       | ai-pod     | RTX A5000   | $432.00            |
| neon         | db-cluster | pro         | $25.00             |

Costs are estimated based on provider pricing models and updated on every `deploy` or `list`.

---

## `cloudctl providers`

Show all available providers and their capabilities.

```bash
cloudctl providers
```

Output:

```bash
Provider     Supported Types
aws          vm, database, k8s, storage, load-balancer, cdn
gcp          vm, k8s, database, storage, serverless, ai
digitalocean vm, k8s, database, storage, load-balancer, cdn
runpod       ai, serverless, storage, worker
neon         database, branch, endpoint
...
```

Use this to discover what each provider supports.

---

## `cloudctl secrets`

Securely store API keys and tokens using the system keyring.

```bash
cloudctl secrets <provider> <key>
```

Arguments:

| Provider | Description                                    |
| -------- | ---------------------------------------------- |
| provider | Provider name (e.g. aws, runpod)               |
| key      | Credential key (e.g. api_token, access_key_id) |

Example:

```bash
cloudctl secrets digitalocean api_token
cloudctl secrets aws access_key_id
cloudctl secrets aws secret_access_key
```

You’ll be prompted to enter the value securely. It is encrypted and never logged.

---

## `cloudctl plugins`

Install and manage custom providers.

```bash
cloudctl plugins install <path>
cloudctl plugins list
cloudctl plugins remove <name>
```

Example:

```bash
cloudctl plugins install ./my-custom-provider.py
```

Plugins must define a `Provider` subclass and register via `register_provider()`

---

## `cloudctl notify`

Trigger test notifications for Slack or Discord.

```bash
cloudctl notify "Test message" --status info
cloudctl notify "Deployment failed" --status error
```

Useful for testing integrations.

---

## Global Options

| Option        | Description                            |
| ------------- | -------------------------------------- |
| --dry-run     | Preview actions without making changes |
| --verbose     | Show detailed logs                     |
| --config PATH | Use custom config file                 |
| --help        | Show help for any command              |
| --version     | Show CLI version                       |

Every command is designed to be discoverable, predictable, and scriptable.
