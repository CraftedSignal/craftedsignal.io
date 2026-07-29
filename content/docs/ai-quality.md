---
title: "AI Quality"
description: "Track AI operation volume, first-pass rate, quality scores, retries, errors, hot rules, trends, and improvement proposals."
weight: 11
section: "Administration"
---

## Overview

AI Quality is the owner-facing view for monitoring AI-assisted workflows. It helps administrators see where AI is working, where users retry often, and which prompt or workflow improvements should be acknowledged or dismissed.

The page is available at **AI Quality** in the account menu for owners.

---

## Metrics

AI Quality tracks:

- Total AI operations.
- First-pass rate.
- Average quality score where scored.
- Error rate.
- Per-feature operation count.
- Per-platform operation count.
- Average retries.
- Average duration.
- Frequently retried rules.
- Error categories.
- Quality trend over 7, 30, or 90 days.

These metrics are computed from logged AI activity. Providers that do not return native cost still record token counts and operation metadata.

---

## Improvement proposals

The page can show improvement proposals for recurring issues such as low first-pass rate, high retry count, repeated errors, or feature-specific quality drops.

Owners can acknowledge proposals they plan to address or dismiss proposals that are not relevant.

---

## Relation to AI logs

AI Quality complements the AI usage log described in [AI Assistance](/docs/ai/). The usage log keeps the prompt, response, model, token counts, cost estimate, and activity. AI Quality rolls that activity up into operational metrics.

---

## Related docs

- [AI Assistance](/docs/ai/) - AI features, guardrails, privacy, and logging.
- [Recommendations](/docs/recommendations/) - AI-assisted posture summaries and action queues.
- [Security](/docs/security/) - audit and data boundaries.
