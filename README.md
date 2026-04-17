# Archived: obsidian-cli skill moved to `claude-code-skills`

This repository is now a historical source snapshot.

The canonical `obsidian-cli` skill lives here:

- Repository: `LeoLin990405/claude-code-skills`
- Path: `productivity/obsidian-cli`

## Why

`obsidian-cli` is now bundled directly into the canonical monorepo so it no longer needs to remain a self-owned submodule.

## Canonical Install

```bash
git clone --recurse-submodules https://github.com/LeoLin990405/claude-code-skills.git
ln -s "$PWD/claude-code-skills/productivity/obsidian-cli" ~/.claude/skills/obsidian-cli
```
