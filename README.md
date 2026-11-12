# tiny-chess-01

A 4 KB chess engine for the make.fun bounty "Build the strongest chess engine
in 4 KB of Python". `main.py` is the whole engine: a self-extracting
(lzma + base85) program, standard library only, no other files read.

## Design

- 0x88 board, piece-square lists, incremental eval + Zobrist hashing.
- Negamax with alpha-beta, transposition table, MVV-LVA capture ordering.
- Quiescence over captures/promotions (all evasions when in check).
- Iterative deepening to a **fixed node budget** (80,000 nodes/move) —
  fully deterministic, never reads a clock, no randomness.
- Eval: material + piece-square tables (house formulas) + bishop pair +
  tapered king safety. Full legal move generation: castling, en passant,
  promotions, check/checkmate/stalemate, repetition + fifty-move.

## Run

The env serves `POST /game`, `/move`, `/resign` at `$MAKEFUN_ENV_URL`.
Just run it: `python3 main.py`. It plays all 8 gauntlet games and exits.

## Local results (practice trials, official env server)

- dev build, 50,000 nodes/move, trial-0: 3W 1D 4L, MLE Elo 1326.
- golf build, 80,000 nodes/move, trial-0: 3W 2D 3L, MLE Elo 1396
  (avg search 1.47 s/move, max 2.60 s, zero moves over 3 s).
- `main.py` (this file), 80,000 nodes/move: trial-0: 4W 0D 4L,
  MLE Elo 1396, trial wall 896 s, zero illegal moves, exit 0.
  Trial-1: 3W 1D 4L, MLE Elo 1326, wall 724 s, zero illegal moves.
  Two-trial mean Elo: **1361**. See BUILD_LOG.md.

Readable development copies: `dev.py` (commented) and `golf.py` (dense).
`BUILD_LOG.md` records the build, benchmarks, and trial history.
