---
title: "Targets & Mappings"
description: "Connect SIEM targets, keep read-only or auto-sync settings, manage field mappings, log source mappings, and apply suggested overrides."
weight: 13
section: "Integrations"
---

## Overview

Targets are the SIEM or detection platforms CraftedSignal reads from, tests against, and deploys to. They are managed under **Detection > Targets**.

Targets currently include Splunk, Microsoft Sentinel, Elastic Security, CrowdStrike, and Rapid7 InsightIDR depending on enabled integrations.

---

## Target settings

A target can include:

- Name and description.
- Platform type.
- API URL or platform-specific workspace settings.
- API token, client secret, or webhook settings.
- Splunk HEC settings for test event injection.
- Automatic rule sync toggle.
- Read-only mode.

Read-only targets block deployments, drift fixes, and test event injection. They are still useful for import, visibility, and health checks.

---

## Field mappings

Field mappings override how Sigma fields translate to target-specific field names.

Example:

| Sigma field | Platform field |
|-------------|----------------|
| `Image` | `process_name` |
| `CommandLine` | `process_command_line` |
| `ParentImage` | `parent_process_name` |

Use field mappings when your SIEM schema differs from the default compiler assumptions. Mappings are saved immediately when added or removed.

---

## Log source mappings

Log source mappings tell the compiler which table, index, sourcetype, log set, or event type to use for a Sigma `logsource` combination.

For Splunk, a mapping can include index and sourcetype. For Sentinel, the same concept maps to a table. For Rapid7 it maps to a log set. For CrowdStrike it maps to an event type.

---

## Suggested overrides

The target edit page can analyze Sigma rules and suggest missing field or log source mappings. Suggestions can be applied one by one or in bulk above a confidence threshold.

Use this workflow after importing rules from a new source, connecting a new target, or seeing unmapped fields in compiled previews.

---

## When to update mappings

Update mappings when:

- A compiled query references fields that do not exist in the target.
- A rule has the right logic but runs against the wrong table, index, or sourcetype.
- A new telemetry source lands in the SIEM with different naming.
- Simulations or live tests produce telemetry but detections do not match.
- Coverage recompute shows weak or missing telemetry for modeled attack paths.

---

## Related docs

- [Platform Guides](/docs/platforms/) - connection details by SIEM.
- [Rules](/docs/rules/) - compiled previews and mapping warnings.
- [Testing](/docs/testing/) - test execution against connected targets.
- [Simulations](/docs/simulations/) - simulation results that expose mapping gaps.
