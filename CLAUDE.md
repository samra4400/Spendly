# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"Spendly" — a Flask + SQLite expense tracker built as a step-by-step student exercise. Most features are still stubs; placeholder routes and comments are labelled with the step number (e.g. "coming in Step 3"). Implement stubs in place rather than restructuring.

## Commands

- Virtualenv lives in `.myenv/` (not `venv/`): `source .myenv/bin/activate`
- Install deps: `pip install -r requirements.txt`
- Run dev server: `python app.py` (debug mode, port **8080**, not Flask's default 5000)
- Run tests: `pytest`; single test: `pytest path/to/test_file.py::test_name` (pytest and pytest-flask are installed, but no tests exist yet)
- No linter or build step is configured.

## Architecture

- `app.py` — single Flask app holding every route. `/`, `/register`, `/login` render templates (GET only; no form handling yet). `/logout`, `/profile`, `/expenses/add`, `/expenses/<int:id>/edit`, `/expenses/<int:id>/delete` are placeholders that return plain strings.
- `database/db.py` — comment-only stub. It is meant to provide `get_db()` (SQLite connection with `row_factory` and foreign keys enabled), `init_db()` (`CREATE TABLE IF NOT EXISTS`), and `seed_db()` (sample data). The DB file `expense_tracker.db` is gitignored.
- `templates/` — `base.html` (navbar, Google Fonts, blocks `title`, `head`, `content`) is extended by `landing.html`, `login.html`, `register.html`. Navbar links use `url_for()` with view function names (`landing`, `login`, `register`), so renaming those functions in `app.py` breaks the templates.
- `static/css/style.css` is the only stylesheet; `static/js/main.js` is essentially empty.
