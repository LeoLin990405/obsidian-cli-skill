# Obsidian CLI Skill for Claude Code

A comprehensive Claude Code skill that enables interaction with Obsidian vaults via the CLI (v1.12.6+).

## Features

- **80+ CLI commands** organized by category
- File management (create, read, append, move, delete)
- Full-text search with context
- Tag and property (frontmatter) management
- Backlink and link graph queries
- Plugin and theme management
- Daily notes, bookmarks, templates
- Bases (database views) queries
- Vault statistics and workspace management
- Sync and version history

## Installation

```bash
cd ~/.claude/skills
git clone https://github.com/LeoLin990405/obsidian-cli-skill.git obsidian-cli
```

## Requirements

- **Obsidian 1.12.0+** with CLI enabled (`"cli": true` in obsidian.json)
- **Catalyst license** (early access)
- **macOS/Linux**
- Obsidian app must be running

## Usage

The skill is automatically loaded when Claude Code detects Obsidian-related queries. Commands are executed with `timeout 5 obsidian <command>` to prevent hanging.

### Quick Examples

```bash
# Vault overview
timeout 5 obsidian vault

# Search
timeout 5 obsidian search query="keyword" path="folder"

# Tags
timeout 5 obsidian tags total counts

# Properties
timeout 5 obsidian property:read name="status" file="My Note"

# Links
timeout 5 obsidian backlinks file="My Note" total
timeout 5 obsidian orphans total
```

## License

MIT
