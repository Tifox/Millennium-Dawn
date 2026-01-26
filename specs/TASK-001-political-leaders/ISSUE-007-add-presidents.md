# ISSUE-007: Add Estonian Presidents

## Summary
Add all Estonian Presidents from 1992-2024: Lennart Meri, Arnold Ruutel, Toomas Hendrik Ilves, Kersti Kaljulaid, and Alar Karis.

## Context
Estonia has a parliamentary system where the President is the ceremonial head of state, elected by Parliament or an electoral college. While the Prime Minister holds executive power, the President plays an important diplomatic and symbolic role. Adding all post-independence Presidents provides historical completeness and potential for events involving the presidency. This issue covers all five Presidents since Estonian independence restoration.

## Requirements
- [ ] Add character definitions for all 5 Presidents to `common/characters/EST.txt`
- [ ] Verify portraits exist for each President
- [ ] Presidents can be added as country leaders (ceremonial) or advisors
- [ ] Add appropriate traits reflecting each President's background

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/characters/EST.txt` | Edit | Add 5 President character definitions |
| `gfx/leaders/EST/*.dds` | Verify/Create | Large portraits for each President |
| `gfx/leaders/EST/small/*.dds` | Verify/Create | Small portraits for advisor slots |

## Implementation

### Available Portraits
The following President portraits exist in `gfx/leaders/EST/`:
- `lennart_meri.dds` (16664 bytes - small)
- `kersti_kaljulaid.dds` (16664 bytes - small)

Missing portraits that need to be sourced:
- Arnold Ruutel
- Toomas Hendrik Ilves
- Alar Karis

---

### President 1: Lennart Meri (1992-2001)

**Background**: Writer, filmmaker, diplomat. Considered the father of modern Estonian independence. Served as Foreign Minister before presidency. Strong advocate for NATO and EU membership.

```pdx
	EST_lennart_meri = {
		name = "Lennart Meri"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/lennart_meri.dds"
			}
		}
		country_leader = {
			ideology = conservatism
			traits = {
				father_of_the_nation
				diplomat
				anti_russia
			}
			expire = "2006.3.14"
			id = -1
		}
	}
```

**Historical Notes**:
- Born: March 29, 1929
- Died: March 14, 2006
- Party: Pro Patria (Isamaa)
- Known for: Restoring Estonian independence, diplomacy, cultural preservation

---

### President 2: Arnold Ruutel (2001-2006)

**Background**: Agrarian politician with Soviet-era experience as Chairman of the Supreme Soviet of the Estonian SSR. Represents the transition generation. Focused on rural issues.

```pdx
	EST_arnold_ruutel = {
		name = "Arnold Ruutel"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/arnold_ruutel.dds"
			}
		}
		country_leader = {
			ideology = agrarianism
			traits = {
				agrarian_leader
				experienced_politician
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

**Historical Notes**:
- Born: May 10, 1928
- Party: Estonian People's Union / Estonian Farmers' Party
- Known for: Rural advocacy, transitional figure from Soviet era

**Note**: If `agrarianism` ideology doesn't exist, use `conservatism` or `centrism`.

---

### President 3: Toomas Hendrik Ilves (2006-2016)

**Background**: Diplomat, journalist, political scientist. Raised partly in the United States. Strong pro-Western orientation. Advocate for digital Estonia and cybersecurity. Served as Foreign Minister.

```pdx
	EST_toomas_ilves = {
		name = "Toomas Hendrik Ilves"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/toomas_ilves.dds"
			}
		}
		country_leader = {
			ideology = social_democrat
			traits = {
				western_liberalism
				diplomat
				pro_american
				e_government_pioneer
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

**Historical Notes**:
- Born: December 26, 1953
- Party: Social Democratic Party (formerly)
- Known for: Digital Estonia advocacy, strong Western ties, cybersecurity focus
- Served during 2007 Russian cyberattacks

---

### President 4: Kersti Kaljulaid (2016-2021)

**Background**: Economist and auditor. First female President of Estonia. Youngest President at inauguration (46). Independent, non-partisan. Appointed by Parliament as compromise candidate.

```pdx
	EST_kersti_kaljulaid = {
		name = "Kersti Kaljulaid"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/kersti_kaljulaid.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				economist
				first_female_leader
				independent_politician
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

**Historical Notes**:
- Born: December 30, 1969
- Party: Independent
- Known for: First female President, youngest President, EU Court of Auditors experience
- Outspoken on social issues and equality

---

### President 5: Alar Karis (2021-present)

**Background**: Biologist, academic, former Rector of University of Tartu. State Auditor General before presidency. Non-partisan, academic background.

```pdx
	EST_alar_karis = {
		name = "Alar Karis"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/alar_karis.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				academic
				independent_politician
				bureaucrat
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

**Historical Notes**:
- Born: March 26, 1958
- Party: Independent
- Known for: Academic background, Auditor General, University rector

---

## Combined Code Block

Add all five Presidents together in `common/characters/EST.txt`:

```pdx
	# Estonian Presidents

	EST_lennart_meri = {
		name = "Lennart Meri"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/lennart_meri.dds"
			}
		}
		country_leader = {
			ideology = conservatism
			traits = {
				father_of_the_nation
				diplomat
				anti_russia
			}
			expire = "2006.3.14"
			id = -1
		}
	}

	EST_arnold_ruutel = {
		name = "Arnold Ruutel"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/arnold_ruutel.dds"
			}
		}
		country_leader = {
			ideology = conservatism
			traits = {
				agrarian_leader
				experienced_politician
			}
			expire = "2050.1.1"
			id = -1
		}
	}

	EST_toomas_ilves = {
		name = "Toomas Hendrik Ilves"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/toomas_ilves.dds"
			}
		}
		country_leader = {
			ideology = social_democrat
			traits = {
				western_liberalism
				diplomat
				pro_american
			}
			expire = "2050.1.1"
			id = -1
		}
	}

	EST_kersti_kaljulaid = {
		name = "Kersti Kaljulaid"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/kersti_kaljulaid.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				economist
				first_female_leader
			}
			expire = "2050.1.1"
			id = -1
		}
	}

	EST_alar_karis = {
		name = "Alar Karis"
		portraits = {
			civilian = {
				large = "gfx/leaders/EST/alar_karis.dds"
			}
		}
		country_leader = {
			ideology = liberalism
			traits = {
				academic
				bureaucrat
			}
			expire = "2050.1.1"
			id = -1
		}
	}
```

## Acceptance Criteria
- [ ] All 5 Presidents appear as selectable leaders
- [ ] Portraits display correctly (or show generic if missing)
- [ ] No error logs related to missing files or syntax errors
- [ ] Character traits apply correctly
- [ ] Represents full presidential history from 1992-2024

## Portrait Requirements

| President | Status | Action Needed |
|-----------|--------|---------------|
| Lennart Meri | Exists (small) | Source larger version |
| Arnold Ruutel | Missing | Source and create |
| Toomas Hendrik Ilves | Missing | Source and create |
| Kersti Kaljulaid | Exists (small) | Source larger version |
| Alar Karis | Missing | Source and create |

Portrait specifications:
- Large: 156x210 or 512x512 pixels, DDS format (DXT5)
- Small: 65x67 pixels, DDS format

## Dependencies
- Depends on: None
- Blocks: None (can be implemented independently)

## Notes

### Presidential Role in Estonia
In HOI4/Millennium Dawn context, Presidents are typically represented as:
1. **Country Leaders** - For ceremonial head of state representation
2. **Advisors** - For political cabinet positions

Since Estonia is parliamentary, the PM is the actual executive. Presidents could be:
- Alternative leader options
- Political advisors providing stability bonuses
- Event-specific figures

### Future Considerations
Consider creating events for:
- Presidential elections (every 5 years)
- State of the Nation addresses
- Diplomatic missions led by President
