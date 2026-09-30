# Neon Descent

A first-person roguelite shooter with an arcade feel. It's a single HTML file with no build step.

There are two worlds, picked with the **World** button on the title screen:

- **Neon City**. A cyberpunk megacity at night. You fight robots and androids with energy guns and a mono-katana.
- **Iron Keep**. A castle overrun by goblins and orcs. You fight with a crossbow, black-powder guns, a storm staff and a knight's sword. The boss is King Grakk on his flying throne.

There are also two looks, switchable any time under **Settings → Graphics**. Each world has its own version of both looks:

- **Pong (1972)**, the default. A black screen, glowing white vector lines, square "Pong ball" shots, pixel fonts, and every enemy drawn as an animated stick figure. Iron Keep gets its own stick figures: goblins with big ears, orcs with tusks, a hooded shaman and a horned warlord with a hammer.
- **Arcade**. Full-colour low-poly 3D with bloom and motion trails.
  - Neon City is a WipEout Fury–style city of lit tower blocks, neon signs, hazard stripes and billboards. The menus and HUD are flat colour blocks with wide type and cut corners.
  - Iron Keep has stone walls with battlements, torches, heraldic banners and mountains. It uses gilded serif menus.

## Play

Open `index.html` in a desktop browser (Chrome, Edge, Firefox). You need an internet connection because Three.js and the fonts load from CDNs. If your browser blocks ES modules from `file://`, serve the folder instead:

```sh
npx serve neon-descent      # or: python3 -m http.server -d neon-descent
```

Click **Start run**. The game locks the mouse pointer. Press `Esc` to pause.

| Key | Action |
| --- | --- |
| `W` `A` `S` `D` | Move |
| Mouse | Aim. Left button fires |
| `F`, `V` or right mouse button | Melee swing |
| `Space` | Jump onto crates and over shockwaves. Extra jumps with Grav Boots |
| `Shift` | Dash, with brief invulnerability |
| `1`–`5`, wheel, `Q` | Switch weapon (`Q` swaps to your last weapon) |
| `Esc` / `P` | Pause |
| `M` | Mute |

It also plays on phones and tablets. Tap **Start run** with a finger and the game switches to touch controls. Landscape works best, but portrait is supported too.

| Touch | Action |
| --- | --- |
| Left thumb, anywhere on the left half | Floating move stick |
| Right thumb, anywhere on the right half | Aim |
| **FIRE** | Hold to shoot. Slide your thumb on it to aim while firing |
| **MELEE** | Swing your blade |
| **JUMP** / **DASH** | Jump and dash. The dash button fills as it recharges |
| Weapon button | Tap to cycle weapons |
| **II** | Pause |

On touch, aim assist nudges your sights toward the nearest enemy, and auto-fire shoots when your sights lock on. You can turn either off in Settings.

## Melee

The blade swings in a wide arc in front of you and hits every enemy in it for heavy damage, with knockback. If an enemy is just out of reach in front of you, the swing lunges you toward it. The swing also knocks enemy shots out of the air, so a well-timed slash parries a volley. The **Vibro Edge** augment (called **Vorpal Edge** in Iron Keep) adds damage and heals you on melee kills.

## The loop

- Each floor is generated from a seed: rooms joined by corridors, with pillars, crates and baked lighting.
- Walking into a sealed room locks it behind a barrier and brings in waves of enemies. Clear every room to open the exit portal.
- Each portal leads to a choice of 1 of 3 upgrades (23 perks in common, rare and legendary tiers, plus weapon unlocks). They're called augments in Neon City and boons in Iron Keep.
- Every fifth floor is a boss fight, and the district changes after it:
  - Neon City: Neon Strip, Holo Market, Corporate Spire, Undercity.
  - Iron Keep: Goblin Warrens, Iron Keep, Orc Warcamp, Witchfire Crypt.
- Scrap (Neon City) or gold (Iron Keep) is kept when you die. Spend it in the Workshop or Armoury on permanent upgrades: more HP, more damage, starting weapons, an extra dash, rerolls and a revive. The two worlds share one bank and one set of upgrades.

Arcade features: a score multiplier from kill chains, multi-kill callouts, weak-point crits, hit-stop, slow-mo on the last kill in a room, floating damage numbers and a local high-score table.

### Arsenal

| Slot | Neon City | Iron Keep | Notes |
| --- | --- | --- | --- |
| 1 | M-9 Blaster | Hand Crossbow | Infinite ammo |
| 2 | Scattergun | Blunderbuss | Shotgun spread, big knockback |
| 3 | Pulse Rifle | Repeater | Full auto |
| 4 | Hammer RL | Bombard | Splash damage. You can blast-jump |
| 5 | Volt Rail | Storm Staff | Pierces every enemy in a line |
| Melee | Mono-Katana | Knight's Sword | Parries shots |

### Hostiles

| Role | Neon City | Iron Keep |
| --- | --- | --- |
| Fast melee lunger | Spider Drone | Goblin Cutthroat |
| Runs at you and explodes (chain-reacts) | Mine Bot | Goblin Sapper with a powder keg |
| Burst fire | Android Trooper | Orc Archer with fire arrows |
| Flying spread shot | Hover Drone | Goblin Shaman |
| Sniper (the laser locks white just before it fires) | Android Marksman | Goblin Sharpshooter on stilts |
| Floor-slam shockwave (jump it) | Heavy Mech | Orc Warlord |
| Two-phase boss | The Warden | King Grakk |

Elites glow gold.

## How the looks work

Pong mode:

- Levels are drawn as black occluding shapes with white edges traced from the tile grid, plus a dot-grid floor and a dashed Pong "net" down each room.
- Enemies are flat stick figures that always turn to face you, with walk, flap, scuttle and slam animations. They shatter into white sticks when killed. The bosses are vector-outline 3D models with a drawn-on face.
- The final pass converts everything to monochrome phosphor with 6-level dithering, a soft CRT glow and heavier scanlines.

Arcade mode:

- The scene renders to a low-resolution buffer (360p by default) and is upscaled with sharp or soft filtering.
- World lighting is baked into vertex colours (Gouraud style, with 2× overbright modulation and grid-traced shadows). Up to 12 dynamic point lights for muzzle flashes, projectiles, explosions and torches are added in the shader.
- Neon City raises the walls into towers of different heights with lit windows (an emissive texture), and adds vertical blade signs, billboards, floor decals and a skyline. Iron Keep adds battlements, flickering torches, banners and a mountain skyline.
- Post-processing adds bloom, frame-blend motion trails, a little chromatic aberration, a vignette and 15-bit colour with a Bayer dither. An optional CSS scanline overlay sits on top.
- Textures are painted at startup on small canvases. All sound effects and both soundtracks (synthwave for Neon City; plucked lute arpeggios and war drums for Iron Keep) are generated live with the Web Audio API.

Settings (world, graphics, sensitivity, FOV, volume, render resolution, trails, scanlines, shake, invert Y, touch auto-fire and aim assist) and upgrade progress are saved in `localStorage`.

For tinkering, the page exposes `window.__neon` in the dev console. For example, `__neon.giveWeapon('rail')` or `__neon.player.hp = 999`.
