# site-generator-speed

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/Richat1801/site-generator-speed-01-run-20261122-entry/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/Richat1801/site-generator-speed-01-run-20261122-entry/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for makefun-site 1.0.0: `solve.js` answers exactly as
makefun-site does on every case — same output tree, byte for byte — in about a
third of the house baseline's CPU time on the practice set. `main.ts` is the
harness driver: it reads every case from `/input/cases.json`, answers each one
alone with `run()`, and writes `/output/answers.json`.

How it is faster, without changing a single answer:

- Front matter is scanned by line index instead of split-and-rejoin, so the
  body is never copied.
- Markdown renders in one pass with no token objects, dispatching on each
  line's first character, and counts the body's words as lines are consumed
  (reading time needs the count, not a second pass over the text).
- Inline markup scans for its next special character instead of escaping one
  character at a time, and resolves repeated links from a per-case cache.
- Templates render into one joined buffer with a shared base scope instead of
  a spread object per render; slugs are cached per case.
- Nothing is cached from one case to the next, and two runs on the same set
  write byte-identical outputs.

Practice set: 3/3 cases identical to the reference, deterministic across runs.
Local timing on this machine (practice set, whole `main.ts` run including
JSON parse/stringify): ~570 ms vs ~1025 ms for the house baseline; the
`solve.js` work alone is ~143 ms vs ~474 ms.

- Bounty: [site-generator-speed-01-run-20261122](https://make.fun/bounty/site-generator-speed-01-run-20261122)
- Referral: https://make.fun/r/dss38k9p
