# oh-my-pi-config

Personal configuration for [oh-my-pi](https://github.com/oh-my-pi/oh-my-pi), an AI coding assistant framework.

## Overview

This repo contains a single `config.yml` that customizes model roles, retry behavior, tool intercepts, skills, and editor integrations.

## Configuration Highlights

### Model Roles

| Role | Model | Usage |
|------|-------|-------|
| `default` | `claude-sonnet-4-6` (medium) | General-purpose tasks |
| `plan` / `slow` | `claude-opus-4-6` (high) | Complex reasoning, planning |
| `task` / `vision` | `gpt-5.4` (high) | Agentic tasks, image understanding |
| `smol` / `commit` | `gpt-5.4-mini` (low) | Quick, cheap operations |

Models cycle in order: `smol → default → plan → task → slow`.

### Retry & Fallback

- **Max retries:** 2, with a 2 s base delay
- **Fallback chains** kick in per role (e.g. `plan` falls back to `gpt-5.4:xhigh`)
- **Revert policy:** `cooldown-expiry` — returns to primary after cooldown

### Compaction

Context compaction is enabled with `context-full` strategy:
- Reserves 16 384 tokens for headroom
- Keeps 20 000 recent tokens
- Auto-continues after compaction

### Bash Interceptor

Shell commands are redirected to structured tool equivalents:

| Command | Redirected to |
|---------|--------------|
| `cat`, `head`, `tail` | `read` tool |
| `grep`, `rg`, `ag` | `grep` tool |
| `find`, `fd` | `find` tool |
| `sed -i`, `perl -i` | `edit` tool |
| `echo >`, `cat <<` | `write` tool |

### LSP

- Diagnostics run on both write and edit
- Format-on-write is disabled

### Skills

Enabled skill bundles:
- `frontend-design`
- `superpowers:brainstorming`
- `superpowers:dispatching-parallel-agents`
- `superpowers:subagent-driven-development`
- `superpowers:writing-plans`

Several superpowers extensions are explicitly disabled (e.g. `react-doctor`, `systematic-debugging`, `test-driven-development`).

### Misc

- **Theme:** `titanium` (dark) / `light` (light)
- **Images:** blocked from inline display, auto-resized
- **Fuzzy edit matching:** enabled at 0.95 threshold
- **Todo reminders:** on, max 3
- **Marketplace auto-update:** notify only
