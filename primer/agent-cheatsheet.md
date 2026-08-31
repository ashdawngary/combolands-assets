# Combolands Agent Cheatsheet

## Catalog Navigation

Start with `manifest.json`: it indexes every entity, mechanic, enum table, asset, schema, and reader guide using project-relative paths. Entity records are under `data/entities/`; rules are under `data/mechanics/`; game enum tables are in `data/enums.json`; generated local art is under `assets/`.

## Draft and Drop Mechanics

Default building draft rarity weights ([building-draft-rarity-weights](../data/mechanics/building-draft-rarity-weights.json)):
- Common: 70%
- Uncommon: 24%
- Rare: 5%
- Masterwork: 1%

Rarity multipliers use `clamped_city_size_minus_one` clamped to 0..10 ([rarity-milestone-multipliers](../data/mechanics/rarity-milestone-multipliers.json)). Coefficients:
- Common: -0.03
- Uncommon: 0.05
- Rare: 0.15
- Masterwork: 0.15

Pool selection is without replacement: `refresh_currently_draftable, exclude_ignored_and_dismissed, apply_optional_rarity_guarantee, add_by_building_roll_chance, choose_then_remove_until_count` ([roll-pool-eligibility](../data/mechanics/roll-pool-eligibility.json)).

Guild and neutral pool restrictions are exact runtime filters ([guild-category-filters](../data/mechanics/guild-category-filters.json)):
- `guild`: `hamlet_or_later_and_major_guild_category_selected`
- `neutral`: `hamlet_or_later_and_any_minor_neutral_category_selected`

Consumable rolls use pool weight 1.0 and replacement `False` ([consumable-roll-multipliers](../data/mechanics/consumable-roll-multipliers.json)). ConsumablesPool.GetRoll assigns every eligible tag weight 1f and removes each choice; it does not read ConsumableData.RollChance.

Drop-choice configuration precedence: `pack_type_generated_category, specific_categories_override_generated_category, specific_rarities_assigned_last` ([drop-choice-order](../data/mechanics/drop-choice-order.json)).

## Triggers, Crops, Harvesting, and Irrigation

Harvester targets `eligible_adjacent_activatable_buildings` with `random_without_replacement`; `crop_filter` is `False` ([harvesting](../data/mechanics/harvesting.json)). The runtime selection has no crop/category filter.

Crop cooldowns and irrigated cooldowns are recorded per entity ([crop-growth](../data/mechanics/crop-growth.json)):
- `apple-orchard`: 3; irrigated 1
- `apple-orchard-empty`: 3; irrigated 1
- `berry-bush`: 4; irrigated 1
- `berry-bush-empty`: 4; irrigated 1
- `cotton-field`: 3; irrigated 1
- `cotton-field-empty`: 3; irrigated 1
- `sunflower-farm`: 3; irrigated 1
- `sunflower-farm-empty`: 3; irrigated 1
- `vineyard`: 3; irrigated 1
- `vineyard-empty`: 3; irrigated 1
- `wheat-field`: 3; irrigated 1
- `wheat-field-empty`: 3; irrigated 1

Entity ranges are managed-constructor-derived spatial rules ([adjacency-and-range](../data/mechanics/adjacency-and-range.json)):
- `apiary`: 3
- `apothecary`: 3
- `archery-range`: 3
- `balloon`: 4
- `bank`: 4
- `barn`: 2
- `barracks`: 2
- `berry-picker`: 2
- `blacksmith`: 4
- `botany-stall`: 3
- `brewery`: 2
- `carpenter`: 2
- `church`: 5
- `circus`: 4
- `clipper`: 4
- `composter`: 3
- `conduit`: 3
- `corrupted-fort`: 2
- `corrupted-grove`: 2
- `corrupted-lantern`: 2
- `corrupted-mine`: 2
- `corrupted-obelisk`: 2
- `corrupted-shrine`: 2
- `corrupted-temple`: 2
- `corrupted-tower`: 2
- `crane`: 7
- `deli-stall`: 4
- `depot`: 5
- `field-hospital`: 3
- `fish-farm`: 2
- `fish-trap`: 5
- `fisher`: 5
- `fishing-boat`: 2
- `forester`: 2
- `fortress`: 5
- `fountain`: 2
- `galleon`: 4
- `gardeners-cottage`: 3
- `granary`: 3
- `grocer-stall`: 4
- `guard-tower`: 4
- `harvester`: 2
- `herbalist`: 2
- `hideout`: 2
- `hunting-lodge`: 2
- `inn`: 4
- `jousting-arena`: 2
- `kitchen`: 4
- `lighthouse`: 8
- `marketplace`: 5
- `nitrary`: 3
- `ocean-temple`: 5
- `pirate-ship`: 5
- `port`: 5
- `quarry`: 2
- `sawmill`: 3
- `scarecrow`: 5
- `seaweed-farm`: 2
- `shaman`: 4
- `sparring-yard`: 3
- `stable`: 4
- `stockpile`: 2
- `sunflower-farm`: 3
- `sunflower-farm-empty`: 3
- `supplies-stall`: 4
- `tannery`: 5
- `tax-collector`: 2
- `tool-shed`: 4
- `town-hall`: 6
- `trading-post`: 5
- `training-ring`: 2
- `trawler`: 5
- `treasury`: 7
- `university`: 4
- `warehouse`: 3
- `watch-tower`: 5
- `well`: 4
- `windmill`: 4
- `witch-hut`: 3
- `wizards-tower`: 4
- `woodcutter`: 2
- `workshop`: 4

Trigger timing uses `last_in_first_out` queue discipline ([trigger-execution-priority](../data/mechanics/trigger-execution-priority.json)). Effective building execution: `not_built_this_turn_first, end_of_turn_priority_ascending, random_tiebreaker`.

## Important Relationship Types

The entity graph uses: `activates_adjacent`, `activates_relative_position`, `affected_by_category`, `affected_by_mechanic`, `affects_category`, `allows_build_inside_category_range`, `alternate_placed_type`, `harvests_category`, `irrigates_category`, `requires_entity`, `targets_category`, `targets_entity`, `transforms_into`, `transforms_when_in_range_of_category`.
