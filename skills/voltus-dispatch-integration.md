---
name: Receive and act on Voltus grid dispatches
description: Poll the Voltus API for grid dispatches and drive site curtailment, using the real dispatch lifecycle (updates, cancellations, commitments).
api: openapi/voltus-openapi.yml
operations:
  - voltus#get-sites
  - voltus#get-dispatches
  - voltus#get-dispatch
---

# Receive and act on Voltus grid dispatches

Use this when an agent or service must react to Voltus demand-response events for
enrolled sites.

## Setup

- Base URL: `https://api.voltus.co/2022-04-15` (production) or
  `https://sandbox.voltus.co/2022-04-15` (sandbox).
- Every request carries `X-Voltus-API-Key: <key>`. Production keys come from your
  Voltus account manager and are scoped to specific sites. The sandbox key
  `secret` is public and works only against `sandbox.voltus.co`.
- Build against the sandbox first. It is anonymous and returns a canned MISO
  Operating Reserves dispatch.

## Steps

1. **Confirm your site inventory** - call `voltus#get-sites`
   (`GET /2022-04-15/sites`). Map each `id` to your own asset by
   `customer_location_id` (the identifier you gave Voltus, e.g. a store number).
   Each site carries `meters[]` with an `asset_type` of `load`, `battery`,
   `generator` or `poi`.
2. **Poll for dispatches** - call `voltus#get-dispatches`
   (`GET /2022-04-15/dispatches`). A 1-minute cadence is the pattern Voltus
   recommends for its OpenADR VENs and is a safe default here.
3. **Decide per dispatch**:
   - Skip it when `authorized` is `false` - that is a cancelled dispatch; ramp
     the sites back up if you had already started curtailing.
   - It is in progress when `start_time <= now` and (`end_time` is null or
     `end_time >= now`).
   - `end_time` may be **absent** on ancillary-services dispatches and is added
     later as an update. Never assume it is present.
4. **Curtail per site** - for each entry in `sites[]`, reduce load by
   `commitment` kW. Use `commitment`, not `drop_by`: `drop_by` was deprecated on
   2023-12-13 and carries the same value only for backwards compatibility.
5. **Re-read on change** - `modification_number` increments every time a dispatch
   is updated and `start_time`/`end_time` can move earlier or later. Track
   `(id, modification_number)` and re-apply on any increase. Fetch a single
   dispatch with `voltus#get-dispatch` (`GET /2022-04-15/dispatches/{id}`).
6. **Respect the program context** - `program` gives `market` (e.g. MISO),
   `program_type` (e.g. `ancillary_services`) and `timezone`. Ancillary-services
   programs dispatch with little notice; capacity programs give more.

## Rules

- Handle every dispatch idempotently. Voltus documents no idempotency key; the
  de-duplication key you actually have is `id` + `modification_number`.
- All times are RFC 3339, UTC on the wire.
- Errors are `{"message": ..., "type": ...}` where `type` is the HTTP status
  phrase. Back off on `429` using `x-ratelimit-remaining` / `x-ratelimit-reset`
  (observed limits: 301 production, 101 sandbox).
- A `403` (`Permission denied`) means the key is not entitled to that account or
  site - not a retryable condition.
- Prefer webhooks over polling once you are stable: see
  `skills/voltus-webhook-dispatch-listener.md`.

## Reference

- Tutorial: https://api.voltus.co/docs/tutorials/get-sandbox-dispatches
- Example code: https://github.com/voltusdev/voltus-api-examples/blob/main/polling/poller.py
- Conventions: `conventions/voltus-conventions.yml`
