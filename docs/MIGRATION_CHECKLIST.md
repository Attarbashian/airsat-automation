# AirSat automation migration — corrected final state

## Objective

Move GitHub Actions execution from the private processing repository to the public `Attarbashian/airsat-automation` repository **without changing the established AirSat processing logic**.

## Corrected production state

- Private processing source remains authoritative.
- Public automation repository only orchestrates/checks out that source.
- `airsat-auto` remains the private production/generated-data target.
- Cloudflare dispatches on-demand GeoTIFF requests to the public automation repository.

## Production workflows mirrored from the original private setup

- Action 5: `17 2 * * *`
- Pending GeoTIFF recovery: `*/5 * * * *`
- Supabase heartbeat: `40 20 * * *`
- On-demand GeoTIFF: workflow_dispatch from Cloudflare

## Corrections after audit

Unauthorized/extra behavior changes introduced during migration were reverted:

- removed the planner 6-hour freshness-safety modification;
- restored the original GeoTIFF processor exactly;
- removed GeoTIFF retry/tiled-fallback/requeue changes;
- restored the original pending-request recovery script;
- removed the added rasterio dependency/fallback;
- removed the invented combined `AirSat Cloud Maintenance` production workflow;
- restored the original split heartbeat and pending-recovery workflows and schedules.

## Allowed migration-only differences

The public workflows differ from the original only where necessary to run the same code from another repository:

- private runner checkout using `AIRSAT_RUNNER_READ_PAT`;
- private target checkout using `AIRSAT_AUTO_RW_PAT`;
- `runner/` working paths;
- secret-name mapping for the existing public repository secrets.

## Private repository

All non-scheduling source files now match the pre-cutover baseline.  
Only the three old private schedules are disabled to avoid duplicate runs and private Actions-minute consumption.

## Backups

- `backup/pre-public-automation-cutover-2026-09-22`
- `backup/post-cutover-geotiff-experiments-2026-09-23`
