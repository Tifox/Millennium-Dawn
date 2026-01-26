# ISSUE-001: Estonia Joins the European Union

## Summary
Add a historical event for Estonia's accession to the European Union on May 1, 2004.

## Context
Estonia joined the European Union on May 1, 2004, as part of the largest single EU enlargement. This was a major milestone in Estonia's post-Soviet integration with Western Europe. The event should trigger automatically on the historical date and provide appropriate bonuses reflecting EU membership benefits.

## Requirements
- [x] Create country event for EU accession
- [x] Trigger automatically after May 1, 2004
- [x] Fire only once per game
- [x] Add appropriate effects (political power, stability, opinion modifiers)
- [x] Add localization for event title, description, and options
- [x] Set country flag to track EU membership

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add new event estonia.200 |
| `localisation/english/MD_focus_EST_l_english.yml` | Modify | Add localization strings |

## Implementation

### Step 1: Add Event to events/Estonia.txt

Add the following event after the existing events (around line 965):

```pdx
# 2004 - Estonia joins the European Union
country_event = {
	id = estonia.200
	title = estonia.200.t
	desc = estonia.200.d
	picture = GFX_computer

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2004.5.1
		NOT = { has_country_flag = EST_joined_eu }
		NOT = { has_global_flag = EST_eu_accession_happened }
	}

	mean_time_to_happen = {
		days = 1
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_eu_accession_happened
		}
	}

	option = {
		name = estonia.200.a
		log = "[GetDateText]: [This.GetName]: event estonia.200.a executed"

		set_country_flag = EST_joined_eu

		add_political_power = 150
		add_stability = 0.05

		# Economic benefits of EU membership
		add_ideas = EST_eu_member_benefits

		# Improved relations with EU members
		every_country = {
			limit = {
				OR = {
					tag = GER
					tag = FRA
					tag = ITA
					tag = POL
					tag = SWE
					tag = FIN
				}
			}
			add_opinion_modifier = { target = EST modifier = EST_fellow_eu_member }
		}

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add National Spirit (in common/ideas if needed)

If not already existing, create the EU member benefits idea:

```pdx
EST_eu_member_benefits = {
	picture = generic_political_unity
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		political_power_gain = 0.05
		stability_factor = 0.05
		trade_opinion_factor = 0.15
	}
}
```

### Step 3: Add Opinion Modifier (in common/opinion_modifiers if needed)

```pdx
EST_fellow_eu_member = {
	value = 25
}
```

### Step 4: Add Localization to MD_focus_EST_l_english.yml

Add after the existing event localizations (around line 620):

```yml
 # EU Accession 2004
 estonia.200.t: "Estonia Joins the European Union"
 estonia.200.d: "On this historic day, May 1st, 2004, Estonia has officially become a member of the European Union. After years of democratic reforms and economic restructuring following our independence from the Soviet Union, we have achieved one of our greatest foreign policy goals. As part of the largest single EU enlargement in history, Estonia joins alongside nine other nations. This marks the beginning of a new chapter in Estonian history, bringing new opportunities for trade, investment, and closer cooperation with our European partners."
 estonia.200.a: "A new era for Estonia begins!"

 EST_eu_member_benefits: "EU Membership"
 EST_eu_member_benefits_desc: "As a member of the European Union, Estonia benefits from access to the single market, structural funds, and closer political cooperation with fellow member states."
 EST_fellow_eu_member: "Fellow EU Member"
```

## Acceptance Criteria
- [x] Event fires automatically after May 1, 2004 when playing as Estonia
- [x] Event fires only once per game
- [x] Country flag EST_joined_eu is set after event
- [x] Political power and stability bonuses are applied
- [x] Opinion modifiers with major EU countries are applied
- [x] All localization displays correctly in English
- [x] Event has appropriate picture

## Dependencies
- Depends on: None
- Blocks: ISSUE-003 (Euro adoption requires EU membership)

## Testing Notes
1. Start game as Estonia on January 1, 2000
2. Fast forward to May 1, 2004
3. Verify event fires within days of the date
4. Verify all effects are applied correctly
5. Verify event does not fire again on subsequent playthroughs
