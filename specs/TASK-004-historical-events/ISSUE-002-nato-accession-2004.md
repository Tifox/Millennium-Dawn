# ISSUE-002: Estonia Joins NATO

## Summary
Add a historical event for Estonia's accession to NATO on March 29, 2004.

## Context
Estonia became a member of NATO on March 29, 2004, along with six other countries in the fifth and largest round of NATO enlargement. This was a crucial security milestone for Estonia, providing collective defense guarantees against potential aggression. The event predates EU accession by about a month.

## Requirements
- [x] Create country event for NATO accession
- [x] Trigger automatically after March 29, 2004
- [x] Fire only once per game
- [x] Add military and diplomatic effects
- [x] Add localization for event title, description, and options
- [x] Set country flag to track NATO membership
- [x] Include war support boost and military cooperation bonuses

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `events/Estonia.txt` | Modify | Add new event estonia.201 |
| `localisation/english/MD_focus_EST_l_english.yml` | Modify | Add localization strings |

## Implementation

### Step 1: Add Event to events/Estonia.txt

Add the following event after estonia.200:

```pdx
# 2004 - Estonia joins NATO
country_event = {
	id = estonia.201
	title = estonia.201.t
	desc = estonia.201.d
	picture = GFX_computer

	fire_only_once = yes

	trigger = {
		tag = EST
		date > 2004.3.29
		NOT = { has_country_flag = EST_joined_nato }
		NOT = { has_global_flag = EST_nato_accession_happened }
	}

	mean_time_to_happen = {
		days = 1
	}

	immediate = {
		hidden_effect = {
			set_global_flag = EST_nato_accession_happened
		}
	}

	option = {
		name = estonia.201.a
		log = "[GetDateText]: [This.GetName]: event estonia.201.a executed"

		set_country_flag = EST_joined_nato

		add_political_power = 100
		add_war_support = 0.10
		add_stability = 0.03

		# NATO membership benefits
		add_ideas = EST_nato_member

		# Improved relations with NATO allies
		USA = {
			add_opinion_modifier = { target = EST modifier = EST_nato_ally }
		}
		GER = {
			add_opinion_modifier = { target = EST modifier = EST_nato_ally }
		}
		GBR = {
			add_opinion_modifier = { target = EST modifier = EST_nato_ally }
		}
		POL = {
			add_opinion_modifier = { target = EST modifier = EST_nato_ally }
		}

		# Russia not happy about NATO expansion
		SOV = {
			add_opinion_modifier = { target = EST modifier = EST_nato_expansion_tension }
		}

		ai_chance = { base = 100 }
	}
}
```

### Step 2: Add National Spirit (in common/ideas if needed)

```pdx
EST_nato_member = {
	picture = generic_military_cooperation
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		war_support_factor = 0.10
		army_org_factor = 0.05
		training_time_army_factor = -0.05
	}
}
```

### Step 3: Add Opinion Modifiers (in common/opinion_modifiers if needed)

```pdx
EST_nato_ally = {
	value = 30
}

EST_nato_expansion_tension = {
	value = -25
}
```

### Step 4: Add Localization to MD_focus_EST_l_english.yml

```yml
 # NATO Accession 2004
 estonia.201.t: "Estonia Joins NATO"
 estonia.201.d: "On March 29th, 2004, Estonia has officially become a member of the North Atlantic Treaty Organization. This historic achievement represents the culmination of years of military reform and diplomatic effort. As part of the largest NATO enlargement in history, Estonia joins alongside Latvia, Lithuania, Bulgaria, Romania, Slovakia, and Slovenia. Article 5 of the NATO treaty now guarantees our collective defense - an attack on Estonia is an attack on all NATO members. Our security is now intertwined with the most powerful military alliance in history."
 estonia.201.a: "Our security is guaranteed!"

 EST_nato_member: "NATO Membership"
 EST_nato_member_desc: "As a full member of NATO, Estonia benefits from collective defense guarantees under Article 5, access to advanced military training, and integration with the world's most powerful military alliance."
 EST_nato_ally: "NATO Ally"
 EST_nato_expansion_tension: "NATO Expansion Concerns"
```

## Acceptance Criteria
- [x] Event fires automatically after March 29, 2004 when playing as Estonia
- [x] Event fires only once per game
- [x] Country flag EST_joined_nato is set after event
- [x] War support and stability bonuses are applied
- [x] Positive opinion modifiers with NATO countries are applied
- [x] Negative opinion modifier with Russia is applied
- [x] All localization displays correctly in English
- [x] Event has appropriate picture

## Dependencies
- Depends on: None
- Blocks: None

## Testing Notes
1. Start game as Estonia on January 1, 2000
2. Fast forward to March 29, 2004
3. Verify event fires within days of the date
4. Check opinion modifiers with USA, Germany, UK, Poland (should be positive)
5. Check opinion modifier with Russia (should be negative)
6. Verify event does not fire again if reloading save
