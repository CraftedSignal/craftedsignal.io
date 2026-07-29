---
title: "Approvals"
description: "Review rule changes before deployment with query diffs, metadata, tests, projected impact, noise budget context, multi-approver policy, and audit history."
weight: 10
section: "Features"
---

## Overview

Approvals enforce separation of duties for detection changes. Authors submit changes, reviewers inspect impact, and deployment only proceeds when the required policy is satisfied.

Approvals are visible under **Risk > Approvals** and on the home action queue.

---

## What reviewers see

The approval detail page shows:

- Requester, requested time, and rule version.
- Previous and proposed query.
- Metadata such as severity, platform, frequency, and ATT&CK mapping count.
- Compile state.
- Test status.
- Required signatures.
- Projected alerts per day.
- Query latency.
- Noise budget impact.
- Risk level.
- Review comments and final decision.

Critical rules can surface stricter review prompts such as second approver, passing tests, noise budget confirmation, and monitoring soak.

---

## Multi-approver policy

Companies can require more than one signature. The approval queue and detail page show progress as `current/required`, and the request graduates only after enough unique approvers sign.

Self-approval is blocked when separation of duties applies.

---

## Expiration and escalation

Approval requests can expire. Stale requests remain visible so teams can act before work disappears from the queue. Notifications can alert users when approval is requested, granted, or rejected.

---

## Audit trail

Every approval decision is recorded with identity, timestamp, decision, and comment. The deployment record links back to the approved version so auditors can reconstruct what changed and why.

---

## Related docs

- [Secure Detection Workflows](/docs/secure-workflows/) - approval gates and breakglass.
- [Deployment & Rollback](/docs/deployment/) - deployment state and rollback.
- [Roles & Permissions](/docs/roles-permissions/) - who can approve.
- [Noise Budgets](/docs/noise-budgets/) - volume controls reviewers should check.
