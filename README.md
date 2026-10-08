# renovate-osv-poetry-versioning

Minimal reproduction: with `osvVulnerabilityAlerts: true`, an OSV vulnerability
alert for a **poetry** dependency using a caret range (`weasyprint = "^69.0"`)
makes Renovate generate a vulnerability packageRule that sets
`versioning: "pep440"` (the pypi datasource default), overriding the
`versioning: "poetry"` the poetry manager assigned. The caret constraint is then
invalid under pep440, so the dependency is skipped with
`skipReason: "invalid-value"` and **no security PR is created**.

## Contents
- `backend/pyproject.toml` — poetry dep `weasyprint = "^69.0"` (affected by GHSA-jf6q-chmf-3h3v, GHSA-r543-q48m-4c9j, PYSEC-2026-3940; fix `>= 70.0`)
- `backend/poetry.lock` — locks weasyprint 69.0
- `renovate.json` — `{ "osvVulnerabilityAlerts": true }`

## Expected
Renovate creates a `renovate/pypi-weasyprint-vulnerability` branch.

## Actual
`weasyprint` ends up as:
```json
{ "versioning": "pep440", "currentValue": "^69.0", "updates": [], "skipReason": "invalid-value" }
```
