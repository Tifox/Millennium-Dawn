# ISSUE-002: Election Triggers

## Summary
Set up on_actions to automatically trigger Estonian parliamentary elections on historical dates (2003, 2007, 2011, 2015, 2019, 2023).

## Context
Estonian parliamentary elections occur every 4 years in early March. This issue creates the trigger system that fires election events on the correct historical dates. The triggers must respect game conditions (not at war, democratic government) and should work for both player and AI Estonia.

## Requirements
- [ ] Create `99_EST_on_actions.txt` file for Estonia-specific on_actions
- [ ] Add monthly check for election dates
- [ ] Trigger elections on: March 2003, March 2007, March 2011, March 2015, March 2019, March 2023
- [ ] Prevent duplicate elections with country flags
- [ ] Create scripted trigger for election eligibility

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/on_actions/99_EST_on_actions.txt` | Create | New file for Estonia on_actions |
| `common/scripted_triggers/99_EST_scripted_triggers.txt` | Create | New file for Estonia triggers |

## Implementation

### Step 1: Create Estonia On Actions File

Create new file `common/on_actions/99_EST_on_actions.txt`:

```pdx
on_actions = {
	# Estonia Monthly Actions
	on_monthly_EST = {
		effect = {
			# 2003 Parliamentary Election - March 2, 2003
			if = {
				limit = {
					date > 2003.3.1
					date < 2003.4.1
					NOT = { has_country_flag = EST_2003_election_held }
					EST_can_hold_election = yes
				}
				set_country_flag = EST_2003_election_held
				country_event = { id = est_election.1 days = 2 }
			}

			# 2007 Parliamentary Election - March 4, 2007
			if = {
				limit = {
					date > 2007.3.1
					date < 2007.4.1
					NOT = { has_country_flag = EST_2007_election_held }
					EST_can_hold_election = yes
				}
				set_country_flag = EST_2007_election_held
				country_event = { id = est_election.1 days = 4 }
			}

			# 2011 Parliamentary Election - March 6, 2011
			if = {
				limit = {
					date > 2011.3.1
					date < 2011.4.1
					NOT = { has_country_flag = EST_2011_election_held }
					EST_can_hold_election = yes
				}
				set_country_flag = EST_2011_election_held
				country_event = { id = est_election.1 days = 6 }
			}

			# 2015 Parliamentary Election - March 1, 2015
			if = {
				limit = {
					date > 2015.2.28
					date < 2015.4.1
					NOT = { has_country_flag = EST_2015_election_held }
					EST_can_hold_election = yes
				}
				set_country_flag = EST_2015_election_held
				country_event = { id = est_election.1 days = 1 }
			}

			# 2019 Parliamentary Election - March 3, 2019
			if = {
				limit = {
					date > 2019.3.1
					date < 2019.4.1
					NOT = { has_country_flag = EST_2019_election_held }
					EST_can_hold_election = yes
				}
				set_country_flag = EST_2019_election_held
				country_event = { id = est_election.1 days = 3 }
			}

			# 2023 Parliamentary Election - March 5, 2023
			if = {
				limit = {
					date > 2023.3.1
					date < 2023.4.1
					NOT = { has_country_flag = EST_2023_election_held }
					EST_can_hold_election = yes
				}
				set_country_flag = EST_2023_election_held
				country_event = { id = est_election.1 days = 5 }
			}

			# Future elections every 4 years after 2023
			if = {
				limit = {
					date > 2027.3.1
					date < 2027.4.1
					NOT = { has_country_flag = EST_2027_election_held }
					EST_can_hold_election = yes
				}
				set_country_flag = EST_2027_election_held
				country_event = { id = est_election.1 days = 7 }
			}
		}
	}

	# Estonia Weekly Actions (for snap election cooldown)
	on_weekly_EST = {
		effect = {
			# Clear snap election flag after 3 years (allows new snap elections)
			if = {
				limit = {
					has_country_flag = { flag = EST_snap_election_called days > 1095 }
				}
				clr_country_flag = EST_snap_election_called
			}
		}
	}
}
```

### Step 2: Create Estonia Scripted Triggers File

Create new file `common/scripted_triggers/99_EST_scripted_triggers.txt`:

```pdx
# Estonia Election Eligibility Trigger
EST_can_hold_election = {
	original_tag = EST
	exists = yes
	is_subject = no
	has_government = democratic
	NOT = { has_war = yes }
	NOT = { has_country_flag = EST_snap_election_called }
	NOT = { has_country_flag = EST_election_in_progress }
}

# Check if Estonia is in a democratic coalition
EST_has_democratic_government = {
	original_tag = EST
	OR = {
		has_government = democratic
		is_in_array = { ruling_party = 2 }  # Reform Party
		is_in_array = { ruling_party = 3 }  # Isamaa
		is_in_array = { ruling_party = 4 }  # Social Democrats
		is_in_array = { ruling_party = 15 } # Centre Party
	}
}

# Check if EKRE can participate in elections
EST_ekre_can_participate = {
	OR = {
		date > 2012.3.1
		has_country_flag = EST_ekre_has_formed
	}
}

# Check if Estonia has had recent election
EST_recent_election = {
	OR = {
		has_country_flag = { flag = EST_2003_election_held days < 1460 }
		has_country_flag = { flag = EST_2007_election_held days < 1460 }
		has_country_flag = { flag = EST_2011_election_held days < 1460 }
		has_country_flag = { flag = EST_2015_election_held days < 1460 }
		has_country_flag = { flag = EST_2019_election_held days < 1460 }
		has_country_flag = { flag = EST_2023_election_held days < 1460 }
		has_country_flag = { flag = EST_2027_election_held days < 1460 }
	}
}

# Trigger for government instability (can cause snap elections)
EST_government_unstable = {
	original_tag = EST
	OR = {
		has_stability < 0.3
		check_variable = { government_coalition_strength < 40 }
		AND = {
			has_country_flag = EST_election_reform_victory
			NOT = { is_in_array = { ruling_party = 2 } }
		}
		AND = {
			has_country_flag = EST_election_centre_victory
			NOT = { is_in_array = { ruling_party = 15 } }
		}
	}
}
```

### Step 3: Register Estonia On Actions

The on_actions system in Millennium Dawn uses country-specific hooks. The `on_monthly_EST` is automatically called for Estonia. Verify this works by checking `common/on_actions/MD_on_actions.txt` for the pattern.

If not already present, you may need to add to `common/on_actions/MD_on_actions.txt`:

```pdx
on_actions = {
	on_monthly = {
		effect = {
			# ... existing code ...

			# Estonia monthly actions
			if = {
				limit = { original_tag = EST }
				EST = { on_monthly_EST = yes }
			}
		}
	}
}
```

However, the standard pattern in MD is to use dedicated `on_monthly_[TAG]` hooks that are automatically called, so the `99_EST_on_actions.txt` file should work as-is.

### Step 4: Add Election Date Constants (Optional Enhancement)

For cleaner code, you can add scripted variables. Create or modify `common/scripted_variables/99_EST_scripted_variables.txt`:

```pdx
# Estonian Election Years
EST_election_2003 = 2003
EST_election_2007 = 2007
EST_election_2011 = 2011
EST_election_2015 = 2015
EST_election_2019 = 2019
EST_election_2023 = 2023
EST_election_interval = 4
```

## Acceptance Criteria
- [ ] `99_EST_on_actions.txt` file exists in `common/on_actions/`
- [ ] `99_EST_scripted_triggers.txt` file exists in `common/scripted_triggers/`
- [ ] Elections trigger in March of each historical year
- [ ] Elections do not trigger during war
- [ ] Elections do not trigger for non-democratic governments
- [ ] Elections do not fire multiple times in same year (flag protection)
- [ ] Snap elections prevent regular elections until cooldown expires

## Dependencies
- Depends on: ISSUE-001 (election events must exist)
- Blocks: ISSUE-003 (leader rotation uses election results)

## Testing

### Test 1: Historical Election Timing
```
# Console commands
tag EST
tdebug
nextyear 2003
```
Wait until March 2003 and verify `est_election.1` fires.

### Test 2: War Prevents Elections
```
tag EST
declare_war SOV
nextyear 2003
```
Verify election does NOT fire while at war.

### Test 3: Election Flag Prevention
```
tag EST
set_country_flag EST_2003_election_held
nextyear 2003
```
Verify election does not fire again.

### Test 4: Non-Democratic Government
```
tag EST
set_politics democratic no
nextyear 2003
```
Verify election does not fire for non-democratic government.

## Historical Election Dates Reference

| Year | Date | Day of Week |
|------|------|-------------|
| 2003 | March 2 | Sunday |
| 2007 | March 4 | Sunday |
| 2011 | March 6 | Sunday |
| 2015 | March 1 | Sunday |
| 2019 | March 3 | Sunday |
| 2023 | March 5 | Sunday |

Note: Estonian elections are always held on a Sunday in early March.
