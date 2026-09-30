# Neon Descent

A first-person roguelite shooter with an arcade feel and a PlayStation 2–era look. It's a single HTML file with no build step.

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
| `Space` | Jump onto crates and over shockwaves. Extra jumps with Grav Boots |
| `Shift` | Dash, with brief invulnerability |
| `1`–`5`, wheel, `Q` | Switch weapon (`Q` swaps to your last weapon) |
| `Esc` / `P` | Pause |
| `M` | Mute |

## The loop

- Each floor is generated from a seed: rooms joined by corridors, with pillars, crates and baked lighting.
- Walking into a red sector seals it with an energy barrier and warps in waves of enemies. Clear every sector to open the exit portal.
- Each portal leads to a choice of 1 of 3 **augments** (23 perks in common, rare and legendary tiers, plus weapon unlocks).
- Every fifth floor is a boss fight against **The Warden**, and the theme changes after it: Foundry, then Cryo Vault, Biohazard Labs, Void Core.
- **Scrap** is kept when you die. Spend it in the **Workshop** on permanent upgrades such as more HP, more damage, starting weapons, extra dash, rerolls and a revive.

Arcade features: a score multiplier from kill chains, multi-kill callouts, weak-point crits, hit-stop, slow-mo on the last kill in a sector, floating damage numbers and a local high-score table.

### Arsenal

M-9 Blaster (infinite ammo), Scattergun, Pulse Rifle, Hammer RL (you can rocket-jump) and Volt Rail (pierces every enemy in a line).

### Hostiles

Skitter (melee lunger), Tick (runs at you and explodes, and chain-reacts), Sentry (burst fire), Wisp (flying spread shot), Juggernaut (floor slam shockwave: jump it), Lancer (sniper: the laser locks white just before it fires) and The Warden (boss with two phases). Elites glow gold.

## How the PS2 look works

- The scene renders to a low-resolution buffer (360p by default) and is upscaled with sharp or soft filtering.
- World lighting is baked into vertex colours (Gouraud style, with 2× overbright modulation and grid-traced shadows). Up to 12 dynamic point lights for muzzle flashes, projectiles and explosions are added in the shader.
- Post-processing adds bloom, frame-blend motion trails, a little chromatic aberration, a vignette and 15-bit colour with a Bayer dither. An optional CSS scanline overlay sits on top.
- Textures are painted at startup on 64×64 canvases. All sound effects and the synthwave soundtrack are generated live with the Web Audio API.

Settings (sensitivity, FOV, volume, render resolution, trails, scanlines, shake, invert Y) and Workshop progress are saved in `localStorage`.

For tinkering, the page exposes `window.__neon` in the dev console. For example, `__neon.giveWeapon('rail')` or `__neon.player.hp = 999`.
