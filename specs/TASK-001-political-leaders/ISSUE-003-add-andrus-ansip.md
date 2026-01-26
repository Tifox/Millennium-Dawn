# ISSUE-003: Add Andrus Ansip

## Summary
Add Andrus Ansip as a country leader for Estonia (Prime Minister 2005-2014, Reform Party).

## Context
Andrus Ansip was Estonia's longest-serving Prime Minister, holding office from 2005 to 2014. As leader of the Reform Party, he guided Estonia through EU and Eurozone accession, the 2008 financial crisis, and the 2007 Bronze Soldier crisis with Russia. He later served as European Commissioner for the Digital Single Market. His nearly decade-long tenure makes him one of the most significant Estonian political figures of the modern era.

## Requirements
- [ ] Add character definition to `common/characters/EST.txt`
- [ ] Verify portrait exists (already present as `Andrus_Ansip.dds`)
- [ ] Character should be available as country leader with liberalism ideology
- [ ] Add appropriate leader traits reflecting his long tenure and achievements

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/characters/EST.txt` | Edit | Add EST_andrus_ansip character definition |
| `gfx/leaders/EST/Andrus_Ansip.dds` | Verify | Portrait exists (131168 bytes - leader-sized) |

## Implementation

### Step 1: Verify Existing Portrait
Portrait `Andrus_Ansip.dds` already exists in `gfx/leaders/EST/` at 131168 bytes (correct size for leader portrait).
No additional portrait work needed.

### Step 2: Add Character Definition
Add the following to `common/characters/EST.txt` inside the `characters = { }` block, before the closing brace:

```pdx
	EST_andrus_ansip = {
		name = "Andrus Ansip"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Andrus_Ansip.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				western_liberalism
				prime_minister_trait
				pro_american
				e_government_pioneer
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

### Step 3: Alternative Trait Configuration
If `e_government_pioneer` doesn't exist as a trait, use this alternative:

```pdx
	EST_andrus_ansip = {
		name = "Andrus Ansip"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Andrus_Ansip.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				western_liberalism
				experienced_politician
				pro_american
				economic_reformer
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

## Acceptance Criteria
- [ ] Character appears in Estonia's leader selection when switching to liberalism ideology
- [ ] Portrait displays correctly in-game (using existing Andrus_Ansip.dds)
- [ ] No error logs related to missing files or syntax errors
- [ ] Character traits apply correctly

## Historical Notes
- **Full Name**: Andrus Ansip
- **Born**: October 1, 1956
- **Party**: Estonian Reform Party (Eesti Reformierakond)
- **PM Term**: April 13, 2005 - March 26, 2014 (nearly 9 years)
- **Other Roles**:
  - Mayor of Tartu (1998-2004)
  - European Commissioner for Digital Single Market (2014-2019)
- **Key Events During Tenure**:
  - Eurozone accession (2011)
  - 2007 Bronze Soldier riots and Russian cyberattacks
  - 2008 global financial crisis navigation
  - Development of e-Estonia digital government

## Suggested Traits
| Trait | Justification |
|-------|---------------|
| western_liberalism | Reform Party is a classic liberal party |
| prime_minister_trait | Longest-serving PM in Estonian history |
| pro_american | Strong pro-NATO, pro-Western orientation |
| e_government_pioneer | Led Estonia's digital transformation |

## Alternative Traits
| Trait | Justification |
|-------|---------------|
| experienced_politician | Nearly 9 years as PM |
| economic_reformer | Guided Estonia through financial crisis |
| crisis_manager | Handled Bronze Soldier crisis and cyberattacks |

## Dependencies
- Depends on: None
- Blocks: None (can be implemented independently)

## Notes
This is likely the starting leader for Estonia in the 2000 bookmark if one exists, given his long tenure starting in 2005.
