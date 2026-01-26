# ISSUE-001: e-Residency Launch

## Summary
Add an event for the launch of Estonia's groundbreaking e-Residency program on December 1, 2014.

## Context
Estonia became the first country to offer e-Residency, a government-issued digital identity that allows non-residents to access Estonian services and establish EU-based companies. The program has attracted entrepreneurs, freelancers, and digital nomads worldwide.

### Historical Details
- **Launch:** December 1, 2014
- **Purpose:** Enable global entrepreneurs to access Estonian/EU business environment
- **Features:** Digital ID, company registration, banking, contracts
- **Target:** Location-independent entrepreneurs
- **Milestone:** First e-resident was UK journalist Edward Lucas

## Requirements
- [ ] Create e-Residency launch event
- [ ] Trigger after December 1, 2014
- [ ] Add minor trade/diplomatic bonuses
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.600 |
| `common/ideas/estonia.txt` | Modify | Add EST_e_residency idea |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2014 - e-Residency Program Launches
country_event = {
	id = estonia.600
	title = estonia.600.t
	desc = estonia.600.d
	picture = GFX_computer

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2014.12.1
		NOT = { has_country_flag = EST_e_residency_launched }
		NOT = { has_global_flag = EST_e_residency_happened }
	}

	mean_time_to_happen = {
		days = 7
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_e_residency_happened
		}
	}

	option = {
		name = estonia.600.a
		log = "[GetDateText]: [This.GetName]: event estonia.600.a executed"

		set_country_flag = EST_e_residency_launched

		add_political_power = 25

		# e-Residency benefits
		add_ideas = EST_e_residency

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add National Spirit

```pdx
EST_e_residency = {
	picture = generic_digital_government
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		trade_opinion_factor = 0.10
		political_power_gain = 0.03
	}
}
```

### Step 3: Add Localization

```yml
 # e-Residency Launch
 estonia.600.t: "e-Residency Program Launches"
 estonia.600.d: "Estonia has become the first country in the world to offer e-Residency - a government-issued digital identity available to anyone in the world. This groundbreaking program allows entrepreneurs and digital nomads to establish and manage EU-based businesses without physically residing in Estonia.\n\nWith our digital ID card, e-residents can sign documents, access banking services, and register companies from anywhere on Earth. This positions Estonia as the world's first 'digital nation' and opens new possibilities for our economy."
 estonia.600.a: "Welcome to the future of digital citizenship."

 EST_e_residency: "e-Residency Program"
 EST_e_residency_desc: "Estonia's e-Residency program attracts global entrepreneurs, enhancing trade relationships and administrative efficiency."
```

## Acceptance Criteria
- [ ] Event fires after December 1, 2014
- [ ] National spirit is applied
- [ ] Political power bonus received
- [ ] All localization displays correctly

## Dependencies
- Depends on: None
- Blocks: ISSUE-002 (100k milestone)

## Testing Notes
1. Start game in 2014
2. Advance to December 2014
3. Verify event fires
4. Check national spirit is applied
