# ISSUE-004: Air Defense Acquisition

## Summary
Add an event for Estonia's acquisition of modern air defense systems (IRIS-T, NASAMS).

## Context
Estonia has invested heavily in air defense following Russia's 2022 invasion of Ukraine. The country has purchased IRIS-T SLM and NASAMS systems to protect against missile and aircraft threats.

### Historical Details
- **IRIS-T SLM:** German medium-range system
- **NASAMS:** Norwegian/American system
- **Purchase Timeline:** 2022-2024
- **Purpose:** Protect against Russian cruise missiles, aircraft
- **Investment:** Hundreds of millions of euros
- **Context:** Lessons from Ukraine war

## Requirements
- [ ] Create air defense acquisition event
- [ ] Trigger after 2023
- [ ] Add air defense bonuses
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.703 |
| `common/ideas/estonia.txt` | Modify | Add EST_modern_air_defense idea |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2023+ - Modern Air Defense Systems
country_event = {
	id = estonia.703
	title = estonia.703.t
	desc = estonia.703.d
	picture = GFX_report_event_anti_air

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2023.6.1
		has_country_flag = EST_ukraine_response
		NOT = { has_country_flag = EST_air_defense_acquired }
		NOT = { has_global_flag = EST_air_defense_happened }
	}

	mean_time_to_happen = {
		months = 6
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_air_defense_happened
		}
	}

	option = {
		name = estonia.703.a
		log = "[GetDateText]: [This.GetName]: event estonia.703.a executed"

		set_country_flag = EST_air_defense_acquired

		add_political_power = -75

		# Modern air defense
		add_ideas = EST_modern_air_defense

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add National Spirit

```pdx
EST_modern_air_defense = {
	picture = generic_air_defense
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		air_defence_speed_factor = 0.15
		air_interception_agility_factor = 0.10
		static_anti_air_damage_factor = 0.20
	}
}
```

### Step 3: Add Localization

```yml
 # Air Defense Acquisition
 estonia.703.t: "Modern Air Defense Systems Acquired"
 estonia.703.d: "Estonia has taken delivery of advanced air defense systems to protect our airspace and critical infrastructure. The IRIS-T SLM and NASAMS systems represent a major upgrade to our air defense capabilities.\n\nLessons from Ukraine have shown the critical importance of air defense against Russian cruise missiles and aircraft. These systems will provide vital protection for our cities, military bases, and key infrastructure.\n\nThis represents one of the largest military investments in Estonian history."
 estonia.703.a: "Our skies will be defended."

 EST_modern_air_defense: "Modern Air Defense Network"
 EST_modern_air_defense_desc: "Estonia has deployed IRIS-T and NASAMS air defense systems, significantly enhancing protection against aerial threats."
```

## Acceptance Criteria
- [ ] Event fires after mid-2023 with Ukraine response flag
- [ ] National spirit is applied with air defense bonuses
- [ ] Political power cost applied
- [ ] All localization displays correctly

## Dependencies
- Depends on: TASK-008 ISSUE-001 (Ukraine Response)
- Blocks: None

## Testing Notes
1. Complete Ukraine response event
2. Advance to late 2023
3. Verify air defense event fires
4. Check national spirit bonuses
