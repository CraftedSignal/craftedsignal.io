---
title: "Air-gapped Mode"
description: "Run the platform with all outbound network access blocked, including DNS. For regulated and isolated environments where the platform must not reach the internet."
weight: 12
section: "Operations"
---

## Overview

Air-gapped mode blocks every outbound network call the platform could make: DNS lookups, HTTP requests, TLS dials. Only loopback and private address space (RFC1918, ULA, link-local) are permitted. Once enabled, the platform can reach your internal SIEMs, internal LLM (Ollama, self-hosted compatible endpoint), and internal threat feed mirror — and nothing else.

Air-gapped mode is for customer-operated environments. It is different from secured SaaS or sovereign GCP: Google-managed services and GitHub Actions still require cloud control-plane access. For a true disconnected deployment, run the single binary or a customer-managed Kubernetes deployment inside the isolated network.

---

## Enabling it

Pass `--airgapped` on the command line:

```bash
craftedsignal --config /etc/craftedsignal/config.yml --airgapped
```

On startup the log emits:

```
WARN airgap mode enabled — DNS and public outbound are blocked; use IP literals for SIEM/AI/feed endpoints
```

---

## What the mode enforces

- **DNS lookups** fail with `airgap: outbound network access blocked`. Every destination in your configuration must be an IP literal — not a hostname.
- **HTTP clients using `http.DefaultTransport`** have their dialer rewired to reject anything outside loopback / RFC1918 / ULA / link-local.
- **Dials to `0.0.0.0/0` public space** are refused regardless of transport.

Loopback and private space are allowed because an air-gapped operator is expected to reach internal services over private IPs.

---

## Configuration checklist

Before enabling the flag:

- SIEM endpoints must be IPs: `https://10.20.30.40:8089` instead of `https://splunk.internal`.
- AI provider must be internal: `OLLAMA_HOST=http://10.20.30.50:11434`.
- Threat feed must be an internal mirror. Bundles can be uploaded manually via the dashboard or `csctl feed import`.
- OIDC issuer (if used) must point at an internal IdP on private IP.
- SMTP, webhook destinations and backup remotes must resolve to internal endpoints only.
- `security.master_secret` must come from the customer's protected secret-management process. Treat it like a root key: back it up, restrict access and test restore before production.
- NTP, TLS trust roots, and any other system services are out of scope for the flag — configure the host accordingly.

---

## Key management

In air-gapped deployments, application-level encryption still uses per-company tenant keys for credentials and sensitive settings. Those tenant keys are wrapped by key material derived from `security.master_secret`.

Operational requirements:

- Store `security.master_secret` outside the application database and outside the Git repository.
- Back up the master secret with the database; losing either one makes encrypted credentials unrecoverable.
- Use the customer's vault, HSM-backed secret system or sealed-secret process to inject the value at runtime when available.
- Rotate the master secret only with a planned re-wrap operation for tenant keys.
- Restrict shell, backup and config-file access because anyone with the database and master secret can decrypt protected tenant data.

GCP KMS, Confidential Space and Confidential GKE Nodes are cloud controls for connected or sovereign GCP deployments. They are not required for a fully disconnected binary deployment.

---

## Artifact and update flow

Disconnected environments need an internal release process:

1. Download release artifacts, checksums, signatures and SBOMs in a connected staging environment.
2. Verify them before importing into the isolated network.
3. Mirror container images into an internal registry or copy the binary package through the approved media process.
4. Pin deployments by immutable digest or verified binary checksum.
5. Keep the previous release available for rollback.

For threat intelligence and rule content, use an internal feed mirror or manual bundle upload. Do not configure the platform to fetch public repositories directly.

---

## Operational implications

- Update bundles (rule content, threat feed) must be copied in manually.
- `csctl` will refuse outbound calls too when the environment sets `CRAFTEDSIGNAL_AIRGAPPED=1`.
- Any library or SDK that builds its own raw TCP socket (bypassing `http.DefaultTransport`) is out of scope. Pair the flag with an OS-level network namespace or outbound-deny firewall for defence in depth.
- Monitoring, alerting and backups must use internal systems. Export audit logs to the customer SIEM or GRC archive through internal collectors.

---

## Related

- [Deployment](/docs/deployment/) — single-binary on-prem install guide.
- [Cloud Sovereignty](/docs/cloud-sovereignty/) — secured SaaS, customer KMS, private cloud and air-gapped operating models.
- [Threat Feed](/docs/threat-feed/) — manual bundle upload.
- [AI](/docs/ai/) — self-hosted Ollama configuration.
