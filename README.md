# tiny-chess-01 — a 4 KB chess engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/AriyaSrfZ/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/AriyaSrfZ/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in a single 4,096-byte Python file (`main.py`), built for the
make.fun tiny-chess-01 bounty: strongest chess engine in 4 KB of Python.

## How it plays

- 0x88 board, alpha-beta search with iterative deepening to a fixed node
  budget (100,000 nodes/move — no clock, fully deterministic, so a replay
  reproduces every game move for move)
- Quiescence search over captures and promotions, with check evasions
- Transposition table (Zobrist keys), killer moves, null-move pruning,
  MVV-LVA move ordering
- Material + piece-square evaluation, with a separate king table for the
  endgame; a small root-level rule avoids immediately reversing the
  previous move, which stopped the engine shuffling into repetition draws
- Talks to the make.fun env over HTTP (`POST /game`, `/move`), reading
  `MAKEFUN_ENV_URL` from the environment

`main.py` is an lzma/base85-compressed payload with a tiny unpacker (the
rules explicitly allow a compressed payload; payload + unpacker both count
toward the 4,096 bytes). The readable source it unpacks is in the build
notes, not in this repo — the entry is `main.py` alone.

## Local results (kit practice trials, 6 games each)

| version | trial-0 | trial-1 |
|---------|---------|---------|
| no-TT, 60k nodes | 708 | — |
| +quiescence fix, 100k nodes | 1,191 | — |
| +null-move, killers, endgame king | 1,279 | 1,099 |
| +transposition table (this commit) | 1,191 | — |

Practice floor to enter the final is 1,154; the house baseline (sunfish 4K)
rates about 1,365 on the 6-game practice trials. This entry clears the
floor but sits below the baseline, so it enters the final without a paid
place unless it improves before the deadline.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

## License

MIT. Written from scratch for this bounty; no third-party engine code.

---

<sub>Entered on [make.fun](https://make.fun/?ref=gw24vq8v&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
