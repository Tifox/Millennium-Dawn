# ISSUE-001: Add Kristen Michal

## Summary
Add Kristen Michal as a country leader character representing the current Prime Minister of Estonia (July 2024-present).

## Context
Kristen Michal became Prime Minister of Estonia on July 23, 2024, succeeding Kaja Kallas who was appointed EU High Representative for Foreign Affairs. Michal is a member of the Reform Party (liberalism) and previously served as Minister of Economic Affairs and Infrastructure.

### Biographical Information
- **Full Name:** Kristen Michal
- **Born:** October 18, 1975
- **Party:** Estonian Reform Party (Eesti Reformierakond)
- **Ideology:** Liberalism
- **Previous Positions:**
  - Minister of Economic Affairs and Infrastructure (2015-2016)
  - Member of Riigikogu (Parliament)
  - Secretary General of Reform Party
- **Background:** Business and IT sector experience

## Requirements
- [ ] Create character entry in `common/characters/EST.txt`
- [ ] Add appropriate ideology (liberalism)
- [ ] Add appropriate traits (western_liberalism, economist, tech_savy)
- [ ] Add portrait reference (`gfx/leaders/EST/kristen_michal.dds`)
- [ ] Source and add portrait image file
- [ ] Add localization for description if needed

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/characters/EST.txt` | Modify | Add EST_kristen_michal character |
| `gfx/leaders/EST/kristen_michal.dds` | Create | Add leader portrait |
| `localisation/english/MD_focus_EST_l_english.yml` | Modify | Add leader description |

## Implementation

### Step 1: Add Character to common/characters/EST.txt

Add after EST_kaja_kallas (around line 465):

```pdx
EST_kristen_michal = {
	name = "Kristen Michal"
	portraits = {
		civilian = {
			large = "gfx/leaders/EST/kristen_michal.dds"
		}
	}
	country_leader = {
		desc = "EST_KRISTEN_MICHAL_DESC"
		expire = "2050.1.1"
		ideology = liberalism
		traits = {
			western_liberalism
			economist
			tech_savy
		}
	}
}
```

### Step 2: Add Localization

Add to `localisation/english/MD_focus_EST_l_english.yml`:

```yml
 EST_KRISTEN_MICHAL_DESC: "Kristen Michal is an Estonian politician serving as Prime Minister since July 2024. A member of the Reform Party, he previously served as Minister of Economic Affairs and has extensive experience in the business and IT sectors. He succeeded Kaja Kallas after her appointment as EU High Representative for Foreign Affairs."
```

### Step 3: Verify Scripted Effects

The `EST_political_leaders.txt` already includes Michal at liberalism_leader = 3. Verify the date trigger is correctly set:

```pdx
# In EST_political_leaders.txt, around line 50-51, verify:
if = { limit = { has_country_flag = do_not_retire } subtract_from_variable = { liberalism_leader = 1 } }
set_temp_variable = { b = 1 }
# Should transition after Kaja Kallas
```

## Acceptance Criteria
- [ ] Kristen Michal appears as leader when liberalism is ruling party after 2024
- [ ] Character has correct portrait displayed
- [ ] Character traits are appropriate (western_liberalism, economist, tech_savy)
- [ ] No error.log entries related to EST_kristen_michal

## Dependencies
- Depends on: None
- Blocks: None

## Testing Notes
1. Start game as Estonia on January 1, 2024
2. Set ruling party to liberalism
3. Use console: `set_ruling_party liberalism`
4. Fast forward past July 2024
5. Verify Michal appears as leader through normal leader rotation
