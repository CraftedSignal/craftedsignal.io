---
title: "Threat Feed"
description: "Curated threat briefs with Sigma rules, IOCs, runbooks, playbooks, MITRE mappings, affected products, and per-tenant relevance scoring. Briefs answer whether the threat affects you, whether you can see it, and whether you are covered."
weight: 8
section: "Core Concepts"
---

## Overview

The threat feed delivers curated briefs to the platform as signed bundles. Each brief carries a narrative, Sigma rules, IOCs, runbooks, playbooks, MITRE ATT&CK mappings, and metadata about the vendors, products, operating systems and CVEs it affects.

The feed is not a second queue. It is an intelligence surface that scores each brief against your business surface, imported rules, observed telemetry and modeled risks. Relevant briefs can create or raise Backlog work; irrelevant briefs stay out of the operator's way.

Regulated and air-gapped deployments can upload bundles manually; SaaS deployments receive them automatically over a signed channel.

---

## What a brief contains

- **Narrative** — what the threat does, who is behind it, how it spreads.
- **Suggested rules** — Sigma YAML, normalized and deduplicated against your existing library.
- **IOCs** — domains, IPs, hashes, user agents, with suggested hunting queries pre-generated.
- **Runbooks and playbooks** — response steps that can move with adopted rules and hunts.
- **MITRE mapping** — tactics, techniques, sub-techniques. Feeds the threat-model weight.
- **Threat actor** — free-text actor name on the brief, normalized against the [threat-actor catalog](/docs/threat-actors/) on ingest.
- **Affected vendors / products / OS** — structured lists. Briefs render vendor, product, and OS chips when these fields are populated, so you can scan a feed and immediately see which entries hit your stack.
- **CVE enrichment** — CVSS, EPSS, and CISA KEV flags ride alongside each brief in a shared metadata index.

---

## Segmented bundles

Bundles are delivered as **monthly segments** under a single manifest, not as one monolithic file. Each release contains:

- A `manifest.json` index listing every monthly segment with its checksum, brief count, and last-modified date.
- One segment file per month with that period's briefs and digests.
- A shared CVE metadata file referenced by every segment, refreshed whenever EPSS/KEV data changes.

The platform downloads the manifest, fetches the segments and CVE metadata in parallel, and merges them into a single state. Incremental updates only touch the segments that changed — old segments are reused from cache. SaaS tenants get this automatically; air-gapped operators sync the manifest and segments together.

The bundle format is backward-compatible with the legacy single-file `bundle.json`, but new releases use segments by default.

---

## Relevance and risk scoring

Every brief gets a **relevance score** and a derived **risk priority** per company. The scoring asks:

- **Does this affect us?** Based on industry, business services, manually entered assets, imported inventory, affected products, operating systems, CVEs, watchlists and previous accept/dismiss decisions.
- **Can we see it?** Based on connected SIEMs, observed log sources, field mappings, source freshness and required telemetry.
- **Are we covered?** Based on existing rules, tests, health, ATT&CK mapping, related risks, hunts and runbooks.

The relevance score blends:

- Industry profile match (finance, healthcare, SaaS, regulated EU, etc.).
- Affected vendor / product / OS overlap with what you actually run.
- MITRE technique overlap with your accepted attack paths.
- Threat actor overlap with actors already pinned to your hunts or detections.
- Watchlist matches — keywords or asset names you've explicitly flagged.

A brief's MITRE techniques are matched against your accepted attack paths: overlapping paths show as **related risks** and unmatched techniques as **coverage gaps** you can model in one step. See [Risks -> Threat-feed relevance](/docs/risks/#threat-feed-relevance). A high score raises a brief's priority in the feed, exposure views and [Backlog](/docs/backlog/) when there is concrete work to do.

An actively exploited critical CVE can still escalate to the top when the platform cannot prove whether the affected product exists in your environment. That item is framed as urgent verification, not assumed exposure.

---

## Per-tenant explanation

When Threat Feed and Brief Customization are enabled for a tenant, CraftedSignal can generate a company-specific explanation for a brief. The explanation uses industry, modeled services, technology stack, existing detections, known telemetry, and previous affected/not-affected decisions to explain why the brief matters and what action is likely useful.

The original brief remains the source of truth. Customized content is tenant-scoped, reviewable, and does not change rules, hunts, risks or Backlog state without user action.

---

## The adoption flow

1. **Review** — narrative, affected vendors/products/OS, risk priority, "does this affect us", "can we see it", "are we covered", MITRE coverage, and suggested rules.
2. **Adopt** a rule in one click: creates a detection in your library linked back to the brief, including any runbook or playbook content carried by the brief.
3. **Hunt the IOCs** — the brief page generates platform-specific queries from the brief's IOC list. A **Create hunt** button turns the generated query into a new hunt immediately. The hunt is pre-populated with:
   - Title: `"IOC Hunt: <brief title>"`.
   - MITRE tactics and techniques pulled from the brief's TTP list.
   - A backlink to the source brief so you can navigate between the hunt and the brief in both directions.
   - One hunt query carrying the generated IOC query string, marked with source `ioc_generated`.
4. **Mark affected** when it applies, or **mark not affected** with a reason. The decision is remembered and used for future scoring.
5. **Watchlist** the brief for periodic re-check if it is not actionable right now.
6. **Dismiss** with a reason when there is no work to do.

Adoption, affected/not-affected, watchlist and dismiss decisions are per-tenant. The same brief can be urgent for one company and irrelevant for another without affecting either.

---

## Per-brief decisions

The decision controls at `/threat-feed/<slug>` record a per-company acknowledgement. A brief still exists in the feed, but it only contributes to active work when it is relevant, unresolved, watchlisted, or urgently needs verification. Re-open the brief any time to change the decision.

Decisions are tenant-scoped. A SaaS instance with multiple companies tracks separate decisions per (company, brief) pair.

---

## The dashboard's intelligence tab

The Intelligence tab on the dashboard is the operations view of the feed. Cards include:

- **Actively exploited** — briefs flagged as KEV (CISA Known Exploited Vulnerabilities).
- **Unactioned briefs (30d)** — high-priority briefs that haven't been adopted, hunted, watchlisted, marked not affected, or dismissed.
- **TI-related open risks** — open risks that share techniques with recent high-relevance briefs.
- **IOCs in scope** and **watchlist hits**.
- **Recently exploited software** — the last five briefs naming KEV CVEs, with threat actor and CVSS chips.
- **Recent high/critical CVEs** — distinct CVE IDs (CVSS ≥7) from the last 30 days, linking back to the originating brief.
- **Top briefs** — the highest-risk relevant briefs from the last 30 days, with urgent-verification cases separated from confirmed affected threats.

These cards are wired to the same indexes that drive the feed page and Backlog, so changing a brief decision, adopting a rule, starting a hunt, or adding business context updates the dashboard immediately.

---

## Air-gapped delivery

Upload a signed bundle via the dashboard or `csctl feed import`. The bundle is sealed with your tenant's public key and will not decrypt outside the environment it was issued for. Segmented bundles work the same way: upload the manifest plus the segments together, and the platform reconstructs the feed locally without any outbound traffic.

See [Air-gapped Mode](/docs/airgapped/) for the full constraint envelope.

---

## Related

- [Threat Model](/docs/threat-model/) — briefs re-weight your risk score.
- [Backlog](/docs/backlog/) — where relevant threats become prioritized work.
- [Threat Intake](/docs/threat-intake/) — candidate threats queued for SOC review.
- [Risks](/docs/risks/) — where briefs relate to your modeled risks.
- [Threat Actors](/docs/threat-actors/) — how brief actor strings are normalized into the catalog.
- [Library](/docs/library/) — reusable rule and hunt templates.
- [Hunts](/docs/hunts/) — IOC queries seed new hunts.
- [Rules](/docs/rules/) — adopt a brief's detection into your library.
- [Runbooks & Playbooks](/docs/runbooks-playbooks/) — response steps attached to rules, hunts, libraries, and briefs.
