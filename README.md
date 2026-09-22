# AirSat Automation

Public orchestration layer for the AirSat production platform.

This repository intentionally contains **automation only**. The scientific processing source and the production application/data repository remain private:

- `Attarbashian/airsat-processing-runner` — private scientific processing source
- `Attarbashian/airsat-auto` — private production app and generated-data target

## Why this repository exists

AirSat needs permanent cloud automation without depending on a personal computer and without consuming the monthly GitHub Actions minutes of the private repositories. Standard GitHub-hosted runners for this public automation repository execute the orchestration, while private source is checked out at runtime using repository secrets.

## Security

No secret values belong in this repository.

Workflows use GitHub Actions secrets to access:
- the private AirSat repositories,
- Google Earth Engine,
- Supabase,
- optional email/SMS services.

The workflows have no `pull_request` trigger and are designed for `workflow_dispatch` / scheduled production execution only.

## Migration state

The workflows in `.github/workflows/` are the **staging-safe** variants:
- no automatic Action 5 schedule yet,
- no automatic maintenance schedule yet,
- GeoTIFF processing is available for manual testing.

After successful end-to-end testing, replace the staging Action 5 and maintenance files with the versions under `templates/production/`.

See `docs/MIGRATION_CHECKLIST.md`.
