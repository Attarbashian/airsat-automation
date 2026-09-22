# AirSat Automation

Public orchestration layer for the AirSat production platform.

The scientific processing source and the production application/data repository remain private:

- `Attarbashian/airsat-processing-runner` — private scientific processing source
- `Attarbashian/airsat-auto` — private production app and generated-data target

## Production status

This repository is now the **active production automation layer** for AirSat.

Active automation:
- Daily AirSat data orchestrator (Action 5): `17 2 * * *`
- Cloud maintenance / Supabase keepalive / pending recovery: `17,47 * * * *`
- On-demand GeoTIFF processing: dispatched by the AirSat Cloudflare Worker
- GeoTIFF ready-email delivery: Resend

The old schedules in `airsat-processing-runner` are disabled and kept only for manual rollback/reference.

## Security

No secret values are stored in this public repository.

Production workflows use GitHub Actions secrets to access:
- the private AirSat repositories,
- Google Earth Engine,
- Supabase,
- Resend,
- optional SMS services.

The workflows have no `pull_request` trigger.

## Source-of-truth

Processing code remains authoritative in the private `airsat-processing-runner` repository. This public repository owns orchestration only.
