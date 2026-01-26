# TASK-003: Alternative Political Leaders for Estonia

## Overview

This task adds alternative political leaders for Estonia's non-mainstream political paths, specifically for the **EKRE (Conservative People's Party)** nationalist path and the **Centre Party** centrist/social democratic path.

## Scope

### EKRE Leaders (Nationalist Path)
EKRE (Eesti Konservatiivne Rahvaerakond / Estonian Conservative People's Party) is a right-wing populist, national conservative party. Leaders from this party should be available for the nationalist political path.

**Leaders to Add:**
- **Mart Helme** - Founder and former chairman of EKRE, former Minister of the Interior
- **Martin Helme** - Current chairman, former Minister of Finance, son of Mart Helme

**Ideology Mapping:** `nationalism` / `Nat_Populism` (right-wing populist/nationalist)

### Centre Party Leaders (Centrist Path)
The Estonian Centre Party (Eesti Keskerakond) is a centrist to centre-left party with a significant Russian-speaking voter base. It has historically been associated with social democratic and populist policies.

**Leaders to Add:**
- **Edgar Savisaar** - Founder and longtime chairman (already partially implemented in scripted effects)
- **Juri Ratas** - Former Prime Minister (2016-2021), former party chairman
- **Jaan Toots** - Historical political figure, represents party's early history

**Ideology Mapping:** `social_democrat` / `progressivism` / `oligarchism` (for Centre Party's more centrist-populist wing)

## Issues

| Issue | Title | Priority | Status |
|-------|-------|----------|--------|
| ISSUE-001 | Add EKRE Leaders | High | TODO |
| ISSUE-002 | Add Centre Party Leaders | Medium | TODO |
| ISSUE-003 | Focus Tree Integration | High | TODO |

## File Structure

```
TASK-003-alternative-leaders/
├── README.md                              # This file
├── ISSUE-001-add-ekre-leaders.md          # EKRE leader definitions
├── ISSUE-002-add-centre-party-leaders.md  # Centre Party leader definitions
└── ISSUE-003-focus-tree-integration.md    # Focus tree paths for leaders
```

## Related Files

| File | Purpose |
|------|---------|
| `common/characters/EST.txt` | Character definitions (military advisors currently) |
| `common/scripted_effects/EST_political_leaders.txt` | Scripted effects for leader rotation |
| `common/national_focus/05_estonia.txt` | Estonia focus tree |
| `localisation/english/MD_focus_EST_l_english.yml` | Localization for focuses |

## Dependencies

- TASK-001 (Political Leaders) should be completed or coordinated with, as both involve adding characters
- TASK-002 (Election Events) may reference these leaders for election outcomes

## Implementation Notes

1. **Character Definition Pattern**: Follow the existing pattern in `EST.txt` for character IDs:
   - Format: `EST_[firstname]_[surname]`
   - Example: `EST_mart_helme`, `EST_martin_helme`

2. **Portrait Requirements**: Each leader needs:
   - Large portrait: `gfx/leaders/EST/[Name].dds` (156x210 or similar)
   - Small portrait: `gfx/leaders/EST/small/[Name]_small.dds` (65x67)

3. **Leader Traits**: Use appropriate traits from Millennium Dawn's trait system:
   - `nationalist_Nat_Populism` for EKRE leaders
   - `neutrality_oligarchism` or `western_socialism` for Centre Party leaders

4. **Focus Tree Integration**: The existing nationalist path (`EST_the_estonian_party`, `EST_estonia_first`, etc.) can be extended to include EKRE-specific content.

## Testing Checklist

- [ ] All characters load without errors
- [ ] Portraits display correctly
- [ ] Leaders can be set via console commands
- [ ] Focus tree paths correctly enable/promote leaders
- [ ] Localization displays properly
