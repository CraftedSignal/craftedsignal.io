---
title: "Backlog"
description: "A single risk-scored work queue for threats, coverage gaps, missing telemetry, rule tuning, broken rules, tests, mappings, approvals, findings, and residual-risk decisions."
weight: 10
section: "Features"
aliases:
  - /docs/recommendations/
---

## Overview

The Backlog is the operating queue for detection risk. It turns platform signals into concrete work so analysts, detection engineers, SOC managers, risk owners, CTI and CISOs can answer the same three questions:

- **Does this affect us?**
- **Can we see it?**
- **Are we covered?**

Backlog items are visible under **Risk > Backlog** and can also appear in the home action queue and runtime risk views.

---

## What creates Backlog items

Backlog items can come from:

- Relevant threat briefs, urgent CVEs, watchlist matches, and threat-model gaps.
- Pentest, red-team, purple-team and ethical-hack findings.
- Future CTEM, vulnerability scanner, BAS, CSPM and asset inventory inputs.
- Coverage gaps in ATT&CK, D3FEND, and modeled attack paths.
- Rule health and quality checks.
- Broken or unmapped translations.
- Analyst feedback and false-positive signals.
- Missing tests.
- Missing runbooks and playbooks.
- Missing telemetry and log sources.
- Target field or log source mapping gaps.
- Data health, connector health, approval and setup work.

Every item has a work type, scope, risk score, related rule or business object when available, description, reason, and a target URL that takes the user to the right place to act.

---

## Risk scoring

Each Backlog item carries a 0-100 runtime risk score. The score can change as the environment changes, for example when:

- A new threat brief or exploited CVE becomes relevant.
- A business service is marked critical.
- A rule goes silent, gets noisy, starts failing tests, or drifts in the SIEM.
- A log source disappears or a required mapping is missing.
- A risk owner escalates an item.
- An operator marks a threat as affecting or not affecting the environment.

Meta-work such as missing mappings, missing ownership, or missing business context does not outrank live threats by itself. It inherits priority from what it blocks.

The default rule is: **no threat, no work**. The exception is urgent verification where waiting would be risky, for example an actively exploited critical CVE that names a product you may run but have not modeled yet.

---

## Acting on Backlog items

Common actions include:

- Open the affected rule and tune logic.
- Generate a rule from a coverage gap.
- Generate or refresh tests.
- Add or review runbooks and playbooks.
- Fix field or log source mappings.
- Enable or verify telemetry such as CloudTrail, GuardDuty, VPC Flow Logs, EDR, identity logs, email logs, DNS logs, or application logs.
- Open the related threat brief, risk, hunt, target, finding, group, or approval.
- Start a hunt or promote a hunt query to a rule.
- Accept residual risk with a recorded reason.
- Mark **does affect us** or **does not affect us** so future briefs, findings and gaps are re-scored automatically.

Hidden Backlog items can be viewed and restored later.

---

## Business context

CraftedSignal should still work when the customer gives almost no business context. The platform starts with connected SIEMs, imported rules, known technologies, threat brief metadata, observed telemetry, service-catalog presets and rule mappings.

Operators can improve relevance over time with low-friction inputs:

- A plain-text description of important assets, applications and technologies.
- Manual service and data-asset entries.
- Regular imports from spreadsheets or source repositories.
- Later CMDB, CSPM, vulnerability scanner, CTEM, attack-surface and network-scan connectors.

Each accepted or dismissed item teaches the model what applies to this environment.

---

## What each persona gets

- **SOC analysts** get the next useful action, not a generic dashboard tile.
- **Detection engineers** get rule tuning, test, mapping and telemetry blockers in one place.
- **SOC managers** get load, quality, urgent work and blocked work without opening every rule.
- **CTI analysts** get relevance and coverage answers for each brief.
- **Threat modellers and risk managers** get risks tied to rules, hunts, findings, evidence and residual decisions.
- **CISOs and ISOs** get exportable evidence for ISO 27001, SOC 2, NIS2, DORA and internal control reviews.

---

## Runtime risk view

The dashboard aggregates Backlog, threat relevance, rule quality, telemetry coverage, target health, approvals and risk lifecycle state into one view. It should tell a SOC immediately what it needs to know, how it is doing, and what to focus on next.

---

## Related docs

- [Health & Analytics](/docs/health-analytics/) - coverage, health, and detection value metrics.
- [Threat Feed](/docs/threat-feed/) - relevance scoring, affected-product decisions, and rule adoption.
- [Risks](/docs/risks/) - risk states, runtime priority, and lifecycle.
- [Runbooks & Playbooks](/docs/runbooks-playbooks/) - missing or stale response steps.
- [Targets & Mappings](/docs/targets-mappings/) - field and log-source mapping work.
- [AI Assistance](/docs/ai/) - optional generation controls and review requirements.
