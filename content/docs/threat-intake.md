---
title: "Threat Intake"
description: "SOC triage queue for candidate threats from CTI, risk requests, Threats, and future exposure or vulnerability sources."
weight: 9
section: "Core Concepts"
---

## Overview

Threat Intake is the SOC-owned queue for candidate threats that may need detection, hunting, or validation work. It keeps triage separate from risk approval so CTI-to-SOC review is not blocked by a governance decision.

Threat Intake is visible under **Threats > Threat Intake**.

---

## What enters intake

Intake can include candidate scenarios from:

- Threat briefs.
- Risk requests.
- Threat model work.
- Attack-path review.
- Future exposure or vulnerability tooling.

Each item includes source, severity, mapping status, related service where available, name, description, and associated attack-path steps.

---

## Triage actions

SOC reviewers use intake to decide whether a candidate threat should become work:

- Accept scenarios that need detection, hunting, simulation, or validation.
- Dismiss candidates that do not apply.
- Filter by source, severity, mapping status, service, search text, and page size.
- Move accepted work into risks, hunts, attack paths, or detection backlog as appropriate.

---

## How it fits with risks

Risks track accepted exposure over time. Threat Intake is earlier: it is where a potential scenario is reviewed before the SOC commits to sustained tracking or detection work.

---

## Related docs

- [Threat Modeling & Risk Scoring](/docs/threat-model/) - modeled services, assets, and attack paths.
- [Risks](/docs/risks/) - accepted attack paths and lifecycle tracking.
- [Threat Feed](/docs/threat-feed/) - curated briefs that can feed intake.
- [Threat Hunting](/docs/hunts/) - validating accepted hypotheses.
