# ISSUE-001: NATO Enhanced Forward Presence

## Summary
Add an event for the arrival of NATO's Enhanced Forward Presence (eFP) battlegroup in Estonia in 2017.

## Context
As part of NATO's response to Russia's 2014 annexation of Crimea, the alliance established Enhanced Forward Presence battlegroups in the Baltic states and Poland. The UK leads the battlegroup in Estonia, which became operational in 2017.

### Historical Details
- **Established:** 2017 Warsaw Summit decision
- **Leader Nation:** United Kingdom
- **Size:** ~1,000 troops
- **Location:** Tapa military base
- **Contributing Nations:** UK, France, Denmark, others
- **Purpose:** Deterrence and reassurance

## Requirements
- [ ] Create eFP arrival event
- [ ] Trigger after 2017
- [ ] Add military/stability bonuses
- [ ] Improve UK relations
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.700 |
| `common/ideas/estonia.txt` | Modify | Add EST_nato_efp idea |
| `common/opinion_modifiers/Estonia.txt` | Modify | Add NATO bonding modifier |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2017 - NATO Enhanced Forward Presence Arrives
country_event = {
	id = estonia.700
	title = estonia.700.t
	desc = estonia.700.d
	picture = GFX_report_event_military_parade

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2017.4.1
		is_in_faction = yes
		NOT = { has_country_flag = EST_efp_arrived }
		NOT = { has_global_flag = EST_efp_happened }
	}

	mean_time_to_happen = {
		days = 14
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_efp_happened
		}
	}

	option = {
		name = estonia.700.a
		log = "[GetDateText]: [This.GetName]: event estonia.700.a executed"

		set_country_flag = EST_efp_arrived

		add_stability = 0.05
		add_political_power = 25

		# NATO presence
		add_ideas = EST_nato_efp

		# Improved UK relations
		ENG = {
			add_opinion_modifier = { target = EST modifier = EST_efp_partner }
		}

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add National Spirit

```pdx
EST_nato_efp = {
	picture = generic_nato_presence
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		stability_factor = 0.03
		army_org_factor = 0.05
		planning_speed = 0.05
	}
}
```

### Step 3: Add Opinion Modifier

```pdx
EST_efp_partner = {
	value = 25
}
```

### Step 4: Add Localization

```yml
 # NATO eFP
 estonia.700.t: "NATO Battlegroup Arrives in Estonia"
 estonia.700.d: "NATO's Enhanced Forward Presence has arrived in Estonia. The UK-led multinational battlegroup, stationed at Tapa, represents NATO's strongest commitment to Baltic security since the Cold War.\n\nApproximately 1,000 troops from Britain, France, Denmark, and other allied nations will maintain a continuous presence on Estonian soil. This marks a historic moment: for the first time, allied forces are permanently stationed in Estonia.\n\nThe battlegroup serves as a deterrent tripwire, ensuring that any attack on Estonia would immediately involve multiple NATO nations."
 estonia.700.a: "NATO stands with Estonia."

 EST_nato_efp: "NATO Enhanced Forward Presence"
 EST_nato_efp_desc: "A UK-led NATO battlegroup provides continuous military presence and training, enhancing Estonian defense capabilities."

 EST_efp_partner: "eFP Partner"
```

## Acceptance Criteria
- [ ] Event fires after April 2017 if in faction
- [ ] National spirit is applied
- [ ] UK opinion bonus applied
- [ ] All localization displays correctly

## Dependencies
- Depends on: Estonia being in NATO faction
- Blocks: None

## Testing Notes
1. Verify Estonia is in NATO faction
2. Advance to 2017
3. Verify event fires
4. Check national spirit and opinion effects
