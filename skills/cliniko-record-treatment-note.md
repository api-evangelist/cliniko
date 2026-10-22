---
name: record-treatment-note
description: Write a clinical treatment note for a patient, optionally from a template.
api: Cliniko API
generated: '2026-10-21'
method: generated
source: openapi/cliniko-openapi.yml
operations:
- GetPatient-Get
- ListTreatmentNotesForPatient-Get
- CreateTreatmentNote-Post
- UpdateTreatmentNote-Patch
---

# Record Treatment Note

## Steps
1. Confirm the patient with `GetPatient-Get` and review prior notes via `ListTreatmentNotesForPatient-Get`.
2. Create the note with `CreateTreatmentNote-Post` (patient_id, title, content; optionally a treatment note template).
3. Amend drafts with `UpdateTreatmentNote-Patch`.

## Rules
- Auth: HTTP Basic, API key as username, blank password. Base URL shard comes from the API key suffix (e.g. `https://api.au1.cliniko.com/v1`).
- Send a `User-Agent` containing your app name and a contact email, or requests may be blocked.
- Rate limit: 200 requests/minute per user; on 429 wait until `X-RateLimit-Reset`.
- Lists are paginated (default 50, max 100 via `per_page`); follow `links.next`.
- No idempotency keys are documented: before retrying a create (POST), search for the record first to avoid duplicates.
- This is patient health data: request only what the task needs.
