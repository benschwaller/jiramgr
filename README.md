# jiramgr

Lightweight Jira card manager for the command line. Pure Python 3 — no pip, no go-jira, no dependencies.

## Quick start

```bash
./jiramgr setup             # one-time interactive walkthrough
./jiramgr mine              # list your sprint cards
./jiramgr install-templates # copy templates to ~/.jiramgr/templates/
```

`setup` walks you through everything: instance URL, API token (stored securely at `~/.jiramgr/config.json`, chmod 600), project selection, custom field auto-detection (Epic Link, Sprint, Story Points), and epic label.

## Requirements

- Python 3 (preinstalled on macOS/Linux)

## Install

```bash
chmod +x jiramgr
# optionally: ln -s "$PWD/jiramgr" /usr/local/bin/jiramgr
```

## Usage

### Create

```bash
# Quick create
jiramgr story "Add dark mode"                        # pick epic, sprint prompt
jiramgr task "Update deps" -e EPIC-42               # skip epic picker
jiramgr spike "Research K8s" --sprint               # auto-assign to active sprint
jiramgr story "Audit logs" -e EPIC-10 --backlog     # fully non-interactive

# Template create — opens $EDITOR with structured fields
jiramgr story -t                                     # fill out description, AC, points, assignee
jiramgr story -t "Pre-filled title"                  # pre-fill the summary
jiramgr task -e EPIC-42 -t --sprint                  # combine flags with template
```

| Flag | Description |
|---|---|
| `-e, --epic <KEY>` | Epic key (skip interactive picker) |
| `-t, --template` | Open `$EDITOR` with a structured template |
| `--sprint` | Assign to current active sprint |
| `--backlog` | Leave in backlog |

Types: `story`, `task`, `spike` — all linked to an epic automatically.

### Templates

```bash
jiramgr install-templates   # copy defaults to ~/.jiramgr/templates/
```

Edit `~/.jiramgr/templates/story.md` (or `task.md`, `spike.md`) to customize the fields your team always needs. Template sections:

| Section | What it sets |
|---|---|
| `# Title` | Issue summary |
| `## Description` | Issue description (ADF) |
| `## Acceptance Criteria` | Checklist appended to description |
| `## Story Points` | Numeric story points field |
| `## Assignee` | Blank or "me" = yourself; email = that user |
| `## Epic` | Issue key, or blank for interactive picker |

### View

```bash
jiramgr mine            # your cards in the active sprint
jiramgr mine --backlog  # your cards not in any sprint
```

### Move

```bash
jiramgr move ISSUE-123           # interactive status picker
jiramgr move ISSUE-123 "Done"   # transition directly
```

### Comment

```bash
jiramgr comment ISSUE-123                   # prompts for text
jiramgr comment ISSUE-123 "Fixed the bug"   # inline
```

## Config

```
~/.jiramgr/
├── config.json     # chmod 600 — credentials and field IDs
└── templates/      # customizable .md templates per issue type
```

Reconfigure anytime:

```bash
jiramgr setup
```

## Troubleshooting

| Problem | Fix |
|---|---|
| `No config found` | Run `jiramgr setup` |
| `HTTP 401` | Token expired — re-run `jiramgr setup` |
| `HTTP 404` | Wrong instance URL — re-run `jiramgr setup` |
| No epics found | Check `epic_label` in config |
| No active sprint | No sprint running — card goes to backlog |
