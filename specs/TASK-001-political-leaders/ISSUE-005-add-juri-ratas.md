# ISSUE-005: Add Juri Ratas

## Summary
Add Juri Ratas as a country leader for Estonia (Prime Minister 2016-2021, Centre Party).

## Context
Juri Ratas served as Prime Minister from 2016 to 2021, representing the Centre Party. He came to power after Taavi Roivas lost a no-confidence vote. The Centre Party traditionally draws support from Russian-speaking minorities and has a more social democratic orientation. Ratas led two different coalition governments and navigated the COVID-19 pandemic before resigning amid a corruption scandal involving his party. Adding him provides the first non-Reform Party PM since 2005 and represents a different ideological option.

## Requirements
- [ ] Add character definition to `common/characters/EST.txt`
- [ ] Verify portrait exists (already present as `Juri_Ratas.dds`)
- [ ] Character should be available as country leader with social_democrat ideology
- [ ] Add appropriate leader traits reflecting his Centre Party background

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/characters/EST.txt` | Edit | Add EST_juri_ratas character definition |
| `gfx/leaders/EST/Juri_Ratas.dds` | Verify | Portrait exists (131168 bytes - leader-sized) |

## Implementation

### Step 1: Verify Existing Portrait
Portrait `Juri_Ratas.dds` already exists in `gfx/leaders/EST/` at 131168 bytes (correct size for leader portrait).
No additional portrait work needed.

### Step 2: Add Character Definition
Add the following to `common/characters/EST.txt` inside the `characters = { }` block, before the closing brace:

```pdx
	EST_juri_ratas = {
		name = "Juri Ratas"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Juri_Ratas.dds"
			}
		}
		country_leader = {
			ideology = social_democrat
			traits = {
				centrist_democrat
				coalition_builder
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

### Step 3: Alternative Trait Configuration
If the above traits don't exist, use this alternative:

```pdx
	EST_juri_ratas = {
		name = "Juri Ratas"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Juri_Ratas.dds"
			}
		}
		country_leader = {
			ideology = social_democrat
			traits = {
				political_dancer
				pro_russian_sentiment
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

## Acceptance Criteria
- [ ] Character appears in Estonia's leader selection when switching to social_democrat ideology
- [ ] Portrait displays correctly in-game (using existing Juri_Ratas.dds)
- [ ] No error logs related to missing files or syntax errors
- [ ] Character traits apply correctly
- [ ] Represents ideological alternative to Reform Party leaders

## Historical Notes
- **Full Name**: Juri Ratas
- **Born**: July 2, 1978
- **Party**: Estonian Centre Party (Eesti Keskerakond)
- **PM Term**: November 23, 2016 - January 26, 2021
- **Other Roles**:
  - Mayor of Tallinn (2005-2007, acting)
  - President of Riigikogu (2021-2023)
  - Deputy Mayor of Tallinn
- **Notable**:
  - First non-Reform PM since 2005
  - Led Estonia during COVID-19 pandemic
  - Resigned due to corruption scandal in party
  - Centre Party has historically appealed to Russian-speaking minority

## Suggested Traits
| Trait | Justification |
|-------|---------------|
| centrist_democrat | Centre Party occupies middle ground |
| coalition_builder | Successfully formed two different coalitions |
| political_dancer | Known for political flexibility |

## Alternative Traits
| Trait | Justification |
|-------|---------------|
| pro_russian_sentiment | Centre Party has Russian-speaking voter base |
| populist | Centre Party uses populist messaging |
| corruptible | Resigned amid corruption scandal |

## Ideology Notes
The Centre Party (Keskerakond) can be represented as:
- `social_democrat` - Their economic policies lean left
- `progressivism` - Alternative if more centrist representation needed

## Dependencies
- Depends on: None
- Blocks: None (can be implemented independently)

## Notes
Juri Ratas provides an important alternative to the Reform Party dominance and represents the possibility of political change through elections or events. His Centre Party background and different voter base (including Russian-speakers) makes him useful for alternative political paths.
