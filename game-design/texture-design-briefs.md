# Texture Design Briefs — Artefact Survival SMP

> **For the asset artist** — these briefs describe the creative direction for each texture. All PNGs go into `resource-pack/assets/minecraft/textures/artefact/` and should be 16×16 unless noted.

---

## 1. Golden Blade — "Godslayer's Mercy"

**Vibe:** Royal, divine, lethal. A weapon that feels like it belongs in a god's hand. Golden sunburst meets cold steel.

**Color Palette:**
- Primary: `#FFD700` (gold) — warm, rich, not pale
- Secondary: `#FF8C00` (dark orange) — edge glow
- Accent: `#FFFFFF` (white) — core highlight
- Shadow: `#8B6914` (dark gold) — depth
- Details: `#00BFFF` (deep sky blue) — tiny sapphire inlays, 2-3 pixels

**Shape Language (16×16 Inventory Icon):**
- Silhouette: long trident head with 3 prongs
- Center prong is extended, dangerous — like a longsword blade
- Side prongs curve outward like wings
- Crossguard has a small blue gem inset
- Shaft is golden with wrapped grip texture (diagonal lines, 2px spacing)
- Glow effect: 1px white highlight along the top edge of the blade

**Held Model (thunder_trident_hand.png):**
- Same design at a slight angle (trident held-pose rotation)
- Blade extends upward ~60% of the frame
- Grip at bottom ~30%
- Throwing variant: same, rotated to horizontal

**Unique Visual FX (plugin handles these, but texture should support them):**
- Golden particles trail on throw (plugin particle, not texture)
- The texture should have enough brightness contrast that enchanted glint overlay looks good

**Reference Mood:** Zeus's lightning bolt as a melee weapon. Think god-killer spear from mythology.

---

## 2. Thunder Crossbow — "Storm's Fury"

**Vibe:** Cracking with electricity, volatile, industrial. This isn't a hunter's tool — it's a siege weapon you can carry.

**Color Palette:**
- Primary: `#4A4A4A` (dark steel) — main body
- Secondary: `#1E90FF` (electric blue) — energy coils
- Accent: `#FFD700` (gold) — trim/rivets
- Glow: `#00FFFF` (cyan) — charged segments
- Shadow: `#1A1A1A` (near black) — crevices

**Shape Language (16×16):**
- Base crossbow silhouette but heavier — thicker stock, reinforced
- The "string" is actually a glowing energy line (cyan, 1px thick)
- Two copper coil rings around the front barrel (orange/copper tones)
- Small lightning bolt symbol on the side (1-2 pixels, cyan)
- Charging handle is gold-tipped

**Animation States (separate sprites):**
1. **Standby** — coils dark, energy line dim
2. **Pulling 0** — energy line starts glowing, small sparks at the edges
3. **Pulling 1** — coils pulse brighter, sparks intensify
4. **Pulling 2** — fully charged, white-hot core, visible glow bleed
5. **Loaded (arrow)** — energy bolt nocked, blue-white glow
6. **Loaded (firework)** — for rocket crossbow use (if needed)

**Reference Mood:** A railgun crossbow from sci-fi fantasy. Think Dawnguard crossbow from Skyrim but electrified.

---

## 3. Riptent — "Tide's Embrace"

**Vibe:** Flowing, aquatic, graceful. Moving water frozen into weapon form. This is Poseidon's backup trident — elegant but deadly.

**Color Palette:**
- Primary: `#00CED1` (dark turquoise) — blade
- Secondary: `#E0FFFF` (light cyan) — water surface highlights
- Accent: `#006400` (dark green) — deep ocean tones
- Glow: `#7FFFD4` (aquamarine) — inner glow
- Shaft: `#2F4F4F` (dark slate gray) — weathered metal

**Shape Language (16×16):**
- Trident base shape but more organic — prongs curve like waves
- Less rigid than Golden Blade, more flowing lines
- 3 prongs with wave-like curves connecting them
- Center has a droplet-shaped gap (not solid)
- Shaft looks like coral-encrusted metal
- Small bubble details (1px circles) around the head

**Held Model:**
- Same flowing design
- The trident held angle should show the wave curves
- Slight transparency feel — the blue should be bright enough to suggest water

**Reference Mood:** Trident made of solidified sea foam. Think Atlantean weapon from fantasy — elegant, fluid, ancient.

---

## 4. La Falce (Scythe) — "The Final Cut"

**Vibe:** Dark, oppressive, inevitable. This is the weapon that ends the game. It should feel dangerous just sitting in your inventory.

**Color Palette:**
- Primary: `#2B0000` (blood dark) — blade
- Secondary: `#8B0000` (dark red) — blade edge
- Accent: `#FF4444` (bright red) — pulsing rune glow
- Metal: `#1C1C1C` (near black) — handle & mechanism
- Trim: `#A0522D` (sienna) — leather-wrapped grip

**Shape Language (16×16):**
- Mace base but completely reimagined
- The "mace head" is replaced by a curved scythe blade (crescent moon shape)
- Blade has a razor-thin edge (1px white highlight on curve)
- Runic symbols carved into the flat of the blade (dark red glow, 2-3 pixels)
- Handle is wrapped in dark leather (cross-hatch texture)
- A small chain dangles from the pommel (1-2 pixels)
- The overall silhouette should be distinctly different from a mace

**Held Model:**
- Scythe held at a dramatic angle
- Blade curve visible over the wielder's shoulder (classic scythe pose)
- The red glow should be visible even in the held view

**Unique Visual FX (plugin):**
- Red particle trail on swing
- Screen flash on kill
- Prisoners see a red tint (server-side)
- The texture should be dark enough that particles pop against it

**Reference Mood:** Death's own weapon. Think the Scythe from Soul Eater or Darksiders — oversized, menacing, alive with dark energy.

---

## 5. Ruby — "Blood Drop"

**Vibe:** Precious, volatile, condensed power. A gem that burns from within. Small but significant.

**Color Palette:**
- Primary: `#DC143C` (crimson) — main body
- Secondary: `#FF1493` (deep pink) — internal glow
- Accent: `#FFD700` (gold) — setting/crown
- Shadow: `#4A0000` (dark red) — facets
- Highlight: `#FFFFFF` (white) — specular catchlight

**Shape Aviation (16×16):**
- Emerald-cut gem (octagonal face) — faceted, geometric
- 3 main facets visible (top face, left face, right face)
- White catchlight in top-left quadrant (1-2 pixels)
- Internal glow radiating from center (slightly lighter red)
- Gold bezel setting around the gem (1px thick)
- Sitting on a small gold nub (the "ring" part of echo_shard's shape)

**No Held Model:**
- Ruby uses echo_shard's existing item-display model
- It appears in the inventory and hotbar — no third-person held view needed
- The texture should look good at 16×16 since it's a small item

**Variant (Optional):**
- If Rubies are stackable, the stacked icon remains the same (Minecraft handles stack rendering)

**Reference Mood:** The Heart of the Ocean from Titanic but fantasy. A droplet of congealed power. The One Ring if it were a ruby.

---

## Notes for the Artist

### Technical Constraints
- **Resolution:** Strictly 16×16 pixels per layer
- **Format:** PNG, no transparency banding
- **File size:** Keep under 5KB per texture
- **Naming:** Use lowercase `snake_case` as shown in the directory structure

### Style Guide
- **No realistic shading** — use flat pixel art with 3-4 shades per color
- **No anti-aliasing** — sharp edges, 1px details
- **Glow effects:** Use the brightest color + 1px bleed for glow
- **Trident base items** (Golden Blade, Riptent): Texture must map to the trident model's UV. The trident texture is a tall thin strip. Verify against the vanilla `trident.png` UV layout.
- **Crossbow base** (Thunder Crossbow): Multiple sprite states. Each state is a separate 16×16.
- **Mace base** (La Falce): Single sprite for the item + mace held model uses a different projection.

### Texture UV References
Check the following vanilla textures for UV layout before starting:
- `assets/minecraft/textures/entity/trident.png` — trident item uses entity texture, not a standard item
- `assets/minecraft/textures/item/crossbow*.png` — crossbow states
- `assets/minecraft/textures/item/mace.png` — mace item
- `assets/minecraft/textures/item/echo_shard.png` — current/shard

> For trident-based items (Golden Blade, Riptent), note that the trident's item texture is pulled from `entity/trident.png`, not from `item/`. The custom model needs its own texture location. Our approach uses custom model JSONs that point to textures in `item/` folder, so they decouple from the entity texture. This means the artist should create standalone 16×16 item textures, not entity textures.
