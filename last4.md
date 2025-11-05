# GreedCard — Complete Collections, Legends, Bosses & Store (Merged + Improved)

Last updated: 2025-08-25  
Author: merged & improved from provided design files

---

## Purpose & Scope

This single consolidated document merges and improves the provided design notes about:
- Collections & items (Wanderer’s Remnants, Crimson Court, Deepforge Arsenal, Moonlit Carnival, Phoenix’s Legacy),
- Passive buff stacks and slot limits,
- Region legends & event rewards (Eternal Steppe, Golden Fish Lake, Mountain Wolves),
- Bosses (win items, penalties, cross-collection rewards, boss weapon skills),
- Magic-type single-use store items,
- Implementation guidance (DataStore keys, server authority, anti-exploit), and
- Balancing recommendations and telemetry hooks.

This version standardizes formats (2 active skills + 1 passive rule for main items), clarifies player flows and constraints, sets example drop rates and penalties, and provides technical keys to help implementation and prototyping.

---

## High-Level Rules & Conventions

- Main reward items (weapons/buffs): typically 2 active skills + 1 passive effect.
- Accessories: passive-focused; may have one minor active or utility.
- Utilities (teleports, one-offs): single-use with daily or run-limited cooldowns.
- Rarity tiers: Common, Uncommon, Rare, Epic, Mythic, Legendary, Godly.
- Passive buff activation: max 5 active buff slots at once; each buff has a per-rarity stack limit.
- Server authority: All reward rolls, purchases, combat resolution, and state changes are server-authoritative and logged.

Core DataStore keys (examples)
- PlayerInventory_<UserId>
- Collections_<UserId> — collected card/item IDs & metadata
- PassiveBuffsActive_<UserId> — active buff slots & counts
- EventChoice_<UserId>_<EventId> — player's chosen event path
- EventRewardsClaimed_<UserId>_<EventId> — claimed rewards
- BossRun_<EventId>_<RunId> — server analytics for each event run
- MagicItemPurchases_<UserId> — record of purchases for rate limits
- TeleportUse_<UserId> — daily teleport usage counts
- WeaponKills_<UserId>_<WeaponType> — weapon mastery counts
- ForgottenHeroBestDamage_<UserId> — boss weakening tracking for legacy bosses

Anti-exploit & server notes
- Validate and apply all state changes server-side.
- Debounce DataStore writes; batch updates where possible.
- Track suspicious patterns (rapid purchases, impossible damage, repeated fast-rolls).
- For any single-use-shop item that moves items (steal/recall), use server escrow: hold offered items in server vault until transaction final to avoid duplication.

Telemetry & tuning
- Log: roll counts, fragment conversions, equip swaps, active buffs, event completion rates.
- Use telemetry to adjust drop rates within suggested ranges.

---

## Passive Buff Collection Items — Stack Limits by Rarity

- Stack limits by rarity (max stacks per buff type):
  - Common: 5
  - Uncommon: 4
  - Rare: 3
  - Epic: 3
  - Mythic: 2
  - Legendary: 2
  - Godly: 1

- Total Passive Buff Slots: up to 5 active slots at once (any combination respecting per-buff stack caps).

- Activation rules:
  - Activating a buff consumes / locks a copy while active (design choice; recommended lock).
  - Activating beyond a buff’s max stacks is blocked.
  - Activating the 6th buff requires replacing an active slot.
  - Godly buffs cannot stack or have duplicates active simultaneously.

Example:
- 5x Common Ancient Coin = +15% gold gain (if additive stacking).
- 2x Mythic General’s Charm = modest multi-effect bonus.
- 1x Godly Phoenix Feather = unique revive effect (non-stack).

Design recommendation: additive stacking with soft caps, and treat Godly as single powerful slot to avoid inflation.

---

## Collections — Consolidated Sets & Highlights

Each collection contains ~15 weapons plus accessories/relics and a cross-collection boss. Below are condensed curated lists for quick reference; full flavor text and exact skill text are retained in the original files.

1) Wanderer’s Remnants
- Theme: exploration, agility, survival.
- Weapon highlights: Rusty Knife (Common), Merchant’s Cane (Epic), Fabled Wanderer’s Edge (Legendary).
- Cross-collection boss: The Lost Vagabond (Easy) — rewards: Endless Journey Satchel (Epic) or Wayfarer’s Blade or Wanderer’s Tenacity (one random).

2) Crimson Court Regalia
- Theme: court intrigue, bleed/fire, status manipulation.
- Weapon highlights: Scarlet Greatsword (Mythic), Sanguine Blade (Legendary).
- Cross-collection boss: The Blood Marquis (Medium) — rewards: Signet of Red Oath (Mythic) or Blood Oath Sabre or Courtly Resolve.

3) Deepforge Arsenal
- Theme: forge, constructs, heavy-hitting weapons.
- Weapon highlights: Cinderstone Greatblade (Mythic), Magma Drill Halberd (Legendary).
- Cross-collection boss: The Iron Colossus (Medium-Hard) — rewards: Colossus Gauntlets or Deepforge Axe or Forgemaster’s Endurance.

4) Moonlit Carnival Trophies
- Theme: illusions, night bonuses, chaotic effects.
- Weapon highlights: Moon Carnival Pike (Legendary), Gleaming Tarot Deck (Mythic relic).
- Cross-collection boss: The Masked Maestro (Hard) — rewards: Pierrot’s Charm or Pierrot’s Edge or Carnival’s Guile.

5) Phoenix’s Legacy
- Theme: rebirth, fire, revive mechanics.
- Weapon highlights: Phoenix Heartblade (Legendary), Eternal Ember Axe (Mythic).
- Cross-collection boss: The Ashen Sovereign (Extreme) — rewards: Solar Phoenix Mantle (Legendary) or Infernal Phoenixblade or Phoenix Spirit.

Design rules
- Each weapon has: Active 1, Active 2, Passive.
- Accessories are passive-dominant; relics may include one strong active with long cooldown.
- Legendary/Godly items are visually distinctive and limited.

---

## Legends & Events — Rewards & Flows

General event flow (server-authoritative)
1. Trigger: collection completion, zone clear, or world spawn.
2. Players form a party or go solo; choice selection either individual or via group vote (design decision).
3. Enter instanced event/boss arena.
4. Server computes combat, determines success, rolls reward on success.
5. Update DataStore with EventChoice and EventRewardsClaimed and apply local world-state changes.
6. Display rewards; let player claim via UI.

Suggested roll & reward ranges (tuning start points)
- Mythic main weapon: 12–30% per successful run (difficulty adjusted).
- Mythic accessory: 12–30%.
- Legendary buff: 2–8%.
- Cosmetic: 20–40%.
Adjust via telemetry.

Given the supplied material, each legend follows a choice (restore vs claim) with divergent rewards and world effects:

Eternal Steppe (White Eagle vs Black Eagle)
- White Eagle: healing, party-centric (Skyfeather Halberd, Unity Crest, White Eagle’s Grace).
- Black Eagle: offense/solo (Nightstorm Blade, Dominion Talisman, Black Eagle’s Might).
- Universal: Steppe’s Calling (Epic utility), Star-Touched Token (rare).
- World Impact: spawn changes (more healers or more bandits) and local NPC dialog updates.

Golden Fish Lake (Free Spirit vs Claim Dragon)
- Free Spirit: Spiritwater Spear, Golden Fish Charm, Lus’ Blessing.
- Claim Dragon: Abyssal Dragon Blade, Obsidian Scale Ring, Dragon’s Pact.
- Universal: Lake’s Memory (Epic), Golden Fish Relic (Rare).
- World Impact: cleansed spawns / corrupted resource nodes depending on choice.

Mountain Wolves (Blue vs Black Wolf)
- Aid Blue Wolf: Frostbite Fang, Guardian’s Howl, Blue Wolf’s Blessing.
- Side Black Wolf: Shadowclaw Blade, Alpha’s Dominance, Black Wolf’s Oath.
- Universal: Mountain Relic utility & Wolf Totem accessory.
- World Impact: protective spawns and hidden quests vs corrupted spawns and dark nodes.

Design notes:
- Use persistent but reversible local world effects (e.g., spawn lists) to show impact without global permanent locks.
- Allow event repeatability with cooldowns; limit legendary rewards frequency to maintain rarity.

---

## Bosses — Win Items, Penalties & Weapon Skills

Bosses per collection (difficulty tiers, sample penalties & rewards):
- Lost Vagabond (Easy): Epic reward, title + minor penalties on loss (gold loss, movement cooldown).
- Blood Marquis (Medium): Mythic reward, loss: random accessory lost + healing reduced.
- Iron Colossus (Medium-Hard): Mythic reward, loss: main weapon locked, gear durability loss.
- Masked Maestro (Hard): Legendary reward, loss: lose an Epic Carnival item, accessory equip blocked.
- Ashen Sovereign (Extreme): Legendary reward, loss: lose a Mythic/Legendary item, major weapon lock, debuff to fire resist.

Boss win weapon skill examples (physical-focused)
- Endless Journey Blade (Epic): Wayfarer’s Slash (AoE + movement) & Trailbreaker (cleanse slow).
- Blood Oath Sabre (Mythic): Crimson Lunge (bleed) & Noble Parry (heal on counter).
- Colossus Axe (Mythic): Earthsplitter (armor reduce) & Forge Bash (stun).
- Pierrot’s Edge (Legendary dagger): Trickster’s Flurry (confuse proc) & Midnight Escape (untargetable dodge).
- Infernal Phoenixblade (Legendary sword): Phoenix Rebirth (on-death revive) & Inferno Slash (massive fire AoE).

Penalty design principles
- Penalties should be meaningful but not permanent (locks/time-limited removal).
- Provide mitigation mechanics (training quests, cooldown reductions) to encourage reattempts.

---

## Cross-Collection Reward Handling

- On boss victory, player receives one random cross-collection reward selected from weapon / accessory / buff lists. Ensure variety to increase replay value.
- Make these rewards account-bound if they are particularly powerful, or offer mint/tradable versions as rarer drops.

---

## Magic-Type Items (Store — Single Use)

Rules
- Single-use items bought from stores; not collectible.
- Tiered pricing aligned with power; server validates, consumes item on use.
- Limitations expressly enforced server-side (no use in PvP for certain items, cannot affect bound/event items).
- Daily or per-session purchase cooldowns recommended for powerful items.

Core list (examples)
- Portal Stone (Rare): teleport to visited location; 1 use.
- Link Sigil (Epic): teleport to another player; requires acceptance; 1 use.
- Clairvoyant Mirror (Rare): peek inventory (target notified).
- Thief’s Glove (Mythic): steal a random eligible item (server escrow).
- Mimic’s Token / Time Twister (Legendary): high cost, heavy restrictions.

Security
- Implement server escrow and atomic transactions for trade/steal operations.
- Log purchases in MagicItemPurchases_<UserId>.

Economy guidance
- Price tiers: Common 100–500, Rare 800–1500, Epic 1800–3000, Mythic 4500–5500, Legendary 6500–8000 (adjust per economy).
- Consider soft currency vs premium currency splits for monetization balance.

---

## Balancing, Telemetry, and Tuning Recommendations

- Start conservative with drop rates; iterate with telemetry (player engagement, completion rates).
- Track event clear rate, number of runs per player, repeat wins, average time to first legendary.
- Use A/B testing for fast-roll pricing and godly-tier monetization to avoid pay-to-win perception.
- Define soft caps for passive stacking and power ceilings for combined equipment synergies.

Suggested passive stacking math (example)
- Additive stacking for same-buff copies with soft cap (e.g., up to +50% effect from same source). For multiplicative or conditional buffs, define explicit formulas per buff.

---

## Implementation Checklist & Next Work Items

- Build server modules:
  - Event manager (instance spawning, boss spawns, reward roll).
  - Reward vault & DataStore handlers (atomic write patterns).
  - Passive buff state machine (slot management & stack enforcement).
  - Magic shop & purchase escrow handler.
- Create client UI screens:
  - Collections browser & museum.
  - Passive Buff activation UI.
  - Event participation panel + choice dialog.
  - Boss reward claim UI.
- Prototyping deliverables I can produce:
  - Lua server prototype for an event (Eternal Steppe recommended).
  - Soul equip/ability server script pseudo-code.
  - CSV of numeric constants (drop rates, costs, cooldowns).
  - UI wireframes or JSON spec for Collections and Passive Buff UI.

---

## Change Log — What I consolidated & why

- Merged multiple overlapping documents into one canonical reference.
- Standardized reward formatting (2 actives + 1 passive).
- Consolidated event flows, DataStore keys, and anti-exploit patterns to be implementation-ready.
- Clarified passive buff stacking and total slot rules to avoid ambiguity.
- Grouped boss rewards and penalties into consistent failure/reward patterns to balance risk/reward.
- Added telemetry and balancing guidance for iterative tuning post-launch.

---

If you want, I’ll now generate a Lua server prototype for one event (Eternal Steppe) that includes: server-authoritative reward roll, DataStore updates for EventChoice and EventRewardsClaimed, passive buff slot validation, and basic anti-exploit checks — or I can produce a CSV of constants for tuning first.