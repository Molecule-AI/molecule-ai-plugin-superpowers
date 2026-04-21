# Local Development Setup

## Prerequisites

- Python 3.11+
- `pip` or `pipx`

## Setup

```bash
# 1. Clone the repo
git clone https://github.com/Molecule-AI/molecule-ai-plugin-superpowers.git
cd molecule-ai-plugin-superpowers

# 2. Install dependencies
pip install -r .molecule-ci/scripts/requirements.txt

# 3. Run validation
python3 .molecule-ci/scripts/validate-plugin.py

# 4. Verify SKILL.md is present and valid
[ -f SKILL.md ] && head -1 SKILL.md | grep -q "^#" && echo "SKILL.md OK" || echo "SKILL.md invalid"
```

## Validating the Plugin

```bash
# Full validation
python3 .molecule-ci/scripts/validate-plugin.py
```

Expected output:
```
✓ plugin.yaml valid: superpowers v1.0.0
  Content: <files found>
  Runtimes: claude_code, deepagents, hermes
```

## Pre-commit Checks

```bash
# Run all checks before committing
python3 .molecule-ci/scripts/validate-plugin.py && \
python3 -c "
import re, sys
with open('plugin.yaml') as f:
    content = f.read()
patterns = [r'sk.ant', r'ghp.', r'AKIA[A-Z0-9]']
if any(re.search(p, content) for p in patterns):
    print('FAIL: possible credentials found')
    sys.exit(1)
print('No credentials: OK')
" && echo "All checks passed"
```

## Adding a New Runtime

1. Edit `plugin.yaml` and add the runtime name to the `runtimes:` list
2. Run `python3 .molecule-ci/scripts/validate-plugin.py` to verify
3. Commit with `chore: add <runtime> to supported runtimes`