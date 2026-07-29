---
title: "Recommendations"
description: "Actionable detection improvements for coverage gaps, tuning, broken rules, missing runbooks and playbooks, mappings, maturity posture, and AI-assisted rule generation."
weight: 10
section: "Features"
---

## Overview

Recommendations turn platform signals into a prioritized action queue. They help teams decide what to fix next across rule health, coverage, mappings, response steps, and overall detection maturity.

Recommendations are visible under **Risk > Recommendations** and can also appear in the home action queue.

---

## Recommendation sources

Recommendations can come from:

- Coverage gaps in ATT&CK and modeled attack paths.
- Rule health and quality checks.
- Broken or unmapped translations.
- Analyst feedback and false-positive signals.
- Missing tests.
- Missing runbooks and playbooks.
- Target field or log source mapping gaps.
- AI-generated posture analysis.

Each recommendation has severity, category, related rule when available, description, and a target URL that takes the user to the relevant place to act.

---

## AI posture summary

When AI assistance is enabled, CraftedSignal can summarize current detection posture, key findings, and priority. It can also calculate a maturity score and show the gap to the target posture.

The summary is advisory. The underlying recommendations, rule health signals, and coverage data remain the source of truth.

---

## Acting on recommendations

Common actions include:

- Open the affected rule and tune logic.
- Generate a rule from a coverage gap.
- Add or review runbooks and playbooks.
- Add tests.
- Fix field or log source mappings.
- Open the related risk, hunt, target, or group.
- Hide recommendations that are not relevant to the tenant.

Hidden recommendations can be viewed and unhidden later.

---

## Related docs

- [Health & Analytics](/docs/health-analytics/) - coverage, health, and detection value metrics.
- [AI Assistance](/docs/ai/) - AI summaries and recommendations.
- [Runbooks & Playbooks](/docs/runbooks-playbooks/) - missing or stale response steps.
- [Targets & Mappings](/docs/targets-mappings/) - mapping recommendations.
