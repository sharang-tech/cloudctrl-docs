Welcome to the CloudCTL Quickstart. In this guide, you’ll:

1. ✅ Install the CLI
2. ✅ Configure a provider (DigitalOcean)
3. ✅ Deploy a VM
4. ✅ List and destroy it
5. ✅ Preview changes safely with `--dry-run`

By the end, you’ll understand the core workflow of CloudCTL and be ready to manage any provider.

---

## Step 1: Install CloudCTRL

CloudCTL is distributed as a standalone binary with native installers for Windows and Linux. No Python required.

### Windows

Download from our [github](http://github.com/sharang-tech/cloudctrl/releases "Download CloudCTRL") and run the `.msi` installer:

```bash
cloudctl-windows-installer.msi
```

### Linux

Download from our [github](http://github.com/sharang-tech/cloudctrl/releases "Download CloudCTRL") and Install the `.deb` package:

```bash
sudo apt install ./cloudctl-linux.deb
```

Open Command Prompt or PowerShell and verify:

```bash
cloudctl --version
```

## Step 2: Configure a Provider

Let’s use DigitalOcean as our first provider. CloudCTL securely stores credentials using your system’s keyring—never in plaintext.

1. Go to [DigitalOcean API Settings](https://cloud.digitalocean.com/account/api/tokens)
2. Click "Generate New Token"
3. Give it a name (e.g., cloudctl-token) and select Read and Write
4. Copy the token

```bash
cloudctl secrets digitalocean api_token
```

> 💡 You’ll be prompted to enter the token securely.

That’s it. Your credential is now encrypted and ready for use.

## Step 3: Deploy a VM

Now deploy your first resource: a **Droplet (VM)** in New York.

```bash
cloudctl deploy vm ubuntu \
  --provider digitalocean \
  --region nyc1 \
  --type s-1vcpu-1gb \
  --name my-first-vm
```

What This Command Does:

* ``deploy vm ubuntu`` — Deploys a VM using the Ubuntu 22.04 image
* ``--provider digitalocean`` — Targets DigitalOcean
* ``--region nyc1`` — Deploys in New York
* ``--type s-1vcpu-1gb`` — Uses the $5/month basic plan
* ``--name my-first-vm`` — Assigns a human-readable name

Within seconds, DigitalOcean provisions your Droplet. CloudCTL returns:

```json
{
  "id": "123456789",
  "name": "my-first-vm",
  "status": "active",
  "ipv4": "159.89.123.45",
  "url": "https://cloud.digitalocean.com/droplets/123456789"
}
```

## Step 4: List Resources

See all your VMs on DigitalOcean:

```bash
cloudctl list vm --provider digitalocean
```

Output:

```bash
ID              NAME           STATUS  REGION  TYPE
123456789       my-first-vm    active  nyc1    s-1vcpu-1gb
```

You can list other types too:

```bash
cloudctl list k8s --provider digitalocean
cloudctl list database --provider digitalocean
```

## Step 5: Destroy the VM

When you’re done, clean up:

```bash
cloudctl destroy 123456789 --provider digitalocean
```

Confirm the action, and the Droplet is permanently deleted.

> ⚠️ Warning: This is irreversible. Use --dry-run first if unsure.

## Step 6: Use Dry-Run Mode (Safety First)

Before making real changes, preview them:

```bash
cloudctl deploy vm ubuntu \
  --provider digitalocean \
  --region sfo3 \
  --type s-2vcpu-2gb \
  --dry-run
```

Output:

```json
{
  "status": "preview",
  "action": "deploy",
  "provider": "digitalocean",
  "resource_type": "vm",
  "model": "ubuntu",
  "region": "sfo3",
  "type": "s-2vcpu-2gb",
  "note": "This is a dry-run. No changes were made."
}
```

No VM is created. No cost incurred. Just a safe preview.

This is infrastructure safety by design.

## Next Steps

You’ve just:

* ✅ Installed CloudCTL
* ✅ Configured a provider
* ✅ Deployed, listed, and destroyed a VM
* ✅ Used --dry-run for safety
