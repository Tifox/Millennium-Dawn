# TASK-005: Current Political Leadership

**Status: PENDING**

## Overview

This task adds Kristen Michal as the current Prime Minister of Estonia and updates the leader rotation system to properly transition from Kaja Kallas to Michal in July 2024.

## Scope

The following additions will be implemented:

1. **Add Kristen Michal** - Current PM since July 23, 2024
2. **Update Leader Rotation Effects** - Extend EST_create_reform_leader to include Michal after Kallas

## Current State

The existing `EST_political_leaders.txt` already contains a reference to Kristen Michal as liberalism_leader index 3, but the character definition is missing from `common/characters/EST.txt`. The leader portrait file `gfx/leaders/EST/kristen_michal.dds` needs to be added.

## Files Affected

| File | Description | Status |
|------|-------------|--------|
| `common/characters/EST.txt` | Add Kristen Michal character definition | Pending |
| `common/scripted_effects/EST_political_leaders.txt` | Verify/update date triggers | Pending |
| `gfx/leaders/EST/kristen_michal.dds` | Add leader portrait | Pending |
| `localisation/english/MD_focus_EST_l_english.yml` | Add leader description | Pending |

## Implementation Strategy

- Add Kristen Michal character with appropriate traits (western_liberalism, economist, tech_savy)
- Update the date trigger in EST_political_leaders.txt to transition from Kallas after 2024.7.23
- Ensure portrait path matches existing conventions

## Related Issues

- ISSUE-001: Add Kristen Michal
- ISSUE-002: Update Leader Rotation Effects

## Priority

**High** - Kristen Michal is the current serving PM, this is a critical gap in the Estonia content.

## Research Sources

- valitsus.ee - Official Estonian Government website
- president.ee - Estonian President's office
- Reuters, ERR News coverage of July 2024 transition
