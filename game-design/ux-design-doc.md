# UX Design Document — Artefact Survival SMP

> **For and3v (plugin implementation) and Joel (design sign-off)**
> This document defines how the artefact system communicates with players — all chat messages, UI patterns, and the onboarding flow.

---

## 1. Messaging Tone Guide

Every server message uses a **consistent voice** — epic, clear, slightly dramatic. Never robotic. Never spammy.

### Tone Rules
1. **Colored labels** — Use `&6` (gold) for mechanic names, `&e` (yellow) for player names, `&a` (green) for positive, `&c` (red) for danger/death, `&7` (gray) for descriptive text
2. **Visual separators** — `&8&m---` for section dividers in longer messages
3. **No abbreviations** — write "you have claimed" not "u claimed"
4. **Sound integration** — every important message gets a matching sound effect

### Prefix Convention
All artefact system messages use a consistent prefix icon:

| Context | Prefix | Color | Example |
|---|---|---|---|
| Kill milestone | ⚔ (U+2694) | Gold `&6` | `&6⚔ &ePlayer &7reached 15 kills!` |
| Weapon claim | ⚔ (U+2694) + weapon emoji | Gold `&6` | `&6⚔ &ePlayer &7has claimed the &6Golden Blade&7!` |
| Weapon drop | 💀 (U+1F480) | Red `&c` | `&c💀 &ePlayer &7dropped the &6Thunder Crossbow&7!` |
| Scythe craft | ☠ (U+2620) | Dark Red `&4` | `&4☠ &ePlayer &7has forged &4La Falce&7!` |
| Scythe kill | 💀 (U+1F480) | Dark Red `&4` | `&4💀 &ePlayer &7was slain by &4La Falce&7!` |
| Scythe reset | ⚡ (U+26A1) | Gold `&6` | `&6⚡ &eThe cycle begins anew!` |
| World border | 🌍 (U+1F30D) | Green `&a` | `&a🌍 &7World border expands to ±1M!` |
| Pickup | ✨ (U+2728) | Aqua `&b` | `&b✨ &ePlayer &7picked up &6Golden Blade&7!` |

---

## 2. In-Game Message Catalog

### 2.1 Kill Milestone Messages

**Reaching +15 kills (weapon available):**
```
&6⚔ &e{player} &7has proven their might! The artefacts recognize you.
&7You have been granted: &6{weapon_name}
&7Type &e/prime &7to see all claimed weapons.
```
Sound: `BLOCK_END_PORTAL_SPAWN` (deep resonance)

**Reaching +15 kills (pool empty → Ruby):**
```
&6⚔ &e{player} &7has proven their might! The pool is empty.
&7You receive: &d❤ Ruby &7(a single-use power surge)
&7Right-click to activate for 20s.
```
Sound: `BLOCK_AMETHYST_CLUSTER_BREAK` (crystal shatter)

**Reaching +30, +45, etc.:**
```
&6⚔ &e{player} &7surpasses &e{kills} kills&7!
```
Sound: `ENTITY_PLAYER_LEVELUP` (quieter, softer)

### 2.2 Weapon Claim & Drop

**A player picks up a dropped artefact:**
```
&b✨ &e{player} &7claimed the &6{weapon_name}&7!
```
Sound: `ENTITY_ITEM_PICKUP` (higher pitch)

**Weapon drops on death:**
```
&c💀 &e{player} &7dropped &6{weapon_name}&7!
&7Any player can claim it.
```
Sound: none (death sound already plays)

### 2.3 Scythe Messages

**Scythe forged:**
```
&4☠ &e{player} &7has forged &4La Falce&7!
&7The age of artefacts has ended.
&8&m----------------------------------------
&4☠ &7On kill: &cSpectator Imprisonment
&4☠ &7On death: &cPrisoners freed, 30min reset timer
```
Sound: `ENTITY_WITHER_SPAWN` (distant, ominous)

**Scythe kill:**
```
&4💀 &e{victim} &7was imprisoned by &4La Falce&7!
&7{prisoner_count} souls currently bound.
```
Sound: `ENTITY_LIGHTNING_BOLT_THUNDER`

**Scythe wielder dies:**
```
&4☠ &e{wielder} &7has fallen! The imprisoned are freed!
&4☠ &7La Falce lies unclaimed...
&6⏳ &7If unclaimed in &e30 minutes&7, the world resets.
```
Sound: `ENTITY_WITHER_DEATH` (defeat tone)

**Reset timer boss bar:**
```
&4☠ The Scythe lies unclaimed... &e{MM:SS} remaining
```
Sound (at 5min left): `BLOCK_NOTE_BLOCK_PLING` (urgent, repeating)
Sound (at 1min left): `ENTITY_EXPERIENCE_ORB_PICKUP` (rapid)

**Game reset:**
```
&6⚡ &eThe cycle begins anew!
&7Artefacts have returned to the world.
&7Kill scores reset. Go forth.
```
Sound: `ENTITY_DRAGON_FIREBALL_EXPLODE` (cataclysmic reboot)

### 2.4 World Border Expansion

```
&a🌍 &7World border expands!
&7Now &e±{radius} &7blocks from center
```
Sound: `BLOCK_BEACON_ACTIVATE` (ascending tone)

### 2.5 Copper Grounding Feedback

**Lightning diverted:**
```
&b⚡ &7Lightning diverted by copper grounding!
```
Sound: `BLOCK_COPPER_HIT` (metallic ping, only for the shooter)

**Lightning hit (no diversion):**
No message — damage speaks for itself.

---

## 3. Cooldown & Status Indicators

### 3.1 Golden Blade Throw Cooldown (20s)

| State | Indicator |
|---|---|
| Ready to throw | Normal item appearance |
| On cooldown (1-20s) | **Action Bar:** `&6⚔ Golden Blade ready in &e{seconds}s` — shown only to wielder |
| Ready notification | **Action Bar:** `&6⚔ &eGolden Blade ready!` — shown when cooldown expires |
| Cooldown sync | On server restart: reset cooldowns (they're short enough) |

### 3.2 Ruby Buff Timer

| State | Indicator |
|---|---|
| Duration remaining | **Action Bar:** `&d❤ Ruby buff: &e{seconds}s remaining` — 20s countdown |
| Cooldown (post-use) | **Action Bar:** `&d❤ Ruby cooldown: &e{seconds}s` — 60s countdown |
| Buff expired | **Action Bar:** `&7Ruby buff faded.` |

### 3.3 Scythe Reset Timer

**Boss Bar** (visible to ALL players):
```
&4☠ La Falce — Unclaimed &7| &e{MM:SS} remaining
```
- Color: RED
- Style: SOLID
- Slowly depletes as time runs out
- Shows at 30min, disappears if Scythe is claimed, reappears if Scythe dropped again

### 3.4 XP Bar Usage (Alternative)

If action bar is too crowded, these can use the XP bar instead:
- **Golden Blade cooldown** — XP bar depletes from full to empty over 20s (blue)
- **Ruby buff** — XP bar depletes from full to empty over 20s (red via `setXpLevel`)
- **Ruby cooldown** — XP bar depletes over 60s (purple)

Proposal: **Use XP bar for Golden Blade cooldown**, **Action Bar for Ruby buff** (since it has more text). Action bar is our primary channel.

---

## 4. `/prime` Command UX

### Layout
The command output uses a **consistent column structure** so players can quickly scan:

```
&6=== &eArtefact Pool &6===
&7Phase: &aNormal
&7Available: &a2    &7Claimed: &a1

&6⚔ &fClaimed:
  &eGolden Blade &8- &7held by &a{player}
  &eThunder Crossbow &8- &7held by &a{player}
  &eRiptent &8- &7held by &a{player}

&6📦 &fAvailable:
  &7{weapons not yet claimed}

&6💎 &fNext Milestone:
  &7{weapon or Ruby} at &e15 kills
```

### Phase-Specific Headers

| Phase | Header Color | Header Text |
|---|---|---|
| Normal | Gold `&6` | `=== Artefact Pool ===` |
| All claimed | Gold `&6` | Same (Ruby listed as reward) |
| Scythe forged | Dark Red `&4` | `=== ☠ The Age of Artefacts has Ended ===` |
| Scythe reset | Dark Red `&4` | `=== ⏳ The World Holds Its Breath ===` |

### Response Latency
The command must respond instantly (sub-100ms). If the data is heavy, cache the pool state and update on change events rather than querying YAML on every `/prime` call.

---

## 5. Player Onboarding Flow

### First Join
New players need to understand the artefact system without reading a wiki. Message triggers:

| Trigger | Message | Timing |
|---|---|---|
| First join | `&6&lArtefact Survival SMP` | Once, on first spawn |
| | `&7Kill players to earn artefacts.` | |
| | `&7Every &e15 kills &7you unlock a mythical weapon.` | |
| | `&7First to collect all three can forge &4La Falce&7.` | |
| | `&e/prime &7to check the artefact pool.` | |
| +5 kills | `&6Tip: &7You're &e5/15 &7kills towards your first artefact.` | Once |
| First death | `&7You dropped any held artefacts — other players can claim them.` | Once |
| First weapon pick up | `&6&l{weapon_name} Acquired!` | Once (also shown in chat) |
| | `&7{weapon_tip}` | |
| Scythe crafted (any player) | (see scythe forge message above — visible to all) | Whenever |

### Weapon Tips (shown on first pickup)

| Weapon | Tip |
|---|---|
| Golden Blade | `&7Throw to oneshot. &e20s &7cooldown. Don't miss.` |
| Thunder Crossbow | `&7Fires lightning. &e33% &7can be grounded by copper.` |
| Riptent | `&7Riptide on land. No fall damage. Unlimited dashes.` |
| La Falce | `&4☠ &7Kills imprison in spectator. Die to free them. &e30min &7reset if lost.` |
| Ruby | `&7Right-click for &e20s &7of power. Single use. Use wisely.` |

### No Wiki Dependency
Everything a player needs to know is delivered **in-game, in context, at the right moment**. No external links required to play effectively.

---

## 6. Sound Effect Reference

| Event | Sound | Volume | Pitch |
|---|---|---|---|
| Milestone reached | `BLOCK_END_PORTAL_SPAWN` | 1.0 | 1.0 |
| Weapon claimed | `ENTITY_ITEM_PICKUP` | 1.0 | 1.5 |
| Weapon dropped | (death sound) | — | — |
| Scythe forged | `ENTITY_WITHER_SPAWN` | 0.6 | 0.8 |
| Scythe kill | `ENTITY_LIGHTNING_BOLT_THUNDER` | 0.8 | 1.0 |
| Scythe wielder death | `ENTITY_WITHER_DEATH` | 0.7 | 0.9 |
| Game reset | `ENTITY_DRAGON_FIREBALL_EXPLODE` | 1.0 | 0.7 |
| Copper divert | `BLOCK_COPPER_HIT` | 0.8 | 1.2 |
| Border expand | `BLOCK_BEACON_ACTIVATE` | 0.6 | 1.0 |
| Ruby activate | `BLOCK_AMETHYST_CLUSTER_BREAK` | 0.8 | 0.9 |
| Buck buff expiry | `ENTITY_PLAYER_BREATH` | 0.5 | 0.5 |
| 5min reset warning | `BLOCK_NOTE_BLOCK_PLING` | 1.0 | 0.5 (repeat every 10s) |
| 1min reset warning | `ENTITY_EXPERIENCE_ORB_PICKUP` | 1.0 | 1.0 (repeat every 2s) |

---

## 7. Accessibility Notes

- **Colorblind considerations:** Messages use icons + text labels, not color alone. Golden Blade isn't just "the gold one" — it's always named.
- **Chat volume:** No message spams more than once per trigger. Cooldown indicators replace chat spam.
- **Sound volume:** Critical sounds (Scythe events, game reset) use high volume. Status sounds (cooldown ready, tick) use low volume.
- **Action bar vs Boss bar:** Action bar is for per-player info. Boss bar is for global events everyone needs to see.
