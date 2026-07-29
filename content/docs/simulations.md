---
title: "Simulations"
description: "Run attack simulations from the CLI, report results to the platform, correlate detections, bind scenarios to rules, and track simulated coverage gaps."
weight: 11
section: "Operations"
---

## Overview

Simulations validate whether detections fire when real technique-shaped activity is executed in a controlled environment. The CLI runs the simulation, reports execution metadata to the platform, and the platform correlates the run against matching detections.

The product page is available at **Assurance > Simulations** when Attack Simulations are enabled.

---

## What the page shows

The Simulations page tracks:

- Techniques simulated.
- Detections validated.
- Coverage gaps.
- Recent run history.
- Techniques that have been simulated but did not match a detection.

Run statuses include `planned`, `executing`, `correlating`, `completed`, and `failed`.

---

## CLI workflow

Use `csctl simulate` to inspect adapters, plan a run, execute it, and report results:

```bash
csctl simulate adapters
csctl simulate list
csctl simulate plan T1059.001
csctl simulate run T1059.001 --live
csctl simulate status <run-id>
csctl simulate cleanup T1059.001
```

Dry-run is the default. Add `--live` only when the target environment is approved for controlled simulation.

If `CSCTL_URL` and `CSCTL_TOKEN` are configured, live runs are reported to the platform and correlation is triggered automatically unless `--skip-correlation` is set.

---

## Rule-bound runs

Run simulations for the techniques mapped to one rule:

```bash
csctl simulate run --rule <detection-id> --live
```

The rule editor has a Simulations tab where teams can inspect results and bind scenarios to the rule. Bindings make expected technique coverage explicit, so future runs can verify whether the rule still catches the scenario it is supposed to catch.

---

## Scope files

For safer operation, use a scope file to limit allowed adapters, techniques, targets, and constraints:

```yaml
scope:
  adapters: ["atomic"]
  techniques: ["T1059.001", "T1105"]
  targets:
    - host: "lab-windows-01"
      os: "windows"
  constraints:
    max_concurrent: 1
    cleanup: "always"
    timeout: 5m
```

Run with:

```bash
csctl simulate run T1059.001 --scope simulations.yaml --live
```

---

## API scopes

API tokens used for simulations should include:

- `simulations:read`
- `simulations:write`

---

## Related docs

- [CLI Reference](/docs/cli/) - full `csctl` command list.
- [Testing](/docs/testing/) - positive and negative detection tests.
- [Health & Analytics](/docs/health-analytics/) - coverage and quality signals.
- [Rules](/docs/rules/) - simulation bindings on rule records.
