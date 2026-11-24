[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/tzf12138/tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/tzf12138/tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess-01

A 4,096-byte Python chess engine for the make.fun tiny-chess-01
competition. `main.py` is the whole engine: it plays every game through
the env, searching with alpha-beta + PVS, null-move pruning, LMR,
quiescence, a transposition table, killer + history move ordering, and a
PeSTO tapered evaluation (material + piece-square tables, /8
quantized, LZMA-compressed payload).

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed,
  Python 3.13, standard library only. It reads, imports or execs no other
  file in the repository.
- Compression is allowed and counted. `main.py` unpacks and execs an
  LZMA payload (`lzma`); the payload and the unpacker are both part of
  its 4,096 bytes. It was produced by the kit's own command:
  `node cli/makefun.mjs compress <source> --out main.py`.
- The rest of the tree (this README, LICENSE) is at most 8,192 bytes in
  all, and no file over 4,096.
- Search uses a fixed node budget per opponent, never a clock: trials
  are replayed and must play the same moves.

## Local results

Practice trials vs the house engines (local env, `--driver process`):
trial-0 4W 2L (Elo 1451), trial-1 2W 4L (Elo 1099), trial-2 3W 3L
(Elo 1279); mean practice Elo ~1276. Per-move think time ~1-5 s,
within the 10 s limit.

## Grade it locally

You need the `makefun` CLI. Download the competition's dev kit,
unpack it, and from inside it run:

    makefun grade-local --kit . --bot <your repository> --driver process

It plays one practice trial against the house engines, replays it, and
prints the Elo the scorer gives it. Before you submit, `makefun check`
runs the site's own pre-submit checks, size caps included.
