# ISSUE-001: EKRE Formation

## Summary
Add a historical event for the formation of the Conservative People's Party of Estonia (EKRE) on March 24, 2012.

## Context
EKRE was formed on March 24, 2012, through the merger of the Estonian National Movement and the Estonian Patriotic Movement. The party adopted a nationalist, euroskeptic, and socially conservative platform. Under the leadership of Mart Helme, EKRE grew from a marginal party to a significant political force.

### Historical Details
- **Founded:** March 24, 2012
- **Origin:** Merger of Estonian National Movement + Estonian Patriotic Movement
- **Leader:** Mart Helme (founder and first chairman)
- **Ideology:** National conservatism, right-wing populism, euroskepticism
- **Initial Platform:** Anti-immigration, EU-skeptic, traditional values

## Requirements
- [ ] Create country event for EKRE formation
- [ ] Trigger after March 24, 2012
- [ ] Fire only once
- [ ] Boost nationalist party popularity
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add event estonia.400 |
| `localisation/english/EST_events_l_english.yml` | Modify | Add localization |

## Implementation

### Step 1: Add Event to events/Estonia.txt

```pdx
# 2012 - EKRE Formation
country_event = {
	id = estonia.400
	title = estonia.400.t
	desc = estonia.400.d
	picture = GFX_report_event_generic_rally

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2012.3.24
		NOT = { has_country_flag = EST_ekre_formed }
		NOT = { has_global_flag = EST_ekre_formation_happened }
	}

	mean_time_to_happen = {
		days = 7
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_ekre_formation_happened
		}
	}

	option = {
		name = estonia.400.a
		log = "[GetDateText]: [This.GetName]: event estonia.400.a executed"

		set_country_flag = EST_ekre_formed

		# Small boost to nationalist support
		add_popularity = {
			ideology = Nat_Populism
			popularity = 0.02
		}

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add Localization

```yml
 # EKRE Formation
 estonia.400.t: "Conservative People's Party Founded"
 estonia.400.d: "A new political party has emerged in Estonia. The Conservative People's Party of Estonia, known as EKRE, has been formed through the merger of the Estonian National Movement and the Estonian Patriotic Movement.\n\nLed by former ambassador Mart Helme, the party advocates for national conservatism, traditional values, and a more skeptical stance toward the European Union. While currently a minor political force, EKRE represents a new voice in Estonian politics."
 estonia.400.a: "We shall watch their development with interest."
```

## Acceptance Criteria
- [ ] Event fires after March 24, 2012
- [ ] Event fires only once
- [ ] Nat_Populism popularity increases slightly
- [ ] Localization displays correctly

## Dependencies
- Depends on: None
- Blocks: ISSUE-002 (EKRE must exist to enter government)

## Testing Notes
1. Start game as Estonia in 2012
2. Advance past March 24, 2012
3. Verify event fires
4. Check popularity change for Nat_Populism ideology
