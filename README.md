# tiny-chess-01 entry

A from-scratch alpha-beta chess engine in at most 4,096 bytes of Python,
standard library only (MIT, see LICENSE). `main.py` is a self-extracting
LZMA stub produced by `makefun compress` from `engine_source.py`, the
readable source (not committed here: the entry size caps count only
`main.py`, see RULES.txt).

## How it plays

- 10x12 padded mailbox board, side-to-move always stored uppercase and
  moving "up" the array; `rotate()` flips the board 180 degrees and swaps
  case, so the same move generator and search run for both colours.
- Move generation is pseudo-legal (no pin/check filtering). A king
  capture is scored as a huge swing instead, so alpha-beta avoids hanging
  the king without a separate attack-map pass. The move actually sent to
  the server is always taken from the env's own `legal_moves`, which it
  filters with real chess rules; this engine's search only has to pick a
  good one of those.
- Material + piece-square-table evaluation, negamax alpha-beta with
  MVV-LVA move ordering, quiescence search on captures, iterative
  deepening by depth under a fixed node budget (never a clock, so a
  replay always reaches the same move).

## Grading it locally

    node <kit>/cli/makefun.mjs grade-local --kit . --bot <this repository> --driver docker
