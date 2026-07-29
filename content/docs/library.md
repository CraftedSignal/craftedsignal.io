---
title: "Library"
description: "Reusable rule and hunt templates with tests, ATT&CK mapping, runbooks, playbooks, comments, imports, and managed source repositories."
weight: 5
section: "Core Concepts"
---

## Overview

The Library is where teams keep reusable detection and hunt templates before they become active work. It supports two collections:

- **Rule Library**: detection templates that can be imported as rules.
- **Hunt Library**: hunt hypotheses and queries that can be launched or adapted.

Entries can include query logic, platform type, severity, tags, ATT&CK mapping, data sources, tests, references, and response steps. That makes the library a source of complete detection packages, not just snippets.

---

## Entry types

Library entries can be written in Sigma or native query languages such as SPL, KQL, LEQL, FQL, and EQL. Sigma entries can be converted into connected target languages before import, while native entries remain useful for teams that intentionally keep platform-specific logic.

Each entry can carry:

- Name, description, severity, and tags.
- Query type and query body.
- ATT&CK tactics and techniques.
- Data sources and external references.
- Positive and negative tests.
- Runbooks and playbooks.
- Comments for tenant-scoped review discussion.

---

## Managed sources

Admins can manage library repositories from the admin area. Sources can be local, company-specific, or remote managed repositories. CraftedSignal caches remote entries per tenant so teams can search, inspect, and import consistently.

The default cloud library can be disabled for customers that only want private content.

---

## Importing content

When a library entry is imported, CraftedSignal creates the destination object with the relevant metadata, tests, ATT&CK mapping, and response steps. Detection templates become rules. Hunt templates become hunts.

Imported content is still reviewable. Teams can edit the generated rule or hunt, run tests, attach it to groups, request approval, and deploy through the normal workflow.

---

## Review flow

Use comments when a template needs discussion before import. Use tags and severity filters to keep large libraries searchable. For response steps, prefer reusable structure but keep environment-specific items out of shared templates unless they are safe for every tenant that can see the entry.

---

## Related docs

- [Rules](/docs/rules/) - production detection objects.
- [Threat Hunting](/docs/hunts/) - hunt workflow and promotion.
- [Runbooks & Playbooks](/docs/runbooks-playbooks/) - response steps that move with library content.
- [CLI Reference](/docs/cli/) - library index commands.
