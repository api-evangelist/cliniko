# Cliniko (cliniko)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
