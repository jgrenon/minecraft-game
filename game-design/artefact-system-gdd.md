# Artefact System — Game Design Document

> **Server:** Artefact Survival SMP  
> **Author:** d3sy / Sombrero Studio  
> **Date:** 2026-06-07  
> **Status:** Final Draft (Ready for Review)  

---

## Design Philosophy

Artefact Survival SMP is a competitive survival multiplayer experience built around **player-vs-player conflict with escalating stakes**. The core loop: **kill → claim a weapon → become more powerful → attract more fights → risk losing everything**.

The system is designed with:
- **High risk, high reward** — artefacts are powerful but drop on death
- **Diminishing pool** — fewer weapons available as they're claimed
- **Escalating climax** — the Scythe endgame forces a server-wide event
- **Safety valves** — copper grounding, reset timer prevent soft-locks

---

## 1. Kill Tracker System

### Persistence
- **Storage:** YAML file (`killtracker.yml`) in the plugin data folder
- **Data per player:** UUID → `{ kills: int, weaponPoolId: string|null }`
- **No SQL dependency** — lightweight for small player count

### Kill Attribution
- **Final blow** determines kill credit (last player to deal damage)
- Damage share tracking is NOT implemented — simplifies logic, prevents disputes
- **Edge case — Suicide / Fall / Mob:** No credit. Kill count unchanged.
- **Edge case — Scythe Imprisonment Death:** Counts as a kill for the wielder

### Milestone Detection
- **Threshold:** Every **+15 kills** (15, 30, 45, 60...)
- Checked on every kill event
- On milestone: check weapon pool and grant reward

### Weapon Pool Management

| Condition | Pool Contents |
|---|---|
| Phase 1 — Before any claims | Golden Blade, Thunder Crossbow, Riptent |
| After 1 claim | 2 remaining weapons |
| After all 3 claimed | **Pool empty → Ruby fallback** |
| After Scythe crafted | Pool **closed permanently** |
| After Scythe reset (30-min expiry) | Pool **restored** to full, kill scores reset to zero |

- **Diminishing pool:** At each +15 milestone, the player receives ONLY the weapons that remain.
- **Weapon assignment:** Random draw from remaining pool. Player cannot choose.
- **Notification:** Server-wide announcement with sound + lightning effect.

---

## 2. Golden Blade

A golden trident — **ranged oneshot assassin weapon**. High skill cap: one throw, one kill, but 20 seconds before you can throw again.

| Property | Value |
|---|---|
| Base Item | Trident (custom texture) |
| Melee Damage | Diamond Sword level (7 HP) |
| Enchantments | Sharpness III, Loyalty III |
| Unbreakable | ✅ Yes |
| Drop on Death | ✅ Yes (glowing effect) |
| **Throw Cooldown** | **20 seconds** |
| **Throw Damage** | **Instant kill** on direct hit |
| Armor Bypass | No — Absorption hearts absorb first |
| Shield Block | Blocks 100%. No cooldown consumed. |
| Non-player entities | Standard damage. No oneshot. |

---

## 3. Thunder Crossbow

A volatile crossbow that calls lightning. **Area denial weapon** — forces opponents to spread out or build copper defenses.

| Property | Value |
|---|---|
| Base Item | Crossbow (custom texture) |
| Reload Time | **0.8 seconds** |
| Unbreakable | ✅ Yes |
| Drop on Death | ✅ Yes (glowing effect) |
| **Lightning Damage** | **5 hearts (10 HP)** — AoE 3-block radius |
| **Fire** | **Suppressed** — no fire spread |
| **Copper Diversion** | 33% if copper blocks within 100 blocks |
| Team Damage | Hits everyone in AoE |

---

## 4. Riptent

A trident that lets you **Riptide across land**. Pure mobility — no direct kill power, unmatched in escaping and chasing.

| Property | Value |
|---|---|
| Base Item | Trident (custom texture) |
| Melee Damage | Iron Sword level (6 HP) |
| Unbreakable | ✅ Yes |
| Drop on Death | ✅ Yes (glowing effect) |
| **Riptide Level** | III |
| **On Land** | ✅ Always treated as in water/rain |
| **Fall Damage** | None during Riptide |
| **Cooldown** | None — chain dashes allowed |

---

## 5. Ruby System

A consumable power-up — the **consolation prize** when all weapons are claimed.

| Property | Value |
|---|---|
| Acquisition | +15 kills with **empty weapon pool** |
| Max Stack | 16 |
| **Usage** | **Single-use** — consumed on right-click |
| **Cooldown** | 1 minute (per player, not per Ruby) |
| **Duration** | 20 seconds |

**Buff Package (20s):** +2 Absorption Hearts, Regeneration VI, Strength III, Invisibility I, Speed II

### Strategy
- Best used pre-engagement (20s window is tight)
- Invisibility + Speed II allows flanks/escapes
- Counter-play: run away for 20s
- Does NOT prevent oneshots — if a Golden Blade hits you during Ruby, you still die

---

## 6. La Falce (Scythe) — Ultimate Artefact

The **endgame weapon**. Merges all three artefacts — closes the pool forever. Kill someone with it and they're **imprisoned in spectator mode**.

### Crafting
- **Recipe:** Golden Blade + Thunder Crossbow + Riptent in **any crafting table** (shapeless)
- **Result:** One La Falce. All 3 artefacts consumed.
- **Pool closes permanently** — no more milestone rewards
- **Global announcement** on craft

| Property | Value |
|---|---|
| Base Item | Mace (custom texture) |
| Sharpness | X (+12.5 melee damage) |
| Attack Speed | Diamond Sword (1.6) |
| Unbreakable | ✅ Yes |
| Drop on Death | ✅ Yes (glowing effect) |

### Upward Dash
- **Right-click** launches wielder **~20 blocks upward**
- **No cooldown** — chain dashes allowed
- **No fall damage** during or after dash
- Mace smash bonus fully applies (bonus damage from height)

### Spectator Imprisonment
- Victim → **Spectator Mode**
- **Inventory cleared** (items dropped at death location)
- **Multiple victims** stack (can imprison several players)
- Victim can still chat

### Wielder Death
- All imprisoned spectators **released** with **full inventory restored**
- Scythe drops (glowing) for **anyone** to claim
- New wielder inherits all current prisoners

### 30-Minute Reset Timer
- **Trigger:** Wielder dies AND nobody picks up Scythe within **30 minutes**
- **Visual:** Boss bar displayed to all players with countdown
- **On expiry:** Full reset — pool restored, kill scores zero, Scythe despawns
- **Cancels if** someone claims the Scythe during the timer

---

## 7. World Border Progression

| Day | Radius (from center ±) | Total Size |
|---|---|---|
| Start (Day 0) | 1,000 blocks | 2,000 × 2,000 |
| Day 1 | 1,001,000 | 2,002,000 × 2,002,000 |
| Day 2 | 2,001,000 | 4,002,000 × 4,002,000 |
| Day 3 | **2,500,000** | **5,000,000 × 5,000,000** (cap) |

| Property | Value |
|---|---|
| **Growth Rate** | +1,000,000 blocks per side per day |
| **Event** | Server dawn (every Minecraft day) |
| **Cap** | 5,000,000 × 5,000,000 total |
| **Dimension** | Overworld only (Nether scaled 1:8) |

Growth announcement on every expansion event. Standard Minecraft border damage outside.

---

## 8. `/prime` Command

**Syntax:** `/prime` (all players) | `/prime admin` (admin only)

### Normal Output
```
=== Artefact Pool ===
Available: 2     Claimed: 1     Phase: Normal

Claimed Weapons:
  Golden Blade - held by Steve

Available Weapons:
  Thunder Crossbow
  Riptent

Milestone Reward: Weapon (next at 15 kills)
World Border: ±1,000 blocks (growing +1M/day)
```

### Variations
- **All claimed:** Shows Ruby as milestone reward
- **Scythe forged:** Shows Scythe holder, pool closed
- **Reset timer:** Shows countdown, "Scythe Lost" phase

---

## 9. Copper Grounding

| Property | Value |
|---|---|
| Diverting Material | Copper Block (any variant) |
| **Effective Radius** | **100 blocks** from lightning impact |
| **Diversion Chance** | **33%** per strike |
| Diversion Target | Nearest copper block within radius |
| Stacking | No — more copper doesn't increase chance |
| Feedback | Lightning visually diverts, small explosion, zap sound |

### Usage
- Just place copper blocks anywhere — no special wiring
- Build copper roofs over your base = partial protection
- Vanilla oxidation/waxing doesn't affect grounding
- Opponents can break your copper blocks to weaken defenses
- Pure luck mechanic — adds tension, not certainty

---

## 10. Plugin Data Structure (YAML)

```yaml
version: 2
config:
  kill_threshold: 15
  ruby_max_stack: 16
  ruby_buff_duration_seconds: 20
  ruby_cooldown_seconds: 60
  golden_blade:
    throw_cooldown_seconds: 20
    oneshot: true
  thunder_crossbow:
    reload_ticks: 16
    lightning_damage: 10
    lightning_radius: 3
    copper_divert_chance: 0.33
    copper_radius: 100
  riptent:
    riptide_level: 3
    on_land: true
    no_fall_damage: true
  scythe:
    sharpness: 10
    dash_height: 20
    spectator_reset_minutes: 30
  world_border:
    initial_radius: 1000
    daily_growth: 1000000
    max_radius: 2500000

players:
  <uuid>:
    kills: 0
    weapons: []
    rubies: 0
    ruby_cooldown_until: null

weapon_pool:
  available: []
  claimed: []
  phase: "normal"

scythe:
  holder_uuid: null
  prisoners: []
  reset_timer_start: null
```

---

## Edge Case Catalog

| Edge Case | Handling |
|---|---|
| Two players hit +15 simultaneously | Server sequential. One gets weapon, other gets next available. |
| Player dies with +14, respawns, gets +15 | Works fine. Milestone checks every kill event. |
| Server restart during Scythe timer | Timer pauses. Resumes on server start. |
| Scythe lost in unloaded chunk | Timer doesn't start until admin confirms. `/artefact reset` fallback. |
| Imprisoned player disconnects | Still in spectator on rejoin. Released on wielder death/timer. |
| Golden Blade throw in void | Loyalty returns before despawn (vanilla behavior). |
| Riptiding into lava | Standard lava damage. Riptide doesn't grant fire resistance. |
| Thunder Crossbow in rain | No change (already summoning lightning). |
| Admin commands | `/artefact reset` — force reset. `/artefact scythe give <player>`. |

---

*This document supersedes all prior notes on the artefact system design. All decisions documented here reflect Joel's final design direction as of 2026-06-07.*
