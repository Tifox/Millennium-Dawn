# ISSUE-004: COVID-19 Pandemic Events for Estonia

## Summary
Add a series of historical events covering Estonia's COVID-19 pandemic experience from 2020-2022, including first case, national lockdown, vaccination campaign, and eventual reopening.

## Context
The COVID-19 pandemic significantly impacted Estonia:
- **February 27, 2020**: First confirmed case in Estonia
- **March 12, 2020**: State of emergency declared, lockdown begins
- **December 2020**: Vaccination campaign starts
- **2021-2022**: Gradual reopening and recovery

These events should create an event chain that reflects the pandemic's impact on stability, economy, and public health in Estonia.

## Requirements
- [x] Create event for first COVID case (February 27, 2020)
- [x] Create event for national lockdown (March 12, 2020)
- [x] Create event for vaccination campaign start
- [x] Create event for reopening/recovery
- [x] Add appropriate stability and economic modifiers
- [x] Implement event chain with proper sequencing
- [x] Add national spirits for pandemic phases
- [x] Add localization for all events and modifiers

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add events estonia.210-214 |
| `localisation/english/MD_focus_EST_l_english.yml` | Modify | Add localization strings |
| `common/ideas/EST_ideas.txt` | Modify | Add pandemic-related national spirits |

## Implementation

### Step 1: Add COVID Events to events/Estonia.txt

Add the following events after the Euro adoption event:

```pdx
### COVID-19 Pandemic Events ###

# First COVID-19 Case in Estonia
country_event = {
	id = estonia.210
	title = estonia.210.t
	desc = estonia.210.d
	picture = GFX_computer

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2020.2.27
		NOT = { has_country_flag = EST_covid_first_case }
		NOT = { has_global_flag = EST_covid_started }
	}

	mean_time_to_happen = {
		days = 1
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_covid_started
		}
	}

	option = {
		name = estonia.210.a
		log = "[GetDateText]: [This.GetName]: event estonia.210.a executed"

		set_country_flag = EST_covid_first_case

		add_stability = -0.02
		add_political_power = -25

		# Queue the lockdown event
		hidden_effect = {
			country_event = {
				id = estonia.211
				days = 14
			}
		}

		ai_chance = { base = 100 }
	}
}

# Estonia Declares State of Emergency - COVID Lockdown
country_event = {
	id = estonia.211
	title = estonia.211.t
	desc = estonia.211.d
	picture = GFX_computer

	fire_only_once = yes
	is_triggered_only = yes

	option = {
		name = estonia.211.a
		log = "[GetDateText]: [This.GetName]: event estonia.211.a executed"

		# Strict lockdown response
		set_country_flag = EST_covid_lockdown_strict

		add_stability = -0.10
		add_war_support = -0.05

		add_ideas = EST_covid_lockdown

		# Economic impact
		set_temp_variable = { treasury_change = -2.0 }
		modify_treasury_effect = yes

		# Queue vaccination event
		hidden_effect = {
			country_event = {
				id = estonia.212
				days = 280
			}
		}

		ai_chance = { base = 75 }
	}

	option = {
		name = estonia.211.b
		log = "[GetDateText]: [This.GetName]: event estonia.211.b executed"

		# Moderate restrictions
		set_country_flag = EST_covid_lockdown_moderate

		add_stability = -0.05
		add_war_support = -0.02

		add_ideas = EST_covid_restrictions

		set_temp_variable = { treasury_change = -1.0 }
		modify_treasury_effect = yes

		hidden_effect = {
			country_event = {
				id = estonia.212
				days = 280
			}
		}

		ai_chance = { base = 25 }
	}
}

# COVID-19 Vaccination Campaign Begins
country_event = {
	id = estonia.212
	title = estonia.212.t
	desc = estonia.212.d
	picture = GFX_computer

	fire_only_once = yes
	is_triggered_only = yes

	option = {
		name = estonia.212.a
		log = "[GetDateText]: [This.GetName]: event estonia.212.a executed"

		# Aggressive vaccination campaign
		set_country_flag = EST_covid_vaccination_aggressive

		add_stability = 0.03
		add_political_power = 50

		set_temp_variable = { treasury_change = -1.5 }
		modify_treasury_effect = yes

		# Queue reopening event
		hidden_effect = {
			country_event = {
				id = estonia.213
				days = 180
			}
		}

		ai_chance = { base = 80 }
	}

	option = {
		name = estonia.212.b
		log = "[GetDateText]: [This.GetName]: event estonia.212.b executed"

		# Standard vaccination rollout
		set_country_flag = EST_covid_vaccination_standard

		add_stability = 0.01

		set_temp_variable = { treasury_change = -0.75 }
		modify_treasury_effect = yes

		hidden_effect = {
			country_event = {
				id = estonia.213
				days = 240
			}
		}

		ai_chance = { base = 20 }
	}
}

# COVID-19 Recovery and Reopening
country_event = {
	id = estonia.213
	title = estonia.213.t
	desc = estonia.213.d
	picture = GFX_computer

	fire_only_once = yes
	is_triggered_only = yes

	option = {
		name = estonia.213.a
		log = "[GetDateText]: [This.GetName]: event estonia.213.a executed"

		set_country_flag = EST_covid_recovered

		add_stability = 0.05
		add_political_power = 75

		# Remove pandemic restrictions
		if = {
			limit = { has_idea = EST_covid_lockdown }
			remove_ideas = EST_covid_lockdown
		}
		if = {
			limit = { has_idea = EST_covid_restrictions }
			remove_ideas = EST_covid_restrictions
		}

		# Add post-pandemic recovery spirit
		add_ideas = EST_post_covid_recovery

		# Economic recovery
		set_temp_variable = { treasury_change = 1.0 }
		modify_treasury_effect = yes

		ai_chance = { base = 100 }
	}
}

# Optional: Second Wave Event
country_event = {
	id = estonia.214
	title = estonia.214.t
	desc = estonia.214.d
	picture = GFX_computer

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2020.10.1
		has_country_flag = EST_covid_first_case
		NOT = { has_country_flag = EST_covid_second_wave }
		OR = {
			has_idea = EST_covid_lockdown
			has_idea = EST_covid_restrictions
		}
	}

	mean_time_to_happen = {
		days = 30
	}

	option = {
		name = estonia.214.a
		log = "[GetDateText]: [This.GetName]: event estonia.214.a executed"

		# Reimpose strict measures
		set_country_flag = EST_covid_second_wave

		add_stability = -0.05
		add_political_power = -30

		set_temp_variable = { treasury_change = -0.5 }
		modify_treasury_effect = yes

		ai_chance = { base = 70 }
	}

	option = {
		name = estonia.214.b
		log = "[GetDateText]: [This.GetName]: event estonia.214.b executed"

		# Keep current measures
		set_country_flag = EST_covid_second_wave

		add_stability = -0.03

		ai_chance = { base = 30 }
	}
}
```

### Step 2: Add National Spirits (in common/ideas/EST_ideas.txt or appropriate file)

```pdx
EST_covid_lockdown = {
	picture = generic_disjointed_gov
	allowed = { always = no }
	allowed_civil_war = { always = no }

	modifier = {
		stability_factor = -0.15
		consumer_goods_factor = 0.05
		production_speed_buildings_factor = -0.10
		political_power_gain = -0.20
		war_support_factor = -0.10
	}
}

EST_covid_restrictions = {
	picture = generic_disjointed_gov
	allowed = { always = no }
	allowed_civil_war = { always = no }

	modifier = {
		stability_factor = -0.08
		consumer_goods_factor = 0.02
		production_speed_buildings_factor = -0.05
		political_power_gain = -0.10
	}
}

EST_post_covid_recovery = {
	picture = generic_production_bonus
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		stability_factor = 0.05
		production_speed_buildings_factor = 0.05
		political_power_gain = 0.05
	}
}
```

### Step 3: Add Localization to MD_focus_EST_l_english.yml

```yml
 # COVID-19 Pandemic Events
 estonia.210.t: "First COVID-19 Case Confirmed"
 estonia.210.d: "The Health Board has confirmed Estonia's first case of COVID-19. A 44-year-old Estonian citizen who returned from Iran via Turkey on February 25th has tested positive for the novel coronavirus. The patient is currently in isolation at the West Tallinn Central Hospital. Health authorities are tracing contacts and monitoring the situation closely. The World Health Organization has already declared the outbreak a Public Health Emergency of International Concern, and we must now prepare for potential community spread."
 estonia.210.a: "We must prepare for what's to come."

 estonia.211.t: "State of Emergency Declared"
 estonia.211.d: "The Estonian government has declared a state of emergency effective immediately. With COVID-19 cases rising across Europe and in Estonia, Prime Minister Juri Ratas has announced sweeping measures to combat the pandemic. Schools will close, public gatherings are banned, and citizens are urged to stay home. The borders are being tightened, with restrictions on travel. This is the first state of emergency in Estonia since we regained independence. The economy will suffer, but protecting lives must come first."
 estonia.211.a: "Implement strict lockdown measures."
 estonia.211.b: "Implement moderate restrictions only."

 estonia.212.t: "COVID-19 Vaccination Campaign Begins"
 estonia.212.d: "After months of lockdowns and restrictions, hope arrives in the form of COVID-19 vaccines. Estonia has secured vaccine supplies through the EU's joint procurement program, and the first doses are now being administered to healthcare workers and the elderly. The government must now decide how aggressively to pursue the vaccination campaign. An aggressive rollout will be more expensive but could bring faster recovery, while a standard approach conserves resources but extends the pandemic's impact."
 estonia.212.a: "Launch an aggressive vaccination campaign."
 estonia.212.b: "Proceed with a standard rollout."

 estonia.213.t: "Estonia Reopens - COVID Recovery"
 estonia.213.d: "After more than a year of restrictions, lockdowns, and uncertainty, Estonia is finally reopening. Vaccination rates have reached sufficient levels to allow a return to normalcy. Restrictions are being lifted, businesses are reopening, and people are returning to their normal lives. The pandemic has left its mark on our economy and society, but Estonia has weathered the storm. Now begins the work of recovery and rebuilding what was lost."
 estonia.213.a: "A new beginning for Estonia."

 estonia.214.t: "COVID-19 Second Wave Hits Estonia"
 estonia.214.d: "As autumn arrives, so does a second wave of COVID-19 infections. Case numbers are rising rapidly, hospitals are filling up again, and the government faces difficult choices. Do we reimpose strict measures and risk further economic damage, or do we try to weather this wave with our current restrictions? The public is growing weary of pandemic measures, but the health system's capacity is being strained."
 estonia.214.a: "Reimpose stricter measures."
 estonia.214.b: "Maintain current restrictions."

 # COVID National Spirits
 EST_covid_lockdown: "COVID-19 Lockdown"
 EST_covid_lockdown_desc: "Estonia is under strict lockdown measures to combat the COVID-19 pandemic. Schools are closed, businesses are shuttered, and citizens are urged to stay home. The economic and social costs are severe, but necessary to protect public health."

 EST_covid_restrictions: "COVID-19 Restrictions"
 EST_covid_restrictions_desc: "Estonia has implemented moderate restrictions to combat COVID-19. While not a full lockdown, social distancing measures, mask mandates, and gathering limits are affecting daily life and the economy."

 EST_post_covid_recovery: "Post-Pandemic Recovery"
 EST_post_covid_recovery_desc: "Estonia has emerged from the COVID-19 pandemic and is now in recovery mode. With high vaccination rates and lifted restrictions, the economy is rebounding and society is returning to normal."
```

## Acceptance Criteria
- [x] First case event fires automatically after February 27, 2020
- [x] Lockdown event fires approximately 14 days after first case
- [x] Player has choice between strict and moderate lockdown
- [x] Vaccination event fires approximately 280 days after lockdown
- [x] Recovery event fires based on vaccination campaign choice
- [x] Second wave event can fire in autumn 2020 if pandemic still active
- [x] National spirits are properly applied and removed
- [x] All economic effects (treasury changes) work correctly
- [x] All localization displays correctly
- [x] Event chain completes properly for AI

## Dependencies
- Depends on: None
- Blocks: None

## Testing Notes
1. Start game as Estonia on January 1, 2020
2. Fast forward to February 27, 2020 - verify first case event
3. Wait 14 days - verify lockdown event triggers
4. Test both lockdown options (strict vs. moderate)
5. Fast forward through vaccination and reopening events
6. Verify national spirits are applied and removed correctly
7. Verify treasury effects are applied
8. Test second wave event by having it trigger in October 2020

## Event Chain Flow

```
estonia.210 (First Case - Feb 27, 2020)
    |
    v
estonia.211 (Lockdown - ~Mar 12, 2020) [14 days later]
    |
    +---> estonia.214 (Second Wave - Oct 2020) [Optional, triggered by date]
    |
    v
estonia.212 (Vaccination - ~Dec 2020) [280 days later]
    |
    v
estonia.213 (Recovery - ~Jun-Aug 2021) [180-240 days based on choice]
```
