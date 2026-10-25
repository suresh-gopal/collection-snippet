GroupTool
=========

A tiny Flask app to surface a CSV mapping of users → groups and to authorize a user via an API key (base64 comparison).

**Files of interest**
- `grouptool.py` — main Flask application and CSV parsing (`load_user_groups`).
- `usergroups.csv` — CSV containing `User,Group` rows.
- `templates/index.html` — Jinja2 template used by the web UI.
- `tests/` — pytest tests for CSV parsing and basic app flows.

Quick start (Windows PowerShell)
-------------------------------
1. From the project folder (`GroupTool`) install runtime/test deps:

```powershell
python -m pip install --upgrade pip
python -m pip install flask pytest
```

2. Run the app:

```powershell
py grouptool.py
```

3. Open the UI in your browser:

http://localhost:5000/grouptool

Usage
-----
- The page initially shows an API-key unlock form. Enter the API key and click "Unlock".
  - Example: `admin` → base64 `YWRtaW4=` is the default expected key stored in `grouptool.py` (`expected_api_key_b64`).
  - After unlocking, the rest of the UI is shown (user dropdown, mapping table, etc.).
- Select a user from the dropdown and click Submit to show that user's group.

CSV format
----------
- `usergroups.csv` must have a header with `User,Group` and rows such as:

```
User,Group
DAM-NA-User,Producers
DAM-SA-User,Consumers
SysAdmin,Admin
```

Note: the application currently loads `usergroups.csv` at startup. If you edit the CSV, restart the server to pick up changes.

Tests
-----
Run the pytest suite from the `GroupTool` directory:

```powershell
pytest -q
```

This runs CSV parsing tests and basic Flask integration tests (test client). Tests are isolated and create temporary CSV files.

Configuration & security
------------------------
- The app uses Flask sessions and requires a `SECRET_KEY` for production. Set a secure value via the `SECRET_KEY` environment variable before running in production. Example (PowerShell):

```powershell
$env:SECRET_KEY = 'a-long-random-secret'
python grouptool.py
```

- The default expected key (base64) is set in the file `grouptool.py` as `expected_api_key_b64 = "YWRtaW4="` (which is `admin`). Change this to a different base64 value if you want another key.

Contact
-------
For specific changes, contact author Suresh Gopal.