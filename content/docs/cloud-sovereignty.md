---
title: "Cloud Sovereignty"
description: "Cloud sovereignty options for CraftedSignal: secured SaaS, customer-controlled keys, Confidential Space attestation, encrypted memory, private cloud and fully air-gapped deployments."
weight: 14
section: "Security"
---

## Overview

CraftedSignal is a detection-governance control plane. It manages detection rules, tests, approvals, evidence, health metadata and audit history; it does not require customer logs to move into CraftedSignal. That architecture gives teams several sovereignty models without changing how detection work is governed.

---

## Operating models

| Model | Infrastructure owner | Key control | Network boundary |
|-------|----------------------|-------------|------------------|
| **Secured SaaS** | CraftedSignal | CraftedSignal-managed KMS/CMEK and application key hierarchy | Hardened GCP runtime with private GKE, private Cloud SQL and controlled ingress |
| **Sovereign SaaS** | CraftedSignal plus customer key project | Customer-governed KEK with attested key access | Managed runtime with customer-controlled key-release policy |
| **Private cloud** | Customer | Customer KMS, HSM or secret-management process | Customer VPC/VNet, private ingress and internal egress controls |
| **Air-gapped** | Customer | Local master secret or customer-managed root secret | No public outbound access; internal IP endpoints and manual bundle workflows |

---

## Secured SaaS controls

The managed SaaS deployment is built as a private application substrate:

- Private GKE runtime and private Cloud SQL, with no public database endpoint.
- Regional Cloud KMS/CMEK for managed infrastructure encryption.
- A dedicated platform KEK path for application-level tenant data-encryption key wrapping.
- Key-broker identity for KEK use, with direct runtime service-account KEK access kept opt-in.
- Cloud Armor and rate controls at the public edge.
- Digest-pinned images, signed releases and Binary Authorization attestations.
- Manual GitHub Actions deployment workflows with environment approval gates during the current deployment phase.
- Independent penetration testing and major-change security reviews, with findings tracked through remediation.
- Application audit logs for deploys, approvals, AI actions, settings changes and user administration.

Your SIEM logs and telemetry stay in your SIEM. CraftedSignal stores the control-plane metadata needed to manage detections and evidence.

---

## Application-level encryption

CraftedSignal protects sensitive tenant data with an application-level key hierarchy:

1. Tenant data is encrypted with tenant data-encryption keys.
2. Tenant data-encryption keys are wrapped by higher-level key material.
3. In GCP-backed deployments, the infrastructure includes a dedicated `platform-kek` Cloud KMS key for that wrapping layer.
4. A key-broker identity is the default holder of KEK permissions; app and worker service accounts do not receive direct KEK permissions unless explicitly configured.

This separates database access from cryptographic access. Database access alone should not be enough to decrypt tenant credentials or sensitive settings.

---

## Customer-controlled keys

For customers that require root key control, the deployment can use customer-governed KMS keys. The key grant should be narrow and tied to the expected workload identity.

In sovereign GCP deployments, Confidential Space Workload Identity Federation can bind key access to workload evidence:

- The workload must attest as `CONFIDENTIAL_SPACE`.
- The Confidential Space support attribute must be stable.
- The workload must run in the expected GCP project.
- The workload must use the expected service account.
- Production policies should also restrict approved image digests or image-signing key fingerprints.

This means a customer-owned KMS key can grant decrypt or encrypt/decrypt access to an attested workload identity instead of a broad human or CI identity.

---

## Encrypted memory

Confidential Space and encrypted memory solve related but different problems:

- **Confidential Space** gates access to confidential resources, such as KMS keys, based on attested workload identity and image evidence.
- **Confidential GKE Nodes** and **Confidential VMs** protect workload memory for supported GCP runtimes by encrypting data while it is in use.

Use Confidential Space when the policy question is "which exact workload is allowed to use this key?" Use Confidential GKE Nodes or Confidential VMs when the policy question is "is workload memory encrypted during processing?" Use both when both conditions matter.

---

## Private cloud

In a private-cloud deployment, the customer operates the infrastructure boundary. Common requirements:

- Private ingress behind the customer's load balancer, WAF and IdP.
- PostgreSQL or SQLite in the customer environment.
- Customer KMS, HSM or secret manager providing the runtime master secret and any external key wrapping.
- Internal SMTP, OIDC, SIEM and AI endpoints.
- Internal artifact registry with digest-pinned releases.
- Change evidence retained in the customer's deployment and ticketing systems.

---

## Air-gapped operation

Air-gapped mode blocks public outbound network access in the application layer and requires internal endpoints:

- SIEM, OIDC, SMTP, AI and feed mirrors must be reachable through loopback or private IP space.
- Public DNS lookups are blocked by the application mode; use private IPs or infrastructure DNS that never leaves the isolated network.
- AI should run locally through an OpenAI-compatible endpoint such as Ollama, or be disabled.
- Threat-feed and rule bundles are copied in through an internal mirror or manual import.
- OS firewalls, Kubernetes NetworkPolicy, network namespaces or egress gateways should enforce the same deny-by-default policy below the application.

See [Air-gapped Mode](/docs/airgapped/) for the command-line flag, configuration checklist and operational constraints.

---

## Terraform references

The public GCP Terraform package documents the infrastructure side of this model:

- Private GCP substrate and CMEK controls.
- Customer KMS adoption.
- Binary Authorization with manual deployment attestations.
- Confidential Space Workload Identity Federation.
- Confidential GKE Nodes for encrypted memory.

Use those docs when planning a sovereign SaaS, private GCP or customer-key deployment.
