# Artefact Survival SMP — Resource Pack

## Overview

This resource pack provides custom item models and textures for the 5 mythical artefacts on the Artefact Survival SMP server. It overrides vanilla item models using the `custom_model_data` NBT component, set by the server-side Paper plugin.

## Custom Model Data IDs (for and3v)

These IDs must match what the plugin writes to `ItemStack#editMeta` → `setCustomModelData(id)`.

| Item | Base Vanilla Item | Custom Model Data ID |
|---|---|---|
| Golden Blade | `minecraft:trident` | `1001` |
| Thunder Crossbow | `minecraft:crossbow` | `1002` |
| Riptent | `minecraft:trident` | `1003` |
| La Falce (Scythe) | `minecraft:mace` | `1004` |
| Ruby | `minecraft:echo_shard` | `1005` |

> **Why `echo_shard` for Ruby?** It's the closest vanilla item to a gem shape — faceted, handheld, recognizable silhouette. Using `custom_model_data` on it means Ruby inherits echo_shard's existing behavior (can't eat/place/wear) and we only override the texture.

## Directory Structure

```
resource-pack/
├── pack.mcmeta                  # Pack metadata
├── pack.png                     # Server icon (TODO)
│
└── assets/
    └── minecraft/
        ├── models/
        │   └── item/
        │       ├── trident.json           # Override → golden_blade / riptent
        │       ├── crossbow.json          # Override → thunder_crossbow
        │       ├── mace.json              # Override → la_falce
        │       └── echo_shard.json         # Override → ruby
        │
        └── textures/
            └── artefact/                  # All custom textures in a namespace folder
                ├── golden_blade.png       # 16×16 inventory icon
                ├── golden_blade_hand.png  # Held model texture
                ├── thunder_crossbow.png
                ├── thunder_crossbow_hand.png
                ├── riptent.png
                ├── riptent_hand.png
                ├── la_falce.png
                ├── la_falce_hand.png
                └── ruby.png
```

## For the Artist

1. All textures go in `assets/minecraft/textures/artefact/`
2. Each item needs an **inventory icon** (16×16 PNG, standard GUI slot scale)
3. Held items (trident, crossbow, mace) need a **hand texture** (wider frame, ~32×16 or 16×16 depending on the held perspective)
4. Trident-based items (Golden Blade, Riptent) should match the trident held-pose — long vertical weapon
5. Crossbow-based item (Thunder Crossbow) should match crossbow held-pose — horizontal, two-handed
6. Mace-based item (La Falce) should match mace held-pose — one-handed, held upward
7. Ruby uses echo_shard's existing model shape — just the gem face texture needs replacing

## Installation

1. Drop the entire `resource-pack/` directory into Paper server's config as the resource pack
2. Configure `server.properties`: `resource-pack=https://...`, `require-resource-pack=true`
3. Players auto-download on join

## Dependencies

- This pack requires **Minecraft 1.21+** (pack format 34+)
- The Paper plugin must set `custom_model_data` on items using the IDs above
- Vanilla resource pack must be enabled (no OptiFine-only features used — pure JSON)
