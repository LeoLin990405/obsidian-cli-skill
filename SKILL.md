---
name: obsidian-cli
description: Interact with Obsidian vaults via the CLI (v1.12.6+). Search, read, create, manage files, tags, properties, tasks, plugins, and more. Use when the user wants to query or manipulate their Obsidian vault programmatically. Requires Obsidian app running with CLI enabled.
triggers:
  - obsidian cli
  - obsidian search
  - obsidian tags
  - obsidian properties
  - vault search
  - vault stats
  - backlinks
  - orphan notes
---

# Obsidian CLI Skill

Comprehensive reference for Obsidian's built-in CLI (80+ commands). Enables programmatic vault management, search, tag/property operations, link graph queries, and more.

## Prerequisites

- **Obsidian app must be running** — CLI connects via IPC to the main process
- **CLI enabled**: `"cli": true` in `~/Library/Application Support/obsidian/obsidian.json`
- **Catalyst license** required (early access feature)
- **CRITICAL**: Always prefix commands with `timeout 5` to prevent hanging:
  ```bash
  timeout 5 obsidian <command>
  ```
- **If "Unable to connect to main process"**: Restart Obsidian:
  ```bash
  killall Obsidian && sleep 2 && open -a Obsidian && sleep 5
  ```

## Usage Pattern

```bash
timeout 5 obsidian <command> [params] [flags]
timeout 5 obsidian vault=<name> <command>   # target specific vault (must be first param)
```

| Syntax | Description | Example |
|--------|-------------|---------|
| `key=value` | Parameter | `query="search term"` |
| `key="val spaces"` | Quoted parameter | `file="My Note"` |
| `flagname` | Boolean flag | `total`, `verbose`, `counts` |
| `file=<name>` | Target file (wikilink-style) | `file="Daily Note"` |
| `path=<exact>` | Target file (exact path) | `path="folder/note.md"` |
| `--copy` | Copy output to clipboard | `search query="x" --copy` |
| `\n` / `\t` | Newline / tab in content | `content="line1\nline2"` |

### Output Formats

| Context | Formats |
|---------|---------|
| Most list commands | `format=json\|tsv\|csv` |
| Search | `format=text\|json` |
| Outline | `format=tree\|md\|json` |
| Properties | `format=yaml\|json\|tsv` |
| Base query | `format=json\|csv\|tsv\|md\|paths` |

---

## Command Reference

### General

```bash
timeout 5 obsidian version              # Show version
timeout 5 obsidian help                  # List all commands
timeout 5 obsidian help search           # Help for specific command
timeout 5 obsidian reload                # Reload app window
timeout 5 obsidian restart               # Restart app
```

### Files & Folders

| Command | Key Params | Description |
|---------|-----------|-------------|
| `file` | file/path | Show file info (path, name, ext, size, created, modified) |
| `files` | folder, ext, `total` | List vault files |
| `folder` | path (req), info | Show folder info |
| `folders` | folder, `total` | List vault folders |
| `open` | file/path, `newtab` | Open a file in Obsidian GUI |
| `create` | name/path, content, template, `overwrite`, `open` | Create/overwrite file |
| `read` | file/path | Read file contents |
| `append` | file/path, content (req), `inline` | Append to file |
| `prepend` | file/path, content (req), `inline` | Prepend after frontmatter |
| `move` | file/path, to (req) | Move/rename file (updates links) |
| `rename` | file/path, name (req) | Rename file (preserves ext, updates links) |
| `delete` | file/path, `permanent` | Delete file (trash by default) |

```bash
# Examples
timeout 5 obsidian files total                           # Count all files
timeout 5 obsidian files folder="Udacity" ext=md         # List .md files in folder
timeout 5 obsidian read file="My Note"                   # Read file content
timeout 5 obsidian create name="New Note" content="# Title\n\nContent"
timeout 5 obsidian create path="folder/note.md" template="my-template" open
timeout 5 obsidian append file="Daily" content="\n- New item" inline
timeout 5 obsidian move file="Old Name" to="new-folder"
timeout 5 obsidian open file="My Note" newtab            # Open in new tab
```

### Daily Notes

```bash
timeout 5 obsidian daily                                 # Open today's daily note
timeout 5 obsidian daily:path                            # Get daily note path
timeout 5 obsidian daily:read                            # Read daily note
timeout 5 obsidian daily:append content="- Task done"    # Append to daily
timeout 5 obsidian daily:prepend content="## Morning"    # Prepend after frontmatter
```

### Search

```bash
timeout 5 obsidian search query="keyword"                    # Search vault
timeout 5 obsidian search query="keyword" path="folder"      # Scope to folder
timeout 5 obsidian search query="keyword" limit=5 total      # Limit + count
timeout 5 obsidian search query="keyword" case                # Case-sensitive
timeout 5 obsidian search:context query="keyword"            # Grep-style context
timeout 5 obsidian search:open query="keyword"               # Open search in GUI
```

### Tags

```bash
timeout 5 obsidian tags total                            # Count all tags
timeout 5 obsidian tags total counts                     # Tags with usage counts
timeout 5 obsidian tags file="My Note"                   # Tags in specific file
timeout 5 obsidian tag name="project" total              # Count files with tag
timeout 5 obsidian tag name="resume" verbose             # List files with tag
```

### Tasks

```bash
timeout 5 obsidian tasks total                           # Count all tasks
timeout 5 obsidian tasks todo                            # List incomplete tasks
timeout 5 obsidian tasks done                            # List completed tasks
timeout 5 obsidian tasks file="Project"                  # Tasks in specific file
timeout 5 obsidian task file="Note" line=5 toggle        # Toggle task status
timeout 5 obsidian task file="Note" line=5 done          # Mark as done
```

### Properties (Frontmatter)

```bash
timeout 5 obsidian properties total counts               # All properties with counts
timeout 5 obsidian properties file="My Note"             # Properties of a file
timeout 5 obsidian property:read name="status" file="Note"       # Read value
timeout 5 obsidian property:set name="status" value="complete" file="Note"  # Set value
timeout 5 obsidian property:set name="tags" value="tag1,tag2" type=list file="Note"
timeout 5 obsidian property:remove name="draft" file="Note"      # Remove property
timeout 5 obsidian aliases total                         # Count all aliases
```

### Links

```bash
timeout 5 obsidian backlinks file="My Note"              # Incoming links
timeout 5 obsidian backlinks file="My Note" total        # Count backlinks
timeout 5 obsidian links file="My Note" total            # Count outgoing links
timeout 5 obsidian unresolved total                      # Count broken links
timeout 5 obsidian unresolved counts verbose             # Broken links with details
timeout 5 obsidian orphans total                         # Files with no incoming links
timeout 5 obsidian deadends total                        # Files with no outgoing links
```

### Outline

```bash
timeout 5 obsidian outline file="My Note"                # Heading tree
timeout 5 obsidian outline file="My Note" format=md      # Markdown format
timeout 5 obsidian outline file="My Note" format=json    # JSON format
timeout 5 obsidian outline file="My Note" total          # Count headings
```

### Bookmarks

```bash
timeout 5 obsidian bookmarks                             # List bookmarks
timeout 5 obsidian bookmarks total verbose                # Detailed list
timeout 5 obsidian bookmark file="My Note" title="Fav"   # Add bookmark
```

### Templates

```bash
timeout 5 obsidian templates total                       # Count templates
timeout 5 obsidian template:read name="daily" resolve title="Test"  # Preview
timeout 5 obsidian template:insert name="daily"          # Insert into active file
```

### Bases (Database Views)

```bash
timeout 5 obsidian bases                                 # List .base files
timeout 5 obsidian base:query file="projects" format=json    # Query base
timeout 5 obsidian base:query file="tasks" format=csv        # Export as CSV
timeout 5 obsidian base:create file="projects" name="New Item" content="..."
```

### Plugins

```bash
timeout 5 obsidian plugins                               # List all plugins
timeout 5 obsidian plugins filter=community versions     # Community plugins with versions
timeout 5 obsidian plugins:enabled                       # Only enabled plugins
timeout 5 obsidian plugin id="dataview"                  # Plugin info
timeout 5 obsidian plugin:enable id="dataview"           # Enable
timeout 5 obsidian plugin:disable id="dataview"          # Disable
timeout 5 obsidian plugin:install id="dataview" enable   # Install + enable
timeout 5 obsidian plugin:uninstall id="dataview"        # Uninstall
```

### Themes & Snippets

```bash
timeout 5 obsidian themes                                # List themes
timeout 5 obsidian theme                                 # Active theme
timeout 5 obsidian theme:set name="Minimal"              # Set theme
timeout 5 obsidian snippets                              # List CSS snippets
timeout 5 obsidian snippet:enable name="custom"          # Enable snippet
```

### Command Palette & Hotkeys

```bash
timeout 5 obsidian commands                              # List all command IDs
timeout 5 obsidian command id="editor:toggle-bold"       # Execute command
timeout 5 obsidian hotkeys total                         # Count hotkeys
timeout 5 obsidian hotkey id="editor:toggle-bold"        # Get hotkey for command
```

### Vault & Workspace

```bash
timeout 5 obsidian vault                                 # Vault info (name, path, files, size)
timeout 5 obsidian vaults verbose                        # All known vaults
timeout 5 obsidian workspace                             # Current workspace tree
timeout 5 obsidian workspaces                            # List saved workspaces
timeout 5 obsidian workspace:save name="coding"          # Save workspace
timeout 5 obsidian workspace:load name="coding"          # Load workspace
timeout 5 obsidian tabs                                  # List open tabs
timeout 5 obsidian recents total                         # Recent files
```

### Sync & History

```bash
timeout 5 obsidian sync:status                           # Sync status
timeout 5 obsidian sync off                              # Pause sync
timeout 5 obsidian sync on                               # Resume sync
timeout 5 obsidian sync:history file="Note" total        # Sync versions
timeout 5 obsidian history file="Note"                   # Local history
timeout 5 obsidian history:restore file="Note" version=3 # Restore version
timeout 5 obsidian diff file="Note" from=1 to=3          # Compare versions
```

### Other

```bash
timeout 5 obsidian random                                # Open random note
timeout 5 obsidian random:read                           # Read random note
timeout 5 obsidian wordcount file="Note"                 # Word count
timeout 5 obsidian web url="https://example.com"         # Open URL in web viewer
```

### Developer

```bash
timeout 5 obsidian devtools                              # Toggle dev tools
timeout 5 obsidian dev:errors                            # Show JS errors
timeout 5 obsidian dev:screenshot path="/tmp/shot.png"   # Screenshot
timeout 5 obsidian eval code="app.vault.getFiles().length"  # Run JS
```

---

## Common Workflows

### Vault Health Check
```bash
timeout 5 obsidian vault                    # Overview
timeout 5 obsidian unresolved total         # Broken links
timeout 5 obsidian orphans total            # Orphan notes
timeout 5 obsidian deadends total           # Dead-end notes
```

### Tag-Based Queries
```bash
timeout 5 obsidian tags total counts        # All tags ranked
timeout 5 obsidian tag name="resume" verbose # Find resume-tagged files
timeout 5 obsidian search query="#project"   # Search tag in content
```

### Property Bulk Operations
```bash
# Read status of a note
timeout 5 obsidian property:read name="status" file="Project"
# Update status
timeout 5 obsidian property:set name="status" value="complete" file="Project"
# Add tags
timeout 5 obsidian property:set name="tags" value="done,archived" type=list file="Project"
```

### Daily Note Workflow
```bash
timeout 5 obsidian daily:append content="\n## $(date +%H:%M) Update\n- Task completed"
timeout 5 obsidian daily:read
```

---

## CLI vs Direct File Access

| Operation | Best Tool | Reason |
|-----------|-----------|--------|
| Create/edit notes | Direct file write | Faster, no timeout risk |
| Batch file ops | Direct file system | More reliable for bulk |
| Search content | CLI `search` or Grep | CLI uses Obsidian's index |
| Tag queries | CLI `tags`/`tag` | Only CLI has tag index |
| Property CRUD | CLI `property:*` | Atomic, no parse errors |
| Backlinks/orphans | CLI `backlinks`/`orphans` | Only CLI has link graph |
| Open in GUI | CLI `open` | Opens specific file |
| Plugin management | CLI `plugin:*` | Only way from terminal |

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| "Unable to connect to main process" | `killall Obsidian && sleep 2 && open -a Obsidian && sleep 5` |
| CLI hangs | Always use `timeout 5` prefix |
| Wrong vault | Use `vault=<name>` as first param |
| Command not found | Check `obsidian help` for latest commands |
| Empty output | Some commands return empty on success; check with `total` flag |
