# ISSUE-004: EKRE Electoral Events

## Summary
Add events for EKRE's electoral milestones, including their 2019 breakthrough election and 2023 election results.

## Context
EKRE has had two notable election performances:
- **2019:** Breakthrough election with 17.8% (19 seats), becoming third-largest party
- **2023:** Slight decline to 16.1% (17 seats), still a significant force

These events provide flavor for EKRE's political trajectory.

### Historical Details

#### 2019 Election (March 3)
- Vote share: 17.8%
- Seats: 19/101
- Result: Third-largest party
- Significance: First major electoral success

#### 2023 Election (March 5)
- Vote share: 16.1%
- Seats: 17/101
- Result: Fourth-largest party (slight decline)
- Context: After government collapse scandal

## Requirements
- [x] Create events for 2019 and 2023 elections
- [x] Include popularity shifts
- [x] Fire only once each
- [x] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add events estonia.403, estonia.404 |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add 2019 Election Event

```pdx
# 2019 - EKRE Electoral Breakthrough
country_event = {
	id = estonia.403
	title = estonia.403.t
	desc = estonia.403.d
	picture = GFX_report_event_generic_rally

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2019.3.3
		has_country_flag = EST_ekre_formed
		NOT = { has_country_flag = EST_ekre_2019_election }
		NOT = { has_global_flag = EST_ekre_2019_happened }
	}

	mean_time_to_happen = {
		days = 3
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_ekre_2019_happened
		}
	}

	option = {
		name = estonia.403.a
		log = "[GetDateText]: [This.GetName]: event estonia.403.a executed"

		set_country_flag = EST_ekre_2019_election

		# EKRE breakthrough
		add_popularity = {
			ideology = Nat_Populism
			popularity = 0.08
		}

		add_stability = -0.02

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add 2023 Election Event

```pdx
# 2023 - EKRE Post-Scandal Election
country_event = {
	id = estonia.404
	title = estonia.404.t
	desc = estonia.404.d
	picture = GFX_report_event_generic_rally

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2023.3.5
		has_country_flag = EST_ekre_formed
		NOT = { has_country_flag = EST_ekre_2023_election }
		NOT = { has_global_flag = EST_ekre_2023_happened }
	}

	mean_time_to_happen = {
		days = 3
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_ekre_2023_happened
		}
	}

	option = {
		name = estonia.404.a
		log = "[GetDateText]: [This.GetName]: event estonia.404.a executed"

		set_country_flag = EST_ekre_2023_election

		# Slight decline post-scandal
		add_popularity = {
			ideology = Nat_Populism
			popularity = -0.02
		}

		ai_chance = { base = 100 }
	}
}
```

### Step 3: Add Localization

```yml
 # EKRE Electoral Events
 estonia.403.t: "EKRE Electoral Breakthrough"
 estonia.403.d: "The parliamentary elections have concluded with a stunning result for the Conservative People's Party (EKRE). The nationalist party has won 17.8% of the vote and secured 19 seats in the Riigikogu, making them the third-largest party.\n\nThis represents a major breakthrough for the party, which has transformed from a marginal political force to a potential kingmaker in coalition negotiations. Their anti-immigration and euroskeptic message has resonated with a significant portion of the electorate."
 estonia.403.a: "The political landscape shifts."

 estonia.404.t: "EKRE Maintains Electoral Support"
 estonia.404.d: "Despite the scandals that ended their time in government, EKRE has maintained substantial support in the parliamentary elections. The party won 16.1% of the vote and 17 seats, a slight decline from 2019 but still a significant presence.\n\nWhile they will remain in opposition as the Reform Party leads the new government, EKRE continues to represent a major political force in Estonian politics."
 estonia.404.a: "The far-right remains a factor."
```

## Acceptance Criteria
- [x] 2019 event fires after March 3, 2019
- [x] 2023 event fires after March 5, 2023
- [x] Both require EKRE formation flag
- [x] Popularity changes are appropriate
- [x] All localization displays correctly

## Dependencies
- Depends on: ISSUE-001 (EKRE Formation)
- Blocks: None

## Testing Notes
1. Start game in 2019
2. Verify 2019 election event fires
3. Advance to 2023
4. Verify 2023 election event fires
5. Check popularity changes
