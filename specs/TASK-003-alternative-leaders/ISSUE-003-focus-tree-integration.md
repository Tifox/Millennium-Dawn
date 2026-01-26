# ISSUE-003: Focus Tree Integration for Alternative Leaders

## Summary
Integrate EKRE and Centre Party leaders into Estonia's focus tree, creating dedicated focus paths that promote specific leaders when completed.

## Context
Estonia's focus tree (`common/national_focus/05_estonia.txt`) already contains:
- A nationalist path starting with `EST_the_estonian_party` (requires `nationalist_fascist_are_in_power = yes`)
- A Centre Party path starting with `EST_center_party` (under neutrality/communism government)
- Various political sub-paths for different factions

This issue adds new focuses that specifically promote the EKRE leaders (Mart Helme, Martin Helme) and Centre Party leaders (Edgar Savisaar, Juri Ratas, Jaan Toots) as country leaders.

## Requirements
- [ ] Add EKRE-specific focus branch to nationalist path
- [ ] Add Centre Party leader promotion focuses
- [ ] Add localization for all new focuses
- [ ] Ensure focuses properly set leaders using `promote_character` or `set_party_leader`
- [ ] Balance focus costs and positions within existing tree

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/national_focus/05_estonia.txt` | Modify | Add new focus branches for EKRE and Centre Party |
| `localisation/english/MD_focus_EST_l_english.yml` | Modify | Add localization for new focuses |

## Implementation

### Step 1: Add EKRE Path Focuses

Add the following focuses to `common/national_focus/05_estonia.txt`. These should be placed after the existing nationalist path focuses (around line 4400+):

```pdx
	# EKRE Path - Nationalist Alternative
	focus = {
		id = EST_rise_of_ekre
		icon = GFX_focus_generic_nationalism

		x = 2
		y = 1
		relative_position_id = EST_the_estonian_party

		cost = 5

		prerequisite = { focus = EST_the_estonian_party }

		search_filters = { FOCUS_FILTER_POLITICAL }

		available = {
			nationalist_fascist_are_in_power = yes
			date > 2012.1.1
		}

		completion_reward = {
			log = "[GetDateText]: [This.GetName]: focus EST_rise_of_ekre executed"
			add_political_power = 100
			add_popularity = { ideology = nationalist popularity = 0.05 }
			recalculate_party = yes
			custom_effect_tooltip = EST_ekre_leaders_available_tt
			set_country_flag = EST_ekre_path_chosen
		}

		ai_will_do = {
			base = 1
			modifier = {
				add = 5
				date > 2015.1.1
			}
		}
	}

	focus = {
		id = EST_mart_helme_leadership
		icon = GFX_focus_generic_political_leader

		x = -1
		y = 1
		relative_position_id = EST_rise_of_ekre

		cost = 5

		prerequisite = { focus = EST_rise_of_ekre }
		mutually_exclusive = { focus = EST_martin_helme_leadership }

		search_filters = { FOCUS_FILTER_POLITICAL }

		available = {
			nationalist_fascist_are_in_power = yes
			has_country_flag = EST_ekre_path_chosen
			date < 2020.11.1
		}

		completion_reward = {
			log = "[GetDateText]: [This.GetName]: focus EST_mart_helme_leadership executed"
			hidden_effect = { kill_country_leader = yes }
			create_country_leader = {
				name = "Mart Helme"
				desc = EST_MART_HELME_DESC
				picture = "Mart_Helme.dds"
				expire = "2050.1.1"
				ideology = Nat_Populism
				traits = {
					nationalist_Nat_Populism
					anti_establishment_firebrand
				}
			}
			add_political_power = 50
			add_stability = -0.03
			add_popularity = { ideology = nationalist popularity = 0.03 }
			recalculate_party = yes
		}

		ai_will_do = {
			base = 1
			modifier = {
				factor = 2
				date < 2018.1.1
			}
		}
	}

	focus = {
		id = EST_martin_helme_leadership
		icon = GFX_focus_generic_political_leader

		x = 1
		y = 1
		relative_position_id = EST_rise_of_ekre

		cost = 5

		prerequisite = { focus = EST_rise_of_ekre }
		mutually_exclusive = { focus = EST_mart_helme_leadership }

		search_filters = { FOCUS_FILTER_POLITICAL }

		available = {
			nationalist_fascist_are_in_power = yes
			has_country_flag = EST_ekre_path_chosen
			date > 2018.1.1
		}

		completion_reward = {
			log = "[GetDateText]: [This.GetName]: focus EST_martin_helme_leadership executed"
			hidden_effect = { kill_country_leader = yes }
			create_country_leader = {
				name = "Martin Helme"
				desc = EST_MARTIN_HELME_DESC
				picture = "Martin_Helme.dds"
				expire = "2050.1.1"
				ideology = Nat_Populism
				traits = {
					nationalist_Nat_Populism
					fiscal_conservative
				}
			}
			add_political_power = 75
			add_stability = 0.02
			set_temp_variable = { treasury_change = 5 }
			modify_treasury_effect = yes
		}

		ai_will_do = {
			base = 1
			modifier = {
				factor = 3
				date > 2020.1.1
			}
		}
	}

	focus = {
		id = EST_ekre_agenda
		icon = GFX_focus_generic_nationalism

		x = 0
		y = 1
		relative_position_id = EST_mart_helme_leadership

		cost = 8.6

		prerequisite = {
			focus = EST_mart_helme_leadership
			focus = EST_martin_helme_leadership
		}

		search_filters = { FOCUS_FILTER_POLITICAL FOCUS_FILTER_STABILITY }

		available = {
			nationalist_fascist_are_in_power = yes
		}

		completion_reward = {
			log = "[GetDateText]: [This.GetName]: focus EST_ekre_agenda executed"
			add_ideas = EST_ekre_government
			add_stability = 0.05
			add_political_power = 100
			add_popularity = { ideology = nationalist popularity = 0.05 }
			recalculate_party = yes
		}

		ai_will_do = {
			base = 1
		}
	}
```

### Step 2: Add Centre Party Path Focuses

Add after the existing Centre Party focuses (around line 2700+):

```pdx
	# Centre Party Leader Path
	focus = {
		id = EST_centre_party_leadership
		icon = GFX_focus_generic_political_discussion

		x = 0
		y = 2
		relative_position_id = EST_center_party

		cost = 5

		prerequisite = { focus = EST_center_party }

		search_filters = { FOCUS_FILTER_POLITICAL }

		available = {
			OR = {
				has_government = communism
				has_government = neutrality
			}
		}

		completion_reward = {
			log = "[GetDateText]: [This.GetName]: focus EST_centre_party_leadership executed"
			add_political_power = 75
			custom_effect_tooltip = EST_centre_leaders_available_tt
			set_country_flag = EST_centre_path_chosen
		}

		ai_will_do = {
			base = 1
		}
	}

	focus = {
		id = EST_savisaar_era
		icon = GFX_focus_generic_political_leader

		x = -2
		y = 1
		relative_position_id = EST_centre_party_leadership

		cost = 5

		prerequisite = { focus = EST_centre_party_leadership }
		mutually_exclusive = { focus = EST_ratas_government }
		mutually_exclusive = { focus = EST_toots_moderate_path }

		search_filters = { FOCUS_FILTER_POLITICAL }

		available = {
			OR = {
				has_government = communism
				has_government = neutrality
			}
			has_country_flag = EST_centre_path_chosen
			date < 2016.11.1
		}

		completion_reward = {
			log = "[GetDateText]: [This.GetName]: focus EST_savisaar_era executed"
			hidden_effect = { kill_country_leader = yes }
			create_country_leader = {
				name = "Edgar Savisaar"
				desc = EST_EDGAR_SAVISAAR_DESC
				picture = "Edgar_Savisaar.dds"
				expire = "2022.10.20"
				ideology = oligarchism
				traits = {
					neutrality_oligarchism
					career_politician
					popular_figurehead
				}
			}
			add_political_power = 100
			add_stability = -0.05
			set_temp_variable = { temp_opinion = 15 }
			change_russian_minority_opinion = yes
		}

		ai_will_do = {
			base = 1
			modifier = {
				factor = 3
				date < 2010.1.1
			}
		}
	}

	focus = {
		id = EST_ratas_government
		icon = GFX_focus_generic_political_leader

		x = 0
		y = 1
		relative_position_id = EST_centre_party_leadership

		cost = 5

		prerequisite = { focus = EST_centre_party_leadership }
		mutually_exclusive = { focus = EST_savisaar_era }
		mutually_exclusive = { focus = EST_toots_moderate_path }

		search_filters = { FOCUS_FILTER_POLITICAL }

		available = {
			OR = {
				has_government = communism
				has_government = neutrality
			}
			has_country_flag = EST_centre_path_chosen
			date > 2014.1.1
		}

		completion_reward = {
			log = "[GetDateText]: [This.GetName]: focus EST_ratas_government executed"
			hidden_effect = { kill_country_leader = yes }
			create_country_leader = {
				name = "Juri Ratas"
				desc = EST_JURI_RATAS_DESC
				picture = "Juri_Ratas.dds"
				expire = "2050.1.1"
				ideology = oligarchism
				traits = {
					neutrality_oligarchism
					coalition_builder
				}
			}
			add_political_power = 150
			add_stability = 0.03
			custom_effect_tooltip = EST_coalition_government_tt
		}

		ai_will_do = {
			base = 1
			modifier = {
				factor = 5
				date > 2016.1.1
			}
		}
	}

	focus = {
		id = EST_toots_moderate_path
		icon = GFX_focus_generic_political_leader

		x = 2
		y = 1
		relative_position_id = EST_centre_party_leadership

		cost = 5

		prerequisite = { focus = EST_centre_party_leadership }
		mutually_exclusive = { focus = EST_savisaar_era }
		mutually_exclusive = { focus = EST_ratas_government }

		search_filters = { FOCUS_FILTER_POLITICAL }

		available = {
			OR = {
				has_government = communism
				has_government = neutrality
			}
			has_country_flag = EST_centre_path_chosen
		}

		completion_reward = {
			log = "[GetDateText]: [This.GetName]: focus EST_toots_moderate_path executed"
			hidden_effect = { kill_country_leader = yes }
			create_country_leader = {
				name = "Jaan Toots"
				desc = EST_JAAN_TOOTS_DESC
				picture = "Jaan_Toots.dds"
				expire = "2050.1.1"
				ideology = oligarchism
				traits = {
					neutrality_oligarchism
					economist
				}
			}
			add_political_power = 100
			set_temp_variable = { treasury_change = 10 }
			modify_treasury_effect = yes
			set_temp_variable = { temp_opinion = 10 }
			change_international_bankers_opinion = yes
		}

		ai_will_do = {
			base = 1
		}
	}

	focus = {
		id = EST_centre_party_consolidation
		icon = GFX_focus_generic_stability

		x = 0
		y = 1
		relative_position_id = EST_ratas_government

		cost = 8.6

		prerequisite = {
			focus = EST_savisaar_era
			focus = EST_ratas_government
			focus = EST_toots_moderate_path
		}

		search_filters = { FOCUS_FILTER_POLITICAL FOCUS_FILTER_STABILITY }

		available = {
			OR = {
				has_government = communism
				has_government = neutrality
			}
		}

		completion_reward = {
			log = "[GetDateText]: [This.GetName]: focus EST_centre_party_consolidation executed"
			add_ideas = EST_centre_party_dominance
			add_stability = 0.1
			add_political_power = 150
		}

		ai_will_do = {
			base = 1
		}
	}
```

### Step 3: Add National Ideas

Add to `common/ideas/EST.txt` (or appropriate ideas file):

```pdx
	EST_ekre_government = {
		allowed = { original_tag = EST }
		allowed_civil_war = { always = yes }
		removal_cost = -1

		picture = generic_nationalism

		modifier = {
			stability_factor = 0.05
			political_power_gain = 0.10
			drift_defence_factor = 0.25
			foreign_subversive_activites = -0.30
		}
	}

	EST_centre_party_dominance = {
		allowed = { original_tag = EST }
		allowed_civil_war = { always = yes }
		removal_cost = -1

		picture = generic_pp_unity_bonus

		modifier = {
			stability_factor = 0.05
			political_power_gain = 0.15
			consumer_goods_factor = -0.02
		}
	}
```

### Step 4: Add Localization

Add to `localisation/english/MD_focus_EST_l_english.yml`:

```yml
 # EKRE Path
 EST_rise_of_ekre: "Rise of EKRE"
 EST_rise_of_ekre_desc: "The Estonian Conservative People's Party (EKRE) has emerged as a powerful force in Estonian politics. Founded in 2012, the party channels growing nationalist sentiment and opposition to liberal establishment policies. It is time to embrace their vision for Estonia's future."
 EST_mart_helme_leadership: "Mart Helme's Leadership"
 EST_mart_helme_leadership_desc: "Mart Helme, the founder of EKRE, shall lead our nation. His uncompromising stance on national sovereignty and traditional values will reshape Estonian politics."
 EST_martin_helme_leadership: "Martin Helme Takes Charge"
 EST_martin_helme_leadership_desc: "Martin Helme, the younger generation of EKRE leadership, brings both his father's nationalist convictions and a sharper focus on fiscal conservatism. Under his leadership, EKRE will chart a new course for Estonia."
 EST_ekre_agenda: "The EKRE Agenda"
 EST_ekre_agenda_desc: "With EKRE firmly in control, we can now implement our full agenda: national sovereignty, traditional values, and an Estonia-first approach to all policy matters."
 EST_ekre_leaders_available_tt: "EKRE leaders Mart Helme and Martin Helme are now available."

 # Centre Party Path
 EST_centre_party_leadership: "Centre Party Leadership"
 EST_centre_party_leadership_desc: "The Centre Party's grip on Estonian politics requires strong leadership. We must choose who will guide the party and the nation forward."
 EST_savisaar_era: "The Savisaar Era"
 EST_savisaar_era_desc: "Edgar Savisaar, the founder of the Centre Party and a titan of Estonian politics, shall lead us. His decades of experience and connection to the Russian-speaking community make him a formidable leader."
 EST_ratas_government: "Ratas Forms Government"
 EST_ratas_government_desc: "Juri Ratas, a pragmatic politician and skilled coalition builder, will lead the Centre Party and Estonia. His ability to work across political divides could bring stability to our nation."
 EST_toots_moderate_path: "Toots' Moderate Path"
 EST_toots_moderate_path_desc: "Jaan Toots represents the Centre Party's moderate, economically-focused wing. Under his leadership, we will prioritize sound economic policy over political controversy."
 EST_centre_party_consolidation: "Consolidate Centre Power"
 EST_centre_party_consolidation_desc: "The Centre Party has established itself as the dominant force in Estonian politics. Now we consolidate our gains and ensure the party's continued influence."
 EST_centre_leaders_available_tt: "Centre Party leaders are now available."
 EST_coalition_government_tt: "Coalition government formed - increased political flexibility."

 # Ideas
 EST_ekre_government: "EKRE Government"
 EST_ekre_government_desc: "EKRE's nationalist agenda is now being implemented across all levels of government, strengthening national identity and sovereignty."
 EST_centre_party_dominance: "Centre Party Dominance"
 EST_centre_party_dominance_desc: "The Centre Party has established itself as the preeminent political force in Estonia, bringing stability and pragmatic governance."
```

## Focus Tree Position Reference

The focuses should be positioned relative to existing focuses:

**EKRE Path:**
```
EST_the_estonian_party (x=34, y=1 relative to EST_estonias_future)
    |
    +-- EST_rise_of_ekre (x=2, y=1 relative to EST_the_estonian_party)
            |
            +-- EST_mart_helme_leadership (x=-1, y=1)
            +-- EST_martin_helme_leadership (x=1, y=1)
                    |
                    +-- EST_ekre_agenda (x=0, y=1)
```

**Centre Party Path:**
```
EST_center_party (existing, x=0, y=1 relative to EST_what_if)
    |
    +-- (existing focuses: EST_soc_lib_policies, EST_pure_center, EST_soc_con_policies)
    |
    +-- EST_centre_party_leadership (x=0, y=2 relative to EST_center_party)
            |
            +-- EST_savisaar_era (x=-2, y=1)
            +-- EST_ratas_government (x=0, y=1)
            +-- EST_toots_moderate_path (x=2, y=1)
                    |
                    +-- EST_centre_party_consolidation (x=0, y=1)
```

## Acceptance Criteria
- [ ] EKRE path focuses appear correctly in the focus tree
- [ ] Centre Party leader focuses appear correctly in the focus tree
- [ ] Completing leader focuses correctly sets the country leader
- [ ] Localization displays correctly for all focuses
- [ ] Focus icons display correctly (or use generic placeholders)
- [ ] AI can navigate the new focus paths appropriately
- [ ] No errors in error.log related to these focuses
- [ ] Focus prerequisites work correctly

## Dependencies
- Depends on: ISSUE-001 (Add EKRE Leaders), ISSUE-002 (Add Centre Party Leaders)
- Blocks: None

## Notes

### Icon Placeholders
The focuses use generic icon references (`GFX_focus_generic_nationalism`, `GFX_focus_generic_political_leader`, etc.). Custom icons can be created later. Existing mod icons that may work:
- `EST_estonian_independent_party` - for nationalist focuses
- `neutral_est` - for Centre Party focuses
- `Election_graphics` - for leader selection focuses

### Balance Considerations
- Focus costs are set to 5 (35 days) for minor focuses and 8.6 (60 days) for significant ones
- Political power rewards are balanced against stability changes
- Date requirements ensure historical plausibility

### Alternative Implementation: Using promote_character
If characters are defined in `common/characters/EST.txt` with the character system, you can use:

```pdx
completion_reward = {
    EST_mart_helme = {
        promote_character = Nat_Populism
    }
}
```

Instead of `create_country_leader`. This is cleaner but requires proper character definitions first.
