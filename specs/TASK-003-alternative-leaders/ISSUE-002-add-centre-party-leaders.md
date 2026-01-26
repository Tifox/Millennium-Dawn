# ISSUE-002: Add Centre Party Leaders

## Summary
Add Edgar Savisaar (expand existing), Juri Ratas, and Jaan Toots as playable country leaders for Estonia's Centre Party political path.

## Context
The Estonian Centre Party (Eesti Keskerakond) is a centrist to centre-left political party with a significant Russian-speaking voter base. It was one of the most influential parties in Estonian politics from the 1990s through the 2010s.

Edgar Savisaar already exists in `EST_political_leaders.txt` under the `oligarchism` ideology. This issue expands his definition and adds additional Centre Party leaders for a more complete political path.

**Historical Background:**
- **Edgar Savisaar** (1950-2022): Founder of the Centre Party, served as Prime Minister (1990-1992) and longtime mayor of Tallinn. A controversial but influential figure in Estonian politics.
- **Juri Ratas** (born 1978): Prime Minister of Estonia (2016-2021), former Centre Party chairman. Known for forming coalition governments including with EKRE.
- **Jaan Toots** (born 1952): Politician and economist, represents the party's earlier history and moderate wing.

## Requirements
- [ ] Expand Edgar Savisaar character definition (currently only in scripted effects)
- [ ] Add Juri Ratas character definition to `common/characters/EST.txt`
- [ ] Add Jaan Toots character definition to `common/characters/EST.txt`
- [ ] Update `common/scripted_effects/EST_political_leaders.txt` for `oligarchism` ideology leader rotation
- [ ] Add localization entries for all leaders
- [ ] Ensure portraits are referenced

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/characters/EST.txt` | Modify | Add character definitions for Centre Party leaders |
| `common/scripted_effects/EST_political_leaders.txt` | Modify | Expand oligarchism leader rotation |
| `localisation/english/MD_characters_EST_l_english.yml` | Create/Modify | Add character localization |

## Implementation

### Step 1: Add Character Definitions

Add the following to `common/characters/EST.txt` inside the `characters = { }` block:

```pdx
	EST_edgar_savisaar = {
		name = "Edgar Savisaar"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Edgar_Savisaar.dds"
				small = "gfx/leaders/EST/small/Edgar_Savisaar_small.dds"
			}
		}
		country_leader = {
			desc = "EST_EDGAR_SAVISAAR_DESC"
			ideology = oligarchism
			expire = "2022.10.20"
			traits = {
				neutrality_oligarchism
				career_politician
				popular_figurehead
			}
		}
	}
	EST_juri_ratas = {
		name = "Juri Ratas"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Juri_Ratas.dds"
				small = "gfx/leaders/EST/small/Juri_Ratas_small.dds"
			}
		}
		country_leader = {
			desc = "EST_JURI_RATAS_DESC"
			ideology = oligarchism
			traits = {
				neutrality_oligarchism
				coalition_builder
			}
		}
	}
	EST_jaan_toots = {
		name = "Jaan Toots"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Jaan_Toots.dds"
				small = "gfx/leaders/EST/small/Jaan_Toots_small.dds"
			}
		}
		country_leader = {
			desc = "EST_JAAN_TOOTS_DESC"
			ideology = oligarchism
			traits = {
				neutrality_oligarchism
				economist
			}
		}
	}
```

### Step 2: Alternative Social Democrat Version

If Centre Party should be represented as `social_democrat` / `socialism` ideology instead:

```pdx
	EST_juri_ratas_socdem = {
		name = "Juri Ratas"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Juri_Ratas.dds"
				small = "gfx/leaders/EST/small/Juri_Ratas_small.dds"
			}
		}
		country_leader = {
			desc = "EST_JURI_RATAS_DESC"
			ideology = socialism
			traits = {
				western_socialism
				coalition_builder
			}
		}
	}
```

### Step 3: Modify Scripted Effects for Leader Rotation

In `common/scripted_effects/EST_political_leaders.txt`, find and expand the `set_oligarchism` section:

```pdx
	else_if = { limit = { has_country_flag = set_oligarchism }
		if = { limit = { check_variable = { oligarchism_leader = 0 } }
			add_to_variable = { oligarchism_leader = 1 }
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

			if = { limit = { has_country_flag = do_not_retire } subtract_from_variable = { oligarchism_leader = 1 } }
			if = { limit = { date < 2016.11.23 } set_temp_variable = { b = 1 } }
		}
		if = { limit = { check_variable = { oligarchism_leader = 1 } NOT = { check_variable = { b = 1 } } }
			add_to_variable = { oligarchism_leader = 1 }
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

			if = { limit = { has_country_flag = do_not_retire } subtract_from_variable = { oligarchism_leader = 1 } }
			if = { limit = { date < 2021.1.26 } set_temp_variable = { b = 1 } }
		}
		if = { limit = { check_variable = { oligarchism_leader = 2 } NOT = { check_variable = { b = 1 } } }
			add_to_variable = { oligarchism_leader = 1 }
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

			if = { limit = { has_country_flag = do_not_retire } subtract_from_variable = { oligarchism_leader = 1 } }
			set_temp_variable = { b = 1 }
		}
	}
```

### Step 4: Add Localization

Add to `localisation/english/MD_characters_EST_l_english.yml`:

```yml
l_english:
 EST_edgar_savisaar:0 "Edgar Savisaar"
 EST_EDGAR_SAVISAAR_DESC:0 "Edgar Savisaar was the founder and longtime chairman of the Estonian Centre Party. A pivotal figure in Estonia's independence movement, he served as Prime Minister from 1990 to 1992 and later as Mayor of Tallinn for over a decade. Known for his populist style and ability to appeal to Russian-speaking minorities, Savisaar remained a controversial but influential figure in Estonian politics until his death in 2022."
 EST_juri_ratas:0 "Juri Ratas"
 EST_JURI_RATAS_DESC:0 "Juri Ratas served as Prime Minister of Estonia from 2016 to 2021, leading coalition governments that included both left-wing and right-wing parties. A pragmatic politician known for his coalition-building skills, Ratas brought the Centre Party back into government after years in opposition. His tenure was marked by economic growth but also controversy over coalition partnerships."
 EST_jaan_toots:0 "Jaan Toots"
 EST_JAAN_TOOTS_DESC:0 "Jaan Toots is an Estonian economist and politician who has been associated with the Centre Party's moderate wing. With a background in economics and public administration, Toots represents the party's focus on social welfare and economic pragmatism rather than ethnic politics."
```

### Step 5: Portrait Placeholder Paths

Ensure portrait files exist at these locations:
- `gfx/leaders/EST/Edgar_Savisaar.dds` (may already exist as `edgar_savisaar.dds`)
- `gfx/leaders/EST/small/Edgar_Savisaar_small.dds`
- `gfx/leaders/EST/Juri_Ratas.dds`
- `gfx/leaders/EST/small/Juri_Ratas_small.dds`
- `gfx/leaders/EST/Jaan_Toots.dds`
- `gfx/leaders/EST/small/Jaan_Toots_small.dds`

**Note:** Check for existing portrait at `gfx/leaders/USoE/juri_ratas.dds` as referenced in `USoE_political_leaders.txt`. This can be reused or moved.

Portrait specifications:
- Large: 156x210 pixels, DDS format (DXT5)
- Small: 65x67 pixels, DDS format (DXT5)

## Acceptance Criteria
- [ ] Edgar Savisaar character is properly defined and accessible
- [ ] Juri Ratas appears in the character database
- [ ] Jaan Toots appears in the character database
- [ ] All leaders can be set via appropriate ideology triggers
- [ ] Leader descriptions display correctly
- [ ] No errors in error.log related to these characters
- [ ] Portraits display correctly

## Dependencies
- Depends on: None
- Blocks: ISSUE-003 (Focus Tree Integration)

## Notes

### Ideology Considerations
The Centre Party can be represented under multiple ideologies depending on gameplay interpretation:
- **`oligarchism`** (neutrality): Reflects the party's centrist, sometimes clientelist politics under Savisaar
- **`socialism`** (democratic): Reflects the party's social democratic policies and left-leaning platform
- **`progressivism`** (democratic): Alternative for representing the party's modernizing faction

The current implementation uses `oligarchism` to match the existing Savisaar entry in `EST_political_leaders.txt`.

### Historical Dates
- Edgar Savisaar resigned as Centre Party chairman in 2016
- Juri Ratas became Prime Minister on November 23, 2016
- Juri Ratas resigned as Prime Minister on January 26, 2021
- Edgar Savisaar died on October 20, 2022

### Existing References
- Juri Ratas is already referenced in `common/scripted_effects/USoE_political_leaders.txt` for the United States of Europe formable. Ensure consistency with that implementation.
