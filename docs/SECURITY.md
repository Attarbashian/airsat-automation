# Security notes

- Never commit PATs, Supabase keys, Earth Engine service-account JSON, Resend keys, or SMS keys.
- Keep public workflows limited to `schedule` and `workflow_dispatch`; do not add secret-bearing `pull_request` jobs.
- Prefer fine-grained PATs restricted to the exact repositories and permissions needed.
- The private processing source remains authoritative. Do not duplicate Python processing code into this public repository.
- Review workflow changes carefully before merging because scheduled workflows can use production secrets after they are on the trusted `main` branch.
