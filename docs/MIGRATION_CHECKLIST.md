# Migration checklist

## Phase 1 — Create the public automation repository
Completed: `Attarbashian/airsat-automation` is public and uses `main`.

## Phase 2 — Install staging files
The files in `.github/workflows/` are intentionally staging-safe:
- Action 5 has no automatic schedule yet.
- Maintenance has no automatic schedule yet.
- GeoTIFF processing is available for manual testing.

Do not disable any current private workflow yet.

## Phase 3 — Create two least-privilege PATs
Create:
- `AIRSAT_RUNNER_READ_PAT`: read-only access to `airsat-processing-runner`
- `AIRSAT_AUTO_RW_PAT`: read/write Contents access to `airsat-auto`

Add them under:
Repository -> Settings -> Secrets and variables -> Actions -> New repository secret

## Phase 4 — Add service secrets
Add the Earth Engine and Supabase secrets listed in `SECRETS.md`.
Add notification secrets only when used.

GitHub does not allow viewing the value of an existing Actions secret. If the original value is no longer available, recreate/rotate that credential at its source rather than trying to expose the old secret.

## Phase 5 — Test private repository access
Actions -> `00 - Test Private Repository Access` -> Run workflow.

Pass criteria:
- both private repositories checkout successfully,
- expected runner files exist,
- `git push --dry-run` to `airsat-auto` succeeds.

No production data is changed.

## Phase 6 — Test external services
Actions -> `01 - Test AirSat Service Connections` -> Run workflow.

Pass criteria:
- Supabase HTTP query succeeds,
- Earth Engine authentication succeeds.

No production data is changed.

## Phase 7 — Test Action 5 manually
Run `05 - AirSat Data Orchestrator`.

Recommended first test:
- pollutant: `NO2`
- scope: `core`
- force_rebuild: `false`

Verify prepare, build, publication to `airsat-auto`, and audit.

## Phase 8 — Test GeoTIFF manually
Use a real pending test request UUID in `Process AirSat GeoTIFF Request`.

Verify Storage ZIP, DB ready state, and signed download.

Do not change Cloudflare dispatch yet.

## Phase 9 — Enable public schedules
After successful tests, replace the staging Action 5 and maintenance files with the matching versions under `templates/production/`.

Production schedules:
- Action 5: daily at `17 2 * * *`
- cloud maintenance: `17,47 * * * *`

## Phase 10 — Switch Cloudflare GeoTIFF dispatch
Only after the public GeoTIFF workflow passes:
- `GITHUB_REPO = Attarbashian/airsat-automation`
- `GITHUB_BRANCH = main`
- `GITHUB_WORKFLOW_FILE = process-geotiff-request.yml`
- rotate/update Cloudflare `GITHUB_TOKEN` for the automation repository

Test one request end-to-end.

## Phase 11 — Disable redundant private schedules
Only after all public workflows are proven:
- disable the schedule in private Action 5
- disable private pending-recovery schedule
- disable private Supabase-heartbeat schedule

Keep files temporarily for rollback.

## Phase 12 — Fix the Action 5 freshness race
After migration is stable, patch the private planner so dynamic layers approaching expiry are refreshed before they cross TTL during the run.

## Rollback
Before Phase 11, simply keep old schedules and Cloudflare target unchanged.
After Phase 11, re-enable private schedules and restore Cloudflare target if rollback is needed.
