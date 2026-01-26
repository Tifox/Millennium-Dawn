# ISSUE-002: Defense Spending Increase

## Summary
Add decisions for Estonia's dramatic increase in defense spending from 2.3% to 5%+ of GDP.

## Context
Following the 2022 invasion, Estonia dramatically increased defense spending. The country moved from 2.3% of GDP in 2022 to targets of 5% by 2025 and 5.4% by 2029 - among the highest in NATO. This reflects Estonia's assessment of the direct threat from Russia.

### Historical Details
- **2022:** 2.3% GDP
- **2024:** 3.4% GDP
- **2025 target:** 5% GDP
- **2029 target:** 5.4% GDP
- **Context:** Highest planned defense spending in NATO
- **Includes:** Military aid to Ukraine (counted separately)

## Requirements
- [ ] Create decision category for defense spending
- [ ] Add escalating spending decisions (3%, 4%, 5%)
- [ ] Include costs and military bonuses
- [ ] Require Ukraine response flag
- [ ] Add localization

## Files to Modify
| File | Action | Description |
|------|--------|-------------|
| `common/decisions/EST_decisions.txt` | Create | New decision file |
| `common/ideas/estonia.txt` | Modify | Add defense spending ideas |
| `common/decisions/categories/EST_decision_categories.txt` | Create | Add category |
| `localisation/english/EST_decisions_l_english.yml` | Create | Add localization |

## Implementation

### Step 1: Create Decision Category

Create `common/decisions/categories/EST_decision_categories.txt`:

```pdx
EST_defense_spending_category = {
	icon = generic_military

	allowed = {
		original_tag = EST
	}

	visible = {
		has_country_flag = EST_ukraine_response
	}
}
```

### Step 2: Create Decisions File

Create `common/decisions/EST_decisions.txt`:

```pdx
EST_defense_spending_category = {

	EST_increase_defense_spending_3 = {
		icon = generic_military

		available = {
			has_country_flag = EST_ukraine_response
			NOT = { has_idea = EST_defense_spending_3 }
			NOT = { has_idea = EST_defense_spending_4 }
			NOT = { has_idea = EST_defense_spending_5 }
		}

		visible = {
			has_country_flag = EST_ukraine_response
		}

		cost = 100
		fire_only_once = yes

		complete_effect = {
			log = "[GetDateText]: [This.GetName]: decision EST_increase_defense_spending_3"
			add_ideas = EST_defense_spending_3
			set_country_flag = EST_defense_3_percent
		}

		ai_will_do = {
			factor = 100
		}
	}

	EST_increase_defense_spending_4 = {
		icon = generic_military

		available = {
			has_country_flag = EST_defense_3_percent
			has_idea = EST_defense_spending_3
			NOT = { has_idea = EST_defense_spending_4 }
			date > 2023.1.1
		}

		visible = {
			has_country_flag = EST_defense_3_percent
		}

		cost = 150
		fire_only_once = yes

		complete_effect = {
			log = "[GetDateText]: [This.GetName]: decision EST_increase_defense_spending_4"
			remove_ideas = EST_defense_spending_3
			add_ideas = EST_defense_spending_4
			set_country_flag = EST_defense_4_percent
		}

		ai_will_do = {
			factor = 100
		}
	}

	EST_increase_defense_spending_5 = {
		icon = generic_military

		available = {
			has_country_flag = EST_defense_4_percent
			has_idea = EST_defense_spending_4
			NOT = { has_idea = EST_defense_spending_5 }
			date > 2024.1.1
		}

		visible = {
			has_country_flag = EST_defense_4_percent
		}

		cost = 200
		fire_only_once = yes

		complete_effect = {
			log = "[GetDateText]: [This.GetName]: decision EST_increase_defense_spending_5"
			remove_ideas = EST_defense_spending_4
			add_ideas = EST_defense_spending_5
			set_country_flag = EST_defense_5_percent
		}

		ai_will_do = {
			factor = 100
		}
	}
}
```

### Step 3: Add National Spirits

```pdx
EST_defense_spending_3 = {
	picture = generic_military_budget
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		consumer_goods_factor = 0.02
		army_org_factor = 0.05
		training_time_factor = -0.10
	}
}

EST_defense_spending_4 = {
	picture = generic_military_budget
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		consumer_goods_factor = 0.04
		army_org_factor = 0.10
		training_time_factor = -0.15
		army_attack_factor = 0.05
	}
}

EST_defense_spending_5 = {
	picture = generic_military_budget
	allowed = { always = no }
	allowed_civil_war = { always = yes }

	modifier = {
		consumer_goods_factor = 0.06
		army_org_factor = 0.15
		training_time_factor = -0.20
		army_attack_factor = 0.10
		army_defence_factor = 0.10
	}
}
```

### Step 4: Add Localization

Create `localisation/english/EST_decisions_l_english.yml`:

```yml
l_english:
 EST_defense_spending_category: "Defense Spending"
 EST_defense_spending_category_desc: "Decisions related to Estonia's defense budget and military modernization."

 EST_increase_defense_spending_3: "Increase Defense Spending to 3%"
 EST_increase_defense_spending_3_desc: "In response to the Russian threat, we will increase defense spending to 3% of GDP, exceeding NATO's 2% guideline."

 EST_increase_defense_spending_4: "Increase Defense Spending to 4%"
 EST_increase_defense_spending_4_desc: "We will further increase defense spending to 4% of GDP, making Estonia one of the highest defense spenders in NATO."

 EST_increase_defense_spending_5: "Increase Defense Spending to 5%"
 EST_increase_defense_spending_5_desc: "Estonia will become the NATO leader in defense spending at 5% of GDP, reflecting the existential threat we face."

 EST_defense_spending_3: "Enhanced Defense Budget (3%)"
 EST_defense_spending_3_desc: "Estonia has increased defense spending to 3% of GDP, enabling improved military readiness."

 EST_defense_spending_4: "Major Defense Investment (4%)"
 EST_defense_spending_4_desc: "Estonia's 4% GDP defense spending enables significant military modernization and capability enhancement."

 EST_defense_spending_5: "Maximum Defense Posture (5%)"
 EST_defense_spending_5_desc: "At 5% of GDP, Estonia has the highest defense spending in NATO, enabling rapid military expansion and modernization."
```

## Acceptance Criteria
- [ ] Decision category appears after Ukraine response
- [ ] Decisions unlock sequentially
- [ ] Each decision adds appropriate national spirit
- [ ] Costs and effects are balanced
- [ ] AI takes decisions appropriately
- [ ] All localization displays correctly

## Dependencies
- Depends on: ISSUE-001 (Ukraine Response)
- Blocks: None

## Testing Notes
1. Complete Ukraine response event
2. Verify decision category appears
3. Take 3% decision, verify spirit applied
4. Advance time, take 4% and 5% decisions
5. Verify all effects stack correctly
