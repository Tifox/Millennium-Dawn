# ISSUE-005: Enhanced Forward Presence Upgrade

## Summary
Add an event for the upgrade of NATO's Enhanced Forward Presence (eFP) to Forward Land Forces (FLF) in the Baltic states.

## Context
Following the 2022 invasion, NATO upgraded its Baltic presence from Enhanced Forward Presence (battalion-sized battlegroups) to Forward Land Forces (brigade-capable forces). The UK-led battlegroup in Estonia was reinforced and upgraded.

### Historical Details
- **Original eFP:** Established 2017, ~1,000 troops
- **UK Leadership:** UK leads Estonia battlegroup
- **2022 Upgrade:** Reinforcement and upgrade to brigade capability
- **Forward Land Forces:** New designation reflecting increased posture
- **Continuous Presence:** Shift from rotational to more permanent

## Requirements
- [ ] Create eFP upgrade event
- [ ] Trigger after Ukraine response and 2022
- [ ] Include military bonuses
- [ ] Add NATO opinion effects
- [ ] Add national spirit for NATO presence
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.503 |
| `common/ideas/estonia.txt` | Modify | Add EST_nato_forward_forces idea |
| `common/opinion_modifiers/Estonia.txt` | Modify | Add NATO bonding modifier |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2022 - NATO Upgrades Baltic Presence
country_event = {
	id = estonia.503
	title = estonia.503.t
	desc = estonia.503.d
	picture = GFX_report_event_military_parade

	fire_only_once = yes

	trigger = {
		tag = EST
		has_country_flag = EST_ukraine_response
		date > 2022.6.1
		is_in_faction = yes
		NOT = { has_country_flag = EST_nato_upgrade }
		NOT = { has_global_flag = EST_nato_upgrade_happened }
	}

	mean_time_to_happen = {
		days = 14
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_nato_upgrade_happened
		}
	}

	option = {
		name = estonia.503.a
		log = "[GetDateText]: [This.GetName]: event estonia.503.a executed"

		set_country_flag = EST_nato_upgrade

		add_stability = 0.05
		add_political_power = 50

		# NATO Forward Forces presence
		add_ideas = EST_nato_forward_forces

		# Improved UK relations (battlegroup leader)
		ENG = {
			add_opinion_modifier = { target = EST modifier = EST_battlegroup_host }
		}

		# Improved NATO relations
		every_country = {
			limit = {
				is_in_faction_with = EST
			}
			add_opinion_modifier = { target = EST modifier = EST_forward_defense_partner }
		}

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add National Spirit

```pdx
EST_nato_forward_forces = {
	picture = generic_nato_presence
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		stability_factor = 0.05
		army_org_factor = 0.10
		planning_speed = 0.10
		army_defence_factor = 0.05
	}
}
```

### Step 3: Add Opinion Modifiers

```pdx
EST_battlegroup_host = {
	value = 30
}

EST_forward_defense_partner = {
	value = 15
}
```

### Step 4: Add Localization

```yml
 # eFP Upgrade
 estonia.503.t: "NATO Upgrades Baltic Defense"
 estonia.503.d: "In response to Russia's invasion of Ukraine, NATO has announced a major upgrade to its presence in the Baltic states. The Enhanced Forward Presence battlegroups will be upgraded to brigade-capable Forward Land Forces, representing a fundamental shift in the alliance's eastern defense posture.\n\nThe UK-led battlegroup in Estonia will be reinforced and prepared for rapid expansion to brigade strength if needed. This represents NATO's strongest commitment yet to Baltic defense.\n\nAdditional troops, equipment, and pre-positioned supplies will enhance our defensive capabilities significantly."
 estonia.503.a: "NATO's commitment to our defense is ironclad."

 EST_nato_forward_forces: "NATO Forward Land Forces"
 EST_nato_forward_forces_desc: "NATO has upgraded its Baltic presence from Enhanced Forward Presence to Forward Land Forces, with brigade-capable forces ready to defend Estonian territory."

 EST_battlegroup_host: "Battlegroup Host"
 EST_forward_defense_partner: "Forward Defense Partner"
```

## Acceptance Criteria
- [ ] Event fires after June 2022 with Ukraine response flag
- [ ] Requires faction membership
- [ ] National spirit is applied
- [ ] UK and NATO opinion bonuses applied
- [ ] Stability and political power bonuses applied
- [ ] All localization displays correctly

## Dependencies
- Depends on: ISSUE-001 (Ukraine Response)
- Blocks: None

## Testing Notes
1. Complete Ukraine response event
2. Verify Estonia is in faction
3. Advance to June 2022
4. Verify NATO upgrade event fires
5. Check national spirit and opinion effects
