# Plugin Development Guide

CloudCTL is designed to be **extensible**. With the plugin system, you can add support for **any infrastructure provider, AI service, or internal tool**—without modifying the core codebase.

This guide walks you through creating a custom plugin from scratch, registering it with CloudCTL, and testing it locally.

---

## What Is a Plugin?

A CloudCTL plugin is a **Python module** that implements the `Provider` interface. It defines:

- How to authenticate
- What resource types it supports (`vm`, `ai`, `database`, etc.)
- How to `deploy`, `list`, and `destroy` resources
- How to estimate costs

Once installed, your plugin integrates seamlessly into the CLI:

```bash
cloudctl deploy vm my-instance --provider my-plugin
```

---

## Prerequisites

* Python 3.9+
* CloudCTL installed (`pip install cloudctl` or `cloudctl-windows-installer.msi` or `sudo apt install ./cloudctl-linux.deb`)
* Basic knowledge of HTTP APIs and Python classes

---

## Step 1: Create Your Plugin Module

Create a new Python file, e.g., my_custom_provider.py.

```py

"""
my_custom_provider.py
Example custom provider plugin for CloudCTL.
"""
from typing import Dict, Any, List
import httpx

from cloudctl.core.provider import Provider
from cloudctl.credentials import get_credential
from cloudctl.console import console


class MyCustomProvider(Provider):
    API_BASE = "https://api.example.com/v1"

    def __init__(self, config: Dict[str, Any], dry_run: bool = False):
        super().__init__(config, dry_run)
        # Load credentials securely
        self.api_key = get_credential("my-plugin", "api_key")
        if not dry_run and not self.api_key:
            raise ValueError("API key not found. Run: cloudctl secrets my-plugin api_key")
        self.headers = {
            "Authorization": f"Bearer {self.api_key}",
            "Content-Type": "application/json"
        }

    def deploy(self, resource_type: str, **kwargs) -> Dict[str, Any]:
        if self.dry_run:
            return self._preview(
                action="deploy",
                resource_type=resource_type,
                provider="my-plugin",
                **kwargs
            )

        if resource_type == "vm":
            return self._deploy_vm(**kwargs)
        else:
            raise ValueError(f"MyCustomProvider does not support '{resource_type}'")

    def _deploy_vm(self, **kwargs) -> Dict[str, Any]:
        name = kwargs.get("name", "cloudctl-vm")
        region = kwargs.get("region", "us-west")
        machine_type = kwargs.get("type", "small")

        payload = {
            "name": name,
            "region": region,
            "type": machine_type
        }

        with httpx.Client() as client:
            resp = client.post(f"{self.API_BASE}/vms", json=payload, headers=self.headers)
            resp.raise_for_status()
            result = resp.json()

        return {
            "id": result["id"],
            "type": "vm",
            "name": result["name"],
            "status": "created",
            "region": region
        }

    def list(self, resource_type: str, **filters) -> List[Dict[str, Any]]:
        if self.dry_run:
            return [self._preview(action="list", resource_type=resource_type, filters=filters)]

        if resource_type != "vm":
            return []

        with httpx.Client() as client:
            resp = client.get(f"{self.API_BASE}/vms", headers=self.headers)
            resp.raise_for_status()
            vms = resp.json().get("vms", [])

        return [
            {
                "id": vm["id"],
                "name": vm["name"],
                "status": vm["status"],
                "region": vm["region"]
            }
            for vm in vms
            if isinstance(vm, dict)
        ]

    def destroy(self, resource_id: str) -> Dict[str, Any]:
        if self.dry_run:
            return self._preview(action="destroy", resource_id=resource_id)

        with httpx.Client() as client:
            resp = client.delete(f"{self.API_BASE}/vms/{resource_id}", headers=self.headers)
            if resp.status_code == 404:
                return {"status": "not_found", "id": resource_id}
            resp.raise_for_status()

        return {"status": "deleted", "id": resource_id}

    def estimate_cost(self, resource_type: str, meta: Dict[str, Any]) -> Dict[str, Any]:
        hourly = 0.05  # $0.05/hour for small VM
        monthly = hourly * 24 * 30
        return {
            "monthly": round(monthly, 2),
            "currency": "USD",
            "details": {"hourly": hourly, "type": meta.get("type", "small")}
        }

    def _preview(self, action: str, **details) -> Dict[str, Any]:
        return {
            "status": "preview",
            "action": action,
            "provider": "my-plugin",
            "note": "This is a dry-run. No changes were made.",
            **details
        }


# Register the provider
from cloudctl.core.providers import register_provider
register_provider("my-plugin", MyCustomProvider)

```

---

## Step 2: Test Your Plugin

Save the file and test it locally:

```bash
# Save API key securely
cloudctl secrets my-plugin api_key

# Deploy a VM (dry-run first)
cloudctl deploy vm test-vm --provider my-plugin --dry-run

# Deploy for real
cloudctl deploy vm test-vm --provider my-plugin --region us-west
```

---

## Step 3: Install the Plugin

Install your plugin so CloudCTL can discover it:

```bash
# Option 1: Install from local file
cloudctl plugins install ./my_custom_provider.py

# Option 2: Install from URL
cloudctl plugins install https://raw.githubusercontent.com/yourorg/plugins/main/my_custom_provider.py
```

List installed plugins:

```bash
cloudctl plugins list
```

---

## Step 4: Uninstall (If Needed)

```bash
cloudctl plugins remove my-plugin
```

---

## Best Practices

* Use `.get()` for API responses — Avoid Pylance errors with `item.get("id")` instead of `item["id"]`
* Handle errors gracefully — Wrap API calls in `try/except`
* Support `--dry-run` — Always return a preview
* Use secure credential storage — Never hardcode keys
* Follow naming conventions — Use lowercase, hyphenated provider names (e.g., `my-plugin`)
* Add logging — Use `console.info()` and `console.warn()` for visibility.

---

## Example Use Cases

| Use case              | Description                                       |
| --------------------- | ------------------------------------------------- |
| Internal AWS-like API | Wrap your company’s internal provisioning system |
| On-Prem Kubernetes    | Manage K8s clusters via REST                      |
| Proprietary AI API    | Integrate a private LLM endpoint                  |
| Legacy Cloud Provider | Add support for a deprecated or niche platform    |

---

## Next Steps

Now that you’ve built a custom plugin:

* Share it with your team
* Publish it in a private repo
* Contribute to the [CloudCTL Plugin Registry](https://github.com/sharang-tech/cloudctrl-plugins)

Or explore:

* [CLI Reference](./cli-references.md)
* [Integrations](../integrations/index.md)
* [Changelog](../changelog.md)

Extend CloudCTL. Own your infrastructure. Build the future.
