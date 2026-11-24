# site-generator-speed (Rust/WASM)

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/tye59870-create/site-generator-speed-wasm/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/tye59870-create/site-generator-speed-wasm/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for makefun-site 1.0.0, make.fun's own static site generator.
`solve.js` returns, for every case's input, exactly the output tree that
makefun-site 1.0.0's build returns for the case's site — each page's
`index.html`, the index and tag pages, `build-report.txt` and each static
file, byte for byte — only faster.

The generator is written in Rust and compiled to WebAssembly
(`wasm32-unknown-unknown`). The compiled module is embedded in `solve.js`
as a base64 string and instantiated at import time; the input is passed to
the module through a compact binary encoding (length-prefixed UTF-8 strings,
u32/f64 little-endian) and the output tree is read back the same way, so no
JSON round-trip is needed. Front-matter parsing, the custom Markdown subset,
the template dialect, and the slug rule (Unicode normalisation) are all
ported to byte-identical behaviour.

## Run it

```sh
node main.ts
```

`main.ts` reads every case from `/input/cases.json` (or
`$MAKEFUN_INPUT_DIR`), answers each with `run` from `solve.js`, and writes
`/output/answers.json` (or `$MAKEFUN_OUTPUT_DIR`). Node 22, standard
library only.

## Test it

The kit's practice set (3 cases) is graded locally with the kit's own
grader, run from the unpacked kit:

```sh
node cli/makefun.mjs grade-local --kit . --bot <repository> --driver process --worktree
```

It prints the scorer's `scores.json`. This build passes all 3 practice
cases (`passed: 1`, `cases_passed: 3`, `cases_total: 3`), reproduced byte
for byte across the two executions.

## License

MIT. See [LICENSE](LICENSE).
