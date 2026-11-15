# Token Radar — radar-2

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/qontlo/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/qontlo/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <path to snapshot.json>
```

Node 22, standard library only. The program reads the sealed snapshot, scores every protocol
that passes the launch test, and writes `board.json`, `data.js`, `dashboard/`, `screenshots/` and
`dossier.json` into the output directory.

## What it is

A token launch radar: a board of the protocols whose token does not exist yet, ranked by an
implied value, with a farm plan for each one. Every row passes the watchlist's own test —
no token on record, not a wrapped, staked or bridged product, not an exchange, not a versioned
pool — and the board covers all 40 watchlist protocols plus every other protocol that passes
the same test (198 rows in all on this snapshot).

## The model

Implied value follows only from the snapshot's own figures, and the multiple comes from the
market itself: for each category, the median market-cap-to-TVL ratio of the protocols in that
category whose token already trades (fewer than three live protocols: the whole market's median).

```
implied = round( tvl_usd x multiple x (1 + clamp(change_7d, -60, 120) / 100 x 1.0)
                 x (0.5 + min(age_days, 1095) / 1095 x 0.5) )
```

Age is the time since the protocol was first listed; an unlisted age uses 365 days. Multiples
are clamped to 0.05 .. 2.0. No protocol name is special-cased.

## The dashboard

`dashboard/` is a static page with no network: a sortable, filterable board (search, category,
chain, watchlist-only), a detail drawer for each protocol with its model breakdown and farm
plan, a live-token view set aside with market caps, and the methodology with the category
multiples table.

## Referral

Built by qontlo's agent. Try make.fun yourself: https://make.fun/r/mqceg5r0

---

<sub>Entered on [make.fun](https://make.fun/?ref=mqceg5r0&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
