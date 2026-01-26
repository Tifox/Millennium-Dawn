# Estonia Development Specs

This folder contains the development specifications for expanding Estonia (EST) content in Millennium Dawn.

## Structure

The specs are organized into **TASKs** (major topics) and **ISSUEs** (small implementable todos):

```
specs/
├── README.md                              # This file
├── TASK-001-political-leaders/            # Prime Ministers & Presidents
├── TASK-002-election-events/              # Election mechanics
├── TASK-003-alternative-leaders/          # EKRE, Centre Party paths
└── TASK-004-historical-events/            # Missing historical events
```

## TASKs Overview

| TASK ID | Topic | Description | Priority | Status |
|---------|-------|-------------|----------|--------|
| TASK-001 | Political Leaders | Add missing PMs and Presidents (2000-2024) | High | |
| TASK-002 | Election Events | Create election mechanics to rotate leaders | High | |
| TASK-003 | Alternative Leaders | Add EKRE/nationalist and Centre Party leaders for focus paths | Medium | |
| TASK-004 | Historical Events | Expand historical flavor events (EU, NATO, Euro, etc.) | Medium | ✅ Done |

## Workflow

### Implementing an ISSUE

1. Navigate to the relevant TASK folder
2. Read the TASK's README.md for context
3. Open the ISSUE file you want to implement
4. Follow the implementation steps with the provided code snippets
5. Check off the requirements and acceptance criteria
6. Test in-game

### ISSUE Status Tracking

Each ISSUE file contains checkboxes for:
- **Requirements**: What must be implemented
- **Acceptance Criteria**: How to verify the implementation works

Mark these as complete (`[x]`) as you work through each issue.

## Code Style

All PDX script code in this spec follows the Millennium Dawn coding conventions:
- Use tabs for indentation (not spaces)
- Character IDs follow pattern: `EST_[name]_[surname]`
- Event IDs follow pattern: `est.[category].[number]`
- Localization keys use lowercase with underscores

## Dependencies

Some ISSUEs depend on others. Check the "Dependencies" section in each ISSUE file before starting work.

## File Paths

All file paths in ISSUE specs are relative to the mod root directory:
```
/Users/tifox/Projects/Millennium-Dawn/
```
