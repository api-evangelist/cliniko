---
name: book-individual-appointment
description: Find an open slot for a practitioner and appointment type at a business, then book an individual appointment for a patient.
api: Cliniko API
generated: '2026-10-21'
method: generated
source: openapi/cliniko-openapi.yml
operations:
- ListBusinesses-Get
- ListPractitionersForBusiness-Get
- ListAppointmentTypesForPractitioner-Get
- GetNextAvailableTime-Get
- GetAllAvailableTimes-Get
- ListPatients-Get
- CreateIndividualAppointment-Post
- GetIndividualAppointmentConflicts-Get
---

# Book Individual Appointment

## Steps
1. List businesses (`ListBusinesses-Get`) and pick the `business_id`.
2. List practitioners at that business (`ListPractitionersForBusiness-Get`) and their appointment types (`ListAppointmentTypesForPractitioner-Get`).
3. Query `GetNextAvailableTime-Get` or `GetAllAvailableTimes-Get` with a from/to date range.
4. Find the patient with `ListPatients-Get` (use the `q[]` filter), or create one (see register-patient skill).
5. Create the booking with `CreateIndividualAppointment-Post` (starts_at, ends_at, patient_id, practitioner_id, appointment_type_id, business_id).
6. Check `GetIndividualAppointmentConflicts-Get` on the new appointment.

## Rules
- Auth: HTTP Basic, API key as username, blank password. Base URL shard comes from the API key suffix (e.g. `https://api.au1.cliniko.com/v1`).
- Send a `User-Agent` containing your app name and a contact email, or requests may be blocked.
- Rate limit: 200 requests/minute per user; on 429 wait until `X-RateLimit-Reset`.
- Lists are paginated (default 50, max 100 via `per_page`); follow `links.next`.
- No idempotency keys are documented: before retrying a create (POST), search for the record first to avoid duplicates.
- This is patient health data: request only what the task needs.
