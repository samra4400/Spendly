# Plan: dependency vulnerability audit and remediation

## Context
Potentially vulnerable packages were identified by inspecting `requirements.txt` and `.myenv`. No authoritative scan has been run (`pip-audit` and `safety` are not installed, and no CVE database was checked), so the findings below are risks to verify, not confirmed CVEs.

## Findings
**Direct deps (all pinned, match `.myenv`):** flask 3.1.3, werkzeug 3.1.6, pytest 8.3.5, pytest-flask 1.3.0.
**Transitive (unpinned):** Jinja2 3.1.6, MarkupSafe 3.0.4, itsdangerous 2.2.0, click 8.5.0, blinker 1.9.0, pluggy, iniconfig, packaging.

1. CVE status of all pins is unverified; a real scan is needed.
2. Transitive deps are not pinned, so builds and audits are not reproducible.
3. pytest and pytest-flask are in the runtime `requirements.txt`, which ships dev tools to production.
4. `.myenv` uses Python 3.15.1 (very new); confirm it is intended and supported.
5. Code-level: `app.py:57` uses `debug=True` (Werkzeug debugger RCE risk if reachable); no `SECRET_KEY` yet, needed once login sessions are added.
6. Front end: only Google Fonts is external; no action.

## Remediation steps
1. Install and run `pip-audit` (`-r requirements.txt` and on the env); needs network.
2. Bump each flagged package to the fixed version in `requirements.txt`, reinstall, smoke-test `/`, `/login`, `/register` and run `pytest`.
3. Split into `requirements.txt` (runtime) and `requirements-dev.txt` (pytest, pytest-flask).
4. Pin transitive deps via a freeze/lock file.
5. Make debug conditional on `FLASK_DEBUG`; load `SECRET_KEY` from the environment.
6. Optional: Dependabot/pip-audit in CI once the project is in git.

## Verification
- `pip-audit` clean after upgrades.
- App starts, the three pages return 200, debugger off by default.
- `pytest` passes once tests exist.
