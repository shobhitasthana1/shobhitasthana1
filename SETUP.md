# Setup Guide — Pac-Man Contribution Arcade

Follow this once. After that, everything regenerates itself daily with zero
manual work.

---

## 0. Why this over Snake (short version)

| Option | Feels like | Interactivity | Maintenance | Visual "wow" |
|---|---|---|---|---|
| **Snake** (`Platane/snk`) | Solved, extremely common | Snake eats squares, no AI | Very active, huge adoption | Low — everyone has it |
| **Pac-Man arcade** ✅ *chosen* | A real, playable arcade cabinet | Full ghost AI (Blinky/Pinky/Inky/Clyde chase logic), power-pellet mode | Actively maintained (v2.3.0), same GitHub Actions pattern as Snake | High — most viewers haven't seen it |
| **3D bar-graph GIF** | A rotating bar chart | None — static camera, bars just grow | Small/newer project, less battle-tested | Medium — novel but not interactive |
| **Tetris / Space Invaders standalone tools** | Interesting concept | Varies | Mostly proof-of-concept repos, not README-embed-ready today | Medium, but risk of broken embeds |

**Verdict:** Pac-Man wins on the combination you asked for — arcade-inspired,
genuinely interactive (real chase AI, not just a moving sprite), actively
maintained, drop-in GitHub Actions compatible, and still rare enough on
profiles to actually stand out. Snake is the safe, common choice; this is the
same *mechanism* (contribution data → animated SVG → Actions → output branch)
with a far more premium result.

---

## 1. One-time repository setup

GitHub profile READMEs only render from a **special repository**: one named
*exactly* the same as your username.

1. Go to <https://github.com/new>
2. Repository name: `SHOBHITASTHANA1` (must match your username exactly)
3. Make it **Public**
4. Check "Add a README file"
5. Create repository

GitHub will show a banner on your profile confirming this repo powers your
profile page.

> Already have this repo (your screenshot shows you do, with `assets/`,
> `projects/`, `LICENSE`, `README.md`)? Skip to step 2 — just drop the new
> files into your existing structure.

---

## 2. Drop in the files from this delivery

Copy this exact structure into your `SHOBHITASTHANA1/SHOBHITASTHANA1` repo:

```
SHOBHITASTHANA1/
├── .github/
│   └── workflows/
│       └── pacman-contribution.yml   ← the automation
├── assets/
│   └── arcade/
│       └── README.md                 ← docs explaining where generated SVGs live
├── README.md                         ← your new profile page (arcade section included)
└── SETUP.md                          ← this file (optional to keep, doesn't render on profile)
```

Your existing `assets/badges`, `assets/icons`, `assets/illustrations`,
`assets/image`, `assets/projects/`, and `LICENSE` are untouched — this only
adds a new `assets/arcade/` folder and the workflow.

---

## 3. Permissions — the one setting that trips people up

The workflow needs permission to **push a new branch** (`output`) to your repo.

1. Go to your repo → **Settings** → **Actions** → **General**
2. Scroll to **Workflow permissions**
3. Select **"Read and write permissions"**
4. Save

Without this, the workflow will run but fail silently on the "push to output
branch" step with a permissions error.

---

## 4. First run (don't wait for tomorrow's cron)

1. Go to the **Actions** tab in your repo
2. Click **"Generate Pac-Man Contribution Graph"** in the left sidebar
3. Click **"Run workflow"** → **"Run workflow"** (green button)
4. Wait ~1–2 minutes
5. Once green ✅, switch to the **`output`** branch (the workflow creates it
   automatically — use the branch dropdown on the Code tab) and confirm
   `dist/pacman-contribution-graph.svg` exists

6. Go back to your profile (`github.com/SHOBHITASTHANA1`) and hard-refresh
   (Ctrl+Shift+R / Cmd+Shift+R) — raw.githubusercontent.com URLs are
   aggressively cached by browsers, so a normal refresh sometimes shows the
   old/blank state for a minute.

---

## 5. It's now self-maintaining

- **Daily regeneration:** the `schedule: cron: "0 0 * * *"` trigger reruns
  this every day at 00:00 UTC, pulling your latest contribution data
  automatically. You never touch it again.
- **Instant regeneration on edits:** the `push` trigger on `main` reruns it
  immediately if you edit `README.md` or the workflow file — useful while
  you're tweaking colors/speed.
- **Manual anytime:** the `workflow_dispatch` trigger means you can always
  hit "Run workflow" from the Actions tab if you want to force a refresh
  right after a big commit day.

---

## 6. Customization knobs

In `.github/workflows/pacman-contribution.yml`:

- `games: "pacman,breakout"` → add/remove games. Also supports (per upstream
  docs) `galaga`, `puzzlebobble`, `bomberman`, `minesweeper` if you want to
  experiment — just add them comma-separated and re-run.
- `cron: "0 0 * * *"` → change the time. Format is `minute hour day month
  weekday`, always UTC. E.g. `"0 6 * * *"` = 6:00 AM UTC daily.

In `README.md`:
- The `<sub>` line under the graph is a good spot for your own caption/joke.
- The collapsible `<details>` Breakout section can be deleted if you'd rather
  keep the profile to a single hero animation — the workflow will keep
  generating it either way (harmless, just unused).

---

## 7. Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Image shows as broken link | Workflow hasn't run yet, or repo isn't named exactly as your username | Confirm repo name; run workflow manually (§4) |
| Workflow fails on "push to output branch" | Missing write permissions | Redo §3 |
| Graph shows but never updates | Browser cache | Hard refresh; raw.githubusercontent.com caches ~5 min minimum |
| Ghosts/Pac-Man look empty/sparse | Low recent contribution activity | This is accurate — the maze density reflects your real GraphQL contribution data, it's not a bug |
| Fork shows "workflows disabled" | GitHub disables Actions on forks by default | Only relevant if you forked the *tool's* repo instead of creating your own profile repo — you don't need to fork anything, just copy these files into your own `username/username` repo |

---

You're done. The maze runs itself from here.