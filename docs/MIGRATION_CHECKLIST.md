# Migration status

## Completed

- Public repository `Attarbashian/airsat-automation` created.
- Private-repository access verified.
- Supabase connectivity verified.
- Earth Engine connectivity and 31-province asset verified.
- Real Action 5 production-path run succeeded.
- Daily Action 5 schedule moved to the public automation repository.
- Supabase heartbeat moved to the public automation repository.
- Pending GeoTIFF recovery moved to the public automation repository.
- Old high-frequency private recovery schedule disabled.
- Old private heartbeat schedule disabled.
- Old private Action 5 schedule disabled.
- Planner freshness safety window added to reduce planner/audit TTL races.
- Cloudflare GeoTIFF dispatch switched to `Attarbashian/airsat-automation`.
- Real public GeoTIFF workflow completed successfully.
- Resend ready-email delivery completed successfully with attachment.

## Active production schedules

- AirSat data orchestrator: `17 2 * * *`
- Cloud maintenance / pending recovery: `17,47 * * * *`

## Rollback

Backup branches created before cutover:
- `backup/pre-public-automation-cutover-2026-09-22` in `airsat-processing-runner`
- `backup/pre-public-automation-cutover-2026-09-22` in `airsat-automation`

The old private workflows remain manually dispatchable for rollback/reference but have no active schedules.

## Notes

Historical failed GitHub Actions runs remain visible as immutable execution history. Their schedules have been disabled; they are not active jobs.
