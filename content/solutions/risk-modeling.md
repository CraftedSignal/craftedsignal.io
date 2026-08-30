---
title: "Risk Modeling"
description: "Turn business services, crown-jewel data, attack paths, threats and findings into risk-scored detection priorities your SOC can defend."
weight: 10
stage: "01"
eyebrow: "Risk"
nav_summary: "Model what matters before writing another rule."
hero_image: "/screenshots/coverage-depth.png"
hero_alt: "CraftedSignal coverage view showing exposure and detection depth"
quick_points:
  - "Start with manual input, regular imports, or a plain-text asset description."
  - "ATT&CK techniques, telemetry and findings are weighted by exposure, not counted equally."
  - "Risks flow into Backlog items, hunts, detections, reports, and audit trails."
outcomes:
  - label: "Decision"
    title: "Which gap matters next"
    body: "Prioritize detections by business exposure, threat relevance and telemetry coverage instead of rule count, vendor heatmaps, or the loudest alert queue."
  - label: "Context"
    title: "One operating model"
    body: "Services, data, attack paths, findings, residual risk, and mitigation state live in the same platform analysts use."
  - label: "Proof"
    title: "Reports write themselves"
    body: "Coverage, accepted gaps, Backlog decisions, hunts, and rule changes are tied back to the risks they reduce."
docs:
  - title: "Threat Model"
    url: "/docs/threat-model/"
    description: "How services, data assets, and attack paths become weighted exposure."
  - title: "Risk Ops Board"
    url: "/docs/risks/"
    description: "Risk states, lifecycle transitions, coverage, and audit history."
  - title: "Threat Intake"
    url: "/docs/threat-intake/"
    description: "SOC triage queue for candidate threats before they become sustained risk work."
  - title: "Backlog"
    url: "/docs/backlog/"
    description: "Risk-scored action queue for threats, exposure, coverage, telemetry, and quality gaps."
  - title: "D3FEND Coverage"
    url: "/docs/dfend/"
    description: "How active detections map to defensive techniques and posture."
---

## The problem

Most SOC planning starts too far downstream. Teams know how many rules they have, which ATT&CK cells are colored, and which SIEM alerts are noisy. They often do not know which business service those rules protect, which data asset is still exposed, whether a new threat actually affects them, or why one missing detection should outrank the next.

That creates a familiar operating gap: risk teams talk about crown jewels and attack paths, while detection engineers talk about queries, fields, and false positives. Audits then become spreadsheet exercises because the system of record for detection work is not connected to the system of record for business exposure.

## How CraftedSignal models risk

CraftedSignal starts with the business surface the SOC is defending. You declare services, data assets, and attack paths manually, from regular imports, or from a plain-language asset description when no CMDB exists. Each path maps to attacker techniques and the platform weights those techniques by exposure. A technique on a crown-jewel path is treated differently from the same technique on a low-value path.

Coverage is tracked across the layers where detection actually happens: endpoint, network, identity, cloud, email and application telemetry. That prevents a generic "covered" answer when only one telemetry layer has a rule and the real attack path still has gaps.

## From model to work queue

Accepted attack paths become operational risks. Each risk has a state, owner, priority score, coverage, and timeline. Analysts can hunt it, accept residual risk, escalate it, schedule re-hunts, or link it to rules that reduce the exposure.

The important shift is that risk is not a PDF attached to a ticket. It is a live object that can create Backlog work, create hunts, explain why a rule exists, and show whether the detection program is closing the right gaps.

Threat Intake sits just before this risk lifecycle. SOC reviewers can accept candidate threats that need detection, hunting, simulation, or validation work and dismiss candidates that do not apply. The default is no threat, no work, except urgent verification for actively exploited critical exposure the platform cannot rule out.

## What the SOC gets

Detection engineers get a ranked Backlog that explains why a rule matters. SOC leaders get coverage reporting that separates real risk reduction from activity metrics. CISOs get an answer to "are we protected against this path?" that points to active detections, known blind spots, missing telemetry, residual decisions, and work in progress.

The model also gives threat intelligence and findings a place to land. When a new brief, critical CVE, pentest finding, or future CTEM input arrives, CraftedSignal scores it against the services, technologies, telemetry, rules and paths already known instead of treating every advisory as equal.
