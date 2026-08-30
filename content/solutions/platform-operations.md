---
title: "Platform Operations"
description: "Run CraftedSignal as SaaS, self-hosted, or air-gapped with hardened GCP infrastructure, multi-tenant controls, APIs, CLI, SSO, and optional local generation."
weight: 60
stage: "06"
eyebrow: "Operate"
nav_summary: "Deploy the control plane where your SOC needs it."
hero_image: "/screenshots/rule-editor.png"
hero_alt: "CraftedSignal rule editor showing multi-platform detection logic"
quick_points:
  - "SaaS on private GCP infrastructure, single-binary self-hosted, worker split, and air-gapped deployment options."
  - "Multi-tenant and white-label controls for MSSP and regulated environments."
  - "REST API, Go SDK, csctl, SSO, passkey MFA, RBAC, audit logs, KMS-backed controls, and optional local models."
outcomes:
  - label: "Deployment"
    title: "Start SaaS, move later"
    body: "The same product model supports hosted use, self-hosting, and isolated environments."
  - label: "Scale"
    title: "Tenant-aware operations"
    body: "MSSPs can separate customer data, branding, feature toggles, and approval policy."
  - label: "Control"
    title: "Generation stays governed"
    body: "Drafted rules, tests, runbooks and playbooks stay reviewable. Humans approve, and local models are supported."
docs:
  - title: "Deployment Guide"
    url: "/docs/deployment/"
    description: "Supported platforms, deployment state, rollback, and operational behavior."
  - title: "Air-gapped Mode"
    url: "/docs/airgapped/"
    description: "Outbound restrictions, private address rules, and isolated operation."
  - title: "Generation Assistance"
    url: "/docs/ai/"
    description: "Optional generation with local Ollama support, audit trails, and human approvals."
  - title: "Runbooks & Playbooks"
    url: "/docs/runbooks-playbooks/"
    description: "Reviewable response steps for rules, hunts, library entries, and threat briefs."
  - title: "API Reference"
    url: "/docs/api/"
    description: "REST API for CI/CD, automation, dashboards, testing, and deployment."
  - title: "Targets & Mappings"
    url: "/docs/targets-mappings/"
    description: "SIEM targets, read-only mode, field mappings, log source mappings, and suggestions."
  - title: "Generation Quality"
    url: "/docs/ai-quality/"
    description: "Generation operation metrics, retries, quality trends, errors, and improvement proposals."
  - title: "Security"
    url: "/docs/security/"
    description: "Data boundaries, cloud sovereignty, encryption, credentials, audit logs, SSO, and supply chain."
  - title: "Roles & Permissions"
    url: "/docs/roles-permissions/"
    description: "RBAC, separation of duties, instance claiming, SSO, and passkey MFA."
---

## The problem

Detection engineering has to run in very different environments. Some teams want SaaS because they need speed. Some need self-hosting because their SIEMs, credentials, or policies require it. Some regulated customers need air-gapped operation. MSSPs need multiple tenants, customer-specific toggles, and sometimes white-label surfaces.

If the deployment model is too rigid, the detection program bends around the tool. CraftedSignal is designed so the operating model stays the same while the hosting model changes.

## Deployment models

SaaS is the fastest path: create a tenant, connect SIEMs, import rules, and start testing. Self-hosted deployments use a single binary with configuration, migrations, and local storage options. Larger environments can split server and worker roles so background jobs scale separately.

Managed GCP deployments use private GKE, private Cloud SQL, Cloud KMS/CMEK, Cloud Armor and digest-attested releases managed through Terraform and GitHub Actions. The goal is a control plane that is straightforward for security teams to inspect, operate and explain to procurement.

For cloud sovereignty, customers can stay on secured SaaS, use application-level encryption with customer-governed key access and Confidential Space attestation, or run the platform on-premises or in a private cloud. Supported confidential-computing deployments can add encrypted memory for data in use without changing the detection governance model.

Air-gapped mode blocks outbound network access except private and loopback targets. That lets the platform reach internal SIEMs, internal mail, internal model endpoints, or an internal feed mirror without unexpected internet dials.

## Multi-tenant and white-label operation

MSSP and MDR teams can keep customer data separated by tenant. Feature toggles can differ by customer: generation on for one, off for another; threat feed enabled for one, mirrored manually for another; different approval policy by severity or customer contract.

White-label branding supports customer-facing surfaces without changing the underlying workflow. The important part is that rules, risks, hunts, runbooks, playbooks, approvals, and audit trails stay tenant-scoped.

## APIs, CLI, SDK, and automation

CraftedSignal exposes a web UI for operators, `csctl` for detections-as-code workflows, a REST API for automation, and a Go SDK for deeper integration. Teams can wire validation into CI, sync rules to Git, build reporting, or connect existing ticket and workflow systems.

## Generation on your terms

CraftedSignal can generate, refine, translate and triage detection content, including runbooks and playbooks for rule and hunt response. Existing response steps can be used when refining a rule, and rule changes can mark those response steps stale for review. Generated suggestions do not auto-deploy. They are reviewable and audit-logged. For privacy-sensitive deployments, local model support through Ollama keeps customer data inside your infrastructure, and generation can be disabled entirely when policy requires it.
