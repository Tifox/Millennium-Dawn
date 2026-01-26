# ISSUE-006: Add Kaja Kallas

## Summary
Add Kaja Kallas as a country leader for Estonia (Prime Minister 2021-2024, Reform Party).

## Context
Kaja Kallas became Estonia's first female Prime Minister in January 2021, succeeding Juri Ratas. As leader of the Reform Party, she has taken a strongly pro-Western, anti-Russia stance, particularly vocal after Russia's 2022 invasion of Ukraine. She became one of the most prominent European voices against Russian aggression and was nominated to become EU High Representative for Foreign Affairs. Her leadership during a critical geopolitical period makes her an essential addition.

## Requirements
- [ ] Add character definition to `common/characters/EST.txt`
- [ ] Verify portrait exists (already present as `kaja_kallas.dds`)
- [ ] Character should be available as country leader with liberalism ideology
- [ ] Add appropriate leader traits reflecting her anti-Russia stance and Reform Party leadership

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/characters/EST.txt` | Edit | Add EST_kaja_kallas character definition |
| `gfx/leaders/EST/kaja_kallas.dds` | Verify | Portrait exists (131168 bytes - leader-sized) |

## Implementation

### Step 1: Verify Existing Portrait
Portrait `kaja_kallas.dds` already exists in `gfx/leaders/EST/` at 131168 bytes (correct size for leader portrait).
No additional portrait work needed.

### Step 2: Add Character Definition
Add the following to `common/characters/EST.txt` inside the `characters = { }` block, before the closing brace:

```pdx
	EST_kaja_kallas = {
		name = "Kaja Kallas"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/kaja_kallas.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				western_liberalism
				anti_russia
				pro_american
				first_female_leader
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

### Step 3: Alternative Trait Configuration
If the above traits don't all exist, use this alternative:

```pdx
	EST_kaja_kallas = {
		name = "Kaja Kallas"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/kaja_kallas.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				western_liberalism
				hawk
				pro_american
				lawyer
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

## Acceptance Criteria
- [ ] Character appears in Estonia's leader selection when switching to liberalism ideology
- [ ] Portrait displays correctly in-game (using existing kaja_kallas.dds)
- [ ] No error logs related to missing files or syntax errors
- [ ] Character traits apply correctly
- [ ] Represents the most recent Reform Party leadership

## Historical Notes
- **Full Name**: Kaja Kallas
- **Born**: June 18, 1977
- **Party**: Estonian Reform Party (Eesti Reformierakond)
- **PM Term**: January 26, 2021 - July 23, 2024
- **Other Roles**:
  - Member of European Parliament (2014-2018)
  - Member of Riigikogu
  - EU High Representative for Foreign Affairs (nominated 2024)
- **Notable**:
  - First female Prime Minister of Estonia
  - Daughter of Siim Kallas (PM 2002-2003)
  - One of Europe's most vocal critics of Russia
  - On Russian "wanted" list since February 2024
  - Strong advocate for Ukraine support and NATO unity

## Suggested Traits
| Trait | Justification |
|-------|---------------|
| western_liberalism | Reform Party liberal ideology |
| anti_russia | Extremely vocal critic of Russia |
| pro_american | Strong pro-NATO stance |
| first_female_leader | Historic appointment |

## Alternative Traits
| Trait | Justification |
|-------|---------------|
| hawk | Hardline foreign policy stance |
| lawyer | Legal background before politics |
| diplomatic_leader | EU High Representative nomination |

## Dependencies
- Depends on: None
- Blocks: None (can be implemented independently)

## Notes
Kaja Kallas is likely the current leader for Estonia in later game start dates. Her strong anti-Russia stance and prominence during the Ukraine crisis make her particularly relevant for modern scenarios. Consider linking her to events related to:
- 2022 Russian invasion of Ukraine
- NATO reinforcement of Baltic states
- EU foreign policy decisions
