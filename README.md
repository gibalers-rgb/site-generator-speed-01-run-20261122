# site-generator-speed-01-run-20261122

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/gibalers-rgb/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/gibalers-rgb/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for makefun-site 1.0.0: `solve.js` answers exactly as
makefun-site's `build` does on every site — same output tree, byte for byte —
and does it in about half the CPU time. On the kit's practice set it is
identical to the reference on all 3 cases, and on 18 further sites drawn from
the competition's own generator it is identical to the reference on every one.

## How it is faster

- The front matter is scanned for its closing line instead of splitting the
  whole source and re-joining the body.
- Markdown is lexed and rendered in one fused pass, with no token objects in
  between; inline markup is scanned in whole runs (a sticky regex) instead of
  one regex per character.
- Slugs are cached per build — titles and tag names repeat across a site —
  with a fast ASCII path beside the Unicode one.
- Link targets are resolved once per source, not once per occurrence.
- Every card and page record is computed once and reused wherever templates
  read it; a `for` loop reuses one scope frame for all its elements.
- `escapeHtml` returns a plain line untouched, and word counts are tallied
  without building an array of words.

Nothing is cached from one case to the next: every `run(input)` builds its
own caches, and the output is a function of the input alone.

## Run it

```sh
node main.ts
```

`main.ts` reads every case from `/input/cases.json` (or `$MAKEFUN_INPUT_DIR`)
and writes `/output/answers.json` (or `$MAKEFUN_OUTPUT_DIR`). Node 22.
Standard library only; no network, no clock, no randomness.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
