# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Spendly — a Flask + SQLite expense tracker built as a step-by-step course project. Many pieces are intentionally stubbed with "Step N" markers (placeholder routes in `app.py`, the empty `database/db.py`, `static/js/main.js`). When implementing a step, replace the corresponding stub rather than adding parallel code.

## Commands (Windows)

`python3` is not available on this machine and the Git Bash environment may inherit a broken `_OLD_VIRTUAL_PATH`, so plain `pip`/`python` can resolve to base Miniconda. Always call the venv interpreter explicitly:

```
venv/Scripts/python -m pip install -r requirements.txt
venv/Scripts/python app.py                      # dev server, debug mode, http://127.0.0.1:5001
venv/Scripts/python -m pytest                   # all tests (pytest + pytest-flask)
venv/Scripts/python -m pytest tests/test_x.py::test_name   # single test
```

No tests exist yet; pytest-flask expects an `app` fixture in `conftest.py`. There is no linter or build step.

## Architecture

- `app.py` — single module holding the Flask app and all routes. Currently landing/register/login only render templates; logout, profile, and expense add/edit/delete are placeholder strings.
- `database/db.py` — to be written in Step 1 and must provide `get_db()` (SQLite connection with `row_factory` set and foreign keys enabled), `init_db()` (`CREATE TABLE IF NOT EXISTS`), and `seed_db()` (sample data). The DB file `expense_tracker.db` is gitignored.
- `templates/` — Jinja templates extending `base.html` (blocks: `title`, `head`, `content`, `scripts`). The navbar links via `url_for('landing'|'login'|'register')`, so renaming route functions breaks templates. Auth forms already POST to `/register` and `/login` with fields `name`/`email`/`password` and render an `error` variable, but the routes don't accept POST yet.
- `static/css/style.css` — all styling, driven by CSS custom properties in `:root` (ink/paper palette, `--accent` green, DM Serif Display / DM Sans fonts). Reuse these tokens and existing classes (`auth-*`, `form-*`, `btn-submit`) for new pages.
- Currency context is Indian rupees ("Track every rupee").
