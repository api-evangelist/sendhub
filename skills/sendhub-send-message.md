---
name: Send an SMS to contacts or groups
description: Authenticate to SendHub and send a text message to one or more contacts or groups, then check delivery.
api: openapi/sendhub-openapi-original.yml
operations: [listContacts, sendMessage, getMessage]
---

# Send an SMS with SendHub

Use SendHub's v1 REST API (`https://api.sendhub.com`) to send a business text message.

## Auth
Every request needs either your line's `username` + `api_key` as query parameters, or
HTTP Basic (username + password/api_key). API access is available on custom plans only.
See `authentication/sendhub-authentication.yml`.

## Steps
1. `listContacts` (`GET /v1/contacts`) — find the contact ids (or group ids via `listGroups`)
   you want to message. Lists are offset-paginated (`limit`/`offset`, `meta.next`).
2. `sendMessage` (`POST /v1/messages`) — body includes `contacts` (array of ids/numbers)
   and/or `groups`, plus `text`. Optionally set `scheduledAt` to schedule.
3. `getMessage` (`GET /v1/messages/{message_id}`) — confirm the message was sent.

## Rules
- Errors return a JSON envelope `{message, code, error, devMessage, moreInfo}` — see
  `errors/sendhub-error-codes.yml`. Handle `429` (rate limited, includes `timeLeft`) and
  `420` (plan API limit reached / no API access on plan).
- There is no idempotency-key header; do not blindly retry a `sendMessage` on timeout —
  check with `getMessage`/`listThreads` first.
