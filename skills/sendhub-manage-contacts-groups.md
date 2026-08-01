---
name: Manage contacts and groups
description: Create and update SendHub contacts, organize them into groups, and manage group membership.
api: openapi/sendhub-openapi-original.yml
operations: [upsertContact, bulkUpsertContacts, createGroup, addRemoveGroupContacts, listGroupContacts]
---

# Manage SendHub contacts and groups

## Auth
`username` + `api_key` query params, or HTTP Basic. See `authentication/sendhub-authentication.yml`.

## Steps
1. `upsertContact` (`POST /v1/contacts`) — create or update a single contact by number.
   For many contacts at once use `bulkUpsertContacts` (`PATCH /v1/contacts`).
2. `createGroup` (`POST /v1/groups`) — create a group to broadcast to.
3. `addRemoveGroupContacts` (`POST /v1/groups/{group_id}/contacts`) — add or remove
   contacts from the group in one call.
4. `listGroupContacts` (`GET /v1/groups/{group_id}/contacts`) — verify membership
   (offset-paginated).

## Rules
- Upsert semantics dedupe on the contact number, but there is no request idempotency key.
- Validation failures return `400` with the JSON error envelope; see
  `conventions/sendhub-conventions.yml` and `errors/sendhub-error-codes.yml`.
