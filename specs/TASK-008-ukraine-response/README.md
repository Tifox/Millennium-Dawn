# TASK-008: Ukraine Response

**Status: DONE**

## Overview

This task adds events and decisions for Estonia's response to the 2022 Russian invasion of Ukraine. Estonia has been one of the most vocal and supportive allies of Ukraine, providing significant per-capita military aid and advocating for strong EU/NATO responses.

## Scope

The following content will be implemented:

1. **Ukraine Invasion Response** - February 2022 immediate response
2. **Defense Spending Increase** - 2% → 3% → 5% GDP decisions
3. **Refugee Acceptance** - Ukrainian refugee integration
4. **Sanctions Leadership** - Estonia's EU sanctions advocacy
5. **Enhanced Forward Presence Upgrade** - eFP to Forward Land Forces transition

## Current State

Estonia lacks events for the major geopolitical shift caused by the 2022 invasion. This content is essential for games extending into the 2020s.

## Files Affected

| File | Description | Status |
|------|-------------|--------|
| `events/Estonia.txt` | Add Ukraine response events | Done |
| `common/ideas/estonia.txt` | Add defense spending/solidarity ideas | Done |
| `common/decisions/EST_decisions.txt` | Add defense spending decisions (new file) | Done |
| `common/decisions/categories/estonia_decisions_categories.txt` | Add defense spending category | Done |
| `common/opinion_modifiers/Estonia.txt` | Add Ukraine solidarity modifiers | Done |
| `localisation/english/EST_events_l_english.yml` | Add event localization | Done |

## Implementation Strategy

- Use event IDs in the estonia.500+ range for Ukraine events
- Include decision category for defense spending choices
- Events should trigger based on Russia-Ukraine war flag
- Include opinion modifiers with Ukraine and NATO allies

## Related Issues

- ISSUE-001: Ukraine Invasion Response (High Priority)
- ISSUE-002: Defense Spending Increase (High Priority)
- ISSUE-003: Refugee Acceptance (Medium Priority)
- ISSUE-004: Sanctions Leadership (Medium Priority)
- ISSUE-005: eFP Upgrade (Medium Priority)

## Priority

**High** - The 2022 invasion fundamentally changed Estonian foreign and defense policy.

## Research Sources

- Estonian Ministry of Defence (kaitseministeerium.ee)
- ERR News coverage
- NATO official announcements
- Reuters, Breaking Defense
- vm.ee (Ministry of Foreign Affairs)

## Key Statistics

- **Defense Spending:**
  - 2022: 2.3% GDP
  - 2024: 3.4% GDP
  - 2025 target: 5% GDP
  - 2029 target: 5.4% GDP
- **Military Aid:** Estonia provided ~1% of GDP in military aid to Ukraine
- **Kaja Kallas:** Became leading EU voice on Russia sanctions
