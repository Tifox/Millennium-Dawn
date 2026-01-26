# ISSUE-001: Add EKRE Leaders

## Summary
Add Mart Helme and Martin Helme as playable country leaders for Estonia's nationalist political path, representing EKRE (Estonian Conservative People's Party).

## Context
EKRE is Estonia's main right-wing populist party, founded in 2012. The party has been part of government coalitions and represents a significant nationalist political force. Adding these leaders enables a more authentic nationalist playthrough for Estonia.

The existing focus tree already has a nationalist path (`EST_the_estonian_party`, `EST_estonia_first`, etc.) that uses `nationalist_fascist_are_in_power = yes` as a requirement. These EKRE leaders will be available as alternatives to the generic nationalist leaders.

**Historical Background:**
- **Mart Helme** (born 1949): Diplomat, politician, founder and former chairman of EKRE. Served as Minister of the Interior (2019-2020).
- **Martin Helme** (born 1975): Politician, current EKRE chairman since 2020. Served as Minister of Finance (2019-2021). Son of Mart Helme.

## Requirements
- [ ] Add Mart Helme character definition to `common/characters/EST.txt`
- [ ] Add Martin Helme character definition to `common/characters/EST.txt`
- [ ] Add leader entries to `common/scripted_effects/EST_political_leaders.txt` for Nat_Populism ideology
- [ ] Add localization entries for leader names and descriptions
- [ ] Ensure portraits are referenced (placeholder paths provided, actual portraits need to be created/sourced)

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/characters/EST.txt` | Modify | Add character definitions for Mart Helme and Martin Helme |
| `common/scripted_effects/EST_political_leaders.txt` | Modify | Add to Nat_Populism leader rotation |
| `localisation/english/MD_characters_EST_l_english.yml` | Create/Modify | Add character localization |

## Implementation

### Step 1: Add Character Definitions

Add the following to `common/characters/EST.txt` inside the `characters = { }` block:

```pdx
	EST_mart_helme = {
		name = "Mart Helme"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Mart_Helme.dds"
				small = "gfx/leaders/EST/small/Mart_Helme_small.dds"
			}
		}
		country_leader = {
			desc = "EST_MART_HELME_DESC"
			ideology = Nat_Populism
			traits = {
				nationalist_Nat_Populism
				anti_establishment_firebrand
			}
		}
	}
	EST_martin_helme = {
		name = "Martin Helme"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Martin_Helme.dds"
				small = "gfx/leaders/EST/small/Martin_Helme_small.dds"
			}
		}
		country_leader = {
			desc = "EST_MARTIN_HELME_DESC"
			ideology = Nat_Populism
			traits = {
				nationalist_Nat_Populism
				fiscal_conservative
			}
		}
	}
```

### Step 2: Modify Scripted Effects for Leader Rotation

In `common/scripted_effects/EST_political_leaders.txt`, modify the `set_Nat_Populism` section. Find the existing block:

```pdx
	else_if = { limit = { has_country_flag = set_Nat_Populism }
		if = { limit = { check_variable = { Nat_Populism_leader = 0 } }
			...
```

Replace or extend to include Mart Helme (earlier) and Martin Helme (later):

```pdx
	else_if = { limit = { has_country_flag = set_Nat_Populism }
		if = { limit = { check_variable = { Nat_Populism_leader = 0 } }
			add_to_variable = { Nat_Populism_leader = 1 }
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

			if = { limit = { has_country_flag = do_not_retire } subtract_from_variable = { Nat_Populism_leader = 1 } }
			if = { limit = { date < 2020.11.8 } set_temp_variable = { b = 1 } }
		}
		if = { limit = { check_variable = { Nat_Populism_leader = 1 } NOT = { check_variable = { b = 1 } } }
			add_to_variable = { Nat_Populism_leader = 1 }
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

			if = { limit = { has_country_flag = do_not_retire } subtract_from_variable = { Nat_Populism_leader = 1 } }
			set_temp_variable = { b = 1 }
		}
	}
```

### Step 3: Add Localization

Create or add to `localisation/english/MD_characters_EST_l_english.yml`:

```yml
l_english:
 EST_mart_helme:0 "Mart Helme"
 EST_MART_HELME_DESC:0 "Mart Helme is the founder and former chairman of the Estonian Conservative People's Party (EKRE). A former diplomat who served as Estonia's ambassador to Russia, he later became a prominent voice for nationalist and Eurosceptic policies. Known for his controversial statements and anti-establishment rhetoric, Helme served as Minister of the Interior from 2019 to 2020."
 EST_martin_helme:0 "Martin Helme"
 EST_MARTIN_HELME_DESC:0 "Martin Helme is the current chairman of EKRE and son of party founder Mart Helme. An economist by training, he served as Minister of Finance from 2019 to 2021. Under his leadership, EKRE has maintained its position as a major political force in Estonia, advocating for fiscal conservatism, national sovereignty, and traditional values."
```

### Step 4: Portrait Placeholder Paths

Ensure portrait files exist at these locations (or create placeholders):
- `gfx/leaders/EST/Mart_Helme.dds`
- `gfx/leaders/EST/small/Mart_Helme_small.dds`
- `gfx/leaders/EST/Martin_Helme.dds`
- `gfx/leaders/EST/small/Martin_Helme_small.dds`

Portrait specifications:
- Large: 156x210 pixels, DDS format (DXT5)
- Small: 65x67 pixels, DDS format (DXT5)

## Acceptance Criteria
- [ ] Mart Helme and Martin Helme appear in the character database
- [ ] Both leaders can be set via console command: `set_ruling_party nationalist` then `set_country_flag set_Nat_Populism` and trigger election
- [ ] Leader descriptions display correctly in the leader selection UI
- [ ] No errors in error.log related to these characters
- [ ] Portraits display correctly (or fallback to generic if placeholders used)

## Dependencies
- Depends on: None
- Blocks: ISSUE-003 (Focus Tree Integration)

## Notes
- The existing `Nat_Populism_leader` rotation in `EST_political_leaders.txt` includes Villu Reiljan, Andres Herkel, and Mart Meesak. The Helme leaders should either replace these or be added as alternatives based on game start date.
- Consider adding `anti_establishment_firebrand` and `fiscal_conservative` traits if they don't exist in the mod's trait database. Alternative traits: `demagogue`, `nationalist_symbol`.
- Martin Helme became EKRE chairman in November 2020, so the date transition should reflect this.
