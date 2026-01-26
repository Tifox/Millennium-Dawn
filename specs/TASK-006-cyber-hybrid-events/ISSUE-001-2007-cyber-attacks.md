# ISSUE-001: 2007 Cyber Attacks

## Summary
Add a major historical event for the 2007 cyber attacks on Estonia, the first coordinated state-level cyber attack on a NATO country.

## Context
The 2007 Estonian cyber attacks occurred from April 27 to May 18, 2007, coinciding with the relocation of the Bronze Soldier war memorial from central Tallinn. The attacks targeted Parliament, government ministries, banks, media outlets, and ISPs with coordinated DDoS attacks originating from Russian-language sources. This was a watershed moment in cyber warfare history and directly led to the establishment of NATO's cyber defense center in Tallinn.

### Historical Details
- **Date:** April 27 - May 18, 2007
- **Context:** Bronze Soldier relocation sparked Russian protests
- **Targets:** Parliament, ministries, banks, media, ISPs
- **Type:** Coordinated DDoS (Distributed Denial of Service) attacks
- **Attribution:** Russian-language origin; Russian government denied involvement
- **Impact:** First major state-level cyber attack; led to NATO Cyber Defence Centre

## Requirements
- [ ] Create country event estonia.300 for cyber attacks
- [ ] Trigger automatically after April 27, 2007
- [ ] Fire only once per game
- [ ] Add stability hit and political power cost
- [ ] Add negative opinion modifier with Russia
- [ ] Set country flag for tracking
- [ ] Add option to unlock cyber defense capability
- [ ] Add localization for all text

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.300 |
| `common/ideas/estonia.txt` | Modify | Add EST_under_cyber_attack and EST_cyber_defense_pioneer ideas |
| `common/opinion_modifiers/Estonia.txt` | Modify | Add EST_cyber_attack_victim modifier |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2007 - Estonia Cyber Attacks
country_event = {
	id = estonia.300
	title = estonia.300.t
	desc = estonia.300.d
	picture = GFX_computer

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2007.4.27
		NOT = { has_country_flag = EST_2007_cyber_attacks }
		NOT = { has_global_flag = EST_cyber_attacks_happened }
	}

	mean_time_to_happen = {
		days = 1
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_cyber_attacks_happened
		}
	}

	# Option A: Focus on defense
	option = {
		name = estonia.300.a
		log = "[GetDateText]: [This.GetName]: event estonia.300.a executed"

		set_country_flag = EST_2007_cyber_attacks

		add_stability = -0.10
		add_political_power = -75

		# Temporary cyber attack disruption
		add_timed_idea = {
			idea = EST_under_cyber_attack
			days = 21
		}

		# Worsen relations with Russia
		SOV = {
			add_opinion_modifier = { target = EST modifier = EST_cyber_attack_perpetrator }
		}
		EST = {
			add_opinion_modifier = { target = SOV modifier = EST_cyber_attack_victim }
		}

		ai_chance = { base = 70 }
	}

	# Option B: Seek international support
	option = {
		name = estonia.300.b
		log = "[GetDateText]: [This.GetName]: event estonia.300.b executed"

		set_country_flag = EST_2007_cyber_attacks
		set_country_flag = EST_seeking_cyber_support

		add_stability = -0.10
		add_political_power = -50

		add_timed_idea = {
			idea = EST_under_cyber_attack
			days = 21
		}

		SOV = {
			add_opinion_modifier = { target = EST modifier = EST_cyber_attack_perpetrator }
		}
		EST = {
			add_opinion_modifier = { target = SOV modifier = EST_cyber_attack_victim }
		}

		# Better relations with NATO allies
		every_country = {
			limit = {
				OR = {
					tag = USA
					tag = GER
					tag = FRA
					tag = ENG
				}
			}
			add_opinion_modifier = { target = EST modifier = EST_cyber_solidarity }
		}

		ai_chance = { base = 30 }
	}
}
```

### Step 2: Add National Spirits to common/ideas/estonia.txt

```pdx
EST_under_cyber_attack = {
	picture = generic_cyber_security
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		stability_factor = -0.05
		political_power_factor = -0.10
		production_factory_efficiency_gain_factor = -0.05
	}
}

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

### Step 3: Add Opinion Modifiers to common/opinion_modifiers/Estonia.txt

```pdx
EST_cyber_attack_victim = {
	value = -50
	decay = 1
	min_trust = -50
}

EST_cyber_attack_perpetrator = {
	value = -25
	decay = 0.5
}

EST_cyber_solidarity = {
	value = 15
}
```

### Step 4: Add Localization

```yml
 # 2007 Cyber Attacks
 estonia.300.t: "Estonia Under Cyber Attack"
 estonia.300.d: "In the wake of our decision to relocate the Bronze Soldier memorial, Estonia is experiencing an unprecedented wave of cyber attacks. Government websites, banks, media outlets, and internet service providers are being overwhelmed by coordinated distributed denial-of-service attacks. Parliament's email system is paralyzed, online banking is disrupted, and news sites are inaccessible.\n\nThe attacks appear to originate from Russian-language sources, though official attribution remains difficult. This represents the first major state-level cyber attack in modern history, and our response will shape the future of cyber defense."
 estonia.300.a: "We must strengthen our cyber defenses."
 estonia.300.b: "Rally international support against this aggression."

 EST_under_cyber_attack: "Under Cyber Attack"
 EST_under_cyber_attack_desc: "Estonia is experiencing coordinated cyber attacks targeting critical infrastructure and government services."

 EST_cyber_defense_pioneer: "Cyber Defense Pioneer"
 EST_cyber_defense_pioneer_desc: "Estonia has become a world leader in cyber defense following the 2007 attacks, hosting NATO's Cyber Defence Centre of Excellence."
```

## Acceptance Criteria
- [ ] Event fires automatically after April 27, 2007 when playing as Estonia
- [ ] Event fires only once per game
- [ ] Stability and political power penalties are applied
- [ ] Opinion modifiers with Russia are applied
- [ ] Temporary national spirit is applied
- [ ] All localization displays correctly
- [ ] Event has appropriate picture

## Dependencies
- Depends on: None
- Blocks: ISSUE-004 (Cyber Defense Response can reference this event)

## Testing Notes
1. Start game as Estonia on January 1, 2007
2. Fast forward to April 27, 2007
3. Verify event fires within days of the date
4. Verify all effects are applied correctly
5. Check opinion modifiers with Russia
6. Verify temporary idea expires after 21 days
