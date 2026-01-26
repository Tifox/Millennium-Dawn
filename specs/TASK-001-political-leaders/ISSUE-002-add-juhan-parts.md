# ISSUE-002: Add Juhan Parts

## Summary
Add Juhan Parts as a country leader for Estonia (Prime Minister 2003-2005, Res Publica/IRL).

## Context
Juhan Parts served as Prime Minister of Estonia from 2003 to 2005. He led the Res Publica party, a center-right conservative party that later merged with Pro Patria Union to form IRL (Isamaa ja Res Publica Liit). Before entering politics, he served as Auditor General. His government focused on judicial reform and anti-corruption measures. Adding him provides coverage for the mid-2000s political landscape.

## Requirements
- [ ] Add character definition to `common/characters/EST.txt`
- [ ] Verify portrait exists or add placeholder portrait
- [ ] Character should be available as country leader with conservatism ideology
- [ ] Add appropriate leader traits reflecting his background

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/characters/EST.txt` | Edit | Add EST_juhan_parts character definition |
| `gfx/leaders/EST/Juhan_Parts.dds` | Verify/Create | Large portrait (156x210 or 512x512) |
| `gfx/leaders/EST/small/Juhan_Parts_small.dds` | Verify/Create | Small portrait (65x67) |

## Implementation

### Step 1: Check Existing Portrait
Portrait `juhan_parts.dds` exists in `gfx/leaders/EST/` (16664 bytes - small format).
A larger leader portrait may need to be sourced for country leader display.

### Step 2: Add Character Definition
Add the following to `common/characters/EST.txt` inside the `characters = { }` block, before the closing brace:

```pdx
	EST_juhan_parts = {
		name = "Juhan Parts"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/juhan_parts.dds"
			}
		}
		country_leader = {
			ideology = conservatism
			traits = {
				western_conservatism
				anti_corruption_advocate
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
- Naming convention: `Juhan_Parts.dds` and `Juhan_Parts_small.dds`

## Acceptance Criteria
- [ ] Character appears in Estonia's leader selection when switching to conservatism ideology
- [ ] Portrait displays correctly in-game
- [ ] No error logs related to missing files or syntax errors
- [ ] Character traits apply correctly

## Historical Notes
- **Full Name**: Juhan Parts
- **Born**: August 27, 1966
- **Party**: Res Publica (2001-2006), later IRL (after merger)
- **PM Term**: April 10, 2003 - April 13, 2005
- **Other Roles**:
  - Auditor General of Estonia (1998-2002)
  - Minister of Economic Affairs and Infrastructure (2007-2014)
- **Notable**: Led judicial reform efforts, strong anti-corruption stance

## Suggested Traits
| Trait | Justification |
|-------|---------------|
| western_conservatism | Res Publica was a center-right conservative party |
| anti_corruption_advocate | Former Auditor General, known for anti-corruption reforms |

## Alternative Traits to Consider
| Trait | Justification |
|-------|---------------|
| bureaucrat | Background as Auditor General |
| reformist | Known for judicial reform initiatives |

## Dependencies
- Depends on: None
- Blocks: None (can be implemented independently)
