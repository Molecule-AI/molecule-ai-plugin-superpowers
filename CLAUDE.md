# superpowers — Molecule AI Plugin

Plugin providing systematic debugging, test-driven development, planning, and verification skills for agent workspaces.

## Overview

This plugin delivers a suite of agent superpowers: systematic debugging, TDD, planning, and verification. It targets `claude_code`, `deepagents`, and `hermes` runtimes.

## Build and Test

```bash
# Validate plugin structure
python3 .molecule-ci/scripts/validate-plugin.py

# Install dependencies
pip install -r .molecule-ci/scripts/requirements.txt

# Run validation
python3 .molecule-ci/scripts/validate-plugin.py .
```

## Project Structure

```
.                       # Plugin root (plugin.yaml defines the package)
SKILL.md                # agentskills.io spec for this plugin
.claude/                 # Agent settings (permissions, hooks)
.molecule-ci/            # CI validation scripts
  scripts/
    validate-plugin.py  # plugin.yaml + content validator
    requirements.txt    # Python deps (pyyaml)
runbooks/                # Operational runbooks
  local-dev-setup.md    # Local development setup guide
```

## Plugin Manifest

```yaml
name: superpowers
version: 1.0.0
description: Agent superpowers — systematic debugging, test-driven development, planning, and verification
author: Molecule AI
runtimes:
  - claude_code
  - deepagents
  - hermes
skills:
  - executing-plans
  - systematic-debugging
  - test-driven-development
  - verification-before-completion
  - writing-plans
```

## Pre-commit Checklist

Run before every commit:

```bash
# 1. Validate plugin.yaml structure
python3 .molecule-ci/scripts/validate-plugin.py

# 2. Check for credentials in all files
python3 -c "
import re, sys
with open('plugin.yaml') as f:
    content = f.read()
patterns = [r'sk.ant', r'ghp.', r'AKIA[A-Z0-9]']
if any(re.search(p, content) for p in patterns):
    print('FAIL: possible credentials found')
    sys.exit(1)
print('No credentials: OK')
"

# 3. Verify SKILL.md exists and is non-empty
[ -s SKILL.md ] || { echo "SKILL.md is empty or missing"; exit 1; }

# 4. Verify plugin.yaml is valid YAML
python3 -c "import yaml; yaml.safe_load(open('plugin.yaml'))" || exit 1

echo "All checks passed"
```

## Adding a New Skill

1. Add the skill file under `skills/` (agentskills.io format)
2. Update `plugin.yaml` `skills:` array if needed
3. Add to `SKILL.md` index if it has a dedicated section
4. Run `python3 .molecule-ci/scripts/validate-plugin.py` to verify

## Release Process

This plugin is released via the Molecule AI platform plugin registry. Version bumps are made in `plugin.yaml` and the change is tagged in this repo. The platform CI/CD pipeline publishes to the registry on merge to `main`.