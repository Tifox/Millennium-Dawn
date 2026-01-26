# TASK-007: EKRE Political Events

**Status: PENDING**

## Overview

This task adds events for the Conservative People's Party of Estonia (EKRE) political trajectory from 2012-2024, including the party's formation, entry into government, and subsequent collapse due to corruption scandals.

## Scope

The following events will be implemented:

1. **EKRE Formation** - Party founded March 24, 2012
2. **EKRE Enters Government** - Coalition with Centre Party + Isamaa (April 2019)
3. **EKRE Government Collapse** - Corruption scandal forces resignation (January 2021)
4. **EKRE Electoral Events** - 2019 breakthrough, 2023 results

## Current State

EKRE leaders (Mart Helme and Martin Helme) already exist in the game with character definitions and scripted effects. This task adds flavor events around their political journey and the controversial EKRE-led government period.

## Files Affected

| File | Description | Status |
|------|-------------|--------|
| `events/Estonia.txt` | Add EKRE political events | Pending |
| `common/ideas/estonia.txt` | Add EKRE government effects | Pending |
| `common/opinion_modifiers/Estonia.txt` | Add EU relations modifiers | Pending |
| `common/on_actions/99_EST_on_actions.txt` | Add triggers if needed | Pending |
| `localisation/english/EST_events_l_english.yml` | Add event localization | Pending |

## Implementation Strategy

- Use event IDs in the estonia.400+ range for EKRE events
- Events should reflect the controversy surrounding EKRE
- Include stability effects and EU opinion impacts
- Link to existing EKRE leader rotation mechanics

## Related Issues

- ISSUE-001: EKRE Formation (Medium Priority)
- ISSUE-002: EKRE Enters Government (High Priority)
- ISSUE-003: EKRE Government Collapse (High Priority)
- ISSUE-004: EKRE Electoral Events (Low Priority)

## Priority

**High** - The EKRE government period (2019-2021) was a significant and controversial chapter in Estonian politics.

## Research Sources

- ERR News coverage of EKRE
- Reuters, BBC, Guardian coverage
- Riigikogu (Parliament) records
- Electoral commission data
