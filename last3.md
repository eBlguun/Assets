# GreedCard — Legends, Rewards & Magic-Item Store (Merged + Improved)

Last updated: 2025-08-25  
Merged from: eternal-steppe-rewards.md, golden-fish-lake-rewards_Version2.md, mountain-wolves-rewards_Version2.md, game-lore_Version5.md, game-lore_Version3.md, magic_type_items_store.md

---

## Purpose & Scope
This file consolidates the three major region legends (Eternal Steppe, Golden Fish Lake, Mountain Wolves), their reward tables, universal event rewards, and the Magic-Type single-use store items. I standardized reward formats, added actionable event flows, suggested drop rates and DataStore keys, and included anti-exploit and balancing guidance so the design is ready for prototyping.

---

## Executive Summary
- Each legend is a player-choice event (two diverging outcomes) that grants mechanical rewards (weapons/accessories/buffs), cosmetics, and utility items.
- Rewards follow a consistent design rule: Mythic/Legendary main items with 2 active skills + 1 passive (weapons/buffs), accessories are passive-focused, utilities grant limited teleport/consumable effects.
- Events should provide visible world impact (NPC/dialogue, local spawn changes, small persistent map effects).
- All randomness and purchases must be server-authoritative; DataStore keys and anti-exploit checks are specified.

---

## Event Flow (common)
1. Trigger: Player meets condition (collection completion, zone clear, or boss spawn).
2. Player(s) enter event instance or world boss arena.
3. If multiplayer, decide voting/consensus method for choice (own-player choice or group vote).
4. On completion, roll server-side for rewards; record event choice & rewards in DataStore.
5. Apply world-state changes (e.g., local spawns, NPC dialogue).
6. Display cosmetic + equip prompt; claim via UI.

DataStore keys examples:
- EventChoice_<UserId>_<EventId> — store player's chosen path
- EventRewardsClaimed_<UserId>_<EventId> — boolean / reward IDs
- EventRun_<EventId>_<RunId> — server record for analytics
- MagicItemPurchases_<UserId> — store single-use item purchases

Anti-exploit notes:
- Server-authoritative combat, roll, and purchase verification.
- Rate-limit DataStore writes (batch updates, debounce).
- Validate teleport/item usage server-side (consumable removal on successful effect).
- Log suspicious patterns (excessive roll attempts, impossible damage).

---

## Legend: The Eternal Steppe
Lore: Wind-swept plain where White Eagle (healing/protection) and Black Eagle (ambition/power) vie for influence. Players collect steppe cards or complete guardian trials to trigger the event.

Trigger: Collect all Steppe cards OR complete both guardian trials OR an occasional world spawn when enough players are present.

Choices & Rewards

Choice 1 — Aid the White Eagle (Harmony / Protection Path)
- Theme: team healing, defense, movement support.
- Main Mythic (weapon): Skyfeather Halberd — Starlit Sweep (wide heal AoE), Steppe Shield (barrier for allies) | Passive: +15% defense, +10% healing effects
- Accessory (Mythic): Unity Crest — Eagle’s Ward (party cleanse), Wind’s Embrace (party move speed) | Passive: +12% status resist, +8% move speed
- Legendary Buff (Aura): White Eagle’s Grace — Harmony Pulse (regen aura), Soaring Hope (instant revive 1 ally) | Passive: +10% all stats when in party
- Cosmetic: Eagle Spirit Wings (white radiant wings or feather aura)

Choice 2 — Side with the Black Eagle (Ambition / Power Path)
- Theme: offense, speed, solo/lead synergy.
- Main Mythic (weapon): Nightstorm Blade — Shadow Dive (teleport + strike), Predator Frenzy (rapid multi-hit) | Passive: +15% crit, +10% atk speed at night
- Accessory (Mythic): Dominion Talisman — Command Roar (fear), Unyielding Charge (stun dash) | Passive: +15% control resist, +10% move speed
- Legendary Buff (Aura): Black Eagle’s Might — Ambition Surge (atk/speed buff), Ruthless Hunt (bonus single-target dmg) | Passive: +10% all stats when solo
- Cosmetic: Black Eagle Aura (shadow wings / dark trail)

Universal Event Rewards (both paths)
- Steppe’s Calling (Epic Utility): Teleport to Steppe (1/day) | Passive: +5% defense & move speed while on steppe
- Star-Touched Token (Rare Accessory): Quick Rally (short self-buff) | Passive: +5% crit, +5% status resist

Suggested drop rates (per successful run)
- Mythic main weapon: 15–30% (scale by difficulty)
- Mythic accessory: 15–30%
- Legendary buff: 3–7%
- Cosmetic: 25–40%
Adjust rates with telemetry.

World impact examples
- Aid White Eagle: more healing NPCs, higher passive regen in Steppe towns.
- Aid Black Eagle: increase bandit/ambition spawn rates, add rare PvP challenge NPCs.

---

## Legend: Golden Fish Lake
Lore: Lake with sealed Golden Fish and a water spirit; Black Dragon corrupted the waters. Players must decide to free spirit or claim dragon power.

Trigger: Recover sacred casket or defeat lake mini-bosses and reach palace; event may be repeatable with cooldown.

Choices & Rewards

Choice 1 — Free the Water Spirit (Spirit’s Blessing Path)
- Theme: healing, water synergy, protection.
- Main Mythic (weapon): Spiritwater Spear — Purify Thrust (cleanse + high water dmg), Tidal Shield (damage absorb 5s) | Passive: +20% healing, +water resist
- Accessory (Mythic): Golden Fish Charm — Blessing Wave (ally heal radius), Mist Dodge (short invuln/evade) | Passive: +15% evade, +swim speed
- Legendary Buff (Aura): Lus’ Blessing — Oasis Regeneration (HP regen over time), Spirit Veil (10s immunity) | Passive: +10% all stats near water
- Cosmetic: Water Spirit Halo or floating spirit pet

Choice 2 — Claim the Black Dragon’s Power (Corruption Path)
- Theme: dark offense, AoE fear, lifesteal trade-offs.
- Main Mythic (weapon): Abyssal Dragon Blade — Black Flame Slash (burn AoE), Dread Surge (fear AoE) | Passive: +15% crit at night, +10% dark attack
- Accessory (Mythic): Obsidian Scale Ring — Shadow Ward (magic shield), Abyssal Leap (dash stun) | Passive: +20% dark resist, +10% atk vs spirits
- Legendary Buff (Aura): Dragon’s Pact — Corrupt Strength (major short atk buff), Dragon’s Roar (AoE lifesteal) | Passive: +10% atk but −5% healing outside water
- Cosmetic: Black Dragon Aura (shadow mist / dragon appendages)

Universal Event Rewards
- Lake’s Memory (Epic Utility): Teleport to Golden Fish Lake (1/day) | Passive: +5% resist in lake area
- Golden Fish Relic (Rare Accessory): Quick Swim (burst speed) | Passive: +5% swim speed, +5% healing

Suggested drop rates
- Mythic weapon/accessory: 12–25%
- Legendary buff: 2–6%
- Cosmetic: 20–35%

World impact examples
- Free spirit: clears some corrupted spawns and grants periodic water-related buffs to nearby fishing/harvest nodes.
- Claim dragon: spawns tougher shadow enemies; opens risk/reward resource nodes with higher drops.

---

## Legend: Mountain Wolves (Great Mountain)
Lore: Blue Wolf (guardian) vs Black Wolf (corrupt conqueror). Collect mountain cards, ally or destroy guardian, influence mountain's fate.

Trigger: Clear mountain shrines, collect relevant cards, or defeat mountain mini-bosses.

Choices & Rewards

Aid the Blue Wolf (Restore Guardian)
- Theme: evasion, defense, night synergy.
- Mythic: Frostbite Fang — Frost Slash (AoE slow), Wolf Spirit Dash | Passive: +20% evasion in forests
- Mythic Relic: Guardian’s Howl — Lunar Shield, Wolf’s Rally (AoE buff) | Passive: +15% defense at night
- Legendary Buff: Blue Wolf’s Blessing — Spirit Heal, Moon’s Grace (cleanse) | Passive: immune to fear & +10% all stats at night
- Cosmetic: Blue Wolf Spirit aura/skin

Side with the Black Wolf (Embrace Corruption)
- Theme: aggressive, bleed, darkness synergy.
- Mythic: Shadowclaw Blade — Shadow Slash (bleed), Predator Leap | Passive: +10% crit, +15% dmg at low HP
- Mythic Accessory: Alpha’s Dominance — Pack Howl, Fear Aura | Passive: +15% attack at night
- Legendary Buff: Black Wolf’s Oath — Blood Hunt (lifesteal boost), Alpha Roar (team atk up) | Passive: +10% move speed in darkness
- Cosmetic: Black Wolf Corruption aura/skin

Universal Rewards
- Mountain Relic (Epic Utility): Teleport to Great Mountain (1/day) | Passive: +5% defense on mountain
- Wolf Totem (Rare Accessory): Quick Howl (speed boost) | Passive: +5% resist & +5% move speed

Drop rates (per successful run)
- Mythic items: 15–30%
- Legendary buff: 3–8%
- Cosmetic: 20–40%

World impact
- Restored Blue Wolf: more protective spawn behaviors; hidden quests open.
- Black Wolf ascendance: increased corrupted spawn, rare dark resource nodes.

---

## Magic-Type Items (Store, Single-Use)
System rules
- Not collectible. Single-use and consumed on use. Available in shops. Tiers determine price and power.
- Server verifies purchase & consumes item on success.
- Price tiers recommended (adjust to economy): Common 100–500 gold, Rare 800–1500, Epic 1800–3000, Mythic 4500–5500, Legendary 6500–8000.

Core items (refined)
| Item Name | Tier | Effect | Limit | Example Price |
|---|---:|---|---:|---:|
| Portal Stone | Rare | Teleport to any visited location | 1 use | 1,200 gold |
| Link Sigil | Epic | Teleport to another player (target must accept) | 1 use | 2,500 gold |
| Clairvoyant Mirror | Rare | Peek at player inventory (target notified) | 1 use | 1,200 gold |
| Ward of Secrecy | Epic | Lock own inventory from peeking for 24h | 1 use | 2,000 gold |
| Thief’s Glove | Mythic | Steal random eligible item | 1 use; restricted | 5,000 gold |
| Messenger’s Feather | Common | Send message to offline player | 1 use | 200 gold |
| Shadow Cloak | Epic | Invisibility in safe zones for 30s | 1 use; breaks on attack | 2,200 gold |
| Recall Rune | Epic | Recall 1 item from storage | 1 use; restricted | 2,500 gold |
| Mimic’s Token | Legendary | Copy item temporarily (1 hour) | 1 use; limited | 7,000 gold |
| Time Twister | Legendary | Undo last inventory action | 1 use; restricted | 7,000 gold |

Design notes
- Clear limitations—no use in PvP/competitive exploits.
- Server must remove item at purchase and only apply effect if conditions validated.
- Consider cooldowns on shop purchases for powerful single-use items to avoid abuse.

DataStore / transaction keys
- MagicItemPurchases_<UserId> — record purchases & daily limits
- MagicItemUseLog_<UserId> — last uses & cooldown enforcement

Anti-exploit & security
- Validate ownership server-side prior to effect.
- For stealing/transferring items, require server escrow and checks to prevent duplication.
- Log and throttle suspicious rapid purchases/uses.

---

## Balance & Tuning Recommendations
- Start conservative with high-rarity drop rates low; adjust via telemetry.
- Example baseline (for events): Mythic 15–25%, Legendary 2–6%, Cosmetic 20–40%. Raise/lower per event difficulty.
- Teleport utilities limited to 1/day to avoid travel abuse.
- Passive stacking behavior: additive with soft caps (e.g., passive buffs stack additively up to +50% from similar sources).
- Night/area synergies: ensure "at night" or "on steppe" bonuses are impactful but not game-breaking. Test with a small cohort.

---

## Implementation Notes & DataStore Keys (consolidated)
- EventChoice_<UserId>_<EventId> — player's path choice (string)
- EventRewardsClaimed_<UserId>_<EventId> — list of claimed reward IDs
- EventRun_<EventId>_<RunId> — server analytics for each run
- MagicItemPurchases_<UserId> — record of purchases, daily caps
- TeleportUse_<UserId> — daily teleport usages
- CosmeticUnlocks_<UserId> — cosmetics earned via events

Server responsibilities
- All reward rolls and purchases are executed server-side and logged.
- Use server validation before granting Mythic/Legendary rewards.
- Debounce DataStore writes (e.g., batch every few seconds or on session end) to avoid rate-limit errors.

---

## What I changed & why (brief)
- Unified similar reward tables into a consistent 2-actives + 1-passive format.
- Standardized rarity names and suggested drop-rate ranges for tuning.
- Added event flow, DataStore keys, and specific anti-exploit points.
- Consolidated world-impact descriptions to connect player choice to persistent changes.
- Cleaned magic-item store list, added price-tier guidance and server checks.

---

If you'd like, I can now:
- Generate a Lua server prototype for one event (choose Eternal Steppe, Golden Fish Lake, or Mountain Wolves) implementing: server-authoritative reward roll, DataStore updates, and anti-exploit checks.
- Or produce a CSV of reward constants and drop rates for tuning.

Tell me which prototype or deliverable to produce first and I’ll generate it now.