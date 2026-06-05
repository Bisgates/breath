# breath

Paced-breathing CLI — a local fork of [breathe-cli](https://github.com/marekkowalczyk/breathe-cli)
with three local changes:

1. **Command is `breath`** (not `breathe`).
2. **Duration is a positional arg, default preset `calm`, default 3 min.**
   - `breath` → calm, 3 min
   - `breath 5` → calm, 5 min
   - `breath --ratio 6-6 5` → custom 5 min at 6-6 (= 5/min)
3. **`breath --stats`** — per-day session counts and total time practiced.
   A mid-session quit counts the *actual* time breathed (wall-clock minus pauses),
   not the target length.

Pure Python standard library, no dependencies, no build step.

## The science behind it

[`docs/breathing-science.html`](docs/breathing-science.html) — a single-file
interactive explainer on what different breathing methods actually do (evidence-graded),
why ~6 breaths/min is the baroreflex resonance point, the CO₂ / exhale-ratio myths,
and the optimal practice for a sedentary worker. Self-contained; open it in a browser.
View it rendered via [htmlpreview](https://htmlpreview.github.io/?https://github.com/Bisgates/breath/blob/main/docs/breathing-science.html).

## How it's wired

The installed command is a thin launcher at `~/.local/bin/breath` that imports
`breath.py` **from this directory**, so editing `breath.py` here changes the
command immediately — this repo is the single source of truth.

```
~/.local/bin/breath  ──imports──▶  ~/project/learn_with_agent/breath/breath.py
```

To reinstall as a proper console script later (optional):

```
pipx install --force /Users/han/project/learn_with_agent/breath      # or: pip install -e .
```

## Usage

```
breath                 # calm, 3 min
breath 5               # calm, 5 min
breath -p balanced     # 5-5 = 6/min, 10 min
breath --ratio 5-6 5   # ~5.5/min, 5 min
breath --stats         # per-day stats
breath --list-presets
breath --safety
```

In-session keys: `space` pause/resume · `s` mute · `q` quit.

## Data

Sessions are logged to `~/.breathe_log.csv` (one row per run):
`date,time,preset,ratio,duration_target_s,duration_actual_s,breaths,completion_pct,status`.
`--stats` aggregates this file by day. `--no-log` skips logging for a run.
