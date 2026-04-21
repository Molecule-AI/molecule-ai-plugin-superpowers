# Superpowers — Agent Development Superpowers

Plugin for Claude Code and compatible runtimes. Five skills that turn a reactive agent
into a disciplined, systematic developer: systematic debugging, test-driven development,
planning, verification, and plan execution.

## What it provides

**Skills (invoke via the Skill tool):**

| Skill | When to invoke |
|-------|---------------|
| `superpowers:systematic-debugging` | Any bug, test failure, or unexpected behavior — find root cause before proposing fixes |
| `superpowers:test-driven-development` | Implementing any feature or bugfix — write the failing test first |
| `superpowers:writing-plans` | Multi-step implementation task with a spec — write the plan before touching code |
| `superpowers:executing-plans` | Implementation plan exists — execute it task-by-task with verification checkpoints |
| `superpowers:verification-before-completion` | About to claim work is done — run verification and show evidence first |

## How the skills compose

```
writing-plans → executing-plans → systematic-debugging (if bug found)
                          ↘ verification-before-completion (before claiming done)
```

Use `writing-plans` to create a plan, then `executing-plans` to implement it. If a bug
is encountered, `systematic-debugging` finds the root cause. `verification-before-completion`
is run before claiming any work is done.

## Installation

### In org template (org.yaml)
```yaml
plugins:
  - superpowers
```

### From URL (community install)
```
github://Molecule-AI/molecule-ai-plugin-superpowers
```

## License
Business Source License 1.1 — © Molecule AI.
