---
title: "Getting Started"
description: "Install CraftedSignal (SaaS or self-hosted), connect your SIEM, import rules, add business context, work the Backlog, and deploy your first tested rule in minutes with csctl."
weight: 1
section: "Getting Started"
---

## Choose your deployment

### SaaS (recommended)

Sign up at [app.craftedsignal.io](https://app.craftedsignal.io) and start using CraftedSignal immediately. No installation required.

The free tier includes all core features: rule editing, runbooks and playbooks, testing, deployment, rollback, MITRE coverage, Backlog, noise tracking, and optional generation assistance.

### Self-hosted

Download the single binary from [GitHub Releases](https://github.com/craftedsignal/releases) and run it on your infrastructure. CraftedSignal has no external database dependencies — SQLite is used by default as a database on the filesystem.

**System requirements:**

| Resource | Minimum |
|----------|---------|
| OS | Ubuntu 22.04+, Debian 12+, RHEL 8+, macOS |
| CPU | 2 cores |
| RAM | 4 GB |
| Disk | 20 GB |

---

## Configuration (self-hosted)

CraftedSignal uses a YAML config file with environment variable overrides:

```yaml
# config.yaml
http:
  port: 8080
  interface: "0.0.0.0"

storage:
  driver: sqlite
  properties:
    path: /data/craftedsignal.db

security:
  master_secret: "your-secret-here"  # Required — generate with: openssl rand -hex 32

temporal:
  embedded: true                     # Run Temporal in-process (no external dependency)
```

Start with:

```bash
./craftedsignal -config config.yaml
```

The `master_secret` is critical — it encrypts all sessions and stored SIEM credentials.

See the full [Configuration](/docs/configuration/) reference for production configs, AI, email, logging, and all available options.

---

## Quick start

### 1. Create your account

**SaaS**: Sign up at [app.craftedsignal.io](https://app.craftedsignal.io). The first user in a workspace automatically becomes the owner.

**Self-hosted**: Open `http://localhost:8080` and register. The first user automatically becomes the owner.

### 2. Connect a SIEM

Go to **Detection > Targets** and add your SIEM connection:

- **Splunk**: REST API endpoint + auth token
- **Microsoft Sentinel**: Workspace ID + app registration
- **CrowdStrike**: API client ID + secret
- **Rapid7 InsightIDR**: API key + region

All credentials are encrypted at rest with your master secret.

See [Targets & Mappings](/docs/targets-mappings/) for read-only targets, automatic sync, field mappings, log source mappings, and suggested overrides.

### 3. Import your current rules

Start by importing what already exists. CraftedSignal uses the import to understand your current detection surface, rule languages, ATT&CK mappings, telemetry assumptions and rule quality.

- **SIEM import**: Pull rules from Splunk, Sentinel, CrowdStrike or Rapid7.
- **Repository import**: Bring your existing detection-as-code repository.
- **Library**: Add standard rules and hunts from shared template repositories.
- **Threat feed**: Adopt rules from briefs that affect your environment.

Rules and hunts can include runbooks and playbooks. Imported content can bring those response steps with it, and sync keeps them attached to the rule as it moves between Git and the web UI.

### 4. Add business context

You do not need a complete CMDB to start. Add the context you have:

- Paste a free-text description of important applications, assets, cloud platforms and data.
- Add services and data assets manually.
- Upload a spreadsheet or repository export on a regular cadence.
- Connect CMDB, scanner, CTEM, CSPM or asset inventory sources later.

If you skip this step, CraftedSignal still derives relevance from SIEM targets, imported rules, observed log sources, threat metadata and service catalog presets. Adding context improves risk scores and reduces irrelevant work.

### 5. Work the Backlog

Go to **Risk > Backlog** to see risk-scored work across new detections, false-positive tuning, missing telemetry, mappings, tests, broken rules, approval work, findings and residual-risk decisions.

### 6. Test and deploy

In the web UI, run tests directly on your rule page. Tests execute against your actual SIEM.

Or use the CLI to push, test, and deploy:

```bash
# Validate rules locally
csctl validate

# Preview changes
csctl push -dry-run

# Push rules — tests run automatically
csctl push -deploy -m "Add webshell detection"
```

---

## CLI: csctl

The `csctl` CLI gives you Git-native workflows for detection engineering.

```bash
csctl init                                           # Create project structure
csctl validate                                       # Check rules offline
csctl diff                                           # Preview changes
csctl push -m "Add webshell detection"               # Push rules (tests run automatically)
csctl push -deploy                                   # Push and deploy to SIEM
csctl pull                                           # Pull latest from platform
csctl sync                                           # Bidirectional sync
csctl simulate list                                  # List available attack simulations
```

Configure authentication via environment variable:

```bash
export CSCTL_TOKEN="your-api-token"
export CSCTL_URL="https://app.craftedsignal.io"    # or your self-hosted URL
```

See the full [CLI Reference](/docs/cli/) for all commands, flags, rule format, and CI/CD examples.

See [Runbooks & Playbooks](/docs/runbooks-playbooks/) for response steps linked to rules, hunts, libraries, and threat briefs.

See [Groups](/docs/groups/) for deployable rule sets and [Approvals](/docs/approvals/) for review queues.

---

## Reverse proxy (self-hosted)

For production self-hosted deployments, put CraftedSignal behind a reverse proxy:

**Nginx:**
```nginx
server {
    listen 443 ssl;
    server_name craftedsignal.internal;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**Caddy:**
```
craftedsignal.internal {
    reverse_proxy localhost:8080
}
```

---

## Health check

```bash
curl http://localhost:8080/healthz
```

Returns `200 OK` when the service is ready to accept requests.
