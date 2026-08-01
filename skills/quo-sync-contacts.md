---
name: Sync contacts into Quo
description: Create and update Quo contacts from an external source, correlating by externalId.
api: Quo Public API v1
base_url: https://api.quo.com
auth: apiKey in Authorization header (no Bearer prefix)
operations:
  - getContactCustomFields_v1
  - listContacts_v1
  - createContact_v1
  - updateContactById_v1
---

# Sync contacts into Quo

Perform a one-way sync of contacts from an external system (CRM, spreadsheet) into a Quo workspace.

## Steps

1. **Discover custom fields.** `GET /v1/contact-custom-fields` (`getContactCustomFields_v1`) to learn which business-specific fields exist. Custom fields can only be *created* inside Quo, but you can populate them via the API.
2. **Check for an existing contact.** `GET /v1/contacts` (`listContacts_v1`) filtered by `externalIds` + `sources` to see whether the record already exists.
3. **Create or update.** If absent, `POST /v1/contacts` (`createContact_v1`) with `defaultFields` (name, company, role, emails, phone numbers), any `customFields`, and an `externalId`/`source` for correlation. If present, `PATCH /v1/contacts/{id}` (`updateContactById_v1`).

## Rules

- Auth: raw API key in the `Authorization` header (no `Bearer`).
- Always set a stable `externalId` + `source` so future syncs match instead of duplicating.
- Invalid custom fields return `400`/`422` — validate against `getContactCustomFields_v1` first.
- Rate limit is 10 req/s per key; batch large syncs with throttling and `429` backoff.
