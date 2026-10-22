---
name: manage-appointment
description: Reschedule or cancel an existing individual appointment.
api: Cliniko API
generated: '2026-10-21'
method: generated
source: openapi/cliniko-openapi.yml
operations:
- ListIndividualAppointments-Get
- GetIndividualAppointment-Get
- UpdateIndividualAppointment-Patch
- CancelIndividualAppointment-Patch
- GetIndividualAppointmentConflicts-Get
---

# Manage Appointment

## Steps
1. Locate the appointment with `ListIndividualAppointments-Get` or `GetIndividualAppointment-Get`.
2. Reschedule with `UpdateIndividualAppointment-Patch` (new starts_at/ends_at), then check `GetIndividualAppointmentConflicts-Get`.
3. Cancel with `CancelIndividualAppointment-Patch` (cancellation reason and note); prefer cancel over delete to keep history.

## Rules
- Auth: HTTP Basic, API key as username, blank password. Base URL shard comes from the API key suffix (e.g. `https://api.au1.cliniko.com/v1`).
- Send a `User-Agent` containing your app name and a contact email, or requests may be blocked.
- Rate limit: 200 requests/minute per user; on 429 wait until `X-RateLimit-Reset`.
- Lists are paginated (default 50, max 100 via `per_page`); follow `links.next`.
- No idempotency keys are documented: before retrying a create (POST), search for the record first to avoid duplicates.
- This is patient health data: request only what the task needs.
