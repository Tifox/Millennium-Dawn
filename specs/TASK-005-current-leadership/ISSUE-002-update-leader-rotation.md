# ISSUE-002: Update Leader Rotation Effects

## Summary
Update the leader rotation effects in `EST_political_leaders.txt` to properly transition from Kaja Kallas to Kristen Michal after July 2024.

## Context
The scripted effects file already has Kristen Michal defined as liberalism_leader = 3, but the date trigger for when the transition should occur may need verification or updating. Kaja Kallas resigned as PM on July 23, 2024, to become EU High Representative.

### Timeline
- **Kaja Kallas:** January 26, 2021 - July 23, 2024
- **Kristen Michal:** July 23, 2024 - present

## Requirements
- [ ] Verify date trigger exists for Kallas → Michal transition
- [ ] Add date trigger if missing: `date < 2024.7.23` in Kallas block
- [ ] Ensure Michal block triggers correctly after that date
- [ ] Test leader rotation works correctly

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/scripted_effects/EST_political_leaders.txt` | Modify | Update/add date trigger for Michal |

## Implementation

### Current State Analysis

The current structure in EST_political_leaders.txt for liberalism:

```pdx
# liberalism_leader = 0: Siim Kallas (date < 2014.3.26)
# liberalism_leader = 1: Taavi Rõivas (date < 2016.3.11)
# liberalism_leader = 2: Kaja Kallas (currently no end date)
# liberalism_leader = 3: Kristen Michal (exists but may not trigger)
```

### Required Changes

Update the Kaja Kallas block (liberalism_leader = 2) to add an end date:

```pdx
if = { limit = { check_variable = { liberalism_leader = 2 } NOT = { check_variable = { b = 1 } } }
	add_to_variable = { liberalism_leader = 1 }
	hidden_effect = { kill_country_leader = yes }

	create_country_leader = {
		name = "Kaja Kallas"
		picture = "kaja_kallas.dds"
		ideology = liberalism
		traits = {
			western_liberalism
		}
	}

	if = { limit = { has_country_flag = do_not_retire } subtract_from_variable = { liberalism_leader = 1 } }
	if = { limit = { date < 2024.7.23 } set_temp_variable = { b = 1 } } # ADD THIS LINE
}
```

This ensures that when the date passes July 23, 2024, the next leader rotation will proceed to Kristen Michal (liberalism_leader = 3).

## Acceptance Criteria
- [ ] Kaja Kallas remains as leader until July 2024
- [ ] Kristen Michal becomes leader after July 2024
- [ ] Leader rotation respects election cycle/government change mechanics
- [ ] No error.log entries related to leader rotation

## Dependencies
- Depends on: ISSUE-001 (Michal character must exist)
- Blocks: None

## Testing Notes
1. Start as Estonia with liberalism ruling party
2. Observe leader through 2021-2024 period
3. After July 2024, trigger a government change or leader rotation
4. Verify Michal appears as the new leader
5. Test with console: `set_variable = { liberalism_leader = 2 }` then trigger `set_leader_EST`
