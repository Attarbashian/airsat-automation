# AirSat Automation

Public orchestration layer for the existing AirSat production system.

## Core rule

**Processing logic is not reimplemented here.**  
The authoritative AirSat processing code remains in the private repository:

- `Attarbashian/airsat-processing-runner` — scientific/processing source
- `Attarbashian/airsat-auto` — production app and generated-data target

This public repository only moves GitHub Actions execution out of the private repository to avoid private-repository Actions-minute limits.

## Production workflows

The active production workflows mirror the pre-migration private workflows:

- `05-airsat-data-orchestrator.yml`
  - original Action 5 behavior
  - schedule: `17 2 * * *`
- `process-geotiff-request.yml`
  - original on-demand GeoTIFF behavior
  - dispatched by the AirSat Cloudflare Worker
- `recover-pending-geotiff-requests.yml`
  - original pending-request recovery behavior
  - schedule: `*/5 * * * *`
- `supabase-heartbeat.yml`
  - original Supabase heartbeat behavior
  - schedule: `40 20 * * *`

The only production adaptations are orchestration-only:
1. checkout the private processing repository into `runner/`;
2. checkout the private `airsat-auto` repository with the public repo's PAT secret;
3. adjust command working paths because processing code now lives under `runner/`;
4. map already-configured public-repo secret names to the same runtime environment variables.

No scientific, GeoTIFF, retry, freshness, recovery, notification, or data-contract logic should be modified in this repository.

## Private-repository schedules

The old private workflows remain available for manual rollback/reference, but their schedules are disabled to prevent duplicate execution and private Actions-minute usage.

## Security

No secret values are committed to this repository.
