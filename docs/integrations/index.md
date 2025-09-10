# Integrations

CloudCTL goes beyond infrastructure management—it connects your CLI workflows to the tools your team uses every day. With built-in integrations for **Slack** and **Discord**, you can receive real-time notifications whenever resources are deployed, destroyed, or scaled.

Whether you're working solo or as part of a team, these alerts ensure visibility, improve incident response, and make infrastructure changes transparent—without leaving your command line.

---

## Supported Integrations

CloudCTL supports the following notification platforms:

| Integration        | Trigger Events                     | Description                                                       |
| ------------------ | ---------------------------------- | ----------------------------------------------------------------- |
| Slack    | `deploy`, `destroy`, `error` | Send formatted messages to Slack channels using incoming webhooks |
| Discord | `deploy`, `destroy`, `error` | Post embed-style alerts to Discord servers via webhooks           |

All integrations support:

- ✅ **Dry-run previews** – See what the alert will look like before making changes
- ✅ **Per-environment configuration** – Use different channels for dev, staging, and production
- ✅ **Secure storage** – Webhook URLs are never stored in plaintext
- ✅ **Rich formatting** – Includes resource type, provider, status, cost estimate, and timestamp

---

## How It Works

Every time you run a `deploy` or `destroy` command, CloudCTL checks your configuration for active integrations. If enabled, it sends a structured notification containing:

- Action taken (`deploy`, `destroy`)
- Resource type and ID
- Provider and region
- Status and timestamp
- Estimated cost (when available)

In `--dry-run` mode, a preview notification is sent so you can verify formatting and routing.

Notifications are sent **asynchronously**, so they don’t slow down your deployments.

---

## Quick Setup

Integrations are configured declaratively in `~/.cloudctl/config.yaml`:

```yaml
notifications:
  work:
    enabled: true
    provider: slack
    webhook_url: https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX
    channels:
      - "#infra-alerts"
  gaming:
    enabled: true
    provider: discord
    webhook_url: https://discord.com/api/webhooks/XXXXXXXXXXXXXXXXXX/xxxxxxxxxxxxxxxxxxxx
    channels:
      - "deploy-logs"
```

Once configured, notifications are automatic on every relevant command.

---

## Best Practices

* Use separate channels for production vs development events
* Enable message threading (Slack) or categories (Discord) for organization
* Test with `--dry-run` before enabling in production
* Rotate webhook URLs periodically for security
