---
name: Register a Voltus webhook and handle dispatch callbacks
description: Replace dispatch polling with push notifications by registering an HTTPS callback and handling dispatch.create / dispatch.update at-least-once.
api: openapi/voltus-openapi.yml
operations:
  - voltus#post-webhook
  - voltus#get-webhooks
  - voltus#delete-webhook
  - voltus#get-dispatch
---

# Register a Voltus webhook and handle dispatch callbacks

## Setup

Same auth as every Voltus call: `X-Voltus-API-Key`. Your listener must be a
publicly reachable HTTPS endpoint.

## Steps

1. **Register the callback** - `voltus#post-webhook`
   (`POST /2022-04-15/webhooks`) with:

   ```json
   {
     "url": "https://example.com/listeners/voltus",
     "events": [{"name": "dispatch.create"}, {"name": "dispatch.update"}]
   }
   ```

   `dispatch.create` and `dispatch.update` are the only supported events.
2. **Verify** - `voltus#get-webhooks` (`GET /2022-04-15/webhooks`) lists what is
   registered, each with its `id`, `url` and `events[]`.
3. **Handle a delivery** - the POST body is a pointer, not the dispatch:

   ```json
   {"event": {"name": "dispatch.create"}, "resource": "/2022-04-15/dispatches/asdf"}
   ```

   Fetch `resource` with your API key (`voltus#get-dispatch`) to get the full
   dispatch, then run the same decision logic as
   `skills/voltus-dispatch-integration.md`.
   A ping with an empty `event.name` and empty `resource` is a liveness check -
   answer `200`.
4. **Acknowledge** - return a `2xx`. A non-2xx causes redelivery, so **every
   handler must be idempotent**: Voltus's own example warns a callback "could get
   called twice if the first time you return a non-200 response".
5. **On `dispatch.update`** - re-fetch and compare. If `end_time` is now in the
   past, or `authorized` is `false`, treat it as a cancellation and ramp back up.
6. **Remove** - `voltus#delete-webhook` (`DELETE /2022-04-15/webhooks/{id}`).
   This is permanent: "It cannot be undone."

## Rules

- There is no signature header and no shared secret. Treat the callback as
  unauthenticated: never act on the payload body itself, always re-fetch the
  dispatch over an authenticated call, and restrict your listener by network
  policy where you can.
- Ordering is not guaranteed. Use `modification_number` to discard stale updates.
- Keep the polling path as a fallback - webhooks and `GET /dispatches` return the
  same dispatch objects.

## Reference

- Reference: https://api.voltus.co/docs/openapi/webhooks
- Example listener: https://github.com/voltusdev/voltus-api-examples/blob/main/webhooks/webhook-server.py
- Event catalog: `asyncapi/voltus-webhooks.yml`
