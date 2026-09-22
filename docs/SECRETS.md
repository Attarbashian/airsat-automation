# Required GitHub Actions secrets

Do **not** put any secret value in a committed file.

## Required for repository access

### `AIRSAT_RUNNER_READ_PAT`
Fine-grained PAT restricted to:
- repository: `Attarbashian/airsat-processing-runner`
- permission: Contents — Read-only

### `AIRSAT_AUTO_RW_PAT`
Fine-grained PAT restricted to:
- repository: `Attarbashian/airsat-auto`
- permission: Contents — Read and write

## Required for Earth Engine

- `EE_SERVICE_ACCOUNT_JSON`
- `EE_PROJECT`
- `EE_PROVINCES_ASSET`
- `EE_PROVINCE_NAME_FIELD` (optional if the processing code can use its default)

## Required for Supabase

- `SUPABASE_URL`
- `SUPABASE_SERVICE_ROLE_KEY`

## Optional notification secrets

Only needed if those notification paths are enabled:
- `RESEND_API_KEY`
- `AIRSAT_EMAIL_FROM`
- `AIRSAT_EMAIL_REPLY_TO`
- `SMSIR_API_KEY`
- `SMSIR_READY_TEMPLATE_ID`

## Separate Cloudflare secret for workflow dispatch

After testing, the Cloudflare Worker must dispatch the **public automation repository**, not the private runner repository.

Cloudflare runtime configuration:
- `GITHUB_REPO = Attarbashian/airsat-automation`
- `GITHUB_BRANCH = main`
- `GITHUB_WORKFLOW_FILE = process-geotiff-request.yml`

`GITHUB_TOKEN` in Cloudflare should be a fine-grained token restricted to `Attarbashian/airsat-automation` with Actions read/write permission. Keep it separate from the PATs above.
