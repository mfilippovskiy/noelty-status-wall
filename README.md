# Noelty status wall

One screen for a tablet on the desk: uptime monitors (UptimeRobot), background jobs
(Healthchecks.io) and the live build of each part of Noelty. A single static page, no build step,
served by GitHub Pages. It polls the three sources from the browser every 20 seconds.

## Open it

`https://mfilippovskiy.github.io/noelty-status-wall/`

The first time, the page asks for two **read-only** API keys and keeps them in that browser's
localStorage. Or pass them once in the address; the page stores them and removes them from the
address bar:

```
https://mfilippovskiy.github.io/noelty-status-wall/#ur=<UptimeRobot read-only key>&hc=<Healthchecks read-only key>
```

Other settings, same place: `api=https://app.noelty.io` (where versions are read from),
`every=20` (seconds between polls, 20 at least), `dim=23-7` (lower the brightness between those
local hours). `#demo` shows made-up data, `#reset` forgets the keys.

## Keys

- Only read-only keys. A main UptimeRobot key or a read-write Healthchecks key must never be
  typed here: whoever holds the tablet holds the key.
- UptimeRobot answers a browser only for read-only and monitor-specific keys.
- A Healthchecks read-only key does not reveal ping URLs.
- This repository holds no keys. Do not commit one.

## Keeping the screen on

The page asks the browser for a screen wake lock (Chrome on Android, HTTPS). The chip in the top
right says whether it holds; if it reads "tap: keep screen on", tap it (it also goes full
screen). The lock is dropped when the page is hidden and taken again when it is back. As a
second belt: Android → Developer options → Stay awake (the screen stays on while charging).

A double tap on an empty spot toggles full screen. Everything shifts by a few pixels every five minutes,
so a picture that never changes does not mark the panel.

## Details of a monitor or a job

Tap a monitor or a background job (not Versions) to open its detail screen: what it is in plain
words, what breaks when it is red and where to look, its numbers, and its history. For a monitor
that is uptime with downtime for 24 hours, 7 and 30 days, response times for the last 24 hours
and the recent up/down events with their reasons. For a job it is when the last report came,
when the next is due, when it turns red, and when it went red before.

The plain-words texts live in the page (`ABOUT_MONITORS`, `ABOUT_JOBS`) and are matched on the
name; a new monitor or job gets a general text until one is added. A job's description in
Healthchecks, if set, replaces the built-in one.

**Back** (or the tablet's back button, or Esc) returns to the wall; with nobody touching it the
screen goes back by itself after 2 minutes. The open screen is in the address (`#show=…`), so a
reload keeps it.

## What the colours mean

Green is up, amber is late or unsure, red is down. A tile fades when its data is older than
three polls (and never sooner than 3 minutes), and the header says "No fresh data": a frozen
screen must not look healthy.

## Sources

- UptimeRobot API v2 `getMonitors` (uptime and downtime for 1, 7 and 30 days, response times for
  24 hours, the last 20 log events).
- Healthchecks.io API v3 `GET /checks/`, and `GET /checks/<unique_key>/flips/` for the job on the
  detail screen. A read-only key reads flips but not individual pings.
- `GET <api>/api/v1/versions` of the product.
