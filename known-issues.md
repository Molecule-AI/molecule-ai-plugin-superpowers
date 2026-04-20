# Known Issues

> Living document for molecule-ai-plugin-superpowers.

---

## 1. Plans files are not validated against the plugin registry at PR time

**Severity:** Low
**Affected versions:** All stable
**Status:** Known; tracked in internal issue `#sup-7`

`plans/*.md` files declare dependencies on other Molecule plugins but the repo
contains no schema validator or CI check to ensure the named plugins actually
exist in the registry at the time the plan is written. A stale plan (pointing to
a renamed or removed plugin) silently passes CI and only surfaces as a missing
plugin at org import time.

**Workaround:** Before adding or updating a `plans/` entry, verify the plugin
exists by checking `manifest.json` in molecule-core.

---

## 2. Skills in `skills/` are orphaned if sub-plugins are removed

**Severity:** Low
**Affected versions:** All stable
**Status:** Known; tracked in internal issue `#sup-12`

If a skill definition is added to `skills/` but its implementing plugin is removed
from the org template's `plugins:` list, the skill file remains on disk but the
skill is never registered. No warning is emitted.

**Workaround:** Keep `skills/` and `plans/` in sync manually. A future CI check
will cross-reference skill names against active plugin list.

---

## 3. Node.js adapter scripts require `node` in the host bridge image

**Severity:** Medium
**Affected versions:** All stable
**Status:** Known; tracked in internal issue `#sup-18`

The TypeScript adapters under `adapters/` are transpiled to Node.js scripts that
run in the host bridge context. The base workspace image must have `node`
available. If `node` is absent, the adapter fails silently (the host bridge does
not surface adapter-level errors to the agent).

**Workaround:** Ensure your workspace template or org-level image includes Node.js,
or use the `host-bridge/` scripts directly with a Node image. The default
`claude-code-default` template includes Node.

---

## 4. `plans/` are not automatically updated when bundled plugins release new versions

**Severity:** Low
**Affected versions:** All stable
**Status:** Known; tracked in internal issue `#sup-21`

The `plans/` directory records which version of each plugin is expected. There is
no automation to update these references when upstream plugins ship new releases.
Over time, plans become stale.

**Workaround:** Run a periodic review of `plans/` against the plugin registry
before major platform releases. A GitHub Actions workflow to detect drift is
planned.
