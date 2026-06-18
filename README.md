# Last Bridge

A comic-sketch-style zombie horde lane shooter — squad-builder runner inspired by mobile "Last Z"-style ads. Drag left/right to steer your squad up a bridge, smash through weapon gates on the left, mow down a nonstop zombie horde on the right, and survive the boss at the end.

No build step, no framework, no install. The game is one HTML file with embedded CSS and JavaScript, rendered on an HTML5 canvas. It's also an installable PWA — add it to your home screen and it runs full-screen, offline-capable, like a native app.

## Play it

Open `index.html` in any modern browser (desktop or mobile), or play the hosted version once GitHub Pages is enabled (see below). On mobile, your browser will offer to "Add to Home Screen" — once installed it opens without browser chrome.

**Controls:** touch-drag or click-drag left/right to steer the squad. There's no other input — the squad fires automatically. Tap the speaker icon (top right) to mute/unmute.

## Features

- Perspective-scrolling bridge with a physical median splitting a left "weapon gate" lane from a right "zombie horde" lane, with solid walls down both edges
- 6 weapons (Pistol, Machine Gun, Shotgun, Dual SMG, Bazooka, Flamethrower), each with a distinct hand-drawn projectile, fire rate, and status effect:
  - **Shotgun** — knocks zombies back and stuns a whole cluster for 1s
  - **Bazooka** — explosive splash damage that throws zombies into a tumbling daze, then staggers them at reduced speed
  - **Flamethrower** — wide fire cone with heavy burn damage-over-time that spreads to nearby zombies
- Weapon leveling: picking up a weapon you already have stacks a +25% damage bonus (up to 5 levels) instead of doing nothing — gates show "NEW WEAPON" vs "LV UP +25% DMG" so the payoff is visible before you commit
- Randomized weapon-gate sequence each run — gates show exactly which weapon they grant, so you choose your loadout; gates keep trickling in even during the boss fight
- Continuous, never-stopping zombie horde that scales in count/HP/speed with distance traveled, with multiple zombie body-type variants for visual variety
- Boss fight whose HP is computed live from your actual DPS (including weapon level) at spawn time, so the fight is a fair ~16s race regardless of how strong your run got
- Comic/sketch visual style: jittered hand-inked outlines, cross-hatch shading, paper-grain texture, halftone dots, and comic-marker fonts (Bangers / Permanent Marker) throughout — UI included
- Procedural sound effects (WebAudio — no audio files) for firing, gate breaks, weapon level-ups, explosions, zombie deaths, boss hits, and win/lose, with a mute toggle
- HUD with current weapon + level, weapon-tier progress, squad "lives" bar (with a running total shown on every pickup), and boss health bar
- Installable PWA: manifest, service worker (offline caching), and a full icon set

## Project structure

```
.
├── index.html       # the entire game — HTML, CSS, and JS in one file
├── manifest.json    # PWA manifest
├── sw.js            # service worker (offline caching)
├── icons/           # app icons (see below)
├── README.md
└── LICENSE
```

There's intentionally no build tooling (no npm, no bundler). Everything needed to run or edit the game is in `index.html`; all tunable values (weapon stats, zombie scaling, spawn rates, weapon-leveling bonus, etc.) live in the `CONFIG` object near the top of the `<script>` block.

### Icons

| File | Size | Purpose |
|---|---|---|
| `icons/icon-192.png` | 192×192 | Standard app icon |
| `icons/icon-512.png` | 512×512 | Standard app icon |
| `icons/icon-maskable-192.png` | 192×192 | Maskable (full-bleed, safe-zone aware) |
| `icons/icon-maskable-512.png` | 512×512 | Maskable (full-bleed, safe-zone aware) |
| `icons/apple-touch-icon.png` | 180×180 | iOS home screen icon |
| `icons/favicon-32.png`, `favicon-16.png`, `favicon.ico` | — | Browser tab favicon |

All icons were generated programmatically (a simple script using Pillow) — there are no external image assets, so the repo has zero binary asset licensing to worry about. Regenerate or swap them any time; just keep the same filenames/sizes referenced in `manifest.json` and the `<head>` of `index.html`.

## Run locally

Just open the file:

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
open index.html        # macOS
# or: start index.html   # Windows
# or: xdg-open index.html  # Linux
```

PWA features (service worker, install prompt) require serving over `http(s)://`, not `file://`. Serve it locally for the full experience:

```bash
python3 -m http.server 8000
# then open http://localhost:8000 in your browser
```

## Deploy with GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under "Build and deployment", set **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`.
4. Save. GitHub will publish at `https://<your-username>.github.io/<your-repo>/`.

All paths (manifest, service worker, icons, font CDN) are relative, so this works correctly whether the repo is served from a domain root or a GitHub Pages project subpath.

Once live over HTTPS, visiting on a phone will offer "Add to Home Screen" (Android/Chrome shows an automatic install prompt; iOS/Safari requires using the Share sheet → "Add to Home Screen").

## Tuning the game

Open `index.html` and find the `CONFIG` object near the top of the script. Notable groups:

- `weapons` — per-weapon damage, fire rate, spread, and special-effect parameters
- `weaponLevelDamageBonus`, `maxWeaponLevel` — how much re-picking a weapon boosts its damage, and the cap
- `bossGateIntervalMin/Max` — how often weapon gates trickle in during the boss fight
- `zombieSpawnInterval*`, `zombieRowCount*`, `zombieHpPerDistance`, `zombieSpeedPerDistance` — horde pacing and difficulty ramp
- `bossFightSeconds`, `bossMinHP` — boss balancing (HP is computed live from squad DPS, not fixed)
- `formationHalfWidth`, `laneLimit`, `dragSensitivity` — squad movement and formation footprint

Visual style helpers (`sketchyStroke`, `crossHatch`, `drawPaperOverlay`) are defined near the bottom of the script — tweak `passes`/`jitter` on `sketchyStroke` calls to make outlines rougher or cleaner.

Sound is fully procedural (see the `SFX` object and `toneSfx`/`noiseSfx` helpers) — no audio files to manage; adjust pitch/duration/filter values directly.

## License

MIT — see [LICENSE](LICENSE).
