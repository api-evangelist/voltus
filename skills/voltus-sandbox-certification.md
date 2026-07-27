---
name: Certify a Voltus integration through the sandbox
description: Work the Voltus sandbox scenario suite and the production test dispatch that gate access to real sites.
api: openapi/voltus-openapi.yml
operations:
  - voltus#get-dispatches
  - voltus#get-dispatch
  - voltus#post-dispatch
---

# Certify a Voltus integration through the sandbox

Voltus will not schedule a production dispatch verification until an integration
has passed every sandbox scenario. This skill is that path.

## Steps

1. **Smoke test anonymously** - point at `https://sandbox.voltus.co` with header
   `X-Voltus-API-Key: secret` and call `voltus#get-dispatches`. You get one canned
   MISO Operating Reserves dispatch over sites `asdf` and `lkjh`. No account is
   needed for this tier.
2. **Get a sandbox key** - a Voltus-issued sandbox key unlocks live dispatch
   simulations against your own sandbox sites.
3. **Run the scenario suite** - create scenarios with
   `POST https://sandbox.voltus.co/2022-04-15/scenarios` (driven by the Marimo
   notebook at
   https://api.voltus.co/docs/tutorials/test-your-dispatch-integration). Posting a
   new scenario overwrites the previous one; "cancel" one by posting a scenario
   with `notification_time` a year in the past. Work through, at minimum:
   - Basic dispatch (start and end known up front)
   - Ancillary-services dispatch (no end time at creation, end added ~10 minutes
     later as an update, optional `metadata` present)
   - Two dispatches (non-abutting and overlapping, via
     `minutes_between_dispatches`)
   - Cancelled dispatch (via `cancel_time`, arriving as `authorized: false`)
4. **Assert against your own behaviour** - for each scenario, poll
   `voltus#get-dispatches` / `voltus#get-dispatch` and confirm your controller
   curtails to `commitment` by `start_time`, tracks `modification_number`
   increases, and ramps back up on cancellation or a moved `end_time`.
5. **Production integration test** - switch to `https://api.voltus.co` with your
   production key and self-schedule a test dispatch with `voltus#post-dispatch`
   (`POST /2022-04-15/dispatches`):

   ```json
   {"start_time": "<future RFC 3339>", "end_time": "<after start_time>"}
   ```

   Both times must be in the future and within the next 72 hours or the request
   fails. Optionally pass `program_id` to dispatch the groups registered under a
   real program instead of the default test program.
6. **Per-site dispatch verification** - Voltus schedules an end-to-end production
   verification for each new site. Coordinate with your account manager.

## Rules

- `POST /dispatches` in production issues a real curtailment instruction to real
  sites. Treat it as a safety-critical action: require human approval, never let
  an unattended agent call it. See `agentic-access/voltus-agentic-access.yml`.
- The public sandbox key never works against production - it returns
  `401 Permission denied`.

## Reference

- https://api.voltus.co/docs/tutorials/create-a-dispatch-integration
- https://api.voltus.co/docs/concepts/start-earning-with-voltus
- `sandbox/voltus-sandbox.yml`
