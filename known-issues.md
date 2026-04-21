# Known Issues — molecule-ai-plugin-superpowers

Issues identified in this plugin repo (GH_TOKEN may be unavailable in automated agent contexts). Each entry has: location, symptom, impact, suggested fix.

Format per entry:
```
## KI-N — Short title

**File:** `<path>:<line>`
**Status:** TODO comment / identified / partially fixed
**Severity:** Critical / High / Medium / Low

### Symptom
...

### Impact
...

### Suggested fix
...
---
```

---

**Policy:** File a GitHub issue before patching silently. Do not merge a workaround without a linked issue.

**Before opening an issue, check:**
- The [open issues](https://github.com/Molecule-AI/molecule-ai-plugin-superpowers/issues)
- The platform constraints in `docs/development/constraints-and-rules.md`
- Any relevant cron learnings in `.claude/cron-learnings.md`

### Severity Definitions

- **Critical:** Plugin fails to install or crashes the agent runtime
- **High:** Plugin produces wrong/broken behavior in normal use
- **Medium:** UX degraded, workaround exists
- **Low:** Cosmetic, negligible user impact