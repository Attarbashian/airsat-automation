# Architecture

```text
Cloudflare Worker
    |
    +--> Supabase Auth / DB / Storage
    |
    +--> workflow dispatch
             |
             v
      airsat-automation  [PUBLIC]
             |
             +--> checkout airsat-processing-runner [PRIVATE]
             |
             +--> checkout airsat-auto [PRIVATE]
             |
             +--> Earth Engine
             |
             +--> commit validated public data to airsat-auto
```

## Static data

`05-airsat-data-orchestrator.yml` preserves the existing Action 5 flow:

Planner -> task matrix -> task runner -> Earth Engine builder -> validator -> immediate validated commit -> final audit.

Only the orchestration repository changes. The scientific Python implementation remains in the private processing repository.

## User GeoTIFF

Cloudflare dispatches `process-geotiff-request.yml` with the Supabase request UUID. The workflow checks out private processing code and `airsat-auto`, processes the request, writes the ZIP to private Supabase Storage, and updates request status.

## Maintenance

`maintenance.yml` replaces two private scheduled workflows:
- Supabase heartbeat
- frequent pending-request recovery

The lightweight probe runs first. Heavy checkout/dependency installation happens only when a stale pending request actually exists.
