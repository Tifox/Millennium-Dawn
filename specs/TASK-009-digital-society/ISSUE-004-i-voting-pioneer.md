# ISSUE-004: i-Voting Pioneer

## Summary
Add an event for Estonia being the first nation to implement internet voting in national elections.

## Context
Estonia pioneered internet voting (i-voting) starting in 2005 local elections, with national elections following in 2007. By 2019, over 40% of votes were cast online, making Estonia the world leader in digital democracy.

### Historical Details
- **First use:** 2005 local elections
- **National elections:** 2007
- **2019 elections:** 43.8% of votes cast online
- **Security:** Based on national digital ID infrastructure
- **Uniqueness:** Only country with nationwide binding i-voting

## Requirements
- [ ] Create i-voting pioneer event
- [ ] Trigger after 2007
- [ ] Add stability/political power bonus
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.603 |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2007 - i-Voting in National Elections
country_event = {
	id = estonia.603
	title = estonia.603.t
	desc = estonia.603.d
	picture = GFX_computer

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2007.3.4
		NOT = { has_country_flag = EST_i_voting }
		NOT = { has_global_flag = EST_i_voting_happened }
	}

	mean_time_to_happen = {
		days = 7
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_i_voting_happened
		}
	}

	option = {
		name = estonia.603.a
		log = "[GetDateText]: [This.GetName]: event estonia.603.a executed"

		set_country_flag = EST_i_voting

		add_political_power = 20
		add_stability = 0.02

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add Localization

```yml
 # i-Voting Pioneer
 estonia.603.t: "Internet Voting in National Elections"
 estonia.603.d: "Estonia has made history by becoming the first nation to use internet voting in national parliamentary elections. Building on our successful pilot in 2005 local elections, Estonian citizens can now cast their votes from anywhere in the world using their digital ID cards.\n\nWhile some have raised security concerns, our robust digital identity infrastructure provides strong authentication. This innovation makes voting more accessible and convenient, particularly for Estonians living abroad.\n\nEstonia continues to lead the world in digital governance."
 estonia.603.a: "Democracy goes digital."
```

## Acceptance Criteria
- [ ] Event fires after March 2007
- [ ] Stability and PP bonuses applied
- [ ] All localization displays correctly

## Dependencies
- Depends on: None
- Blocks: None

## Testing Notes
1. Start game in 2007
2. Advance past March 4, 2007
3. Verify event fires
