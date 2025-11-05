# GreedCard — Merged Game Design Summary (Lore, Progression & Systems)

Last updated: 2025-08-25  
Author: merged & improved from provided drafts

---

## Overview

GreedCard is a Roblox action-RPG mixing card-based itemization with weapon mastery, faction-based seasonal content, collectible legendary character transformations, and persistent social/economic systems (auction house, museums). Core pillars: Combat (weapons & skills), Collection (cards & museums), Progression (titles, souls, honor), and Seasonal Lore Events (Forgotten Hero → Lost Souls → Faction War).

---

## Core Lore (Unified)

- The Magical Cards are ancient artifacts that bend reality. They can grant great power but risk corrupting their users.
- Lucian (also referenced as Lucian_Q3) united the Cards to defeat the Demon Lord. In the final strike the Demon Lord’s essence possessed Lucian’s body, sealing Lucian’s soul away.
- Lucian’s corrupted body became the Lost Demon King. The inner soul was locked in the Hall of Echoes (the Forgotten Hero trial).
- The possession sparked the Holy War between the Relic Guardians and the Demon Army. The battlefield’s fallen became Lost Souls.
- The Magical Cards shattered and scattered across the world; their echoes cause both blessings and curses.

Narrative tone: every system ties back to Lucian’s tragedy and the shards of the Magical Cards.

---

## High-Level Seasonal Roadmap

- Season 1 — Forgotten Hero Trial
  - Trigger: Player reaches Legend rank in a weapon (e.g., Blade Sovereign).
  - Result: Dimensional Rift → Hall of Echoes → solo boss: Forgotten Hero (Lucian’s inner soul).
  - Rewards: Ascended titles, weapon-specific legendary skills, aura cosmetics.

- Season 2 — Lost Souls & Faction War
  - Triggered after Season 1 events and the rise of Lost Souls.
  - Players choose side: Relic Guardians (Guardian side) or Demons (Demon side).
  - Faction progression, seasonal leaderboard, faction-exclusive rewards, end-of-season Temporary Demon Lord / Chief.

Ongoing: Dynamic world events (Soul Clash invasions, world reactive events based on card usage).

---

## Core Systems

### Cards
- Types: Weapon Cards, Character/Legendary Cards, Magic-Type (utility) Cards, Passive Buff Items, Glitch Cards (time-limited).
- Materialization: Cards can be materialized into items (weapons, tools, spells). Materialized items have random stats (Damage, Speed, Crit Rate, Durability).
- Durability: All materialized items degrade with use and can be destroyed when durability reaches 0. Rare enhancements (Mending, Glitch-Infinite) can repair or grant infinite durability.
- Binding: Using a card may bind it to the account (non-tradable). Legendary Character Cards and used/bound items are account-bound.
- Passive Buff Items: Consumable/activatable collection items that grant ongoing stackable passives (up to 5 active buff slots). Multiple copies stack.

Example materialized weapon attributes:
- Name: Flame Sword  
- Damage: 38 | Attack Speed: 1.1x | Crit: 15% | Durability: 420/420 | Enhancement: Flame Enchant | Bound: Yes

### Legendary Character Cards
- Ultra-rare; transform player’s abilities and visuals.
- One active at a time; activating a new one destroys the prior.
- Bound, permanent passives, no durability.

### Card Fusion & Upgrades
- Fuse cards to combine effects and increase rarity.
- Merge a bound item with a same-name mint item to raise Skill Level (improves abilities but does NOT restore durability).
- Skill Level cap example: 5 (each merge +1). Will require mint copies and resources.

### Passive Buff Items (Quick)
- Up to 5 active; stackable by duplicates.
- Examples: Ancient Coin (+3% coins per), Lucky Rabbit’s Foot (+2% drop rate), Book of Endurance (+10 max stamina).
- Activation may "lock" or consume items (design choice; currently optional—recommend lock while active to preserve collection integrity).

---

## Lost Souls System (attachment-based buffs + skills)

Players attach one or more Lost Souls to gain passive buffs and unlock skills. Soul rarity and corruption determine tradeoffs.

Core souls (examples):
- Soldier Soul — +10% Defense, +5% HP regen | Skill: Guardian Stance (shield)
- Mage Soul — +10% Elemental Damage, +10% Mana/Stamina regen | Skill: Arcane Burst (AoE)
- Rogue Soul — +15% Crit Chance, +10% Move Speed | Skill: Phantom Step (dash)
- General Soul — +5% All Stats, aura buff allies | Skill: Rally Cry (ally buff)
- Berserker Soul — +20% Damage, -10% Defense | Skill: Blood Rage
- Cleric Soul — +15% Healing | Skill: Soul Mend (heal)
- Champion Soul (Rare) — +20% Weapon Damage | Skill: Legacy Skill (weapon-unique)
- Corrupted Soul (Very Rare) — +25% Damage but drains HP | Skill: Dark Possession

Design notes:
- Limit simultaneous souls (e.g., max 3 active) to avoid unlimited stacking.
- Corrupted souls have strong power with costs to promote trade-offs and faction synergy.

---

## Weapon Mastery Titles & Progression

Weapon types: Sword, Bow, Spear, Axe, Fist. Kills are counted only when weapon is equipped.

Example progression (Sword shown; others mirror):

- 100 kills — Sword User — +2% sword damage
- 200 kills — Sword Fighter — +5% sword damage, +2% attack speed
- 500 kills — Sword Master — +10% sword damage, +5% crit
- 1000 kills — Blade Sovereign — +15% sword damage, +10% crit, aura & trail

Ultimate titles for multi-weapon mastery:
- Master 2 weapons → Weapon Specialist (+5% all weapon damage)
- Master 3 weapons → Weapon Veteran (+10% all weapon damage, +5% defense)
- Master all weapons → Weapon Master (+15% all weapon damage, +10% attack speed)
- Secret quest + all masteries → Chosen Hero (+20% all buffs, unique cosmetic skill)

---

## Dimensional Rift / Forgotten Hero (Legend Boss)

Trigger: Player reaches Legend rank on any weapon (e.g., Blade Sovereign). The player is teleported to Hall of Echoes to face Forgotten Hero (Lucian’s soul).

Boss summary:
- Multi-weapon phases (Sword, Spear, Bow, Axe, Fist); final phase combines all.
- Base Health example: 100,000 HP (scales by player level). Damage example: 200–500 per hit (phase-dependent).
- Abilities: Weapon Mastery Switch (every 45s), summon ghostly warriors, Final Judgement at low HP (AoE multi-weapon combo).
- Rewards on win: Ascended title (e.g., Blade Sovereign Ascended), Echo of the Forgotten aura, unique weapon skill (weapon-specific), Forgotten Relic (rare token), progress toward Chosen Hero.

Dynamic difficulty & progression tracking:
- DataStore keys:
  - ForgottenHeroBestDamage_<UserId> (stores max percent damage dealt against boss across attempts)
- HP-based weakening:
  - If player deals ≥20% HP in an attempt, next attempt starts with reduced boss HP = max(50%, 100% − bestDamagePercent).
  - Minimum boss HP floor: 50% (prevents trivialization).
- Damage reduction:
  - Each qualifying attempt (≥20%) reduces boss damage output by 3% (caps at 12% after 4 attempts).
- Player buff:
  - Resolve of the Worthy: +5% player damage per qualifying attempt, stacks to +15% (resets on victory).

Retry penalties & mitigation:
- On defeat: banish to world + scaling penalties on weapon durability and cooldown before retry:
  - 1st fail: −20% weapon durability, 5-day cooldown
  - 2nd fail: −30% durability, 6-day cooldown
  - 3rd+ fail: −40% durability, 7-day cooldown (capped)
- Mitigation: complete 2 short training quests to reduce cooldown by 1 day each (min 2 days).
- Retries unlimited but penalties cap; HP/damage reductions capped to preserve challenge.

Anti-exploit:
- Server-verified damage reporting, only count attempts that meet an anti-cheat validation (combat logs & attack rate thresholds).
- Use DataStore SetAsync/GetAsync carefully with rate limits and conflict resolution.

---

## Factions: Relic Guardians (Guardian Side) & Demons (Demon Side)

Players choose a faction when Lost Souls event escalates. Faction systems are season-long competitive progression with ranks and top seasonal leader.

Relic Guardians (Philosophy: Purify & Balance)
- Exclusive mechanics: Soul Purge (cleanse corruption), Aegis Ward (team damage reduction).
- Titles by Honor (example tiers):
  - Tier 1: Shieldbearer of Light — +5% Defense
  - Tier 2: Sentinel of the Dawn — +10% Healing, +5% Regen
  - Tier 3: Warden of Balance — +15% Defense, +5% Team Buffs
  - Tier 4: Champion of Radiance — +20% Defense, +10% Recovery
  - Top: Chief of Relic Guardians — +30% Defense, +20% Team Buffs, Golden Aura + Halo

Demons (Philosophy: Power at any cost)
- Exclusive mechanics: Blood Oath (damage up at low HP), Infernal Infusion (skill power up, drains stamina).
- Titles by Honor:
  - Tier 1: Fang of Darkness — +5% Damage
  - Tier 2: Claw of the Abyss — +10% Crit
  - Tier 3: Warlord of Havoc — +15% Damage, +5% HP
  - Tier 4: Champion of Desolation — +20% Damage, +10% Stamina Regen
  - Top: Temporary Demon Lord — +30% Damage, +20% Defense, Black/Red Aura + Infernal Crown

Faction war features:
- Seasonal leaderboards, territory control events, faction quests (purify vs corrupt).
- End-of-season reward: Temporary Demon Lord or Chief cosmetic and server-level stat changes while active.

---

## Demon Realm Biome & Activities

- Biome: black crystal forests, lava rivers, soul storms.
- Activities:
  - Soul Clusters: Purify/corrupt waves of spirits for rewards.
  - Demon Lords: Mini-bosses dropping corrupted souls.
  - Faction PvE quests and group events.
- Endgame Raid: The Lost Demon King (Lucian’s possessed body) — major group raid with high-tier drops.

---

## World & Event Systems

- Bandit/Enemy event triggers:
  - Area kill counters (e.g., 30 kills spawn Bandit Leader).
  - Timed challenges (10 kills in 60s → +50% XP, 5 min buff).
  - Rare spawns (1–3% chance) with cosmetic/title drops.
- Soul Clash Invasions:
  - Trigger: 2+ players activate Legendary Character Cards simultaneously.
  - Spawn corrupted versions of those characters server-wide for 10–15 minutes; scales with player count.
  - Rewards: temporary buffs, rare materials, chance of corrupted character cards on success. Failure weakens certain worlds (loot / shops closed).

Dynamic world reaction:
- Widespread use of a card type can trigger local/global events (e.g., Fire cards → Firestorm buff, increases fire enemies).
- Legendary card cluster usage can cause "Soul Clash" invasions.

---

## Economy & Social Systems

### Auction House (Item-for-Item)
- Players list tradable items; others submit item offers that the server holds.
- End of auction: seller selects winner or cancels; non-winning offers returned.
- Auto-cancel on seller disconnect: returns all items.
- Security: Server-hold prevents duplication; only tradable items allowed.
- Optional modes: blind bids, minimum offer rarity, favorite offers.
- History and transparency built into UI.

### Museums & Collections
- Players build personal museums to display rare cards and items.
- Museum likes/upvotes yield rewards and social status.

### Item Pack Combination
- Combine 10 eligible items (not bound/magic) to craft a pack. Pack rarity determined by composition and chance.
- Example: 10 Common → Common Pack; 8C +2U → 20% Uncommon Pack chance.
- Packs yield items of same or better rarity.

---

## Balance & Probability Guidelines (examples & defaults)

- Forgotten Relic base drop from Forgotten Hero: 5% ( +2% per qualifying failed attempt, capped at +8% → max 13%).
- Rare boss spawns: 1–3% chance.
- Corrupted Soul drop from Demon Lords: 3–8% (scale by difficulty).
- Legendary Character Card drop: ultra-rare (<<1%); alternative acquisition through collection events or auction house.

---

## Technical & Implementation Notes

- DataStore keys:
  - ForgottenHeroBestDamage_<UserId> — max % of boss HP dealt
  - PlayerFaction_<UserId> — current faction
  - WeaponKills_<UserId>_<WeaponType> — kills per weapon (persisted)
- Validate all client-reported events server-side. Combat damage and drops must be server-authoritative.
- Rate-limit DataStore updates (debounce & batch where possible).
- Anti-exploit checks: check attack frequency, teleport/position anomalies, impossible damage spikes.

---

## Improvements I made (concise)
- Unified terminology: "Magical Cards" → "Cards / Magical Cards" consistently.
- Consolidated repeated lore and mechanics into a single coherent narrative.
- Standardized numeric examples (durability, drop rates, cooldowns, HP & damage examples).
- Added explicit DataStore keys and anti-exploit recommendations.
- Clarified Lost Souls capacity & stacking rules (recommend max active souls).
- Clarified passive buff slot mechanics (max 5) and stacking behavior.
- Converted vague progression triggers into specific event conditions (e.g., Legend rank triggers).
- Provided default caps and floors to avoid trivialization (boss HP floor 50%, damage reduction cap 12%).

---

## Further recommended improvements (I can implement on request)
- Create Lua prototypes for:
  - Forgotten Hero encounter server script (DataStore usage, damage tracking, anti-exploit).
  - Soul attach/detach system with active-soul limits and UI hooks.
  - Auction House server-side flow with server-hold logic.
- Produce UI wireframes for the Prestige Tree, Faction HUD, and Auction House.
- Produce CSV or JSON spec with all numeric constants for tuning.
- Create a task breakdown (issues) for development sprints.
- Simulate balancing (e.g., expected drop rates, player progression time based on XP curves).

---

If you'd like, I can now:
- Generate the Lua/Roblox pseudo-code for one system (pick: Forgotten Hero boss flow, Auction House, or Lost Souls).
- Or produce a tuning spreadsheet (CSV) of constants.
Tell me which next deliverable you want and I’ll implement it right away.