# morocco_localization

Morocco Localization: a Frappe app for ERPNext 16 by Bangicode SARL. Part of the MaroneERP white-label build (Linear project MaroneERP, team CHW).

- Requires: `erpnext` (`required_apps` in `hooks.py`).
- Architecture: `bangicode-infra/docs/adr/0001-app-split.md`.
- Reference the Linear issue (`CHW-123`) in branch names, commits and PRs.

## Commands (inside the dev container, `bangicode-infra/dev/`)

```bash
bench --site marone.localhost run-tests --app morocco_localization
bench --site marone.localhost migrate
bench --site marone.localhost export-fixtures --app morocco_localization
pre-commit run --all-files
```

## Rules

- This is a **generic** app (ADR 0001). Never import, name or depend on a tenant app (`marone_custom`) or on another generic app. Where two generic apps meet, check `"<app>" in frappe.get_installed_apps()` first.
- No client name, identifier (ICE, IF, RC), price, address or logo anywhere: code, fixtures, tests, comments. Client values come from this app's settings DocType.
- No edits to Frappe or ERPNext. Use `hooks.py` (`doc_events`, `extend_doctype_class`, overrides), Custom Fields and Property Setters.
- Every Desk customization is exported to this app's fixtures.
- User-facing labels in French, wrapped in `_()` / `__()`.
- Money: `flt(value, precision)` with the field precision. Submitted documents are cancelled and amended, never deleted.
- Never list `frappe` or `erpnext` in `[project] dependencies`. Use `[tool.bench.frappe-dependencies]`.

## Done

Tests pass on a `*.localhost` site, fixtures exported, `frappe-reviewer` run on the diff (plus `accounting-checker` for GL, TVA, landed cost, valuation or currency changes).
