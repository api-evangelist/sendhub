---
name: Read the two-way texting inbox
description: List inbox threads and read a conversation's messages to power two-way texting workflows.
api: openapi/sendhub-openapi-original.yml
operations: [listThreads, getThread, getSettings]
---

# Read the SendHub inbox

## Auth
`username` + `api_key` query params, or HTTP Basic. See `authentication/sendhub-authentication.yml`.

## Steps
1. `listThreads` (`GET /v1/inbox`) — list conversation threads (offset-paginated;
   `threadUnread` flags unread activity).
2. `getThread` (`GET /v1/threads/{thread_id}`) — read the messages in a thread.
3. `getSettings` (`GET /v1/settings`) — to react to inbound messages in real time, read
   the `sms_webhook_url` setting; SendHub POSTs incoming messages to that URL
   (see `asyncapi/sendhub-webhooks.yml`).

## Rules
- Poll with pagination (`limit`/`offset`, `meta.next`) or prefer the inbound-SMS webhook
  over polling. Errors use the JSON envelope in `errors/sendhub-error-codes.yml`.
