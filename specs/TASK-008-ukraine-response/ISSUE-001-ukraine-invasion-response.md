# ISSUE-001: Ukraine Invasion Response

## Summary
Add an event for Estonia's immediate response to the Russian invasion of Ukraine on February 24, 2022.

## Context
Estonia was among the first and most vocal countries to condemn the Russian invasion of Ukraine. Prime Minister Kaja Kallas became one of the leading voices in Europe calling for strong sanctions and military support for Ukraine. Estonia immediately began providing military aid and pushed for Russia's removal from SWIFT.

### Historical Details
- **Date:** February 24, 2022
- **Immediate Actions:**
  - Strong condemnation of invasion
  - Military aid shipments begun
  - Called for Russia's removal from SWIFT
  - NATO Article 4 consultations invoked
- **Kaja Kallas:** Became prominent EU voice on Russia
- **Per-capita contribution:** Among highest in NATO

## Requirements
- [ ] Create immediate response event
- [ ] Trigger on February 24, 2022 or when Russia-Ukraine war begins
- [ ] Add opinion bonuses with Ukraine and NATO
- [ ] Set up flags for subsequent events
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.500 |
| `common/opinion_modifiers/Estonia.txt` | Modify | Add Ukraine solidarity modifier |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2022 - Estonia Responds to Ukraine Invasion
country_event = {
	id = estonia.500
	title = estonia.500.t
	desc = estonia.500.d
	picture = GFX_report_event_military_parade

	fire_only_once = yes

	trigger = {
		tag = EST
		OR = {
			date > 2022.2.24
			SOV = { has_war_with = UKR }
		}
		NOT = { has_country_flag = EST_ukraine_response }
		NOT = { has_global_flag = EST_ukraine_response_happened }
	}

	mean_time_to_happen = {
		days = 1
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_ukraine_response_happened
			set_global_flag = russia_ukraine_war
		}
	}

	# Option A: Strong support
	option = {
		name = estonia.500.a
		log = "[GetDateText]: [This.GetName]: event estonia.500.a executed"

		set_country_flag = EST_ukraine_response
		set_country_flag = EST_strong_ukraine_support

		add_political_power = -50
		add_stability = -0.05

		# Strong Ukraine support
		UKR = {
			add_opinion_modifier = { target = EST modifier = EST_ukraine_defender }
		}
		EST = {
			add_opinion_modifier = { target = UKR modifier = EST_supports_ukraine }
		}

		# Worsened Russia relations
		SOV = {
			add_opinion_modifier = { target = EST modifier = EST_hostile_stance }
		}

		# Improved NATO relations
		every_country = {
			limit = {
				is_in_faction_with = EST
			}
			add_opinion_modifier = { target = EST modifier = EST_frontline_solidarity }
		}

		ai_chance = { base = 90 }
	}

	# Option B: Measured response
	option = {
		name = estonia.500.b
		log = "[GetDateText]: [This.GetName]: event estonia.500.b executed"

		set_country_flag = EST_ukraine_response

		add_political_power = -25
		add_stability = -0.03

		UKR = {
			add_opinion_modifier = { target = EST modifier = EST_ukraine_supporter }
		}

		SOV = {
			add_opinion_modifier = { target = EST modifier = EST_moderate_stance }
		}

		ai_chance = { base = 10 }
	}
}
```

### Step 2: Add Opinion Modifiers

```pdx
EST_ukraine_defender = {
	value = 100
}

EST_supports_ukraine = {
	value = 75
}

EST_ukraine_supporter = {
	value = 50
}

EST_hostile_stance = {
	value = -75
}

EST_moderate_stance = {
	value = -40
}

EST_frontline_solidarity = {
	value = 25
}
```

### Step 3: Add Localization

```yml
 # Ukraine Response
 estonia.500.t: "Russia Invades Ukraine"
 estonia.500.d: "The unthinkable has happened. Russian forces have launched a full-scale invasion of Ukraine, attacking from multiple directions. Cities are being bombarded, and Ukrainian forces are engaged in desperate defense.\n\nAs a nation that knows firsthand the threat of Russian aggression, Estonia must respond. Our history under Soviet occupation gives us unique moral clarity on this issue. The question is how strongly we commit to Ukraine's defense.\n\nPrime Minister Kallas has already called for the strongest possible response, including Russia's removal from SWIFT and maximum military aid to Ukraine."
 estonia.500.a: "Stand with Ukraine - whatever it takes."
 estonia.500.b: "Condemn the invasion but measure our response."

 EST_ukraine_defender: "Defender of Ukraine"
 EST_supports_ukraine: "Supports Ukraine"
 EST_hostile_stance: "Hostile Stance"
 EST_frontline_solidarity: "Frontline Solidarity"
```

## Acceptance Criteria
- [ ] Event fires on or after February 24, 2022
- [ ] Event fires if Russia-Ukraine war begins
- [ ] Opinion modifiers applied correctly
- [ ] Flags set for subsequent events
- [ ] All localization displays correctly

## Dependencies
- Depends on: None
- Blocks: All other TASK-008 issues

## Testing Notes
1. Start game in early 2022
2. Advance to February 24, 2022
3. Verify event fires
4. Check opinion changes with Ukraine, Russia, NATO
5. Verify flags are set
