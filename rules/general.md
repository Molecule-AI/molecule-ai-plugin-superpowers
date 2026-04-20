# superpowers — Agent Behavioural Rules

## Overview

Agents operating in an org that has loaded the `superpowers` plugin should
follow these conventions. This plugin bundles multiple capabilities; the rules
below apply to all bundled sub-plugins unless overridden by a sub-plugin's own
`rules/` entry.

---

## Error Handling

### Plugin Loading Errors

If a bundled plugin fails to load (missing dependency, invalid skill manifest,
runtime mismatch), the superpowers plugin logs a warning and **continues**
loading the remaining bundled plugins. Do not treat a sub-plugin failure as a
total failure of the superpowers plugin.

```
[WARN] superpowers: failed to load skill <skill-name>: <reason>
[INFO] superpowers: continuing with remaining <N> skills
```

If fewer than half the expected skills load, emit a `critical` log and surface
an alert to the org admin via the platform audit channel.

### Skill Not Found

If an agent invokes a skill registered by a sub-plugin that failed to load,
the runtime returns `SkillNotFoundError`. The agent should log the full skill
name and list available skills via the platform API to find an alternative.

---

## Logging

All sub-plugin loading events go to the platform audit ledger with:
- `event: plugin_load`
- `plugin: superpowers`
- `sub_plugin: <name>`
- `status: success | failed`
- `error: <reason>` (if failed)

Agent-level skill invocations from bundled plugins go to the standard skill
audit trail.

---

## Configuration

Superpowers config is inherited from the org-level `config.yaml`. Sub-plugin
config sections follow the naming convention `<sub-plugin-name>.config`:

```yaml
superpowers:
  enabled: true
  load_timeout_seconds: 30
```

Each sub-plugin may have its own config namespace (e.g., `hitl:` for the HITL
sub-plugin, `careful_bash:` for careful-bash sub-plugin).

---

## Release Process

When bumping the superpowers plugin version:

1. Review `plans/` — ensure all declared plugin dependencies still exist in
   `manifest.json` at the target platform version
2. Update `plugin.yaml` version
3. Update `skills/` manifests if bundled skills have changed their schema
4. Run the integration suite in `tests/` (if present)
5. Tag and push: `git tag vX.Y.Z && git push --tags`
6. Update the parent org template ref to the new tag
7. Validate the org template imports correctly in a dev workspace
