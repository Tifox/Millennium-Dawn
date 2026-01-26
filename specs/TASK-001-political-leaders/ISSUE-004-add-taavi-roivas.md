# ISSUE-004: Add Taavi Roivas

## Summary
Add Taavi Roivas as a country leader for Estonia (Prime Minister 2014-2016, Reform Party).

## Context
Taavi Roivas became Estonia's youngest Prime Minister at age 34, serving from 2014 to 2016. He succeeded Andrus Ansip as Reform Party leader and continued the party's liberal, pro-EU, pro-NATO policies. His government focused on digital innovation and startup culture. He was removed through a vote of no confidence when coalition partners switched allegiances. Adding him provides coverage for the mid-2010s political period.

## Requirements
- [ ] Add character definition to `common/characters/EST.txt`
- [ ] Verify portrait exists (already present as `Taavi_Roivas.dds`)
- [ ] Character should be available as country leader with liberalism ideology
- [ ] Add appropriate leader traits reflecting his youth and digital focus

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/characters/EST.txt` | Edit | Add EST_taavi_roivas character definition |
| `gfx/leaders/EST/Taavi_Roivas.dds` | Verify | Portrait exists (131168 bytes - leader-sized) |

## Implementation

### Step 1: Verify Existing Portrait
Portrait `Taavi_Roivas.dds` already exists in `gfx/leaders/EST/` at 131168 bytes (correct size for leader portrait).
No additional portrait work needed.

### Step 2: Add Character Definition
Add the following to `common/characters/EST.txt` inside the `characters = { }` block, before the closing brace:

```pdx
	EST_taavi_roivas = {
		name = "Taavi Roivas"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Taavi_Roivas.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				western_liberalism
				young_politician
				pro_american
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

### Step 3: Alternative Trait Configuration
If `young_politician` doesn't exist as a trait:

```pdx
	EST_taavi_roivas = {
		name = "Taavi Roivas"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/Taavi_Roivas.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				western_liberalism
				inexperienced
				pro_american
				reformist
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

## Acceptance Criteria
- [ ] Character appears in Estonia's leader selection when switching to liberalism ideology
- [ ] Portrait displays correctly in-game (using existing Taavi_Roivas.dds)
- [ ] No error logs related to missing files or syntax errors
- [ ] Character traits apply correctly

## Historical Notes
- **Full Name**: Taavi Roivas
- **Born**: September 26, 1979
- **Party**: Estonian Reform Party (Eesti Reformierakond)
- **PM Term**: March 26, 2014 - November 23, 2016
- **Other Roles**:
  - Minister of Social Affairs (2012-2014)
  - Member of Riigikogu
- **Notable**:
  - Youngest PM in Estonian history (34 years old)
  - Youngest head of government in EU at time of appointment
  - Removed through no-confidence vote
  - Strong advocate for digital economy and startups

## Suggested Traits
| Trait | Justification |
|-------|---------------|
| western_liberalism | Reform Party liberal ideology |
| young_politician | Youngest PM in Estonian history |
| pro_american | Continued pro-NATO, pro-Western policies |

## Alternative Traits
| Trait | Justification |
|-------|---------------|
| inexperienced | Young and relatively new to high office |
| reformist | Focus on digital innovation |
| popular_figurehead | Initially popular, youthful image |

## Dependencies
- Depends on: None
- Blocks: None (can be implemented independently)

## Notes
Taavi Roivas would be a potential leader for mid-game events or elections occurring in 2014-2016 timeframe.
