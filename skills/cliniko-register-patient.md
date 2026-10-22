---
name: register-patient
description: Search for an existing patient and create or update the patient record.
api: Cliniko API
generated: '2026-10-21'
method: generated
source: openapi/cliniko-openapi.yml
operations:
- ListPatients-Get
- CreatePatient-Post
- GetPatient-Get
- UpdatePatient-Patch
---

# Register Patient

## Steps
1. Search with `ListPatients-Get` (filter by name, email or date of birth) to avoid duplicates.
2. If absent, `CreatePatient-Post` with at least first_name and last_name.
3. Read back with `GetPatient-Get`; amend with `UpdatePatient-Patch`.

## Rules
- Auth: HTTP Basic, API key as username, blank password. Base URL shard comes from the API key suffix (e.g. `https://api.au1.cliniko.com/v1`).
- Send a `User-Agent` containing your app name and a contact email, or requests may be blocked.
- Rate limit: 200 requests/minute per user; on 429 wait until `X-RateLimit-Reset`.
- Lists are paginated (default 50, max 100 via `per_page`); follow `links.next`.
- No idempotency keys are documented: before retrying a create (POST), search for the record first to avoid duplicates.
- This is patient health data: request only what the task needs.
