# GreedCard — Souls, Collection & Legends (Merged + Improved)

Last updated: 2025-08-25  
Author: merged from: soul-acquisition-system.md, soul-possession-system.md, collection-lore-and-worldbuilding_Version2.md, eternal-steppe-lake-legends_Version2.md, eternal-steppe-lake-legends.md, golden-fish-lake-rewards_Version3.md, and related notes.

---

## Purpose & Scope

This document consolidates and improves the game's soul acquisition/possession systems, passive buff mechanics, and the collection-driven worldbuilding (Wanderer’s Remnants, Crimson Court, Deepforge Arsenal, Moonlit Carnival, Phoenix’s Legacy), plus detailed event legends (Eternal Steppe, Golden Fish Lake, Mountain Wolves) and balanced rewards. It clarifies player flows, monetization points, DataStore keys, anti-exploit guidance, and design trade-offs so the team can prototype quickly.

---

## Unified Lore (short)

- The Magical Cards fractured after the Holy War. Their power lingered in relics, biomes, and restless Lost Souls.
- Lucian_Q3’s sacrifice, possession, and Legacy anchor the narrative: the Forgotten Hero, Lost Demon King, and the rise of Lost Souls.
- Collections and relics carry echoes of their past owners—power with cost. Choices in major events (restore vs claim power) shape rewards and world states.

---

## Soul Acquisition System

Goals: provide a reliable free path, meaningful premium options, and upgrade/fusion mechanics for long-term engagement.

1. Soul Catcher Tool
- Description: Mystical device that captures wandering souls into Soul Possessions.
- Free usage: 1 free roll per day per account (encourages daily login).
- Outcome: yields a soul of varying rarity (Rare → Legendary). Chance for higher rarity is small but non-zero.
- Obtainable via: quest rewards, shops, event drops, or limited bundles.

2. Fast Roll (Optional Monetization)
- Players may spend Robux for extra rolls (fast roll) bypassing daily cooldown.
- Pricing model: linear small price for first extra roll, escalating cost / diminishing returns for additional rolls during same day.
- Godly-tier souls: optionally only obtainable via paid rolls or very rare featured methods (design choice—if used, mark as premium and transparent).
- Balance note: ensure a meaningful free path and that Godly/paid souls do not break core balance.

3. Soul Fragments & Fusion
- Duplicate souls convert to Fragments.
- Use Fragments to:
  - Reroll soul stats,
  - Fuse multiple souls to attempt higher rarity (success chance + resource cost),
  - Upgrade a soul’s level.
- Fragment sinks: prevent unlimited merging by requiring increasing fragment costs for higher tiers.

Acquisition Flow (player-facing)
1. Get/own Soul Catcher.
2. Use/roll once/day for a soul.
3. Optionally fast-roll with Robux.
4. Soul appears in Soul Collection → equip, display, or convert duplicates to fragments.

Design choices (recommended)
- Limit fast-rolls per day (e.g., max 5 paid rolls).
- If Godly souls are premium, provide alternative rare routes (events/auctions) to avoid complete paywall.

DataStore keys (examples)
- SoulCollection_<UserId> — player's soul list (IDs, rarity, level, unlocks)
- SoulFragments_<UserId> — fragment counts
- SoulCatcherCooldown_<UserId> — timestamp of next free roll

---

## Soul Possession System (Equip / Effects / Rarity)

Overview:
- Equipping a soul grants 1 active skill + 1–2 passives depending on rarity.
- Players can equip up to N souls simultaneously (recommendation: 1 active possession + up to 2 passive auxiliary souls OR max 3 combined slots—pick one model and lock it).
- Visuals: cosmetic aura, voice-lines, and spectral effects tied to soul lore.
- Souls are collectible and levelable.

Rarity tiers & examples
- Rare: 1 active skill + 1 minor passive (e.g., Fallen Steppe Archer)
- Epic: stronger skills, higher stats
- Mythic: impactful skills, utility passives (e.g., Elemental Slam)
- Legendary: game-changing actives or once-a-day triggers (e.g., Phoenix Martyr rebirth)
- Godly (optional/Robux): ultra-powerful, unique mechanics (Demon Lord, Forgotten Hero)

Equip rules
- One primary soul active (controls major skillset).
- Secondary soul slots for passive bonuses (optional, limit 1-2).
- Equipping bound (non-tradeable) souls persists across sessions.
- Switch cooldown or currency cost may apply to prevent rapid swapping in combat.

Progression & leveling
- Souls gain XP through combat/quests or by spending Fragments/catalysts.
- Level caps by rarity; level increases improve skill power or reduce cooldowns.
- Upgrading Godly-tier souls should be intentionally expensive or time-gated.

Anti-exploit & security
- All skill triggers and damage must be server-validated.
- Soul activation logs recorded for analytics and anti-cheat.

DataStore keys (examples)
- EquippedSoul_<UserId> — current equipped primary soul (soulId, level)
- SoulLevel_<UserId>_<soulId> — level & XP
- SoulSwapCooldown_<UserId> — last swap timestamp

---

## Lost Souls (Attachment-based system & integration)

This system is distinct from Soul Possession but related: Lost Souls are echo fragments bound to items or zones that grant stat buffs and unique weapon skills when attached.

Design summary:
- Lost Souls are attached to a player or an item to provide buffs & skills.
- Types: Soldier, Mage, Rogue, General, Berserker, Cleric, Champion (rare), Corrupted (very rare).
- Trade-offs: Corrupted souls give big damage at HP cost; Guardian-side souls emphasize defense and purification.

Mechanics:
- Limit active Lost Souls: recommend max 3.
- Lock/slot system on a Soul Socket UI to show active slots.
- Attaching/detaching is done at sanctuaries or with a consumable ritual to prevent in-combat swapping.
- Souls may be consumed on attachment (design choice) or remain reusable with cooldown.

Examples (refined)
- Soldier Soul — +10% Defense, +5% HP regen | Skill: Guardian Stance (temporary shield)
- Corrupted Soul — +25% Damage, drains HP 2%/sec | Skill: Dark Possession (berserk burst, short duration)

Integration with factions:
- Guardian side provides Soul Purge to cleanse corruption.
- Demon side offers better yields for using Corrupted Souls.

DataStore keys
- LostSouls_<UserId> — list of owned lost souls & states
- ActiveLostSouls_<UserId> — active attached souls

---

## Passive Buff Items (collection items)

Behavior:
- Up to 5 passive buff slots active at once.
- Each active consumes/locks an item from the player's collection while active.
- Stacking: duplicates stack multiplicatively/additively (decide consistent formula; recommended additive percentage stacking with soft caps).
- Switching: activating a 6th requires replacing an existing active; optionally return replaced item to collection unlocked.

Example items:
- Ancient Coin — +3% coins per activation (stacks)
- Lucky Rabbit’s Foot — +2% drop rate
- Book of Endurance — +10 max stamina

Design note:
- Decide whether activation consumes the item permanently or locks it. Locking preserves collection; consuming provides sink and monetization.

DataStore keys:
- PassiveBuffsActive_<UserId> — array of active buff IDs & counts

---

## Collections, Relics & Worldbuilding (consolidated)

Core collection sets and their biomes:
- Wanderer’s Remnants — Endless Wilds, Whispering Woods (relics contain wanderer echoes)
- Crimson Court Regalia — Scarlet Citadel, Masked Market (tainted treasures, high-risk rewards)
- Deepforge Arsenal — Molten Vaults, Anvil Sanctum (elemental weapon trials)
- Moonlit Carnival — Carnival of Shadows, Hall of Mirrors (puzzles & illusion-based rewards)
- Phoenix’s Legacy — Ashen Peaks, Sanctum of Rebirth (fire trials, rebirth mechanics)

Mechanics:
- Collection completion triggers events (mini-bosses, puzzles) with unique rewards.
- Showcases: player museums display rare cards/relics for social rewards (likes → small in-game rewards).
- Display metadata includes origin lore text, owner echoes, and rarity.

Economy interactions:
- Some relics are bound (non-tradable); others are mint/tradable and usable in Auction House & Pack Combiner systems.

---

## Legends & Event Rewards (Eternal Steppe, Golden Fish Lake, Mountain Wolves)

These region legends are designed as player-choice events (restore vs claim), each giving divergent mechanical & cosmetic rewards.

1) The Eternal Steppe
- Choice: Aid White Eagle (defense/healing) OR Side with Black Eagle (offense/ambition).
- Example rewards (refined):
  - Aid White Eagle: Skyfeather Halberd (Mythic), Unity Crest (Mythic), White Eagle’s Grace (Legendary buff), cosmetic wings.
  - Side Black Eagle: Nightstorm Blade (Mythic), Dominion Talisman, Black Eagle’s Might (Legendary buff), dark wings aura.
- Teleport utility: Steppe’s Calling (1/day).

2) Golden Fish Lake
- Choice: Free Water Spirit (cleansing/healing) OR Claim Black Dragon (corruption/damage).
- Example rewards:
  - Spirit’s Blessing Path: Spiritwater Spear (Mythic), Golden Fish Charm, Lus’ Blessing (Legendary), water halo.
  - Corruption Path: Abyssal Dragon Blade (Mythic), Obsidian Scale Ring, Dragon’s Pact (Legendary), Black Dragon Aura.
- Universal reward: Lake’s Memory (Epic utility teleport).

3) Mountain Wolves
- Dynamic ally/antagonist system (Blue Wolf vs Black Wolf) with similar choice/reward structure.
- Rewards include Frostbite Fang, Guardian’s Howl, Blue Wolf Blessing or Shadowclaw Blade, Alpha’s Dominance, Black Wolf’s Oath, and unique cosmetics.

Design notes:
- Event choices should have visible, lasting world impact (e.g., unlock certain NPCs, change local spawn pools).
- Rewards balanced by offering active skills on weapons, passive accessories, and one-time-use or limited utilities (teleports).

Drop & reward probabilities (guidelines)
- Mythic weapon from event: 15–30% (depending on challenge difficulty)
- Legendary buff token: 3–8%
- Cosmetic drop: 25–40% of successful runs
- Adjust these via telemetry after launch.

---

## Balance, Anti-exploit & Technical Notes

- Server-authoritative systems for:
  - Soul rolls and paid fast-roll validation
  - Soul equip activation and skill triggers
  - Event completion and drop rolls
- DataStore design:
  - SoulCollection_<UserId>, SoulFragments_<UserId>, SoulLevel_<UserId>_<SoulId>
  - PassiveBuffsActive_<UserId>
  - EventChoice_<UserId>_<EventId> — store choice & cooldowns
- Logging & analytics:
  - Track roll counts, fragment conversions, equip swaps, and event outcomes to tune rates and detect abuse.
- Anti-exploit checks:
  - Combat frequency thresholds, impossible damage spikes, teleport or position anomalies.
  - Rate-limit DataStore writes and batch updates where feasible.

---

## Monetization & Fairness Recommendations

- Provide a meaningful free roll and fragment economy so F2P players can progress.
- Fast-roll purchases should be small, transparent, and capped daily.
- If Godly souls are monetized, provide occasional free promos or alternative rare acquisition to reduce paywall perception.
- Use cosmetic-only purchases (skins/auras) for robust revenue without gameplay imbalance.

---

## What changed & why (brief)

- Consolidated multiple documents into one focused souls/collection design.
- Standardized acquisition flows, DataStore keys, and naming.
- Clarified passive/active equip rules, limits, and upgrade/fusion mechanics.
- Added balancing and telemetry hooks for iterative tuning.
- Made monetization choices explicit and provided fairness guidelines.

---

## Next practical deliverables I can produce now
- Lua pseudo-code for Soul Catcher + server-validated roll & DataStore integration.
- Soul equip/ability server-side prototype (skill triggers & anti-exploit checks).
- CSV of numeric constants (drop rates, costs, cooldowns) for tuning.
- Simple UI wireframes or JSON spec for Passive Buff UI and Soul Collection screen.

Pick one of the deliverables above and I’ll generate it immediately (I can start with the Lua Soul Catcher prototype if you want).