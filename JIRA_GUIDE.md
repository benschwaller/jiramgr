# jiramgr

Simple Jira card manager. Pure Python 3, no external dependencies.

## Quick Start

```bash
jiramgr setup    # interactive walkthrough (one time only)
jiramgr mine     # list your cards
```

That's it. `setup` handles everything: instance URL, API token, project key,
custom field IDs (auto-detected), and epic label.

## Requirements

- Python 3 (preinstalled on macOS/Linux)

## Install

```bash
chmod +x bin/jiramgr
# optionally: ln -s $PWD/bin/jiramgr /usr/local/bin/jiramgr
```

## Usage

```bash
# Create
jiramgr story "Add dark mode"                    # pick epic, sprint prompt
jiramgr task "Update deps" -e EPIC-42           # sprint prompt only
jiramgr spike "Research K8s" --sprint            # pick epic, auto-sprint
jiramgr story "Audit logs" -e EPIC-10 --backlog # fully non-interactive

# View
jiramgr mine                  # your cards in the active sprint
jiramgr mine --backlog        # your cards not in any sprint

# Move
jiramgr move ISSUE-123        # interactive: pick a status
jiramgr move ISSUE-123 "Done" # move directly

# Comment
jiramgr comment ISSUE-123                  # prompts for text
jiramgr comment ISSUE-123 "Fixed the bug"  # inline
```

Types: `story`, `task`, `spike`

## Config

Stored at `~/.jiramgr.json` (chmod 600). Created by `jiramgr setup`.

To reconfigure at any time, just run `jiramgr setup` again.

## Troubleshooting

| Problem               | Fix                                          |
|-----------------------|----------------------------------------------|
| `No config found`     | Run `jiramgr setup`                           |
| `HTTP 401`            | Token expired — run `jiramgr setup` to update |
| `HTTP 404`            | `base_url` is wrong — run `jiramgr setup`     |
| No epics found        | Check `epic_label` — run `jiramgr setup`      |
| No active sprint      | No sprint running — card goes to backlog      |
