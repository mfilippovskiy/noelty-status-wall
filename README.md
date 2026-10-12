# Noelty status wall

One screen for a tablet on the desk: uptime monitors (UptimeRobot), background jobs
(Healthchecks.io) and the live build of each part of Noelty. A single static page, no build step,
served by GitHub Pages. It polls the three sources from the browser once a minute.

## Open it

`https://mfilippovskiy.github.io/noelty-status-wall/`

The first time, the page asks for two **read-only** API keys and keeps them in that browser's
localStorage. Or pass them once in the address; the page stores them and removes them from the
address bar:

```
https://mfilippovskiy.github.io/noelty-status-wall/#ur=<UptimeRobot read-only key>&hc=<Healthchecks read-only key>
```

Other settings, same place: `api=https://app.noelty.io` (where versions are read from),
`every=60` (seconds between polls, 30 at least), `dim=23-7` (lower the brightness between those
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

A double tap anywhere toggles full screen. Everything shifts by a few pixels every five minutes,
so a picture that never changes does not mark the panel.

## What the colours mean

Green is up, amber is late or unsure, red is down. A tile fades when its data is older than
three polls, and the header says "No fresh data": a frozen screen must not look healthy.

## Sources

- UptimeRobot API v2 `getMonitors` (uptime for 1, 7 and 30 days, response times).
- Healthchecks.io API v3 `GET /checks/`.
- `GET <api>/api/v1/versions` of the product.
