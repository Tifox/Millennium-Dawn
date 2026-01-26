# ISSUE-003: Defense League Expansion

## Summary
Add an event for the expansion of the Estonian Defense League (Kaitseliit) volunteer force.

## Context
The Estonian Defense League (Kaitseliit) is a volunteer paramilitary organization that forms a key part of Estonia's total defense concept. Membership has grown significantly, particularly after 2022.

### Historical Details
- **Organization:** Kaitseliit (Defense League)
- **Type:** Volunteer paramilitary
- **Membership:** ~26,000 volunteers (2024)
- **Role:** Territorial defense, civil defense, youth training
- **History:** Originally founded 1918, re-established 1990
- **Post-2022:** Significant membership surge

## Requirements
- [ ] Create Defense League expansion event
- [ ] Trigger after 2022
- [ ] Add manpower/defense bonuses
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.702 |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2022+ - Defense League Surge
country_event = {
	id = estonia.702
	title = estonia.702.t
	desc = estonia.702.d
	picture = GFX_report_event_military_training

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2022.6.1
		has_country_flag = EST_ukraine_response
		NOT = { has_country_flag = EST_defense_league_surge }
		NOT = { has_global_flag = EST_defense_league_happened }
	}

	mean_time_to_happen = {
		months = 3
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_defense_league_happened
		}
	}

	option = {
		name = estonia.702.a
		log = "[GetDateText]: [This.GetName]: event estonia.702.a executed"

		set_country_flag = EST_defense_league_surge

		add_stability = 0.02

		# Volunteer manpower
		add_manpower = 3000

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add Localization

```yml
 # Defense League Expansion
 estonia.702.t: "Defense League Membership Surges"
 estonia.702.d: "Following Russia's invasion of Ukraine, the Estonian Defense League (Kaitseliit) has experienced a dramatic surge in volunteer applications. Thousands of Estonians are signing up to join the volunteer paramilitary organization.\n\nThe Kaitseliit, with its roots in our 1918 War of Independence, forms a crucial part of our total defense concept. These volunteers receive military training and stand ready to defend Estonian territory.\n\nThis outpouring of patriotism demonstrates the Estonian people's commitment to defending their homeland."
 estonia.702.a: "The spirit of 1918 lives on."
```

## Acceptance Criteria
- [ ] Event fires after June 2022 with Ukraine response flag
- [ ] Stability and manpower bonuses applied
- [ ] All localization displays correctly

## Dependencies
- Depends on: TASK-008 ISSUE-001 (Ukraine Response)
- Blocks: None

## Testing Notes
1. Complete Ukraine response event
2. Advance to mid-2022
3. Verify Defense League event fires
