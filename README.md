# tiny-chess-4k

A 4KB Python chess engine for the [make.fun](https://make.fun) `tiny-chess-01` competition. `main.py` is 4,095 bytes: an lzma+base85 payload that unpacks to a full engine.

## Engine (entry 2 — rebuild + opening book)

- 0x88 board, signed piece codes, Zobrist hashing (seed 0)
- Negamax + PVS + LMR + NMP + transposition table + killers + quiescence search
- Tapered evaluation (material + piece-square tables, middlegame/endgame)
- Fixed node budget (90,000 nodes/move), deterministic, no clock use
- Compressed 2-entry French Winawer opening book (d1g4, c1h6), fired from
  the transposition table's 16-bit hash before iterative deepening
- Python 3.13 stdlib only

## Layout

- `main.py` — the competition entry (≤ 4,096 bytes as committed)
- `.makefun/owner.json` — payout wallet

## Local development

The readable source lives in `dev/` (not part of this repo): `engine.py` is the
engine, `pack.py` builds `main.py`, `minify.py` is the token minifier, and
`run_trial.sh` runs the practice gauntlet locally.

## Results

### Entry 2 — rebuild + Winawer book (this commit)

Practice gauntlet (8 games vs the house gauntlet), 90,000 nodes/move:

- Trial-0: **3 wins, 3 draws, 2 losses — Elo 1464, spare 361 plies.**
  Checkmates vs depth-2, depth-3 and nodes-2M.
- Trial-1: **2 wins, 3 draws, 3 losses — Elo 1326, spare 176 plies.**
  Checkmates vs depth-2 and depth-4.
- Trial-2: **0 wins, 5 draws, 3 losses — Elo 1173, spare 0 plies.**
- **3-trial mean Elo 1321** (vs entry 1 mean 1130).

The book fires on the French Winawer line (d1g4, c1h6), turning a draw
into a checkmate in 41 plies (spare 159) against the depth-2 anchor.
The Elo gain over entry 1 comes from the rebuilt engine (NMP, TT root-store
fix, no bishop-pair bonus); the book adds Winawer upside when that opening
is drawn as White in the final.

### Entry 1 — v3 engine (commit 00ed8362)

Practice trial-0 (8 games vs the house gauntlet), engine v3 at 90,000 nodes/move:
**2 wins, 1 draw, 5 losses — Elo 1173, spare 261 plies.** Beats the depth-2 and
depth-3 anchors, draws depth-4, loses to the nodes-100k and stronger anchors.

A 200,000-node budget was tested on the same trial and scored worse
(1 win, 0 draws, 7 losses — Elo 871), so the entry keeps the 90,000-node budget.

See the submission history on [make.fun](https://make.fun/competitions/tiny-chess-01).

---

Built with [make.fun](https://make.fun/r/dt1n74r7) — play, build, and win.
