---
name: patient-billing-history
description: Retrieve a patient's invoices and invoice detail.
api: Cliniko API
generated: '2026-10-21'
method: generated
source: openapi/cliniko-openapi.yml
operations:
- GetPatient-Get
- ListInvoicesForPatient-Get
- GetInvoice-Get
---

# Patient Billing History

## Steps
1. Confirm the patient with `GetPatient-Get`.
2. List invoices with `ListInvoicesForPatient-Get` (paginate via links.next).
3. Fetch detail with `GetInvoice-Get`. Invoices are read-only through the public API.

## Rules
- Auth: HTTP Basic, API key as username, blank password. Base URL shard comes from the API key suffix (e.g. `https://api.au1.cliniko.com/v1`).
- Send a `User-Agent` containing your app name and a contact email, or requests may be blocked.
- Rate limit: 200 requests/minute per user; on 429 wait until `X-RateLimit-Reset`.
- Lists are paginated (default 50, max 100 via `per_page`); follow `links.next`.
- No idempotency keys are documented: before retrying a create (POST), search for the record first to avoid duplicates.
- This is patient health data: request only what the task needs.
