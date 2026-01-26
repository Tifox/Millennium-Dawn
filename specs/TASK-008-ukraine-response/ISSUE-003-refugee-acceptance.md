# ISSUE-003: Refugee Acceptance

## Summary
Add an event for Estonia accepting Ukrainian refugees following the 2022 invasion.

## Context
Estonia accepted a significant number of Ukrainian refugees relative to its small population. By 2023, approximately 40,000 Ukrainians had arrived in Estonia (a country of 1.3 million), representing one of the highest per-capita acceptance rates in Europe.

### Historical Details
- **Peak arrivals:** March-April 2022
- **Total by 2023:** ~40,000 refugees
- **Population impact:** ~3% of total population
- **Integration:** Fast-tracked work permits, temporary protection
- **Solidarity:** Strong public support for refugees

## Requirements
- [ ] Create refugee acceptance event
- [ ] Trigger after Ukraine response
- [ ] Include stability and manpower effects
- [ ] Add Ukraine opinion bonus
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.501 |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2022 - Ukrainian Refugees Arrive
country_event = {
	id = estonia.501
	title = estonia.501.t
	desc = estonia.501.d
	picture = GFX_report_event_generic_refugees

	fire_only_once = yes

	trigger = {
		tag = EST
		has_country_flag = EST_ukraine_response
		date > 2022.3.1
		NOT = { has_country_flag = EST_refugees_accepted }
		NOT = { has_global_flag = EST_refugees_happened }
	}

	mean_time_to_happen = {
		days = 14
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_refugees_happened
		}
	}

	# Option A: Welcome all refugees
	option = {
		name = estonia.501.a
		log = "[GetDateText]: [This.GetName]: event estonia.501.a executed"

		set_country_flag = EST_refugees_accepted
		set_country_flag = EST_generous_refugee_policy

		add_stability = 0.02
		add_political_power = -25

		# Manpower from refugees
		add_manpower = 5000

		# Strong Ukraine relations
		UKR = {
			add_opinion_modifier = { target = EST modifier = EST_refugee_sanctuary }
		}

		ai_chance = { base = 80 }
	}

	# Option B: Limited acceptance
	option = {
		name = estonia.501.b
		log = "[GetDateText]: [This.GetName]: event estonia.501.b executed"

		set_country_flag = EST_refugees_accepted

		add_manpower = 2000

		UKR = {
			add_opinion_modifier = { target = EST modifier = EST_refugee_support }
		}

		ai_chance = { base = 20 }
	}
}
```

### Step 2: Add Opinion Modifiers

```pdx
EST_refugee_sanctuary = {
	value = 50
}

EST_refugee_support = {
	value = 25
}
```

### Step 3: Add Localization

```yml
 # Refugee Acceptance
 estonia.501.t: "Ukrainian Refugees Seek Shelter"
 estonia.501.d: "As Russian forces continue their assault on Ukraine, tens of thousands of Ukrainians are fleeing the violence. Many are arriving in Estonia, seeking safety and temporary shelter.\n\nOur small nation faces a decision about how generously to welcome these refugees. With our population of only 1.3 million, even modest refugee numbers represent a significant demographic impact. However, our own history of occupation gives us deep empathy for their plight.\n\nThe government is preparing emergency measures for housing, work permits, and integration assistance."
 estonia.501.a: "Open our doors wide - they are our brothers and sisters."
 estonia.501.b: "Accept refugees within our capacity."

 EST_refugee_sanctuary: "Refugee Sanctuary"
 EST_refugee_support: "Refugee Support"
```

## Acceptance Criteria
- [ ] Event fires after Ukraine response and March 2022
- [ ] Both options provide different effects
- [ ] Manpower bonuses are applied
- [ ] Ukraine opinion improves
- [ ] All localization displays correctly

## Dependencies
- Depends on: ISSUE-001 (Ukraine Response)
- Blocks: None

## Testing Notes
1. Complete Ukraine response event
2. Advance to March 2022
3. Verify refugee event fires
4. Check manpower and opinion effects
