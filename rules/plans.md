# superpowers — Plans

This file documents the plugin dependency manifests under `plans/`.

## What Are Plans?

A `plans/<name>.md` file describes a logical bundle of plugins that form a
"superpower" — a coherent set of capabilities grouped for easy org setup.

## Plans Convention

Each plan file:
- Lives at `plans/<plan-name>.md`
- Declares the plugins it bundles
- Optionally declares environment variables or config keys required
- Does NOT install anything itself — the org template's `plugins:` list
  is the canonical install mechanism

## Reading a Plan

```markdown
# Plan: security-suite

Plugins:
- molecule-security-scan  (>= 1.0.0)
- molecule-compliance      (>= 0.8.0)
- molecule-audit          (>= 0.5.0)

Required env:
- AUDIT_WEBHOOK_URL

Optional env:
- SECURITY_SCAN_CONCURRENCY (default: 4)
```

## Updating Plans

When a sub-plugin releases a new version:
1. Update the version constraint in the relevant `plans/<name>.md`
2. Run integration tests in the org template before tagging
3. Tag superpowers with the same version scheme as molecule-core releases
