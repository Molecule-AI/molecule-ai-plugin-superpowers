# molecule-ai-plugin-superpowers

Plugin aggregator that bundles multiple Molecule AI capabilities into a single plugin
install. Designed for org templates that want a broad "everything included" setup
without enumerating individual plugins.

## Purpose

`superpowers` is a **meta-plugin** — it has no runtime code of its own. Instead it
declares dependencies on other plugins (via `plans/`) and registers skills from the
bundled sub-packages. Agents consuming this plugin get all superpowers capabilities
in one install.

## Key Conventions

| Topic | Convention |
|---|---|
| **Structure** | `plans/` = plugin dependency manifests; `skills/` = skill definitions; `adapters/` = runtime adapters |
| **Plugin deps** | Each `plans/<name>.md` lists the plugins and their required config |
| **Adapter registration** | `adapters/` follows the same structure as other Molecule adapter repos |
| **Skills** | Skills live under `skills/<skill-name>/`; each skill has a `skill.yaml` manifest |
| **Versioning** | Follows molecule-core release tags; bump with the platform release |

## Project Structure

```
molecule-ai-plugin-superpowers/
├── plans/               # Plugin dependency manifests (one per bundled plugin)
│   └── <plugin-name>.md
├── skills/              # Skill definitions
│   └── <skill-name>/
│       └── skill.yaml
├── adapters/            # Runtime adapter implementations
│   └── <runtime>/
├── rules/               # Agent behavioural rules (this plugin's conventions)
│   ├── general.md
│   └── plans.md
├── runbooks/
│   └── local-dev-setup.md
└── plugin.yaml
```

## Dev Setup

```bash
git clone https://github.com/Molecule-AI/molecule-ai-plugin-superpowers
cd molecule-ai-plugin-superpowers

# Install plugin locally (for dev org templates)
# Point your org template at this repo + ref
```

### Local Dev with an Org Template

In your `org.yaml`:
```yaml
plugins:
  - github://Molecule-AI/molecule-ai-plugin-superpowers@<your-branch>
```

## Testing

```bash
# Validate plugin.yaml schema
python3 -c "import yaml; yaml.safe_load(open('plugin.yaml'))"

# Lint skill manifests
for skill in skills/*/skill.yaml; do
  python3 -c "import yaml; yaml.safe_load(open('$skill'))"
done
```

## Release Process

1. Update `plans/` if bundled plugin versions have changed
2. Update `plugin.yaml` version
3. Tag: `git tag vX.Y.Z && git push --tags`
4. Update the parent org template that pins this plugin ref

> **Note:** This is a meta-plugin. No standalone tests run in CI. Verify by
> spinning up an org template that includes this plugin and exercising each
> bundled capability.

## Rules

See `rules/`:
- `rules/general.md` — agent conventions when operating in a superpowers org
- `rules/plans.md` — how to read and update plan manifests

## Known Gotchas

- Adding a new plugin to `plans/` does not automatically install its dependencies;
  each sub-plugin must be listed in the org template's `plugins:` list
- Skills declared in `skills/` are registered at org import time; if a
  sub-plugin is removed from the template, orphaned skill files do not cause errors
- This plugin is TypeScript-heavy (73%); adapters are Node.js scripts that run in
  the host bridge context, not in the agent runtime
