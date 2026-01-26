# ISSUE-001: Add Siim Kallas

## Summary
Add Siim Kallas as a country leader for Estonia (Prime Minister 2002-2003, Reform Party).

## Context
Siim Kallas served as Prime Minister of Estonia from 2002 to 2003 as leader of the Reform Party. He is a prominent liberal politician who later served as European Commissioner. His father-in-law was Lennart Meri (first post-Soviet President). His daughter Kaja Kallas later became Prime Minister (2021-2024). Adding him provides historical accuracy for early 2000s scenarios and election events.

## Requirements
- [ ] Add character definition to `common/characters/EST.txt`
- [ ] Verify portrait exists or add placeholder portrait
- [ ] Character should be available as country leader with liberalism ideology
- [ ] Add appropriate leader traits reflecting his background

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/characters/EST.txt` | Edit | Add EST_siim_kallas character definition |
| `gfx/leaders/EST/Siim_Kallas.dds` | Verify/Create | Large portrait (156x210 or 512x512) |
| `gfx/leaders/EST/small/Siim_Kallas_small.dds` | Verify/Create | Small portrait (65x67) |

## Implementation

### Step 1: Check Existing Portrait
Portrait `siim_kallas.dds` exists in `gfx/leaders/EST/` (16664 bytes - may be small format).
A larger leader portrait may need to be sourced.

### Step 2: Add Character Definition
Add the following to `common/characters/EST.txt` inside the `characters = { }` block, before the closing brace:

```pdx
	EST_siim_kallas = {
		name = "Siim Kallas"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/siim_kallas.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				western_liberalism
				economist
				pro_american
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

### Step 3: Portrait Requirements (if creating new)
If a new portrait is needed:
- Large portrait: 156x210 pixels or 512x512 pixels, DDS format (DXT5 compression)
- Small portrait: 65x67 pixels, DDS format
- Naming convention: `Siim_Kallas.dds` and `Siim_Kallas_small.dds`

## Acceptance Criteria
- [ ] Character appears in Estonia's leader selection when switching to liberalism ideology
- [ ] Portrait displays correctly in-game
- [ ] No error logs related to missing files or syntax errors
- [ ] Character traits apply correctly

## Historical Notes
- **Full Name**: Siim Kallas
- **Born**: October 2, 1948
- **Party**: Estonian Reform Party (Eesti Reformierakond)
- **PM Term**: January 28, 2002 - April 10, 2003
- **Other Roles**:
  - Minister of Finance (1991-1995)
  - President of Bank of Estonia (1991-1995)
  - European Commissioner (2004-2014)
- **Notable**: Father of Kaja Kallas (PM 2021-2024)

## Suggested Traits
| Trait | Justification |
|-------|---------------|
| western_liberalism | Reform Party is a classic liberal party aligned with Western values |
| economist | Served as Finance Minister and Bank of Estonia President |
| pro_american | Strong pro-NATO, pro-Western stance |

## Dependencies
- Depends on: None
- Blocks: None (can be implemented independently)
