# superpowers — Local Dev Setup

## Prerequisites

- `git`
- Python 3.11+ (for YAML validation scripts)
- A running Molecule platform (local or staging)

## Clone and Validate

```bash
git clone https://github.com/Molecule-AI/molecule-ai-plugin-superpowers
cd molecule-ai-plugin-superpowers

# Validate plugin.yaml
python3 -c "import yaml; yaml.safe_load(open('plugin.yaml'))"

# Validate all skill manifests
for f in skills/*/skill.yaml; do
  python3 -c "import yaml; yaml.safe_load(open('$f'))"
done && echo "All skill manifests valid"
```

## Pointing an Org Template at a Dev Branch

In your `org.yaml`:

```yaml
plugins:
  - github://Molecule-AI/molecule-ai-plugin-superpowers@<your-branch-name>
```

Restart the workspace to reload plugins.

## Verifying Sub-Plugins Loaded

After workspace restart, check the audit log for plugin-load events:

```bash
# Via molecule-cli (once CLI is released)
mol platform audit query --since 1h | grep plugin_load | grep superpowers
```

## Testing Skill Availability

In a Claude Code session with the superpowers plugin loaded:

```
Available skills: list what superpowers skills are registered
```

Each bundled skill from the sub-plugins should appear.

## Common Issues

| Symptom | Cause | Fix |
|---|---|---|
| Plugin not listed after restart | Branch ref not pushed | Push branch and update org template ref |
| `yaml.scanner.ScannerError` on plugin load | Malformed `plugin.yaml` | Re-run validation above |
| Skill not found | Sub-plugin failed to load | Check runtime compatibility in `plugin.yaml` |
