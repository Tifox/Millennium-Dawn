# ISSUE-002: Conscription Reform

## Summary
Add an event for Estonia's conscription reform, extending service from 11 months to 12 months universal service.

## Context
Estonia maintains conscription as a key element of its defense strategy. Recent reforms have aimed to make service more universal and extended the standard service period to strengthen the reserve force.

### Historical Details
- **Current System:** 8-11 months depending on role
- **Reform Direction:** Moving toward 12 months universal
- **Annual Intake:** ~4,000 conscripts
- **Reserve Target:** Trained reserve of 60,000+
- **Rationale:** Strengthen readiness against Russian threat

## Requirements
- [ ] Create conscription reform event
- [ ] Trigger after 2022
- [ ] Add manpower/military bonuses
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.701 |
| `common/ideas/estonia.txt` | Modify | Add EST_conscription_reform idea |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2023+ - Conscription Reform
country_event = {
	id = estonia.701
	title = estonia.701.t
	desc = estonia.701.d
	picture = GFX_report_event_military_training

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2023.1.1
		has_country_flag = EST_ukraine_response
		NOT = { has_country_flag = EST_conscription_reform }
		NOT = { has_global_flag = EST_conscription_reform_happened }
	}

	mean_time_to_happen = {
		months = 6
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_conscription_reform_happened
		}
	}

	# Option A: Full reform
	option = {
		name = estonia.701.a
		log = "[GetDateText]: [This.GetName]: event estonia.701.a executed"

		set_country_flag = EST_conscription_reform

		add_political_power = -50

		# Enhanced conscription
		add_ideas = EST_conscription_reform

		# Manpower bonus
		add_manpower = 5000

		ai_chance = { base = 80 }
	}

	# Option B: Incremental approach
	option = {
		name = estonia.701.b
		log = "[GetDateText]: [This.GetName]: event estonia.701.b executed"

		set_country_flag = EST_conscription_reform

		add_political_power = -25
		add_manpower = 2500

		ai_chance = { base = 20 }
	}
}
```

### Step 2: Add National Spirit

```pdx
EST_conscription_reform = {
	picture = generic_conscription
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		conscription_factor = 0.10
		training_time_factor = -0.10
		army_org_factor = 0.03
	}
}
```

### Step 3: Add Localization

```yml
 # Conscription Reform
 estonia.701.t: "Conscription System Reform"
 estonia.701.d: "In response to the heightened threat environment following Russia's invasion of Ukraine, Estonia is reforming its conscription system. The proposed changes would extend the standard service period to 12 months and make conscription more universal.\n\nThis reform aims to strengthen our reserve forces and ensure that more Estonians receive military training. The goal is to maintain a trained reserve of over 60,000 that can be rapidly mobilized if needed.\n\nHowever, these changes will require significant investment and may face some public resistance."
 estonia.701.a: "Implement full conscription reform."
 estonia.701.b: "Take an incremental approach."

 EST_conscription_reform: "Enhanced Conscription System"
 EST_conscription_reform_desc: "Estonia's reformed conscription system produces better-trained conscripts and strengthens the reserve force."
```

## Acceptance Criteria
- [ ] Event fires after 2023 with Ukraine response flag
- [ ] National spirit and manpower bonuses applied
- [ ] All localization displays correctly

## Dependencies
- Depends on: TASK-008 ISSUE-001 (Ukraine Response)
- Blocks: None

## Testing Notes
1. Complete Ukraine response event
2. Advance to 2023
3. Verify conscription reform event fires
4. Check national spirit and manpower effects
