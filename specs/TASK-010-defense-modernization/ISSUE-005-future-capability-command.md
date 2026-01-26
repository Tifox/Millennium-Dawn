# ISSUE-005: Future Capability Command

## Summary
Add an event for the establishment of Estonia's Future Capability Command for defense innovation.

## Context
Estonia is establishing a Future Capability Command to drive innovation in defense technology and leverage the country's tech sector expertise for military applications.

### Historical Details
- **Established:** 2025
- **Purpose:** Defense innovation and emerging technology
- **Focus Areas:** AI, drones, cyber, autonomous systems
- **Connection:** Leverages Estonia's tech startup ecosystem
- **Model:** Similar to US Defense Innovation Unit

## Requirements
- [ ] Create Future Capability Command event
- [ ] Trigger after 2025
- [ ] Add research/innovation bonuses
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.704 |
| `common/ideas/estonia.txt` | Modify | Add EST_future_capability idea |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2025 - Future Capability Command Established
country_event = {
	id = estonia.704
	title = estonia.704.t
	desc = estonia.704.d
	picture = GFX_military_technology

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2025.1.1
		NOT = { has_country_flag = EST_future_capability }
		NOT = { has_global_flag = EST_future_capability_happened }
	}

	mean_time_to_happen = {
		months = 3
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_future_capability_happened
		}
	}

	option = {
		name = estonia.704.a
		log = "[GetDateText]: [This.GetName]: event estonia.704.a executed"

		set_country_flag = EST_future_capability

		add_political_power = 25

		# Innovation command
		add_ideas = EST_future_capability

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add National Spirit

```pdx
EST_future_capability = {
	picture = generic_tech_innovation
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		research_speed_factor = 0.05
		production_factory_efficiency_gain_factor = 0.05
	}
}
```

### Step 3: Add Localization

```yml
 # Future Capability Command
 estonia.704.t: "Future Capability Command Established"
 estonia.704.d: "Estonia has established a new Future Capability Command to drive innovation in defense technology. This unit will leverage our world-class tech sector to develop cutting-edge military capabilities.\n\nFocus areas include artificial intelligence, autonomous systems, drones, and cyber capabilities. By connecting our defense forces with Estonia's thriving startup ecosystem, we aim to punch above our weight in military technology.\n\nEstonia will become a leader in defense innovation, developing solutions that can be adopted by our NATO allies."
 estonia.704.a: "Innovation is our force multiplier."

 EST_future_capability: "Future Capability Command"
 EST_future_capability_desc: "Estonia's defense innovation unit leverages the tech sector to develop advanced military capabilities."
```

## Acceptance Criteria
- [ ] Event fires after 2025
- [ ] National spirit is applied with research bonuses
- [ ] All localization displays correctly

## Dependencies
- Depends on: None
- Blocks: None

## Testing Notes
1. Advance to 2025
2. Verify event fires
3. Check national spirit bonuses
