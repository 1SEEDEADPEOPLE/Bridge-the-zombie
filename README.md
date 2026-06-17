# Last Bridge

A single-file HTML5 zombie-horde lane shooter — squad-builder runner inspired by mobile "Last Z"-style ads. Drag left/right to steer your squad up a bridge, smash through weapon gates on the left, mow down a nonstop zombie horde on the right, and survive the boss at the end.

No build step, no dependencies, no install. It's one HTML file with embedded CSS and JavaScript, rendered on an HTML5 canvas.

## Play it

Open `index.html` in any modern browser (desktop or mobile), or play the hosted version once GitHub Pages is enabled (see below).

**Controls:** touch-drag or click-drag left/right to steer the squad. There's no other input — the squad fires automatically.

## Features

- Perspective-scrolling bridge with a physical median splitting a left "weapon gate" lane from a right "zombie horde" lane
- 6 weapons (Pistol, Machine Gun, Shotgun, Dual SMG, Bazooka, Flamethrower), each with a distinct projectile, fire rate, and status effect (knockback + stun, explosive splash + throw, burn damage-over-time + fire spread)
- Randomized weapon-gate sequence each run — gates show exactly which weapon they grant, so you choose your loadout instead of following a fixed path
- Continuous, never-stopping zombie horde that scales in count/HP/speed with distance traveled
- Boss fight whose HP is computed live from your actual DPS at spawn time, so the fight is a fair ~16s race regardless of how strong your run got
- Comic/Borderlands-inspired visual style: bold ink outlines, flat cel-shaded colors, halftone texture, jagged character silhouettes
- HUD with current weapon, weapon-tier progress, squad "lives" bar, and boss health bar

## Project structure

```
.
├── index.html   # the entire game — HTML, CSS, and JS in one file
├── README.md
└── LICENSE
```

There's intentionally no build tooling (no npm, no bundler). Everything needed to run or edit the game is in `index.html`; all tunable values (weapon stats, zombie scaling, spawn rates, etc.) live in the `CONFIG` object near the top of the `<script>` block.

## Run locally

Just open the file:

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
open index.html        # macOS
# or: start index.html   # Windows
# or: xdg-open index.html  # Linux
```

Some browsers restrict canvas/touch features when loading `file://` paths directly. If anything seems off, serve it locally instead:

```bash
python3 -m http.server 8000
# then open http://localhost:8000 in your browser
```

## Deploy with GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under "Build and deployment", set **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`.
4. Save. GitHub will publish at `https://<your-username>.github.io/<your-repo>/`.

Because the game is already named `index.html` at the repo root, no further configuration is needed.

## Tuning the game

Open `index.html` and find the `CONFIG` object near the top of the script. Notable groups:

- `weapons` — per-weapon damage, fire rate, spread, and special-effect parameters
- `zombieSpawnInterval*`, `zombieRowCount*`, `zombieHpPerDistance`, `zombieSpeedPerDistance` — horde pacing and difficulty ramp
- `bossFightSeconds`, `bossMinHP` — boss balancing (HP is computed live from squad DPS, not fixed)
- `formationHalfWidth`, `laneLimit`, `dragSensitivity` — squad movement and formation footprint

## License

MIT — see [LICENSE](LICENSE).
