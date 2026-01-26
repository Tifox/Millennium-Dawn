# ISSUE-004: Cyber Defense Response

## Summary
Add a follow-up event for the establishment of NATO Cooperative Cyber Defence Centre of Excellence (CCDCOE) in Tallinn, reflecting Estonia's response to the 2007 cyber attacks.

## Context
Following the 2007 cyber attacks, Estonia became a global leader in cyber defense. In 2008, NATO established the Cooperative Cyber Defence Centre of Excellence (CCDCOE) in Tallinn. This center has become the premier institution for cyber defense research, training, and doctrine development.

### Historical Details
- **Established:** May 14, 2008
- **Location:** Tallinn, Estonia
- **Purpose:** Cyber defense research, training, and doctrine
- **Achievements:** Tallinn Manual on international law in cyberspace
- **Exercises:** Annual "Locked Shields" cyber defense exercise

## Requirements
- [ ] Create follow-up event after 2007 cyber attacks
- [ ] Trigger after May 2008
- [ ] Require EST_2007_cyber_attacks flag
- [ ] Grant permanent cyber defense bonus
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.304 |
| `common/ideas/estonia.txt` | Modify | Add permanent cyber defense idea |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2008 - NATO Cyber Defence Centre Established
country_event = {
	id = estonia.304
	title = estonia.304.t
	desc = estonia.304.d
	picture = GFX_computer

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2008.5.14
		has_country_flag = EST_2007_cyber_attacks
		NOT = { has_country_flag = EST_ccdcoe_established }
		NOT = { has_global_flag = EST_ccdcoe_happened }
	}

	mean_time_to_happen = {
		days = 7
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_ccdcoe_happened
		}
	}

	option = {
		name = estonia.304.a
		log = "[GetDateText]: [This.GetName]: event estonia.304.a executed"

		set_country_flag = EST_ccdcoe_established

		add_political_power = 50
		add_stability = 0.03

		# Permanent cyber defense capability
		add_ideas = EST_cyber_defense_pioneer

		# Improved relations with NATO allies
		every_country = {
			limit = {
				is_in_faction_with = EST
			}
			add_opinion_modifier = { target = EST modifier = EST_cyber_defense_hub }
		}

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add National Spirit

Already defined in ISSUE-001, verify EST_cyber_defense_pioneer exists:

```pdx
EST_cyber_defense_pioneer = {
	picture = generic_cyber_security
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		stability_factor = 0.03
		research_speed_factor = 0.02
	}
}
```

### Step 3: Add Opinion Modifier

```pdx
EST_cyber_defense_hub = {
	value = 10
}
```

### Step 4: Add Localization

```yml
 # NATO Cyber Defence Centre
 estonia.304.t: "NATO Cyber Defence Centre Established"
 estonia.304.d: "In response to the devastating cyber attacks we experienced in 2007, NATO has established the Cooperative Cyber Defence Centre of Excellence here in Tallinn. This center will serve as the alliance's premier institution for cyber defense research, training, and doctrine development.\n\nEstonia has transformed tragedy into opportunity, becoming a global leader in cyber defense. The lessons learned from our experience will help protect democracies worldwide from similar attacks."
 estonia.304.a: "Estonia leads the way in cyber defense."

 EST_cyber_defense_hub: "Cyber Defense Hub"
```

## Acceptance Criteria
- [ ] Event fires after May 2008 if 2007 cyber attack event occurred
- [ ] Event does not fire if cyber attack event was skipped
- [ ] Permanent national spirit is applied
- [ ] NATO ally opinion improves
- [ ] All localization displays correctly

## Dependencies
- Depends on: ISSUE-001 (2007 Cyber Attacks must have fired)
- Blocks: None

## Testing Notes
1. Ensure 2007 cyber attack event fires first
2. Advance to May 2008
3. Verify follow-up event fires
4. Verify permanent idea is applied
5. Test that event doesn't fire without prerequisite
