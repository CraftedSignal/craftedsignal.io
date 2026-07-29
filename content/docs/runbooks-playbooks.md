---
title: "Runbooks & Playbooks"
description: "Attach Markdown runbooks and playbooks to rules, hunts, library entries, and threat briefs. Keep response steps synced with rule changes and AI-generated drafts."
weight: 6
section: "Core Concepts"
---

## Overview

CraftedSignal lets rules and hunts carry the response steps analysts need after a detection fires or a hunt returns useful signal.

A **runbook** is the alert-level procedure: what the alert means, how to triage it, what evidence to collect, which false positives to check, and when to escalate.

A **playbook** is the broader incident path: containment, eradication, recovery, communications, handoff, and follow-up detection work.

Both are stored as Markdown. They can be written manually, imported from library or threat-feed content, or drafted by AI from the same rule and hunt context reviewers already inspect.

---

## Where they attach

Runbooks and playbooks can be linked to:

- **Rules**: Give analysts response steps for a specific production or monitoring-mode detection.
- **Hunts**: Capture what to do when a hunt query or cluster looks promising.
- **Library entries**: Ship reusable response steps with shared rule templates.
- **Threat briefs**: Carry campaign-specific response steps into adopted rules or hunts.

When a library entry or threat brief is adopted, its response content moves with the created rule or hunt.

---

## Sync with rules

Runbooks and playbooks are part of the detection lifecycle, not side documents.

- `csctl push`, `pull`, and `sync` preserve response steps with the rule.
- Rule sync includes response steps in change detection, so edits made in Git or the web UI are visible in diffs.
- When rule logic, Sigma source, query, severity, ATT&CK mapping, tags, or hunt context changes, CraftedSignal can mark the response steps stale.
- Hunt promotion carries the hunt's response steps into the new detection.
- Library and threat-feed adoption carries response steps into the created rule or hunt.

This keeps the rule and analyst workflow moving together. A query change can prompt a runbook/playbook review, and existing response steps can be used as context when AI helps refine a rule, tests, or follow-up detection work.

---

## AI-generated drafts

When AI assistance is enabled, AI can draft the runbook and playbook while analyzing a rule. The draft is grounded in:

- Rule logic, query language, and target platform
- Data sources, fields, tags, severity, and confidence
- MITRE ATT&CK mappings
- Positive and negative tests
- Available company and threat context

AI-generated drafts are suggestions. They are editable, previewable, and audit-visible. They do not deploy a rule, change an alert, or publish response steps without a human choosing to use them.

AI can also use existing response steps as context when refining the related rule or tests, so the detection logic and the analyst workflow do not drift into two separate truths.

The generated draft is expected to stay specific to the rule. It should not invent ticket queues, internal tools, hostnames, team names, or contacts. If a useful local reference is unknown, it should leave a short placeholder for your team to fill in.

---

## Recommended structure

Use this structure when writing manually or reviewing an AI draft:

```markdown
## Runbook

### Alert intent

### Triage steps

### Evidence to collect

### False-positive checks

### Escalation criteria

## Playbook

### Containment

### Eradication and recovery

### Communications and handoff

### Follow-up detection work
```

The exact wording can vary by team, but keeping these sections consistent makes response content easier to scan during an incident and easier to review during rule changes.

---

## Lifecycle and freshness

Response content has a status:

| Status | Meaning |
|--------|---------|
| `draft` | Written or generated, but not yet reviewed. |
| `reviewed` | Checked by the team and ready for analysts to use. |
| `archived` | Kept for history, but no longer current. |

CraftedSignal tracks the rule or hunt context used when the runbook or playbook was saved. If the rule logic, Sigma source, query, severity, ATT&CK mapping, tags, hunt hypothesis, hunt query, or hunt verdicts change later, the response content can be marked stale so reviewers know it may need another pass.

Each save records the version, updater, and timestamp. Markdown is capped at 64 KiB per rule or hunt.

---

## Review checklist

Before marking response content ready, check that it:

- Names the alert intent in plain language.
- Mentions the exact fields, entities, or query conditions analysts should inspect.
- Separates true-positive evidence from expected benign behavior.
- Gives clear escalation criteria.
- Avoids response actions that the rule's telemetry cannot justify.
- Includes follow-up tuning or detection work when the alert is noisy or incomplete.

---

## Related docs

- [Rules](/docs/rules/) - rule context and lifecycle.
- [Threat Hunting](/docs/hunts/) - hunt promotion and verdict workflows.
- [AI Assistance](/docs/ai/) - AI controls, privacy, and review requirements.
- [Threat Feed](/docs/threat-feed/) - adopting threat brief rules and hunts.
