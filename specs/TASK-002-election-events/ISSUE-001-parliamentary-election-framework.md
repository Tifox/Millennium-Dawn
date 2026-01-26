# ISSUE-001: Parliamentary Election Framework

## Summary
Create the base parliamentary election event structure for Estonia that presents players with party/coalition choices.

## Context
Estonia needs election events that fire on historical dates and allow players to choose which party wins the election. This forms the foundation that ISSUE-002 (triggers) and ISSUE-003 (leader rotation) build upon. The events should follow Millennium Dawn conventions for political events.

## Requirements
- [ ] Create new namespace `est_election` for election events
- [ ] Create main election event with multiple party options
- [ ] Include AI weighting for historical outcomes
- [ ] Add proper logging for debugging
- [ ] Create news event for election results announcement

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add election events to existing file |
| `localisation/english/EST_events_l_english.yml` | Create | Add localization for election events |

## Implementation

### Step 1: Add Namespace to Estonia.txt

Add the election namespace at the top of `events/Estonia.txt` (after existing namespaces):

```pdx
add_namespace = est_election
```

### Step 2: Create Main Parliamentary Election Event

Add this event to `events/Estonia.txt`:

```pdx
# Estonian Parliamentary Election
country_event = {
	id = est_election.1
	title = est_election.1.t
	desc = est_election.1.d
	picture = GFX_EST_LocalElections

	is_triggered_only = yes

	trigger = {
		original_tag = EST
		has_government = democratic
		NOT = { has_war = yes }
	}

	# Option A: Reform Party Victory (Liberal)
	option = {
		name = est_election.1.a
		log = "[GetDateText]: [This.GetName]: event est_election.1.a executed - Reform Party wins"

		set_temp_variable = { rul_party_temp = 2 }
		change_ruling_party_effect = yes

		set_temp_variable = { party_index = 2 }
		set_temp_variable = { party_popularity_increase = 0.05 }
		set_temp_variable = { temp_outlook_increase = 0.05 }
		add_relative_party_popularity = yes

		add_political_power = 50
		add_stability = 0.02

		set_country_flag = EST_election_reform_victory
		clr_country_flag = EST_election_centre_victory
		clr_country_flag = EST_election_isamaa_victory
		clr_country_flag = EST_election_ekre_victory

		hidden_effect = {
			country_event = { id = est_election.10 days = 1 }
		}

		ai_chance = {
			base = 40
			modifier = {
				is_historical_focus_on = yes
				add = 30
			}
			modifier = {
				has_country_flag = EST_lean_west_flag
				add = 20
			}
		}
	}

	# Option B: Isamaa Victory (Conservative)
	option = {
		name = est_election.1.b
		log = "[GetDateText]: [This.GetName]: event est_election.1.b executed - Isamaa wins"

		set_temp_variable = { rul_party_temp = 3 }
		change_ruling_party_effect = yes

		set_temp_variable = { party_index = 3 }
		set_temp_variable = { party_popularity_increase = 0.05 }
		set_temp_variable = { temp_outlook_increase = 0.05 }
		add_relative_party_popularity = yes

		add_political_power = 50
		add_stability = 0.02

		clr_country_flag = EST_election_reform_victory
		clr_country_flag = EST_election_centre_victory
		set_country_flag = EST_election_isamaa_victory
		clr_country_flag = EST_election_ekre_victory

		hidden_effect = {
			country_event = { id = est_election.10 days = 1 }
		}

		ai_chance = {
			base = 25
			modifier = {
				is_historical_focus_on = yes
				date < 2007.1.1
				add = 25
			}
		}
	}

	# Option C: Centre Party Victory
	option = {
		name = est_election.1.c
		log = "[GetDateText]: [This.GetName]: event est_election.1.c executed - Centre Party wins"

		set_temp_variable = { rul_party_temp = 15 }
		change_ruling_party_effect = yes

		set_temp_variable = { party_index = 15 }
		set_temp_variable = { party_popularity_increase = 0.05 }
		set_temp_variable = { temp_outlook_increase = 0.05 }
		add_relative_party_popularity = yes

		add_political_power = 50
		add_stability = 0.02

		clr_country_flag = EST_election_reform_victory
		set_country_flag = EST_election_centre_victory
		clr_country_flag = EST_election_isamaa_victory
		clr_country_flag = EST_election_ekre_victory

		hidden_effect = {
			country_event = { id = est_election.10 days = 1 }
		}

		ai_chance = {
			base = 20
			modifier = {
				has_country_flag = EST_lean_east_flag
				add = 30
			}
			modifier = {
				is_historical_focus_on = yes
				date > 2015.1.1
				date < 2021.1.1
				add = 40
			}
		}
	}

	# Option D: EKRE Victory (Nationalist) - only available after 2012
	option = {
		name = est_election.1.d
		log = "[GetDateText]: [This.GetName]: event est_election.1.d executed - EKRE wins"

		trigger = {
			OR = {
				date > 2012.1.1
				has_country_flag = EST_ekre_has_formed
			}
		}

		set_temp_variable = { rul_party_temp = 21 }
		change_ruling_party_effect = yes

		set_temp_variable = { party_index = 21 }
		set_temp_variable = { party_popularity_increase = 0.08 }
		set_temp_variable = { temp_outlook_increase = 0.08 }
		add_relative_party_popularity = yes

		add_political_power = 25
		add_stability = -0.02
		add_war_support = 0.05

		clr_country_flag = EST_election_reform_victory
		clr_country_flag = EST_election_centre_victory
		clr_country_flag = EST_election_isamaa_victory
		set_country_flag = EST_election_ekre_victory

		hidden_effect = {
			country_event = { id = est_election.10 days = 1 }
		}

		ai_chance = {
			base = 10
			modifier = {
				is_historical_focus_on = yes
				factor = 0.1
			}
			modifier = {
				has_country_flag = EST_lean_neutral_flag
				add = 15
			}
		}
	}
}
```

### Step 3: Create Election Results News Event

Add this news event to `events/Estonia.txt`:

```pdx
# Estonian Election Results Announcement
news_event = {
	id = est_election.10
	title = est_election.10.t
	desc = est_election.10.d
	picture = GFX_EST_LocalElections

	is_triggered_only = yes

	trigger = {
		original_tag = EST
	}

	option = {
		name = est_election.10.a
		log = "[GetDateText]: [This.GetName]: event est_election.10.a executed"

		effect_tooltip = {
			add_political_power = 25
		}
	}
}
```

### Step 4: Create Snap Election Event (for government collapse scenarios)

```pdx
# Snap Election Called
country_event = {
	id = est_election.2
	title = est_election.2.t
	desc = est_election.2.d
	picture = GFX_EST_LocalElections

	is_triggered_only = yes

	trigger = {
		original_tag = EST
		has_government = democratic
	}

	immediate = {
		hidden_effect = {
			set_country_flag = EST_snap_election_called
		}
	}

	option = {
		name = est_election.2.a
		log = "[GetDateText]: [This.GetName]: event est_election.2.a executed"

		add_political_power = -100
		add_stability = -0.05

		country_event = { id = est_election.1 days = 30 }
	}
}
```

### Step 5: Create Localization File

Create the file `localisation/english/EST_events_l_english.yml`:

```yml
l_english:
 # Parliamentary Elections
 est_election.1.t: "Parliamentary Elections"
 est_election.1.d: "The citizens of Estonia head to the polls today to elect a new Riigikogu. The campaign has been hard-fought, and now the voters will decide the future direction of our nation. Which party will form the next government?"
 est_election.1.a: "The Reform Party secures victory!"
 est_election.1.b: "Isamaa wins the election!"
 est_election.1.c: "The Centre Party triumphs!"
 est_election.1.d: "EKRE achieves an upset victory!"

 # Election Results News
 est_election.10.t: "Estonian Election Results"
 est_election.10.d: "The Estonian parliamentary elections have concluded. A new government has been formed, and the nation looks forward to the policies of the incoming administration."
 est_election.10.a: "We wish them well."

 # Snap Election
 est_election.2.t: "Snap Election Called"
 est_election.2.d: "Following the collapse of the government coalition, President [EST.GetLeader] has called for early parliamentary elections. The nation must return to the polls to elect a new Riigikogu and form a stable government."
 est_election.2.a: "Prepare for the campaign."
```

## Acceptance Criteria
- [ ] `est_election` namespace is added to `events/Estonia.txt`
- [ ] Main election event (`est_election.1`) has 4 party options
- [ ] EKRE option only appears after 2012 or if flag is set
- [ ] AI weighting favors historical outcomes when historical focus is on
- [ ] News event fires after election to announce results
- [ ] All events have proper logging statements
- [ ] Localization file exists and contains all event strings

## Dependencies
- Depends on: None (this is the foundational issue)
- Blocks: ISSUE-002, ISSUE-003

## Testing

Use these console commands to test:
```
tag EST
event est_election.1
```

Verify:
1. Event displays correctly with all options
2. Selecting an option changes the ruling party
3. Party popularity increases for winning party
4. News event fires after selection
5. AI makes historical choices when historical focus is enabled
