# Estonia Development Specs

> **This is the main entry point for all Estonia content development.**
> Start here before working on any TASK.

This folder contains the development specifications for expanding Estonia (EST) content in Millennium Dawn.

## Quick Start

1. **Pick a TASK** from the table below (choose a `Pending` one)
2. **Read the TASK's README** in its folder for full context
3. **Follow the Git Workflow** below to create a branch and PR
4. **Implement the ISSUEs** within the TASK folder
5. **Fix BugBot issues** until the PR is clean
6. **Merge and update** this README to mark the TASK as done

## Structure

The specs are organized into **TASKs** (major topics) and **ISSUEs** (small implementable todos):

```
specs/
├── README.md                              # This file
├── TASK-001-political-leaders/            # Prime Ministers & Presidents
├── TASK-002-election-events/              # Election mechanics
├── TASK-003-alternative-leaders/          # EKRE, Centre Party paths
├── TASK-004-historical-events/            # Missing historical events
├── TASK-005-current-leadership/           # Current PM Kristen Michal
├── TASK-006-cyber-hybrid-events/          # Cyber attacks & hybrid warfare
├── TASK-007-ekre-events/                  # EKRE political events
├── TASK-008-ukraine-response/             # Ukraine war response
├── TASK-009-digital-society/              # E-governance & digital initiatives
└── TASK-010-defense-modernization/        # Military modernization
```

## TASKs Overview

| TASK ID | Topic | Description | Priority | Status |
|---------|-------|-------------|----------|--------|
| TASK-001 | Political Leaders | Add missing PMs and Presidents (2000-2024) | High | ✅ Done |
| TASK-002 | Election Events | Create election mechanics to rotate leaders | High | ✅ Done |
| TASK-003 | Alternative Leaders | Add EKRE/nationalist and Centre Party leaders for focus paths | Medium | ✅ Done |
| TASK-004 | Historical Events | Expand historical flavor events (EU, NATO, Euro, etc.) | Medium | ✅ Done |
| TASK-005 | Current Leadership | Add Kristen Michal as current PM (2024) | High | ✅ Done |
| TASK-006 | Cyber & Hybrid Threats | 2007 cyber attacks, GPS jamming, border incidents | High | ✅ Done |
| TASK-007 | EKRE Political Events | EKRE rise, nationalist rhetoric, coalition dynamics | Medium | ✅ Done |
| TASK-008 | Ukraine Response | Estonia's response to 2022 Russian invasion | High | ✅ Done |
| TASK-009 | Digital Society | E-residency, e-governance, digital initiatives | Low | Pending |
| TASK-010 | Defense Modernization | NATO integration, military spending, conscription | Medium | ✅ Done |

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

## Git Workflow

### Branch Strategy

Each TASK is developed on its own branch and merged via PR:

```
main
└── estonia-update (feature branch)
    ├── task-001-political-leaders → PR to estonia-update
    ├── task-002-election-events → PR to estonia-update
    └── ...
```

### For Each TASK

1. **Create branch from feature branch:**
   ```bash
   git checkout estonia-update
   git checkout -b task-XXX-description
   ```

2. **Stage and commit files:**
   ```bash
   git add <files>
   git commit -m "$(cat <<'EOF'
   Add <description> (TASK-XXX)

   - Change 1
   - Change 2

   Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
   EOF
   )"
   ```

3. **Push and create PR:**
   ```bash
   git push -u origin task-XXX-description
   gh pr create --base estonia-update --head task-XXX-description \
     --title "Add <description> (TASK-XXX)" \
     --body "## Summary\n- Change 1\n- Change 2"
   ```

4. **BugBot review loop:**
   - A GitHub Action runs BugBot to review the PR
   - Check BugBot status and comments:
     ```bash
     gh pr checks <PR_NUMBER>
     gh api repos/OWNER/REPO/pulls/<PR_NUMBER>/comments \
       --jq '.[] | select(.user.login == "cursor[bot]") | {commit: .original_commit_id[0:7], title: (.body | split("\n")[0])}'
     ```
   - **Repeat until clean:**
     1. Check BugBot comments for issues on latest commit
     2. Fix all reported issues
     3. Commit fixes:
        ```bash
        git add <files>
        git commit -m "Fix BugBot issues: <description>"
        git push
        ```
     4. Wait for BugBot to re-run on new commit
     5. If new issues found, repeat from step 1
   - PR is ready when BugBot summary shows no issues

5. **Merge PR** on GitHub, then sync locally:
   ```bash
   git checkout estonia-update
   git pull origin estonia-update
   ```

### Branch Naming

- Use `task-XXX-description` format (e.g., `task-005-current-leadership`)
- Do NOT use `/` in branch names when parent name exists as a branch

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

## Missing GFX Assets (Low Priority)

Custom GFX assets to be created after all TASKs are implemented. Currently using generic placeholders.

### Leader Portraits

| Portrait | Needed For | Size Needed | Status |
|----------|------------|-------------|--------|
| Siim Kallas | TASK-001 | Large (156x210) | Pending (small exists) |
| Juhan Parts | TASK-001 | Large (156x210) | Pending (small exists) |
| Arnold Ruutel | TASK-001 | Large + Small | Pending |
| Toomas Hendrik Ilves | TASK-001 | Large + Small | Pending |
| Kersti Kaljulaid | TASK-001 | Large (156x210) | Pending (small exists) |
| Alar Karis | TASK-001 | Large + Small | Pending |
| Lennart Meri | TASK-001 | Large (156x210) | Pending (small exists) |
| Mart Helme | TASK-003 | Large + Small | Pending |
| Martin Helme | TASK-003 | Large + Small | Pending |
| Edgar Savisaar | TASK-003 | Large + Small | Pending |
| Kristen Michal | TASK-005 | Large + Small | Pending |

### Event Pictures

| Asset | Needed For | Current Placeholder | Status |
|-------|------------|---------------------|--------|
| EU Accession ceremony | TASK-004 | `GFX_computer` | Pending |
| NATO flag ceremony | TASK-004 | `GFX_computer` | Pending |
| Euro currency/adoption | TASK-004 | `GFX_banking_crisis` | Pending |
| COVID lockdown/pandemic | TASK-004 | `GFX_computer` | Pending |
| 2007 Cyber attack visualization | TASK-006 | `GFX_computer` | Pending |
| GPS jamming / electronic warfare | TASK-006 | `GFX_military_technology` | Pending |
| Narva River border | TASK-006 | `GFX_report_event_soviet_soldiers` | Pending |
| EKRE party rally | TASK-007 | `GFX_report_event_generic_rally` | Pending |
| Estonian parliament (Riigikogu) | TASK-007 | `GFX_report_event_generic_parliament` | Pending |
| Ukraine solidarity / military aid | TASK-008 | `GFX_report_event_military_parade` | Pending |
| Ukrainian refugees arriving | TASK-008 | `GFX_report_event_generic_refugees` | Pending |
| EU sanctions meeting | TASK-008 | `GFX_report_event_generic_parliament` | Pending |
| NATO troops in Estonia | TASK-008 | `GFX_report_event_military_parade` | Pending |
| NATO eFP battlegroup arrival | TASK-010 | `GFX_report_event_military_parade` | Pending |
| Military training / conscription | TASK-010 | `GFX_report_event_military_training` | Pending |
| Air defense systems (IRIS-T/NASAMS) | TASK-010 | `GFX_report_event_anti_air` | Pending |
| Defense innovation / tech | TASK-010 | `GFX_military_technology` | Pending |

### Idea Icons

| Icon | Needed For | Current Placeholder | Status |
|------|------------|---------------------|--------|
| Defense spending budget | TASK-008 | `generic_military_budget` | Pending |
| NATO Forward Forces | TASK-008 | `generic_military_cooperation` | Pending |
| Cyber defense | TASK-006 | `generic_cyber_security` | Pending |
| NATO eFP presence | TASK-010 | `generic_military_cooperation` | Pending |
| Conscription reform | TASK-010 | `generic_manpower_bonus` | Pending |
| Air defense network | TASK-010 | `generic_air_bonus` | Pending |
| Defense innovation | TASK-010 | `generic_research_bonus` | Pending |

### Notes
- Custom GFX can be added later without code changes - just update the `picture = ` references
- **Portrait specs:** Large 156x210 or 512x512 DDS (DXT5), Small 65x67 DDS (DXT5)
- **Event picture specs:** 460x400 DDS
- **Idea icon specs:** 68x68 DDS
- Some Estonia-specific GFX already exist: `GFX_EST_EuroVision`, `GFX_EST_LocalElections`, `GFX_EST_VictoryColumng`, `GFX_EST_NationalDay`, `GFX_EST_etv2`, `GFX_EST_SchoolShooting`, `GFX_bronze_night`, `GFX_cyber_attack`, `GFX_copterline_crash`
