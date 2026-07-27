---
name: Report and read Voltus site telemetry
description: Submit interval energy telemetry and controllable load for enrolled sites, and read interval data back within Voltus's documented limits.
api: openapi/voltus-openapi.yml
operations:
  - voltus#get-sites
  - voltus#post-telemetry
  - voltus#post-controllable-load
  - voltus#get-telemetry-kw
---

# Report and read Voltus site telemetry

Telemetry is how Voltus measures performance against a dispatch commitment and
how customers get paid, so accuracy and timeliness matter more than volume.

## Steps

1. **Resolve ids** - `voltus#get-sites` (`GET /2022-04-15/sites`) gives you
   `site_id` and, per site, `meters[].id` (the `meter_id`).
2. **Submit metered telemetry** - `voltus#post-telemetry`
   (`POST /2022-04-15/telemetry`) with a `telemetry[]` array. Each reading
   carries `site_id`, `meter_id`, `timestamp` (RFC 3339), `interval_seconds`,
   `value` and `units`. Voltus's native interval is 30 seconds.
3. **Submit controllable load** - `voltus#post-controllable-load`
   (`POST /2022-04-15/telemetry/controllable-load`) with a
   `controllable_load[]` array (`site_id`, `timestamp`, `interval_seconds`,
   `value`, `units`). This is the aggregate kW your integration actually
   controls at the site, and is normally lower than metered load. Voltus uses it
   to tune market offers and reduce underperformance penalties.
4. **Read it back** - `voltus#get-telemetry-kw`
   (`GET /2022-04-15/telemetry/kw`) with `start_time` (required, exclusive),
   `end_time` (optional, inclusive, defaults to +24h), `interval_seconds` (one of
   30, 60, 300, 900, 1800, 3600, 21600) and one `site_id` per site.

## Rules

- **Hard limits per request**: 10 sites, a 90-day interval, 10,000 data points
  per site. Exceeding them returns `413 Content Too Large`. Page by time window,
  then by site.
- `start_time` and `end_time` must be aligned to 30 seconds.
- Read-back returns what had arrived at request time. If a site's telemetry is
  delayed, recent data may be incomplete - Voltus recommends waiting at least 20
  minutes after the end of an hour-long interval before fetching it, or fetching
  smaller intervals to see what is actually there.
- In the sandbox both POSTs answer `200 OK` but persist nothing, and
  `GET /telemetry/kw` returns `{"data":null}` for the fixture sites.

## Reference

- Tutorials: https://api.voltus.co/docs/tutorials/send-telemetry and https://api.voltus.co/docs/tutorials/get-telemetry
- Example: https://github.com/voltusdev/voltus-api-examples/blob/main/telemetry/get_telemetry.py
