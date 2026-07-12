# Cliniko (cliniko)

Cliniko is practice management software for allied health practices and clinics - physiotherapy, osteopathy, chiropractic, podiatry, psychology, massage, and similar - covering patient records, appointment scheduling and online bookings, practitioners and businesses (locations), treatment notes, invoicing, and payments. Cliniko exposes a well-documented public REST API over HTTPS.

## Access model (read this first)

- **Self-serve API key.** Any Cliniko subscriber generates an API key from within Cliniko (under "My Info"). API access is included in every subscription at no separate charge, and there is a 30-day free trial.
- **Region-sharded base host.** The base URL is `https://api.<shard>.cliniko.com/v1`. The shard is the suffix on your API key (for example a key ending `-uk1` is served from `https://api.uk1.cliniko.com`). Keys generated before sharding have no suffix and belong to the `au1` shard. Known shards include `au1`, `au2`, `au3`, `au4`, `uk1`, `eu1`, `us1`, and `ca1`.
- **HTTP Basic auth.** Pass the API key as the username with an empty password (`curl -u API_KEY:`). The header is `Authorization: Basic ` + base64(`api_key` + `:`).
- **User-Agent is required.** Every request must send a `User-Agent` header of the form `APP_VENDOR_NAME (APP_VENDOR_EMAIL)` containing a valid contact email, plus `Accept: application/json`. Requests without a compliant User-Agent may be automatically blocked.
- **Rate limit.** 200 requests per minute per user. Exceeding it returns `429` with an `X-RateLimit-Reset` header (UNIX timestamp).
- **Pagination.** List endpoints return 50 items per page by default, up to 100 via `per_page`, with `total_entries` and a `links` object (`self`, `next`, `previous`).

Example request:

```shell
curl https://api.au1.cliniko.com/v1/patients \
  -u API_KEY: \
  -H 'Accept: application/json' \
  -H 'User-Agent: MyClinicApp (dev@myclinic.example)'
```

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/cliniko/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/cliniko/refs/heads/main/apis.yml)

## Real vs. modeled

This entry is grounded in Cliniko's public documentation ([docs.api.cliniko.com](https://docs.api.cliniko.com) and the [redguava/cliniko-api](https://github.com/redguava/cliniko-api) GitHub repo). Endpoint paths, methods, the sharded base host, auth, headers, rate limit, and the documented example fields are real. The included OpenAPI captures a **representative subset** of the API (patients, individual appointments, bookings, practitioners, businesses, appointment types, invoices, treatment notes) - Cliniko documents 50+ resources. Request-body schemas are representative (assembled from documented examples), and some optional response fields may be omitted.

## Tags

- Practice Management
- Healthcare
- Allied Health
- Appointments
- Scheduling
- Patients
- EHR
- Clinics
- Bookings
- SaaS

## Timestamps

- **Created:** 2026-07-12
- **Modified:** 2026-07-12

## APIs

### Cliniko Patients API

Create, list, retrieve, update, archive, and unarchive patient records - the people who book in for appointments. Patient records carry demographics, contact details, phone numbers, custom fields, privacy-policy consent, marketing consent, IANA time zone, and links to appointments, invoices, and medical alerts.

- **Human URL:** [https://github.com/redguava/cliniko-api/blob/main/sections/patients.md](https://github.com/redguava/cliniko-api/blob/main/sections/patients.md)
- **Base URL:** `https://api.au1.cliniko.com/v1`

#### Tags

- Patients
- Healthcare
- EHR

#### Properties

- [Documentation](https://docs.api.cliniko.com/)
- [API Reference](https://github.com/redguava/cliniko-api/blob/main/sections/patients.md)
- [OpenAPI](openapi/cliniko-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/cliniko.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/cliniko.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Cliniko Appointments API

Book, retrieve, update, and cancel individual appointments, check for scheduling conflicts, and read the unified bookings feed (which returns individual appointments, group appointments, and unavailable blocks). Appointments link a patient, practitioner, business, and appointment type over a start/end window.

- **Human URL:** [https://github.com/redguava/cliniko-api/blob/main/sections/individual_appointments.md](https://github.com/redguava/cliniko-api/blob/main/sections/individual_appointments.md)
- **Base URL:** `https://api.au1.cliniko.com/v1`

#### Tags

- Appointments
- Scheduling
- Bookings

#### Properties

- [Documentation](https://docs.api.cliniko.com/)
- [API Reference](https://github.com/redguava/cliniko-api/blob/main/sections/individual_appointments.md)
- [OpenAPI](openapi/cliniko-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/cliniko.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/cliniko.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Cliniko Practitioners and Businesses API

List and retrieve practitioners (each backed by a user account) and the businesses / physical locations they work from. Read active and inactive practitioners, and the appointment types a practitioner offers or a business supports, to drive scheduling and online-booking availability.

- **Human URL:** [https://github.com/redguava/cliniko-api/blob/main/sections/practitioners.md](https://github.com/redguava/cliniko-api/blob/main/sections/practitioners.md)
- **Base URL:** `https://api.au1.cliniko.com/v1`

#### Tags

- Practitioners
- Businesses
- Locations

#### Properties

- [Documentation](https://docs.api.cliniko.com/)
- [API Reference](https://github.com/redguava/cliniko-api/blob/main/sections/practitioners.md)
- [OpenAPI](openapi/cliniko-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/cliniko.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/cliniko.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Cliniko Appointment Types API

Create, list, retrieve, update, and delete appointment types - the named services a clinic offers, each with a category, colour, duration in minutes, max attendees, online-booking visibility, and links to billable items, products, and treatment note templates.

- **Human URL:** [https://github.com/redguava/cliniko-api/blob/main/sections/appointment_types.md](https://github.com/redguava/cliniko-api/blob/main/sections/appointment_types.md)
- **Base URL:** `https://api.au1.cliniko.com/v1`

#### Tags

- Appointment Types
- Scheduling
- Services

#### Properties

- [Documentation](https://docs.api.cliniko.com/)
- [API Reference](https://github.com/redguava/cliniko-api/blob/main/sections/appointment_types.md)
- [OpenAPI](openapi/cliniko-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/cliniko.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/cliniko.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Cliniko Invoices API

List and retrieve invoices, filtered by issue date, number, or status, and scoped to an appointment, practitioner, or patient. Invoices carry totals, tax and net amounts, discounts, status, and links to the related business, practitioner, patient, and appointment.

- **Human URL:** [https://github.com/redguava/cliniko-api/blob/main/sections/invoices.md](https://github.com/redguava/cliniko-api/blob/main/sections/invoices.md)
- **Base URL:** `https://api.au1.cliniko.com/v1`

#### Tags

- Invoices
- Billing
- Payments

#### Properties

- [Documentation](https://docs.api.cliniko.com/)
- [API Reference](https://github.com/redguava/cliniko-api/blob/main/sections/invoices.md)
- [OpenAPI](openapi/cliniko-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/cliniko.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/cliniko.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Cliniko Treatment Notes API

Create, list, retrieve, update, and delete treatment notes - the clinical notes taken about a patient visit. Notes use a structured sections/questions format (paragraph, text, radio buttons, checkboxes, and more) and are built from treatment note templates.

- **Human URL:** [https://github.com/redguava/cliniko-api/blob/main/sections/treatment_notes.md](https://github.com/redguava/cliniko-api/blob/main/sections/treatment_notes.md)
- **Base URL:** `https://api.au1.cliniko.com/v1`

#### Tags

- Treatment Notes
- Clinical
- EHR

#### Properties

- [Documentation](https://docs.api.cliniko.com/)
- [API Reference](https://github.com/redguava/cliniko-api/blob/main/sections/treatment_notes.md)
- [OpenAPI](openapi/cliniko-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/cliniko.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/cliniko.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [Domain Security](security/cliniko-domain-security.yml)
- [Authentication](authentication/cliniko-authentication.yml)
- [GitHub Organization](https://github.com/redguava)
- [LinkedIn](https://www.linkedin.com/company/cliniko)
- [Website](https://www.cliniko.com)
- [Documentation](https://docs.api.cliniko.com)
- [Plans](plans/cliniko-plans-pricing.yml)
- [Rate Limits](rate-limits/cliniko-rate-limits.yml)
- [Fin Ops](finops/cliniko-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
