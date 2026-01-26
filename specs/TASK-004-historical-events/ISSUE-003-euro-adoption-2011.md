# ISSUE-003: Estonia Adopts the Euro

## Summary
Add a historical event for Estonia's adoption of the Euro currency on January 1, 2011.

## Context
Estonia adopted the Euro on January 1, 2011, becoming the 17th member of the Eurozone. This marked the culmination of Estonia's economic convergence with the EU and represented a significant step in its European integration. Estonia was the first former Soviet republic to adopt the Euro. The event should require prior EU membership.

## Requirements
- [x] Create country event for Euro adoption
- [x] Trigger automatically after January 1, 2011
- [x] Require EU membership (EST_joined_eu flag)
- [x] Fire only once per game
- [x] Add economic effects reflecting Eurozone benefits
- [x] Add localization for event title, description, and options
- [x] Set country flag to track Euro adoption

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add new event estonia.202 |
| `localisation/english/MD_focus_EST_l_english.yml` | Modify | Add localization strings |

## Implementation

### Step 1: Add Event to events/Estonia.txt

Add the following event after estonia.201:

```pdx
# 2011 - Estonia adopts the Euro
country_event = {
	id = estonia.202
	title = estonia.202.t
	desc = estonia.202.d
	picture = GFX_banking_crisis

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2011.1.1
		has_country_flag = EST_joined_eu
		NOT = { has_country_flag = EST_adopted_euro }
		NOT = { has_global_flag = EST_euro_adoption_happened }
	}

	mean_time_to_happen = {
		days = 1
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_euro_adoption_happened
		}
	}

	option = {
		name = estonia.202.a
		log = "[GetDateText]: [This.GetName]: event estonia.202.a executed"

		set_country_flag = EST_adopted_euro

		add_political_power = 75
		add_stability = 0.03

		# Remove old Estonian Kroon benefits if any
		if = {
			limit = { has_idea = EST_free_currency }
			remove_ideas = EST_free_currency
		}

		# Economic benefits of Euro adoption
		add_ideas = EST_eurozone_member

		# Improved economic relations with Eurozone core
		GER = {
			add_opinion_modifier = { target = EST modifier = EST_eurozone_partner }
		}
		FRA = {
			add_opinion_modifier = { target = EST modifier = EST_eurozone_partner }
		}
		FIN = {
			add_opinion_modifier = { target = EST modifier = EST_eurozone_partner }
		}

		# Treasury effect - conversion costs
		set_temp_variable = { treasury_change = -0.5 }
		modify_treasury_effect = yes

		ai_chance = { base = 100 }
	}

	option = {
		name = estonia.202.b
		log = "[GetDateText]: [This.GetName]: event estonia.202.b executed"
		trigger = {
			is_ai = no
		}

		# Player-only option to represent alternative history
		set_country_flag = EST_rejected_euro

		add_political_power = -50
		add_stability = -0.05

		# Tension with EU for rejecting Euro commitment
		every_country = {
			limit = {
				OR = {
					tag = GER
					tag = FRA
				}
			}
			add_opinion_modifier = { target = EST modifier = EST_euro_rejection }
		}

		ai_chance = { base = 0 }
	}
}
```

### Step 2: Add National Spirit (in common/ideas if needed)

```pdx
EST_eurozone_member = {
	picture = generic_economic_policy
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		political_power_gain = 0.03
		consumer_goods_factor = -0.02
		trade_opinion_factor = 0.10
		production_speed_buildings_factor = 0.03
	}
}
```

### Step 3: Add Opinion Modifiers (in common/opinion_modifiers if needed)

```pdx
EST_eurozone_partner = {
	value = 20
}

EST_euro_rejection = {
	value = -30
}
```

### Step 4: Add Localization to MD_focus_EST_l_english.yml

```yml
 # Euro Adoption 2011
 estonia.202.t: "Estonia Adopts the Euro"
 estonia.202.d: "On January 1st, 2011, Estonia has officially adopted the Euro as its currency, replacing the Estonian Kroon. Estonia becomes the 17th member of the Eurozone and notably the first former Soviet republic to join the common currency. After meeting all the convergence criteria despite the global financial crisis, our adoption of the Euro demonstrates Estonia's strong fiscal discipline and commitment to European integration. The changeover has been smooth, with dual pricing in place since July 2010 to help citizens adapt."
 estonia.202.a: "The Euro is now our currency!"
 estonia.202.b: "We shall keep the Kroon."

 EST_eurozone_member: "Eurozone Member"
 EST_eurozone_member_desc: "As a member of the Eurozone, Estonia benefits from currency stability, lower transaction costs in trade with other Euro countries, and access to the European Central Bank's monetary policy."
 EST_eurozone_partner: "Eurozone Partner"
 EST_euro_rejection: "Rejected Euro Adoption"
```

## Acceptance Criteria
- [x] Event fires automatically after January 1, 2011 when playing as Estonia
- [x] Event only fires if Estonia has the EST_joined_eu flag (EU membership)
- [x] Event fires only once per game
- [x] Country flag EST_adopted_euro is set after accepting
- [x] Economic bonuses are applied via national spirit
- [x] Opinion modifiers with core Eurozone countries are applied
- [x] Alternative option available for players (ahistorical path)
- [x] All localization displays correctly in English

## Dependencies
- Depends on: ISSUE-001 (EU Accession - requires EST_joined_eu flag)
- Blocks: None

## Testing Notes
1. Start game as Estonia on January 1, 2000
2. Let EU accession event fire in May 2004
3. Fast forward to January 1, 2011
4. Verify Euro event fires within days of the date
5. Verify event does NOT fire if EU accession was somehow prevented
6. Test both options (accept Euro vs. reject Euro for players)
7. Verify treasury effect is applied
