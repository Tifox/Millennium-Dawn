# ISSUE-002: EKRE Enters Government

## Summary
Add an event for EKRE entering the Estonian government as part of a coalition in April 2019.

## Context
Following the March 2019 parliamentary elections, EKRE entered government as part of a coalition with the Centre Party (led by PM Jüri Ratas) and Isamaa. This marked the first time a far-right party had been in government in Estonia since independence. EKRE gained significant cabinet positions including Interior (Mart Helme) and Finance (Martin Helme).

### Historical Details
- **Date:** April 29, 2019
- **Coalition:** Centre Party + Isamaa + EKRE
- **PM:** Jüri Ratas (Centre Party)
- **EKRE Ministers:**
  - Mart Helme: Interior Minister
  - Martin Helme: Finance Minister
- **Election Result:** EKRE won 17.8% (19 seats out of 101)
- **Controversies:**
  - "OK" gesture at swearing-in sparked outrage
  - Marti Kuusik resigned after 1 day (domestic violence allegations)
  - Multiple racist and sexist statements by EKRE ministers

## Requirements
- [x] Create event for EKRE government formation
- [x] Trigger after April 29, 2019
- [x] Require EKRE formation flag
- [x] Add stability impact and EU relations penalty
- [x] Add EKRE government national spirit
- [x] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.401 |
| `common/ideas/estonia.txt` | Modify | Add EST_ekre_government idea |
| `common/opinion_modifiers/Estonia.txt` | Modify | Add EU relations modifier |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2019 - EKRE Enters Government
country_event = {
	id = estonia.401
	title = estonia.401.t
	desc = estonia.401.d
	picture = GFX_report_event_generic_parliament

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2019.4.29
		has_country_flag = EST_ekre_formed
		NOT = { has_country_flag = EST_ekre_government }
		NOT = { has_global_flag = EST_ekre_government_happened }
	}

	mean_time_to_happen = {
		days = 3
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_ekre_government_happened
		}
	}

	option = {
		name = estonia.401.a
		log = "[GetDateText]: [This.GetName]: event estonia.401.a executed"

		set_country_flag = EST_ekre_government

		# Controversy causes instability
		add_stability = -0.05
		add_political_power = -25

		# EKRE in government effects
		add_ideas = EST_ekre_government

		# EU relations worsen
		every_country = {
			limit = {
				OR = {
					tag = GER
					tag = FRA
					tag = BEL
					tag = HOL
					tag = SWE
				}
			}
			add_opinion_modifier = { target = EST modifier = EST_far_right_government }
		}

		# Boost nationalist support
		add_popularity = {
			ideology = Nat_Populism
			popularity = 0.05
		}

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add National Spirit

```pdx
EST_ekre_government = {
	picture = generic_political_unity
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		stability_factor = -0.05
		political_power_factor = -0.10
		drift_defence_factor = 0.15
	}
}
```

### Step 3: Add Opinion Modifier

```pdx
EST_far_right_government = {
	value = -20
	decay = 0.5
}
```

### Step 4: Add Localization

```yml
 # EKRE Enters Government
 estonia.401.t: "EKRE Enters Government Coalition"
 estonia.401.d: "Following the March elections, the Conservative People's Party of Estonia (EKRE) has entered the government as part of a coalition with the Centre Party and Isamaa. Prime Minister Jüri Ratas leads the new cabinet, with EKRE securing key ministerial positions.\n\nMart Helme becomes Interior Minister while his son Martin Helme takes the Finance Ministry. This marks the first time since independence that a far-right party has held power in Estonia.\n\nThe new government has already sparked controversy, with critics warning about the impact on Estonia's international reputation and democratic norms."
 estonia.401.a: "A controversial new chapter begins."

 EST_ekre_government: "EKRE Coalition Government"
 EST_ekre_government_desc: "The far-right Conservative People's Party is part of the governing coalition, leading to domestic controversy and strained relations with European partners."

 EST_far_right_government: "Far-Right Coalition Partner"
```

## Acceptance Criteria
- [x] Event fires after April 29, 2019
- [x] Requires EKRE formation flag
- [x] Stability penalty applied
- [x] National spirit added
- [x] EU opinion penalties applied
- [x] Nat_Populism popularity increases
- [x] All localization displays correctly

## Dependencies
- Depends on: ISSUE-001 (EKRE Formation)
- Blocks: ISSUE-003 (EKRE Government Collapse)

## Testing Notes
1. Ensure EKRE formation event fires first
2. Advance to April 2019
3. Verify government formation event fires
4. Check all effects are applied
5. Verify EU opinion changes
