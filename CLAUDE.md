# content-machine

Instagram graphics for `@bvb.immer.weiter` (Borussia Dortmund, German). Read `CONTEXT.md` before any task; it holds the process, inputs and checkpoints.

## Conventions

- Comments only on non-obvious why, never on what.
- README stays short.
- Output (`posts/`) and photos/crests under `media/` are gitignored, so making a post never creates a commit. Photos are usually someone else's copyright.
- Facts live in exactly one file. A club fact goes in `clubs/bvb.yaml`, a player in `media/bvb/players.yaml`, a process step in `CONTEXT.md`.

## Code map

Folders:
- `content_machine/`: the CLI and render pipeline (Python 3.12, `.venv/`).
- `content_machine/sources/`: match data providers, registered in `sources/__init__.py`.
- `templates/`: one folder per post type (`template.html` + `example.yaml`); `_layout.html` and `_base.css` are shared.
- `clubs/bvb.yaml`: every club fact, colour, font, name mapping and template string.
- `media/bvb/`: `players.yaml`, `credits.yaml`, plus gitignored photos. `media/crests/` and `media/competitions/` are gitignored caches.
- `posts/`: output, gitignored.

Flows:
- Match: `cli.cmd_match` → `sources/{apifootball,footballdata}.fixture` → `models.Match` → `cli.match_to_post` (names via club yaml + `media.display_name`, crests via `media.cache_crest`) → `media.pick_background` → `post.yaml` → `render.render`.
- Announcement: `cli.cmd_new` copies `templates/{type}/example.yaml` → `posts/…/post.yaml` → `cli._set_background` → edit by hand → `cli.cmd_render`.
- Render: `render.build_html` (Jinja, `StrictUndefined`, club `strings` as `s`) → `post.html` → headless Chromium waits for `window.__ready` (set in `_layout.html` after fonts load and `.fit` text shrinks) → `post.png` 1080×1350.

Modules:
- `cli.py`: argparse commands, the human "right match?" checkpoint, post folder naming.
- `config.py`: paths relative to `ROOT`, `.env` loading, club yaml loading.
- `media.py`: player name matching, photo lookup, win/draw/loss, background pick, crest download.
- `models.py`: `Team`, `Event`, `Match`, `FixtureSummary`.
- `render.py`: yaml → HTML → PNG, long German dates.

Config:
- `.env` holds `API_FOOTBALL_KEY` and `FOOTBALL_DATA_KEY` (see `.env.example`). Rendering needs neither.
- `clubs/bvb.yaml` `api.source` picks the provider (`footballdata` now).

Build, run, test:
- Setup and the full process: `CONTEXT.md` → Process.
- Commands: `.venv/bin/python -m content_machine.cli --help`.
- No test suite. To check templates, render every `example.yaml` (Conductor's Run button writes them to `.context/examples/`) and look at the PNGs.

When you add, move, or rename files, update this map.
