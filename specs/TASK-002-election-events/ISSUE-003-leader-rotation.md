# ISSUE-003: Leader Rotation

## Summary
Implement leader (Prime Minister) changes based on election results, creating historically accurate leaders for each election outcome.

## Context
When elections occur in Estonia, the Prime Minister changes based on which party wins. This issue implements the leader rotation system that creates appropriate leaders for each party victory. Leaders must be created dynamically as the same party may have different leaders at different times (e.g., Reform Party had Ansip 2005-2014, Roivas 2014-2016, and Kallas 2021+).

## Requirements
- [ ] Create historical Prime Minister leaders for each election year
- [ ] Implement leader switching based on election option chosen
- [ ] Handle multiple leaders per party across different time periods
- [ ] Add leader portraits path references
- [ ] Create leader traits appropriate to each PM

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Update election events to include leader creation |
| `common/scripted_effects/99_EST_scripted_effects.txt` | Modify | Add leader creation effects |
| `localisation/english/EST_events_l_english.yml` | Modify | Add leader description localization |

## Implementation

### Step 1: Create Leader Scripted Effects

Add to `common/scripted_effects/99_EST_scripted_effects.txt`:

```pdx
# Create Estonian Reform Party Leaders
EST_create_reform_leader = {
	if = {
		limit = { date < 2005.4.1 }
		# Siim Kallas (2002-2003) - though game starts in 2000 with Mart Laar
		create_country_leader = {
			name = "Siim Kallas"
			desc = "EST_siim_kallas_desc"
			picture = "gfx/leaders/EST/Siim_Kallas.dds"
			expire = "2050.1.1"
			ideology = liberal_conservative
			traits = {
				trait_economist
			}
		}
	}
	else_if = {
		limit = {
			date > 2005.4.1
			date < 2014.3.1
		}
		# Andrus Ansip (2005-2014)
		create_country_leader = {
			name = "Andrus Ansip"
			desc = "EST_andrus_ansip_desc"
			picture = "gfx/leaders/EST/Andrus_Ansip.dds"
			expire = "2050.1.1"
			ideology = liberal_conservative
			traits = {
				trait_technocrat
				democratic_reformer
			}
		}
	}
	else_if = {
		limit = {
			date > 2014.3.1
			date < 2016.11.1
		}
		# Taavi Roivas (2014-2016)
		create_country_leader = {
			name = "Taavi Roivas"
			desc = "EST_taavi_roivas_desc"
			picture = "gfx/leaders/EST/Taavi_Roivas.dds"
			expire = "2050.1.1"
			ideology = liberal_conservative
			traits = {
				young_leader
				pro_european
			}
		}
	}
	else = {
		# Kaja Kallas (2021+)
		create_country_leader = {
			name = "Kaja Kallas"
			desc = "EST_kaja_kallas_desc"
			picture = "gfx/leaders/EST/Kaja_Kallas.dds"
			expire = "2050.1.1"
			ideology = liberal_conservative
			traits = {
				pro_european
				hawk
			}
		}
	}
}

# Create Estonian Isamaa Leaders
EST_create_isamaa_leader = {
	if = {
		limit = { date < 2003.4.1 }
		# Mart Laar (1999-2002)
		create_country_leader = {
			name = "Mart Laar"
			desc = "EST_mart_laar_desc"
			picture = "gfx/leaders/EST/Mart_Laar.dds"
			expire = "2050.1.1"
			ideology = social_conservatism
			traits = {
				trait_reformer
				pro_western
			}
		}
	}
	else_if = {
		limit = {
			date > 2003.4.1
			date < 2005.4.1
		}
		# Juhan Parts (2003-2005)
		create_country_leader = {
			name = "Juhan Parts"
			desc = "EST_juhan_parts_desc"
			picture = "gfx/leaders/EST/Juhan_Parts.dds"
			expire = "2050.1.1"
			ideology = social_conservatism
			traits = {
				trait_administrator
			}
		}
	}
	else = {
		# Generic Isamaa leader for ahistorical paths
		create_country_leader = {
			name = "Urmas Reinsalu"
			desc = "EST_urmas_reinsalu_desc"
			picture = "gfx/leaders/EST/Urmas_Reinsalu.dds"
			expire = "2050.1.1"
			ideology = social_conservatism
			traits = {
				hawk
				nationalist_symbol
			}
		}
	}
}

# Create Estonian Centre Party Leaders
EST_create_centre_leader = {
	if = {
		limit = { date < 2016.11.1 }
		# Edgar Savisaar (long-time leader, though never PM)
		create_country_leader = {
			name = "Edgar Savisaar"
			desc = "EST_edgar_savisaar_desc"
			picture = "gfx/leaders/EST/Edgar_Savisaar.dds"
			expire = "2050.1.1"
			ideology = agrarianism
			traits = {
				popular_figurehead
				russian_friendly
			}
		}
	}
	else = {
		# Juri Ratas (2016-2021)
		create_country_leader = {
			name = "Juri Ratas"
			desc = "EST_juri_ratas_desc"
			picture = "gfx/leaders/EST/Juri_Ratas.dds"
			expire = "2050.1.1"
			ideology = agrarianism
			traits = {
				smooth_talking_charmer
				coalition_builder
			}
		}
	}
}

# Create Estonian EKRE Leaders
EST_create_ekre_leader = {
	# Mart Helme is the founding leader
	create_country_leader = {
		name = "Mart Helme"
		desc = "EST_mart_helme_desc"
		picture = "gfx/leaders/EST/Mart_Helme.dds"
		expire = "2050.1.1"
		ideology = despotism
		traits = {
			nationalist_symbol
			anti_immigration
		}
	}
}
```

### Step 2: Update Election Event with Leader Creation

Modify the election event options in `events/Estonia.txt` to call the leader creation effects:

```pdx
# Estonian Parliamentary Election (updated with leader rotation)
country_event = {
	id = est_election.1
	title = est_election.1.t
	desc = est_election.1.d
	picture = GFX_EST_LocalElections

	is_triggered_only = yes

	trigger = {
		original_tag = EST
		has_government = democratic
		NOT = { has_war = yes }
	}

	immediate = {
		hidden_effect = {
			set_country_flag = EST_election_in_progress
		}
	}

	# Option A: Reform Party Victory (Liberal)
	option = {
		name = est_election.1.a
		log = "[GetDateText]: [This.GetName]: event est_election.1.a executed - Reform Party wins"

		hidden_effect = {
			EST_create_reform_leader = yes
		}

		set_temp_variable = { rul_party_temp = 2 }
		change_ruling_party_effect = yes

		set_temp_variable = { party_index = 2 }
		set_temp_variable = { party_popularity_increase = 0.05 }
		set_temp_variable = { temp_outlook_increase = 0.05 }
		add_relative_party_popularity = yes

		add_political_power = 50
		add_stability = 0.02

		set_country_flag = EST_election_reform_victory
		clr_country_flag = EST_election_centre_victory
		clr_country_flag = EST_election_isamaa_victory
		clr_country_flag = EST_election_ekre_victory
		clr_country_flag = EST_election_in_progress

		hidden_effect = {
			country_event = { id = est_election.10 days = 1 }
		}

		ai_chance = {
			base = 40
			modifier = {
				is_historical_focus_on = yes
				add = 30
			}
			modifier = {
				has_country_flag = EST_lean_west_flag
				add = 20
			}
		}
	}

	# Option B: Isamaa Victory (Conservative)
	option = {
		name = est_election.1.b
		log = "[GetDateText]: [This.GetName]: event est_election.1.b executed - Isamaa wins"

		hidden_effect = {
			EST_create_isamaa_leader = yes
		}

		set_temp_variable = { rul_party_temp = 3 }
		change_ruling_party_effect = yes

		set_temp_variable = { party_index = 3 }
		set_temp_variable = { party_popularity_increase = 0.05 }
		set_temp_variable = { temp_outlook_increase = 0.05 }
		add_relative_party_popularity = yes

		add_political_power = 50
		add_stability = 0.02

		clr_country_flag = EST_election_reform_victory
		clr_country_flag = EST_election_centre_victory
		set_country_flag = EST_election_isamaa_victory
		clr_country_flag = EST_election_ekre_victory
		clr_country_flag = EST_election_in_progress

		hidden_effect = {
			country_event = { id = est_election.10 days = 1 }
		}

		ai_chance = {
			base = 25
			modifier = {
				is_historical_focus_on = yes
				date < 2007.1.1
				add = 25
			}
		}
	}

	# Option C: Centre Party Victory
	option = {
		name = est_election.1.c
		log = "[GetDateText]: [This.GetName]: event est_election.1.c executed - Centre Party wins"

		hidden_effect = {
			EST_create_centre_leader = yes
		}

		set_temp_variable = { rul_party_temp = 15 }
		change_ruling_party_effect = yes

		set_temp_variable = { party_index = 15 }
		set_temp_variable = { party_popularity_increase = 0.05 }
		set_temp_variable = { temp_outlook_increase = 0.05 }
		add_relative_party_popularity = yes

		add_political_power = 50
		add_stability = 0.02

		clr_country_flag = EST_election_reform_victory
		set_country_flag = EST_election_centre_victory
		clr_country_flag = EST_election_isamaa_victory
		clr_country_flag = EST_election_ekre_victory
		clr_country_flag = EST_election_in_progress

		hidden_effect = {
			country_event = { id = est_election.10 days = 1 }
		}

		ai_chance = {
			base = 20
			modifier = {
				has_country_flag = EST_lean_east_flag
				add = 30
			}
			modifier = {
				is_historical_focus_on = yes
				date > 2015.1.1
				date < 2021.1.1
				add = 40
			}
		}
	}

	# Option D: EKRE Victory (Nationalist) - only available after 2012
	option = {
		name = est_election.1.d
		log = "[GetDateText]: [This.GetName]: event est_election.1.d executed - EKRE wins"

		trigger = {
			OR = {
				date > 2012.1.1
				has_country_flag = EST_ekre_has_formed
			}
		}

		hidden_effect = {
			EST_create_ekre_leader = yes
		}

		set_temp_variable = { rul_party_temp = 21 }
		change_ruling_party_effect = yes

		set_temp_variable = { party_index = 21 }
		set_temp_variable = { party_popularity_increase = 0.08 }
		set_temp_variable = { temp_outlook_increase = 0.08 }
		add_relative_party_popularity = yes

		add_political_power = 25
		add_stability = -0.02
		add_war_support = 0.05

		clr_country_flag = EST_election_reform_victory
		clr_country_flag = EST_election_centre_victory
		clr_country_flag = EST_election_isamaa_victory
		set_country_flag = EST_election_ekre_victory
		clr_country_flag = EST_election_in_progress

		hidden_effect = {
			country_event = { id = est_election.10 days = 1 }
		}

		ai_chance = {
			base = 10
			modifier = {
				is_historical_focus_on = yes
				factor = 0.1
			}
			modifier = {
				has_country_flag = EST_lean_neutral_flag
				add = 15
			}
		}
	}
}
```

### Step 3: Add Leader Localization

Add to `localisation/english/EST_events_l_english.yml`:

```yml
 # Leader Descriptions
 EST_siim_kallas_desc: "A prominent economist and politician, Siim Kallas served as Prime Minister and later became European Commissioner. He is known for his economic reforms and pro-European stance."
 EST_andrus_ansip_desc: "Andrus Ansip is Estonia's longest-serving Prime Minister, known for his liberal economic policies, digital governance initiatives, and staunch pro-EU and pro-NATO positions."
 EST_taavi_roivas_desc: "The youngest Prime Minister in Estonian history, Taavi Roivas represents a new generation of Estonian politicians committed to modernization and European integration."
 EST_kaja_kallas_desc: "Kaja Kallas, daughter of former PM Siim Kallas, leads the Reform Party with a strong pro-European and pro-NATO stance. She has been a vocal critic of Russian aggression."
 EST_mart_laar_desc: "A historian turned politician, Mart Laar implemented radical free-market reforms in the 1990s that transformed Estonia into one of the most economically free nations in the world."
 EST_juhan_parts_desc: "Juhan Parts led the Res Publica party to a surprise victory in 2003. His government focused on anti-corruption measures and continued Estonia's path to EU membership."
 EST_urmas_reinsalu_desc: "A conservative politician and former Minister of Justice, Urmas Reinsalu is known for his strong stance on national security and defense matters."
 EST_edgar_savisaar_desc: "A veteran politician who played a key role in Estonia's independence movement, Edgar Savisaar leads the Centre Party with a more Russia-friendly approach than other Estonian parties."
 EST_juri_ratas_desc: "Juri Ratas became Prime Minister in 2016 after forming an unexpected coalition. He represents a more moderate wing of the Centre Party and has worked to reduce ethnic tensions."
 EST_mart_helme_desc: "The founder and chairman of EKRE, Mart Helme is a former diplomat who has become the face of Estonian nationalism and euroscepticism."
```

### Step 4: Create Portrait References

Note: Leader portraits must exist at the specified paths. If portraits don't exist, create placeholder entries or use existing generic portraits.

Expected portrait locations:
```
gfx/leaders/EST/Siim_Kallas.dds
gfx/leaders/EST/Andrus_Ansip.dds
gfx/leaders/EST/Taavi_Roivas.dds
gfx/leaders/EST/Kaja_Kallas.dds
gfx/leaders/EST/Mart_Laar.dds
gfx/leaders/EST/Juhan_Parts.dds
gfx/leaders/EST/Urmas_Reinsalu.dds
gfx/leaders/EST/Edgar_Savisaar.dds
gfx/leaders/EST/Juri_Ratas.dds
gfx/leaders/EST/Mart_Helme.dds
```

If portraits are missing, you can use a fallback:
```pdx
picture = "gfx/leaders/Generic/Portrait_Europe_Generic_3.dds"
```

### Step 5: Historical Accuracy Table

| Election Year | Historical Winner | PM | Implemented |
|--------------|-------------------|-----|-------------|
| 2003 | Res Publica (Isamaa coalition) | Juhan Parts | Yes |
| 2005 (coalition collapse) | Reform Party | Andrus Ansip | Yes |
| 2007 | Reform Party | Andrus Ansip | Yes |
| 2011 | Reform Party | Andrus Ansip | Yes |
| 2014 (Ansip resignation) | Reform Party | Taavi Roivas | Yes |
| 2015 | Reform Party | Taavi Roivas | Yes |
| 2016 (coalition collapse) | Centre Party | Juri Ratas | Yes |
| 2019 | Reform Party (Centre coalition) | Juri Ratas | Yes |
| 2021 (coalition collapse) | Reform Party | Kaja Kallas | Yes |
| 2023 | Reform Party | Kaja Kallas | Yes |

## Acceptance Criteria
- [ ] Each party option creates an appropriate leader for the time period
- [ ] Leader names match historical PMs
- [ ] Leader traits reflect their political positions
- [ ] Leader descriptions are localized
- [ ] Portrait paths are correctly specified (or use generic fallbacks)
- [ ] Leaders change when different party wins subsequent elections
- [ ] Multiple elections with same party winner update to correct leader for that era

## Dependencies
- Depends on: ISSUE-001 (election event structure), ISSUE-002 (election triggers)
- Blocks: None (final issue in this task)

## Testing

### Test 1: 2003 Election - Isamaa Victory
```
tag EST
event est_election.1
# Select option B (Isamaa)
# Verify Juhan Parts becomes leader
```

### Test 2: 2007 Election - Reform Victory
```
tag EST
set_country_flag EST_2003_election_held
nextyear 2007
# Wait for March, select Reform Party
# Verify Andrus Ansip becomes leader
```

### Test 3: 2019 Election - Centre Victory
```
tag EST
set_country_flag EST_2015_election_held
nextyear 2019
# Wait for March, select Centre Party
# Verify Juri Ratas becomes leader
```

### Test 4: Leader Trait Verification
After each leader is created, open the politics panel and verify:
- Leader name is correct
- Leader portrait displays (or generic if missing)
- Leader traits are appropriate

## Notes

- The ideology tokens (e.g., `liberal_conservative`, `social_conservatism`, `agrarianism`, `despotism`) must match those defined in the mod's ideology files
- If specific ideology subtypes don't exist, use the closest match or create them
- Leader traits should be checked against `common/country_leader/` trait definitions
- Some leaders may need custom traits created if the default traits don't fit
