# ISSUE-003: Border Incidents

## Summary
Add events for Russian border provocations including the Narva River buoy removal incident and airspace violations.

## Context
Russia has engaged in various border provocations with Estonia, including removing border demarcation buoys from the Narva River (2024), frequent airspace violations, and other aggressive acts designed to test Estonia's resolve and NATO's response.

### Historical Details
- **May 2024:** Russia removes border buoys from Narva River
- **Ongoing:** Regular Russian airspace violations
- **Context:** Part of broader hybrid warfare strategy
- **Response:** Estonia protests through diplomatic channels, NATO solidarity

## Requirements
- [ ] Create event for Narva buoy incident
- [ ] Create generic airspace violation event
- [ ] Include diplomatic response options
- [ ] Add opinion modifiers
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add events estonia.302, estonia.303 |
| `common/opinion_modifiers/Estonia.txt` | Modify | Add border incident modifiers |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Narva Buoy Event

```pdx
# 2024 - Narva River Buoy Incident
country_event = {
	id = estonia.302
	title = estonia.302.t
	desc = estonia.302.d
	picture = GFX_report_event_soviet_soldiers

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2024.5.1
		NOT = { has_country_flag = EST_narva_buoy_incident }
		NOT = { has_global_flag = EST_narva_buoy_happened }
	}

	mean_time_to_happen = {
		days = 7
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_narva_buoy_happened
		}
	}

	# Option A: Diplomatic protest
	option = {
		name = estonia.302.a
		log = "[GetDateText]: [This.GetName]: event estonia.302.a executed"

		set_country_flag = EST_narva_buoy_incident

		add_stability = -0.02
		add_political_power = -15

		SOV = {
			add_opinion_modifier = { target = EST modifier = EST_border_provocation_target }
		}
		EST = {
			add_opinion_modifier = { target = SOV modifier = EST_border_provocation_victim }
		}

		ai_chance = { base = 80 }
	}

	# Option B: Seek EU/NATO condemnation
	option = {
		name = estonia.302.b
		log = "[GetDateText]: [This.GetName]: event estonia.302.b executed"

		set_country_flag = EST_narva_buoy_incident

		add_stability = -0.02

		SOV = {
			add_opinion_modifier = { target = EST modifier = EST_border_provocation_target }
		}
		EST = {
			add_opinion_modifier = { target = SOV modifier = EST_border_provocation_victim }
		}

		every_country = {
			limit = {
				is_in_faction_with = EST
			}
			add_opinion_modifier = { target = EST modifier = EST_border_solidarity }
		}

		ai_chance = { base = 20 }
	}
}
```

### Step 2: Add Airspace Violation Event

```pdx
# Recurring - Russian Airspace Violations
country_event = {
	id = estonia.303
	title = estonia.303.t
	desc = estonia.303.d
	picture = GFX_military_aircraft

	trigger = {
		tag = EST
		date > 2000.1.1
		NOT = { has_country_flag = EST_airspace_violation_recent }
		SOV = {
			NOT = { has_war_with = EST }
			exists = yes
		}
	}

	mean_time_to_happen = {
		months = 24
		modifier = {
			factor = 0.5
			date > 2022.2.24
		}
		modifier = {
			factor = 0.7
			SOV = { has_war = yes }
		}
	}

	immediate = {
		hidden_effect = {
			set_country_flag = {
				flag = EST_airspace_violation_recent
				days = 365
			}
		}
	}

	option = {
		name = estonia.303.a
		log = "[GetDateText]: [This.GetName]: event estonia.303.a executed"

		add_stability = -0.01

		SOV = {
			add_opinion_modifier = { target = EST modifier = EST_airspace_violator }
		}

		ai_chance = { base = 100 }
	}
}
```

### Step 3: Add Opinion Modifiers

```pdx
EST_border_provocation_victim = {
	value = -30
	decay = 0.5
}

EST_border_provocation_target = {
	value = -15
}

EST_border_solidarity = {
	value = 10
}

EST_airspace_violator = {
	value = -10
	decay = 1
}
```

### Step 4: Add Localization

```yml
 # Border Incidents
 estonia.302.t: "Narva River Buoy Incident"
 estonia.302.d: "Russian authorities have unilaterally removed border demarcation buoys from the Narva River, which forms the border between Estonia and Russia. This provocative act challenges our territorial sovereignty and violates international agreements.\n\nThe removal of these markers, some of which have been in place for decades, appears designed to create ambiguity about the exact border location. We must respond to this provocation."
 estonia.302.a: "Lodge a formal diplomatic protest."
 estonia.302.b: "Seek EU and NATO condemnation."

 estonia.303.t: "Russian Airspace Violation"
 estonia.303.d: "Our air defense systems have detected a Russian military aircraft entering Estonian airspace without authorization or flight plan. NATO air policing fighters have been scrambled to intercept.\n\nSuch violations have become increasingly common and represent a pattern of Russian intimidation tactics."
 estonia.303.a: "Summon the Russian ambassador."
```

## Acceptance Criteria
- [ ] Narva buoy event fires once in May 2024
- [ ] Airspace violation event can fire repeatedly with cooldown
- [ ] Opinion modifiers are applied correctly
- [ ] All localization displays correctly

## Dependencies
- Depends on: None
- Blocks: None

## Testing Notes
1. Test Narva event by advancing to May 2024
2. Test airspace violation event triggers
3. Verify cooldowns work correctly
