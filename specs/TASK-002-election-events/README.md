# TASK-002: Election Events

## Overview

This task implements a parliamentary election system for Estonia that rotates leaders based on historical election cycles. Estonia holds parliamentary elections every four years, with the Prime Minister being the head of government who leads the country in Millennium Dawn.

## Scope

Create election mechanics that:
1. Trigger elections on historical dates (2003, 2007, 2011, 2015, 2019, 2023)
2. Present players with election event choices
3. Rotate leaders (Prime Ministers) based on election outcomes
4. Support both historical and ahistorical paths

## Historical Context

Estonia is a parliamentary republic where the Prime Minister is the head of government. The Riigikogu (Parliament) has 101 seats, and elections are held every 4 years in early March.

### Historical Prime Ministers by Election

| Election | Prime Minister | Party | Term |
|----------|---------------|-------|------|
| 2003 | Juhan Parts | Res Publica (Union of Pro Patria and Res Publica) | 2003-2005 |
| 2003 | Andrus Ansip | Reform Party | 2005-2014 |
| 2007 | Andrus Ansip | Reform Party | 2007-2011 |
| 2011 | Andrus Ansip | Reform Party | 2011-2014 |
| 2015 | Taavi Roivas | Reform Party | 2014-2016 |
| 2015 | Juri Ratas | Centre Party | 2016-2021 |
| 2019 | Juri Ratas | Centre Party | 2019-2021 |
| 2019 | Kaja Kallas | Reform Party | 2021-2024 |
| 2023 | Kaja Kallas | Reform Party | 2023-present |

### Major Political Parties

| Party | Ideology | Party Index (MD) |
|-------|----------|------------------|
| Estonian Reform Party (Reformierakond) | Liberal, Pro-EU | 2 (Liberal) |
| Isamaa (Pro Patria) | Conservative, Christian Democratic | 3 (Conservative) |
| Estonian Centre Party (Keskerakond) | Centrist, Social Liberal | 15 (Agrarian/Centre) |
| EKRE | Nationalist, Eurosceptic | 21 (Nationalist) |
| Social Democratic Party | Social Democratic | 4 (Social Democrat) |

## Issues

| Issue | Title | Description | Priority |
|-------|-------|-------------|----------|
| ISSUE-001 | Parliamentary Election Framework | Create base event structure for elections | High |
| ISSUE-002 | Election Triggers | Set up on_actions to trigger elections | High |
| ISSUE-003 | Leader Rotation | Implement leader changes based on results | High |

## Dependencies

- **TASK-001**: Political Leaders - Leaders must be defined before they can be rotated via elections

## File Structure

Files created/modified by this task:

```
events/
  Estonia.txt                           # Add election events

common/
  on_actions/
    99_EST_on_actions.txt               # New file for Estonia on_actions
  scripted_triggers/
    99_EST_scripted_triggers.txt        # New file for Estonia triggers

localisation/english/
  EST_events_l_english.yml              # New file for event localization
```

## Implementation Order

1. **ISSUE-001**: Create the base election event structure in `events/Estonia.txt`
2. **ISSUE-002**: Set up triggers in `common/on_actions/99_EST_on_actions.txt`
3. **ISSUE-003**: Implement leader rotation with `create_country_leader` effects

## Testing

To verify the implementation works:

1. Start a game as Estonia in 2000
2. Use console command `tdebug` to see event IDs
3. Fast forward to March 2003 and verify election event fires
4. Select an option and verify leader changes
5. Repeat for subsequent election years

## Notes

- Estonia uses a coalition government system; this implementation simplifies to single-party winners
- The mod's party system uses indices (2=Liberal, 3=Conservative, 15=Centre, 21=Nationalist)
- Elections should respect game rules (historical focus on/off)
- AI should make reasonable choices weighted toward historical outcomes
