# TASK-001: Political Leaders

## Overview

This task adds missing Estonian Prime Ministers and Presidents from 2000-2024 to provide comprehensive political leadership options for Estonia.

## Scope

### Prime Ministers (2000-2024)
| Name | Term | Party | Ideology |
|------|------|-------|----------|
| Siim Kallas | 2002-2003 | Reform Party | liberalism |
| Juhan Parts | 2003-2005 | Res Publica/IRL | conservatism |
| Andrus Ansip | 2005-2014 | Reform Party | liberalism |
| Taavi Roivas | 2014-2016 | Reform Party | liberalism |
| Juri Ratas | 2016-2021 | Centre Party | social_democrat |
| Kaja Kallas | 2021-2024 | Reform Party | liberalism |

### Presidents (1992-2024)
| Name | Term | Background |
|------|------|------------|
| Lennart Meri | 1992-2001 | Writer, diplomat |
| Arnold Ruutel | 2001-2006 | Agrarian/rural background |
| Toomas Hendrik Ilves | 2006-2016 | Social Democrat, diplomat |
| Kersti Kaljulaid | 2016-2021 | Independent, economist |
| Alar Karis | 2021-present | Independent, biologist |

## Files Affected

| File | Purpose |
|------|---------|
| `common/characters/EST.txt` | Character definitions |
| `localisation/english/MD_EST_characters_l_english.yml` | Character name localization (if needed) |
| `gfx/leaders/EST/` | Portrait images (.dds files) |
| `gfx/leaders/EST/small/` | Small portrait images (.dds files) |

## Issues

| Issue | Title | Priority | Status |
|-------|-------|----------|--------|
| ISSUE-001 | Add Siim Kallas | High | Not Started |
| ISSUE-002 | Add Juhan Parts | High | Not Started |
| ISSUE-003 | Add Andrus Ansip | High | Not Started |
| ISSUE-004 | Add Taavi Roivas | High | Not Started |
| ISSUE-005 | Add Juri Ratas | High | Not Started |
| ISSUE-006 | Add Kaja Kallas | High | Not Started |
| ISSUE-007 | Add Estonian Presidents | Medium | Not Started |

## Implementation Order

1. Start with ISSUE-001 through ISSUE-006 (Prime Ministers) as these are independent
2. Complete ISSUE-007 (Presidents) which is also independent
3. All issues can be implemented in parallel as they have no dependencies on each other

## Notes

### Portrait Availability

The following portraits already exist in `gfx/leaders/EST/`:
- `Andrus_Ansip.dds` (131168 bytes - leader-sized)
- `Taavi_Roivas.dds` (131168 bytes - leader-sized)
- `Juri_Ratas.dds` (131168 bytes - leader-sized)
- `kaja_kallas.dds` (131168 bytes - leader-sized)
- `juhan_parts.dds` (16664 bytes - small)
- `siim_kallas.dds` (16664 bytes - small)
- `kersti_kaljulaid.dds` (16664 bytes - small)
- `lennart_meri.dds` (16664 bytes - small)

Missing portraits will need to be sourced and processed to the correct format.

### Ideology Mapping

| Estonian Party | HOI4 Ideology |
|----------------|---------------|
| Reform Party (Eesti Reformierakond) | liberalism |
| Centre Party (Keskerakond) | social_democrat |
| IRL/Isamaa | conservatism |
| Res Publica | conservatism |
| Social Democrats | social_democrat |
| EKRE | Nat_Populism |

### Character ID Convention

All characters follow the pattern: `EST_firstname_lastname` (lowercase, underscores)
- Example: `EST_siim_kallas`, `EST_juhan_parts`
