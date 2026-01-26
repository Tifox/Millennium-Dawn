# ISSUE-004: Sanctions Leadership

## Summary
Add an event for Estonia's leadership role in advocating for strong EU sanctions against Russia.

## Context
Estonia, particularly under PM Kaja Kallas, was one of the strongest voices in the EU pushing for maximum sanctions against Russia. Estonia was the first country to call for Russia's removal from SWIFT and consistently advocated for the harshest possible measures.

### Historical Details
- **Key Advocate:** PM Kaja Kallas
- **First SWIFT call:** Estonia among first to demand removal
- **Sanctions packages:** Supported all EU sanctions rounds
- **Energy:** Pushed for gas/oil embargoes despite Baltic dependence
- **Recognition:** Kallas became prominent international figure

## Requirements
- [ ] Create sanctions advocacy event
- [ ] Trigger after Ukraine response
- [ ] Include EU/NATO opinion bonuses
- [ ] Add Russia opinion penalty
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.502 |
| `common/opinion_modifiers/Estonia.txt` | Modify | Add sanctions leadership modifier |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2022 - Estonia Leads Sanctions Advocacy
country_event = {
	id = estonia.502
	title = estonia.502.t
	desc = estonia.502.d
	picture = GFX_report_event_generic_parliament

	fire_only_once = yes

	trigger = {
		tag = EST
		has_country_flag = EST_strong_ukraine_support
		date > 2022.3.1
		NOT = { has_country_flag = EST_sanctions_leader }
		NOT = { has_global_flag = EST_sanctions_leadership_happened }
	}

	mean_time_to_happen = {
		days = 7
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_sanctions_leadership_happened
		}
	}

	# Option A: Push for maximum sanctions
	option = {
		name = estonia.502.a
		log = "[GetDateText]: [This.GetName]: event estonia.502.a executed"

		set_country_flag = EST_sanctions_leader
		set_country_flag = EST_maximum_sanctions

		add_political_power = 25

		# Kallas gains international prestige
		add_stability = 0.02

		# Improved EU relations
		every_country = {
			limit = {
				OR = {
					tag = GER
					tag = FRA
					tag = POL
					tag = SWE
					tag = FIN
				}
			}
			add_opinion_modifier = { target = EST modifier = EST_sanctions_leadership }
		}

		# Russia sees Estonia as hostile
		SOV = {
			add_opinion_modifier = { target = EST modifier = EST_sanctions_advocate }
		}

		ai_chance = { base = 90 }
	}

	# Option B: Support sanctions but avoid spotlight
	option = {
		name = estonia.502.b
		log = "[GetDateText]: [This.GetName]: event estonia.502.b executed"

		set_country_flag = EST_sanctions_leader

		add_political_power = 10

		ai_chance = { base = 10 }
	}
}
```

### Step 2: Add Opinion Modifiers

```pdx
EST_sanctions_leadership = {
	value = 20
}

EST_sanctions_advocate = {
	value = -30
}
```

### Step 3: Add Localization

```yml
 # Sanctions Leadership
 estonia.502.t: "Estonia Leads Sanctions Push"
 estonia.502.d: "As the European Union debates its response to Russia's invasion, Estonia has emerged as one of the loudest voices calling for maximum sanctions. Prime Minister Kaja Kallas has been tireless in her advocacy, calling for Russia's removal from SWIFT, energy embargoes, and comprehensive economic isolation.\n\n'This is not a time for half-measures,' Kallas declared. 'Russia must pay the full price for its aggression.'\n\nHer stance has earned respect from Baltic neighbors and Eastern European allies, while Western European countries are increasingly persuaded by her arguments. However, this vocal opposition has made Estonia a particular target of Russian hostility."
 estonia.502.a: "Lead the charge for maximum sanctions."
 estonia.502.b: "Support sanctions without seeking the spotlight."

 EST_sanctions_leadership: "Sanctions Leadership"
 EST_sanctions_advocate: "Sanctions Advocate"
```

## Acceptance Criteria
- [ ] Event fires after Ukraine response with strong support flag
- [ ] EU opinion bonuses applied
- [ ] Russia opinion penalty applied
- [ ] Political power and stability effects applied
- [ ] All localization displays correctly

## Dependencies
- Depends on: ISSUE-001 (Ukraine Response - strong support option)
- Blocks: None

## Testing Notes
1. Choose strong support in Ukraine response event
2. Advance to March 2022
3. Verify sanctions event fires
4. Check opinion effects
