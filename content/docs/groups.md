---
title: "Groups"
description: "Organize rules into deployable groups with targets, quality status, approval policy, batch deployment, CSV export, and health tracking."
weight: 6
section: "Core Concepts"
---

## Overview

Groups bundle detection rules so a team can review, deploy, and track them together. They are useful for service-owned detections, platform-specific release sets, customer-specific content, or SOC team ownership boundaries.

Groups are visible under **Detection > Groups** in the product.

---

## What a group controls

A group can carry:

- Name and description.
- Rule membership.
- Deployment target.
- Approval requirement and approver count.
- Deploy status and last deployment time.
- Quality status based on the rules inside the group.

When SIEM integrations are enabled, groups can be selected and deployed in bulk. Groups without rules or without a target cannot be selected for deployment.

---

## Quality and health

The group list and detail view surface operational status so owners know what needs attention:

- Broken rules that fail compile checks.
- Rules that need review.
- Pending deploys.
- Failed deploys.
- Rules that are active but not firing.
- Last deployed timestamp.

Use group filters to focus on stale or unhealthy rules before deploying another batch.

---

## Deployment behavior

Deploying a group submits the rules in that group through the same validation, testing, approval, and audit flow as an individual rule deployment. If a group has approval enabled, pending approval state is shown on the group and the request appears in the approvals queue.

For larger changes, deploy groups instead of clicking through each rule. That keeps release context, approvals, and rollback history easier to understand.

---

## Export

The group list can be exported as CSV for offline review, audit preparation, or customer reporting.

---

## Related docs

- [Rules](/docs/rules/) - rule metadata and lifecycle states.
- [Deployment & Rollback](/docs/deployment/) - deploy state and rollback behavior.
- [Approvals](/docs/approvals/) - review queues and multi-approver policy.
- [Targets & Mappings](/docs/targets-mappings/) - connecting groups to deployment targets.
