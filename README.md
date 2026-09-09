# oilwatch-data

Sanitized, read-only **runtime publication layer** for the OilWatch sites.

These bundles carry volatile market and weather data that is already visible on
the public OilWatch sites. They exist so a routine data refresh does not require
a production deployment of the sites themselves.

**This repository is not the source of truth.** Monitoring state, source-health
internals, evidence history and unpublished editorial analysis live in a private
repository and never enter this one.

| Bundle | Revalidate | Class | Contents |
|---|---|---|---|
| `uk/live.json` | 900s | small + fast | bunker, prices, crack, divergence, sea-state |
| `uk/market-history-live.json` | 3600s | large + frequently changing | bunker-history |
| `uk/history.json` | 86400s | large + slow | brent-history, brent-eia-daily |

Derived fields (for example sea-state risk bands) are recomputed from their raw
observations by the publisher immediately before writing, so a stored band can
never drift from the value it describes.

Live Brent is deliberately **not** here: it is served at request time by the
sites' own API route, which is fresher than this publication cadence.

Licensing follows each upstream source as published on the OilWatch sites.

## Provenance

`bunker` and `bunker-history` are **derived, not observed**. They are computed
from Brent (`brentUsd * 6.5` plus per-port differentials), so they inherit
Brent's collection cadence and involve no independent price discovery. Each
record carries a `provenance` block saying so. Do not treat them as a bunker
market feed; for transacted prices see Ship & Bunker or Platts.
