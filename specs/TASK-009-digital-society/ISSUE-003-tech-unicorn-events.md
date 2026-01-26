# ISSUE-003: Tech Unicorn Events

## Summary
Add events for Estonian tech companies achieving "unicorn" status (valuation over $1 billion).

## Context
Despite its small size, Estonia has produced multiple tech unicorns, the highest number per capita in Europe. These include Skype, Wise (TransferWise), Bolt, and Pipedrive.

### Historical Details

#### Skype
- **Founded:** 2003
- **Unicorn:** Early pioneer
- **Acquired:** By Microsoft (2011) for $8.5 billion

#### Wise (TransferWise)
- **Founded:** 2011
- **Unicorn Status:** 2021
- **Valuation:** $11 billion at IPO

#### Bolt
- **Founded:** 2013
- **Unicorn Status:** 2021
- **Business:** Ride-hailing, food delivery

#### Pipedrive
- **Founded:** 2010
- **Unicorn Status:** 2020
- **Business:** Sales CRM software

## Requirements
- [ ] Create composite event for tech success
- [ ] Trigger after 2020
- [ ] Add research and factory bonuses
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.602 |
| `common/ideas/estonia.txt` | Modify | Add EST_tech_hub idea |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2020+ - Estonian Tech Unicorns
country_event = {
	id = estonia.602
	title = estonia.602.t
	desc = estonia.602.d
	picture = GFX_computer

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2020.1.1
		NOT = { has_country_flag = EST_tech_unicorns }
		NOT = { has_global_flag = EST_unicorns_happened }
	}

	mean_time_to_happen = {
		months = 6
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_unicorns_happened
		}
	}

	option = {
		name = estonia.602.a
		log = "[GetDateText]: [This.GetName]: event estonia.602.a executed"

		set_country_flag = EST_tech_unicorns

		add_political_power = 35

		# Tech hub status
		add_ideas = EST_tech_hub

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add National Spirit

```pdx
EST_tech_hub = {
	picture = generic_tech_innovation
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		research_speed_factor = 0.03
		production_factory_efficiency_gain_factor = 0.05
	}
}
```

### Step 3: Add Localization

```yml
 # Tech Unicorns
 estonia.602.t: "Estonia: Land of Tech Unicorns"
 estonia.602.d: "Estonia has achieved something remarkable for a nation of 1.3 million people: we have produced more tech 'unicorns' per capita than any other country in Europe. Companies like Skype, Wise, Bolt, and Pipedrive have all achieved valuations exceeding $1 billion.\n\nOur investment in digital infrastructure, tech education, and startup-friendly policies has paid off spectacularly. Estonia is now recognized globally as a tech innovation hub, punching far above its weight in the global technology economy."
 estonia.602.a: "Estonian innovation leads the world."

 EST_tech_hub: "Tech Innovation Hub"
 EST_tech_hub_desc: "Estonia's tech sector has produced multiple billion-dollar companies, attracting talent and investment while boosting research capabilities."
```

## Acceptance Criteria
- [ ] Event fires after 2020
- [ ] National spirit is applied with research/production bonuses
- [ ] All localization displays correctly

## Dependencies
- Depends on: None
- Blocks: None

## Testing Notes
1. Advance to 2020
2. Verify event fires
3. Check national spirit bonuses
