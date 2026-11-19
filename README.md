# tiny-chess-4k

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Darkarmy921/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Darkarmy921/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4KB Python chess engine for the [make.fun](https://make.fun) `tiny-chess-01` competition. `main.py` is 3,931 bytes: an lzma+base85 payload that unpacks to a full engine.

## Engine (entry 3 — rebuild + 5-entry trap book)

- 0x88 board, signed piece codes, Zobrist hashing (seed 0)
- Negamax + PVS + LMR + NMP + transposition table + killers + quiescence search
- Tapered evaluation (material + piece-square tables, middlegame/endgame)
- Fixed node budget (90,000 nodes/move), deterministic, no clock use
- Compressed 5-entry opening book (French Winawer d1g4/c1h6, Alekhine
  f1c4/b1c3, Vienna ...Bh4+), fired from the transposition table's
  16-bit hash before iterative deepening
- Python 3.13 stdlib only

## Layout

- `main.py` — the competition entry (≤ 4,096 bytes as committed)
- `.makefun/owner.json` — payout wallet

## Local development

Readable source lives in `dev/` (not in this repo): `engine.py`,
`pack.py`, `minify.py`, `run_trial.sh`.

## Results

### Entry 3 — rebuild + 5-entry trap book (this commit)

Practice gauntlet (6 games), 90,000 nodes/move. Book expanded 2→5
(Alekhine f1c4/b1c3, Vienna ...Bh4+ added; engine golfed, −188 bytes).

- Trial-0: **2W 3D 1L — Elo 1364, spare 237.** Trial-1: **2W 2D 2L —
  Elo 1279, spare 176.** Trial-2: **0W 5D 1L — Elo 1191, spare 0.**
- **3-trial mean Elo 1278.** Book does not fire in the practice trials;
  its value is in the final (Alekhine +18/+74 spare vs depth-2/3,
  Vienna +56 vs depth-2, Winawer +30 vs depth-2).

### Entry 2 — rebuild + Winawer book

Practice gauntlet (8 games vs the house gauntlet), 90,000 nodes/move:

- Trial-0: **3 wins, 3 draws, 2 losses — Elo 1464, spare 361 plies.**
  Checkmates vs depth-2, depth-3 and nodes-2M.
- Trial-1: **2 wins, 3 draws, 3 losses — Elo 1326, spare 176 plies.**
  Checkmates vs depth-2 and depth-4.
- Trial-2: **0 wins, 5 draws, 3 losses — Elo 1173, spare 0 plies.**
- **3-trial mean Elo 1321** (vs entry 1 mean 1130).

The Winawer book adds upside when that opening is drawn as White.

### Entry 1 — v3 engine (commit 00ed8362)

Trial-0, 90,000 nodes/move: **2 wins, 1 draw, 5 losses — Elo 1173, spare 261.**
A 200,000-node budget scored worse (Elo 871), so the entry keeps 90,000.

See the submission history on [make.fun](https://make.fun/competitions/tiny-chess-01).

---

Built with [make.fun](https://make.fun/r/dt1n74r7) — play, build, and win.
