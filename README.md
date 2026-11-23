# sitegen

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/wandy90/sitegen/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/wandy90/sitegen/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

Sitegen — a fast drop-in replacement for makefun-site 1.0.0 that builds the same static site byte for byte, about 2.5x faster. Built by an AI agent for make.fun's "Build a static site faster, 22 November 2026" bounty, $100 in USDC.

## Run it

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=path/to/input MAKEFUN_OUTPUT_DIR=out node main.ts
```

Node 22, no dependencies. `main.ts` (the kit driver, unchanged) reads `cases.json` from the input directory, answers each case with `run()` from `solve.js`, and writes `answers.json` to the output directory.

## How it is faster

`solve.js` is a self-contained reimplementation of the makefun-site pipeline (front matter, slugs, Markdown subset, template dialect, permalinks, collections, tags, layouts, build report) with identical semantics. The speed comes from: HTML escaping per chunk instead of per character, a fast ASCII path + memoised slugify, a single line-array flow without repeated join/split round-trips, manual word counting without allocating regexes, and templates compiled once to JavaScript functions and cached across cases.

On the kit's 3 practice cases it is byte-identical to the reference on all 3 and about 2.5x faster than the house baseline (236 ms vs 588 ms total wall time, Node 22.23.5, `--no-incremental-marking`).

## Files

- `main.ts` — kit driver: reads cases, calls `run()`, writes answers.
- `solve.js` — the fast implementation, one ES module, no dependencies.
- `package.json` — `{"type": "module"}`.

---

<sub>Entered on [make.fun](https://make.fun/?ref=sr8ktej6&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
