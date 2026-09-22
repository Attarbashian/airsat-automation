# AirSat Automation

Public production orchestration layer for the AirSat platform.

The scientific processing source and production application/data repository remain private:

- `Attarbashian/airsat-processing-runner` — private scientific processing source
- `Attarbashian/airsat-auto` — private production app and generated-data target

## Production responsibilities

### Daily AirSat data orchestrator
`.github/workflows/05-airsat-data-orchestrator.yml`

Runs daily at:

`17 2 * * *`

The public workflow checks out the private processing repository, plans only missing/stale/corrupt work, runs Earth Engine processing, validates each unit, publishes validated outputs to `airsat-auto`, and runs the global audit.

The private planner includes a six-hour freshness safety window so dynamic outputs approaching TTL expiry are refreshed before the final audit can mark them stale.

### Cloud maintenance
`.github/workflows/maintenance.yml`

Runs twice per hour at:

`17,47 * * * *`

The lightweight probe:
- keeps Supabase active,
- checks for stale pending download requests,
- runs heavy recovery only when pending work exists.

### User GeoTIFF exports
`.github/workflows/process-geotiff-request.yml`

Cloudflare dispatches this workflow when a user submits an export request. The workflow executes the private processor, uses Earth Engine, uploads the ZIP to private Supabase Storage, updates the request to ready, and sends ready notifications when enabled.

## Security

No production secret values are committed here.

Private source is checked out at runtime using least-privilege GitHub Actions secrets. The public workflows do not use pull-request triggers with production secrets.

## Rollback

Pre-cutover backup branches were created on 2026-09-22:

`backup/pre-public-automation-cutover-2026-09-22`

The legacy private workflows remain manually dispatchable for emergency rollback, but their schedules are disabled.
