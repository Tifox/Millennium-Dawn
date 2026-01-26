# ISSUE-003: EKRE Government Collapse

## Summary
Add an event for the collapse of the EKRE coalition government in January 2021 following a corruption scandal.

## Context
The Ratas government fell in January 2021 when the Centre Party became embroiled in a corruption scandal involving the Porto Franco real estate development. PM Jüri Ratas resigned on January 13, 2021, leading to the collapse of the coalition. This ended EKRE's time in government.

### Historical Details
- **Date:** January 13, 2021
- **Cause:** Porto Franco corruption scandal
- **Resignation:** PM Jüri Ratas (Centre Party)
- **Result:** Coalition collapsed, EKRE out of government
- **Successor:** Kaja Kallas (Reform Party) formed new government
- **EKRE Impact:** Lost all ministerial positions

## Requirements
- [ ] Create event for government collapse
- [ ] Trigger after January 13, 2021
- [ ] Require EKRE government flag
- [ ] Remove EKRE government national spirit
- [ ] Add stability recovery
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.402 |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2021 - EKRE Government Collapse
country_event = {
	id = estonia.402
	title = estonia.402.t
	desc = estonia.402.d
	picture = GFX_report_event_generic_parliament

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2021.1.13
		has_country_flag = EST_ekre_government
		NOT = { has_country_flag = EST_ekre_government_collapsed }
		NOT = { has_global_flag = EST_ekre_collapse_happened }
	}

	mean_time_to_happen = {
		days = 3
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_ekre_collapse_happened
		}
	}

	option = {
		name = estonia.402.a
		log = "[GetDateText]: [This.GetName]: event estonia.402.a executed"

		set_country_flag = EST_ekre_government_collapsed

		# Remove EKRE government effects
		remove_ideas = EST_ekre_government

		# Stability recovery from end of controversy
		add_stability = 0.05
		add_political_power = -50

		# EU relations begin to recover
		every_country = {
			limit = {
				has_opinion_modifier = { target = EST modifier = EST_far_right_government }
			}
			remove_opinion_modifier = { target = EST modifier = EST_far_right_government }
		}

		# Liberalism returns to power historically
		add_popularity = {
			ideology = liberalism
			popularity = 0.03
		}

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add Localization

```yml
 # EKRE Government Collapse
 estonia.402.t: "Coalition Government Collapses"
 estonia.402.d: "Prime Minister Jüri Ratas has resigned following a corruption scandal involving the Porto Franco real estate development. The Centre Party's involvement in the scandal has brought down the entire coalition government, ending EKRE's controversial time in power.\n\nThe Reform Party, led by Kaja Kallas, is expected to form a new government. This marks the end of a turbulent chapter in Estonian politics, as the far-right EKRE loses its ministerial positions.\n\nEuropean partners have quietly expressed relief at the development."
 estonia.402.a: "The coalition crumbles."
```

## Acceptance Criteria
- [ ] Event fires after January 13, 2021
- [ ] Requires EKRE government flag
- [ ] EKRE government spirit removed
- [ ] Stability bonus applied
- [ ] EU opinion penalties removed
- [ ] Liberalism popularity increases
- [ ] All localization displays correctly

## Dependencies
- Depends on: ISSUE-002 (EKRE Government)
- Blocks: None

## Testing Notes
1. Ensure EKRE government event fires first
2. Advance to January 2021
3. Verify collapse event fires
4. Check idea is removed
5. Verify EU opinion modifiers are cleared
