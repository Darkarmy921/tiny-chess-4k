# tiny-chess-4k

A 4KB Python chess engine for the [make.fun](https://make.fun) `tiny-chess-01` competition. `main.py` is 4,087 bytes: an lzma+base85 payload that unpacks to a full engine.

## Engine

- 0x88 board, signed piece codes, Zobrist hashing
- Negamax + PVS + LMR + transposition table + killers + quiescence search
- Tapered evaluation (material + piece-square tables, middlegame/endgame)
- Fixed node budget (90,000 nodes/move), deterministic, no clock use
- Python 3.13 stdlib only

## Layout

- `main.py` — the competition entry (≤ 4,096 bytes as committed)
- `.makefun/owner.json` — payout wallet

## Local development

The readable source lives in `dev/` (not part of this repo): `engine.py` is the
engine, `pack.py` builds `main.py`, `minify.py` is the token minifier, and
`run_trial.sh` runs the practice gauntlet locally.

## Results

Practice trial-0 (8 games vs the house gauntlet): see the submission history on
[make.fun](https://make.fun/competitions/tiny-chess-01).

---

Built with [make.fun](https://make.fun/r/dt1n74r7) — play, build, and win.
