# ISSUE-002: GPS Jamming Events

## Summary
Add events for Russian GPS jamming interference affecting Estonia and the Baltic region, particularly intensifying from 2024 onwards.

## Context
Russia has conducted extensive GPS jamming operations in the Baltic region as part of its hybrid warfare strategy. These operations disrupt civilian aviation, maritime navigation, and military communications. The jamming intensified significantly following Russia's invasion of Ukraine in 2022.

### Historical Details
- **Ongoing:** GPS/GNSS jamming detected regularly since 2022
- **2024 Peak:** Significant increase in jamming incidents
- **Targets:** Civilian aircraft, maritime vessels, emergency services
- **Source:** Traced to Kaliningrad and Russian Baltic Fleet
- **Impact:** Flight disruptions, navigation hazards, emergency service interference

## Requirements
- [ ] Create country event for GPS jamming incidents
- [ ] Trigger after 2024 with appropriate conditions
- [ ] Fire periodically (not fire_only_once)
- [ ] Add minor stability impact
- [ ] Include option to invest in countermeasures
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.301 |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2024+ - GPS Jamming Incidents
country_event = {
	id = estonia.301
	title = estonia.301.t
	desc = estonia.301.d
	picture = GFX_military_technology

	trigger = {
		tag = EST
		date > 2024.1.1
		NOT = { has_country_flag = EST_gps_jamming_recent }
		SOV = {
			OR = {
				has_war = yes
				has_country_flag = russia_ukraine_war
			}
		}
	}

	mean_time_to_happen = {
		months = 18
		modifier = {
			factor = 0.5
			date > 2024.6.1
		}
	}

	immediate = {
		hidden_effect = {
			set_country_flag = {
				flag = EST_gps_jamming_recent
				days = 180
			}
		}
	}

	# Option A: Invest in countermeasures
	option = {
		name = estonia.301.a
		log = "[GetDateText]: [This.GetName]: event estonia.301.a executed"

		add_political_power = -25

		ai_chance = { base = 60 }
	}

	# Option B: Raise at NATO
	option = {
		name = estonia.301.b
		log = "[GetDateText]: [This.GetName]: event estonia.301.b executed"

		add_stability = -0.02

		every_country = {
			limit = {
				is_in_faction_with = EST
			}
			add_opinion_modifier = { target = EST modifier = EST_hybrid_threat_solidarity }
		}

		ai_chance = { base = 40 }
	}
}
```

### Step 2: Add Localization

```yml
 # GPS Jamming Events
 estonia.301.t: "GPS Jamming Detected"
 estonia.301.d: "Our defense forces and civilian aviation authorities have detected significant GPS jamming signals originating from Russian territory. Commercial flights are experiencing navigation disruptions, and emergency services report intermittent GPS failures.\n\nThis is part of Russia's ongoing hybrid warfare campaign against the Baltic states. We must decide how to respond to this provocation."
 estonia.301.a: "Invest in signal resilience systems."
 estonia.301.b: "Raise this issue with our NATO allies."

 EST_hybrid_threat_solidarity: "Solidarity Against Hybrid Threats"
```

## Acceptance Criteria
- [ ] Event can fire after 2024 when Russia is at war
- [ ] Event respects 180-day cooldown between occurrences
- [ ] Both options work correctly
- [ ] Localization displays correctly

## Dependencies
- Depends on: TASK-008 (Ukraine response for war flag checks)
- Blocks: None

## Testing Notes
1. Start game in 2024 with Russia-Ukraine war active
2. Fast forward several months
3. Verify event fires
4. Verify cooldown prevents immediate refire
