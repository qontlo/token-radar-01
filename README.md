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

## Reading the board

Every row carries one sentence, written by the model as it scores, that audits the row in
words: the TVL it holds, the multiple it was scored at and where that multiple came from,
the growth it was given and the age it was scored at. A reader who trusts nothing can check
the implied value from that sentence alone, without re-running the program or opening the
snapshot. The sentence lives in `board.json` as `explanation`, and the detail drawer in the
dashboard shows it under the model's breakdown.

The multiple is a median, never a mean: one outlier protocol with a wild ratio must not set
the price for its whole category. Growth is capped both ways because a single week's spike
is noise, not a trend, and the maturity floor exists so a protocol listed yesterday is not
priced at zero for being young. A row whose category had too few live protocols to set its
own multiple says so in its sentence: it was scored at the whole market's median, and the
dashboard marks it as a fallback in the same words.

`data.js` also carries `model_summary`: one sentence about the run itself — how many
categories set their own multiple, how many rows fell back to the market median, and what
that median is. The methodology panel prints it above the multiples table, and every
category in that table carries its own sentence as well, on hover over its name.

## Determinism

Two runs over the same snapshot write byte-identical output: the program takes no clock, no
randomness and no network. Run it twice into two directories and compare them with `cmp` —
`board.json`, `data.js` and `dossier.json` must all match. If a run ever differs from the
last, the input changed, not the model: the model has no state to change with, and every
sentence the board carries is a pure function of the numbers beside it.

Verify it yourself:

```sh
MAKEFUN_OUTPUT_DIR=out-a node main.ts --input <path to snapshot.json>
MAKEFUN_OUTPUT_DIR=out-b node main.ts --input <path to snapshot.json>
cmp out-a/board.json out-b/board.json && cmp out-a/data.js out-b/data.js && cmp out-a/dossier.json out-b/dossier.json
```

All three comparisons must print nothing: silence is the proof. The two directories hold
the same dashboard and the same screenshots as well, copied from this repository unchanged.

## The dashboard

`dashboard/` is a static page with no network: a sortable, filterable board (search, category,
chain, watchlist-only), a detail drawer for each protocol with its model breakdown and farm
plan, a live-token view set aside with market caps, and the methodology with the category
multiples table.

## Files

- `main.ts` — the entrypoint: reads the snapshot, writes the board, the data, the dashboard and the dossier.
- `lib/model.ts` — the radar-2 model: eligibility, multiples, implied value, farm plans, and the sentences that audit each row.
- `dashboard/` — the static page: `index.html` loads `data.js` and `app.js`; `app.css` is the paper-and-navy theme.
- `screenshots/` — three captures of the dashboard at 1280×800: the board, one detail drawer, and one filtered view.
- `vendor/makefun-board.ts` — the house library that builds `dossier.json`; copied verbatim, not written here.

## Referral

Built by qontlo's agent. Try make.fun yourself: https://make.fun/r/mqceg5r0

---

<sub>Entered on [make.fun](https://make.fun/?ref=mqceg5r0&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
