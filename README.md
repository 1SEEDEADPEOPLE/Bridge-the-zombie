# Last Bridge

A comic-sketch-style zombie horde lane shooter — squad-builder runner inspired by mobile "Last Z"-style ads. Drag left/right to steer your squad up a bridge, smash through weapon gates on the left, mow down a nonstop zombie horde on the right, and survive a gauntlet of bosses at the end of each level. Clear a level, pick an upgrade, and the bridge gets harder — survive as many levels as you can.

No build step, no framework, no install. The game is one HTML file with embedded CSS and JavaScript, rendered on an HTML5 canvas. It's also an installable PWA — add it to your home screen and it runs full-screen, offline-capable, like a native app.

## Play it

Open `index.html` in any modern browser (desktop or mobile), or play the hosted version once GitHub Pages is enabled (see below). On mobile, your browser will offer to "Add to Home Screen" — once installed it opens without browser chrome.

**Controls:** touch-drag or click-drag left/right to steer the squad (it fires automatically). Tap the speaker icon (top right) to mute/unmute, the pause icon (top left) to pause, and **WEAPON PERKS** on the main menu to browse every weapon's unlock tree without starting a run.

## Features

- **Levels**: each level is a stretch of bridge ending in a boss gauntlet. Clearing all of a level's bosses opens a 3-card upgrade choice, then advances to the next level — harder zombies, tougher gates, and a new background palette (25 in rotation).
- **Bosses spawn "mixed"**: a new Butcher joins on a timer regardless of whether the previous one is dead, so a fast player sees them sequentially while a slower player can end up fighting several at once. Boss count scales with level (up to 5).
- **Uncapped weapon leveling**: picking up a weapon you already have stacks more damage — forever. Growth is linear for the first 5 levels, then softens (so numbers stay sane deep into a long run) but never stops mattering, which is what makes late-run bosses still beatable.
- **Milestone perks**: every weapon unlocks a brand-new effect at level 10, 20, and 30 — not just bigger numbers. Pistol gets piercing rounds and crits; machine gun gets armor-shred and pierce; shotgun gets explosive, then incendiary shells; dual SMG gets ricochet; bazooka gets a bigger blast, a second detonation, and a concussive stun; flamethrower gets a slow, a bigger burn, and a death-combustion effect. A "PERK UNLOCKED" callout fires the moment you cross a milestone, and gates hint "PERK UNLOCK!" when picking them would cross one.
- **Weapon Perks codex**: a menu screen (no run required) listing every weapon's full perk tree, so you always know what you're working toward.
- **Machine gun plays like a laser**: very fast fire rate, brighter/longer tracer rendering, tuned to feel like sustained beam fire rather than discrete shots.
- **Roguelite upgrades**: after every level, pick 1 of 3 (damage, fire rate, armor, splash/burn/stun boosts, bonus boss damage, wider spread, extra squad on supply gates, a kill-streak revive, or an instant weapon level-up). Upgrades stack and persist for the whole run, and only reset when you're defeated.
- Perspective-scrolling bridge with a physical median splitting a left "weapon gate" lane from a right "zombie horde" lane, with solid walls down both edges.
- Randomized weapon-gate sequence each level — gates show exactly which weapon they grant, so you choose your loadout; gates keep trickling in during boss fights too.
- Red "debuff" gates are a real hazard: breaking one costs squad *and* temporarily weakens your damage.
- Continuous, never-stopping zombie horde with multiple body-type variants, scaling in count/HP/speed with both distance and level.
- Pause button with a Resume/Restart overlay.
- Comic/sketch visual style: jittered hand-inked outlines, cross-hatch shading, paper-grain texture, halftone dots, comic-marker fonts (Bangers / Permanent Marker) throughout — UI included, no emoji anywhere.
- Procedural sound effects (WebAudio — no audio files) with a mute toggle.
- HUD positioned dynamically below the pause/mute buttons (measured at runtime, so it can't overlap on any device/notch), with current weapon + level, level number, squad "lives" bar, weakened-debuff indicator, and boss health bars.
- Installable PWA: manifest, service worker (offline caching), and a full icon set including correctly-sized, full-bleed Apple touch icons.

## Project structure

```
.
├── index.html       # the entire game — HTML, CSS, and JS in one file
├── manifest.json    # PWA manifest
├── sw.js            # service worker (offline caching)
├── icons/           # app icons
├── README.md
└── LICENSE
```

There's intentionally no build tooling. Everything needed to run or edit the game is in `index.html`; all tunable values (weapon stats + perk trees, level scaling, the upgrade pool, boss quota curve, background palettes, etc.) live in the `CONFIG` object and the `UPGRADE_POOL`/`BACKGROUNDS` arrays near the top of the `<script>` block.

> **Note on `.gitignore`:** this repo doesn't ship one. It's a file Git uses to skip junk like `.DS_Store` or editor folders — genuinely optional for this project, and its leading-dot filename makes it awkward to download/rename on some devices anyway. Add your own later if you want one; nothing here depends on it.

## Run locally

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
python3 -m http.server 8000
# open http://localhost:8000
```

PWA features (service worker, install prompt, correct icons) require `http(s)://`, not `file://`.

## Deploy with GitHub Pages

1. Push this repo to GitHub.
2. **Settings → Pages** → Source: `Deploy from a branch`, branch `main`, folder `/ (root)` → Save.
3. Live at `https://<your-username>.github.io/<your-repo>/`.

If you update the game after it's already installed as a PWA, bump `CACHE_NAME` in `sw.js` so the service worker actually refreshes cached clients.

## Tuning the game

Open `index.html` and find the `CONFIG` object. Notable groups:

- `weapons` — per-weapon damage, fire rate, spread, special effects, and the `perks` array (the level-10/20/30 unlock tree shown in the in-game codex)
- `weaponLevelDamageBonus`, `weaponPerkLevels` — leveling curve and milestone levels (see `weaponLevelMultiplier()` for the actual uncapped, diminishing-returns formula)
- `UPGRADE_POOL` — the 12 stacking roguelite upgrades offered after each level
- `bossQuotaBase/PerLevels/Max`, `bossSpawnIntervalMin/Max`, `bossHpRampPerExtra` — boss-gauntlet pacing
- `levelDifficultyPerLevel` — how much harder each new level is
- `BACKGROUNDS` — the 25 per-level color palettes

## License

MIT — see [LICENSE](LICENSE).
