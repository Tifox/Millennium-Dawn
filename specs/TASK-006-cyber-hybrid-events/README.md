# TASK-006: Cyber Security & Hybrid Threats

**Status: PENDING**

## Overview

This task adds events for Estonia's experience with Russian cyber and hybrid warfare, including the landmark 2007 cyber attacks, GPS jamming incidents, and border provocations. Estonia was the first NATO country to experience state-level cyber attacks and has since become a global leader in cyber defense.

## Scope

The following events will be implemented:

1. **2007 Cyber Attacks** - Bronze Soldier-related DDoS attacks from Russia
2. **GPS Jamming Events** - Russian GPS interference (2024+)
3. **Border Incidents** - Narva River buoy removal, airspace violations
4. **Cyber Defense Response** - NATO Cyber Defence Centre of Excellence

## Current State

Estonia currently lacks events covering its significant experience with Russian hybrid warfare. These events are historically important and would add considerable depth to playing Estonia.

## Files Affected

| File | Description | Status |
|------|-------------|--------|
| `events/Estonia.txt` | Add cyber/hybrid events | Pending |
| `common/ideas/estonia.txt` | Add cyber defense national spirits | Pending |
| `common/opinion_modifiers/Estonia.txt` | Add opinion modifiers | Pending |
| `common/on_actions/99_EST_on_actions.txt` | Add triggers if needed | Pending |
| `localisation/english/EST_events_l_english.yml` | Add event localization | Pending |

## Implementation Strategy

- Use event IDs in the estonia.300+ range for cyber/hybrid events
- Events should be fire_only_once with appropriate date triggers
- Include opinion modifier effects with Russia
- Add national spirit for cyber defense capability

## Related Issues

- ISSUE-001: 2007 Cyber Attacks (High Priority)
- ISSUE-002: GPS Jamming Events (Medium Priority)
- ISSUE-003: Border Incidents (Medium Priority)
- ISSUE-004: Cyber Defense Response (Low Priority)

## Priority

**High** - The 2007 cyber attacks were a landmark event in cyber warfare history and are essential for historical accuracy.

## Research Sources

- ICDS (icds.ee) - Bronze Soldier crisis report
- NATO StratCom COE - 2007 cyber attacks analysis
- ERR News, Reuters coverage
- ccdcoe.org - NATO Cooperative Cyber Defence Centre of Excellence
