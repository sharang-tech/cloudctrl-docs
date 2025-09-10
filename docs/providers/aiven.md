# Aiven

Aiven delivers fully managed open source data infrastructure, including PostgreSQL, Kafka, Redis, ClickHouse, InfluxDB, and Grafana. With CloudCTL, you can deploy and manage these services across multiple clouds and regions—all from a single, consistent CLI.

No more juggling dashboards or writing custom API scripts. CloudCTL unifies Aiven’s powerful data platform under one command language, with full support for `deploy`, `list`, `destroy`, cost estimation, and `--dry-run` safety.

---

## 🔧 Supported Resource Types

CloudCTL supports the following Aiven services:

| Resource Type     | Maps To                  | Description                                                   |
| ----------------- | ------------------------ | ------------------------------------------------------------- |
| `database`      | PostgreSQL, MySQL, Redis | Fully managed relational and in-memory databases              |
| `streaming`     | Apache Kafka             | Scalable, managed event streaming platform                    |
| `analytics`     | ClickHouse, InfluxDB     | High-performance analytics and time-series databases          |
| `observability` | Grafana                  | Managed observability dashboards with data source integration |

All resources support full lifecycle operations—creation, listing, and deletion—with real-time cost tracking and dry-run previews.

---

## 🚀 Quick Start

Get started with Aiven in three steps.

### 1. Generate an Authentication Token

1. Log in to the [Aiven Console](https://console.aiven.io)
2. Go to **Account Settings > Authentication Tokens**
3. Click **Create New Token**
4. Assign the token to your project and grant **Admin** or **Developer** role
5. Copy the token (it will only be shown once)

### 2. Save Credentials Securely

```bash
cloudctl secrets aiven auth_token
```

You’ll be prompted to enter the token. It is stored encrypted in your system keyring—never in plaintext.

### 3. Set Project Name

Edit `~/.cloudctl/config.yaml`:

```YAML
providers:
  aiven:
    enabled: true
    project: your-aiven-project-name
```

The project name is required for all Aiven API calls.

---

## Example Commands

### Deploy a PostgreSQL Database

```bash
cloudctl deploy database analytics-db \
  --provider aiven \
  --engine pg \
  --plan startup-4 \
  --region google-europe-west3 \
  --name analytics-cluster
```

* Auto-configures connection pool and SSL
* Enables private networking (if enabled in project)
* Returns connection URI and status

### Launch a Kafka Cluster

```bash
cloudctl deploy streaming events-stream \
  --provider aiven \
  --plan startup-4 \
  --region aws-us-east-1 \
  --name kafka-cluster
```

* Sets up Kafka with Schema Registry and Kafkacat access
* Returns bootstrap server address and access credentials
* Supports SASL/SSL and ACLs

### Deploy a ClickHouse Analytics Database

```bash
cloudctl deploy analytics clickhouse-db \
  --provider aiven \
  --engine clickhouse \
  --plan startup-4 \
  --region google-us-central1 \
  --name ch-analytics
```

* Ideal for real-time analytics and large datasets
* Returns HTTP and native endpoints
* Supports replication and distributed tables

### Create a Grafana Dashboard

```bash
cloudctl deploy observability monitoring-dash \
  --provider aiven \
  --plan startup-4 \
  --region google-europe-west3 \
  --name grafana-dash
```

* Auto-provisions a managed Grafana instance
* Integrates with other Aiven services (PostgreSQL, InfluxDB)
* Returns dashboard URL and admin credentials

### Deploy a Redis Instance

```bash
cloudctl deploy database cache-store \
  --provider aiven \
  --engine redis \
  --plan startup-4 \
  --region aws-us-west-2 \
  --name redis-cache
```

* Persistent or cache-optimized configurations
* Returns connection endpoint and port
* Supports Redis 6+ with TLS

---

## List Resources

List any deployed Aiven services:

```bash
# List all databases
cloudctl list database --provider aiven

# List Kafka clusters
cloudctl list streaming --provider aiven

# List ClickHouse/InfluxDB instances
cloudctl list analytics --provider aiven

# List Grafana dashboards
cloudctl list observability --provider aiven
```

Output includes:

* ID (Service Name)
* Name
* Engine
* Plan
* Region
* State (running, creating, etc.)

---

## Destroy Resources

Safely terminate services:

```bash
cloudctl destroy analytics-cluster --provider aiven
cloudctl destroy kafka-cluster --provider aiven
cloudctl destroy grafana-dash --provider aiven
```

> ⚠️ Warning: Deletion is irreversible and results in data loss. Use --dry-run first.

### Dry-Run Mode

Preview changes safely:

```bash
cloudctl destroy analytics-cluster --provider aiven --dry-run
```

---

## Cost Estimation

CloudCTL estimates monthly costs based on Aiven’s pricing tiers:

```bash
cloudctl cost --provider aiven
```

Output:

```bash
Provider: aiven
Resource: analytics-cluster (database)
Engine: pg
Plan: startup-4
Estimated Monthly Cost: $30.00 USD

Resource: kafka-cluster (streaming)
Plan: startup-4
Estimated Monthly Cost: $50.00 USD
```

Estimates include compute, storage, and network transfer.

---

## Authentication & Security

CloudCTL uses Aiven Authentication Tokens with encrypted storage.

* Tokens are scoped to your project and team
* Full audit log in Aiven Console
* Never stored in config files

---

## Best Practices

* Use separate tokens for CI/CD and personal access
* Enable VPC peering for private connectivity
* Use `--dry-run` before production changes
* Monitor resource usage via Aiven Console
* Backup critical data before deletion
