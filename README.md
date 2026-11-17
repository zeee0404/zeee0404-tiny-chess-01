# zeee0404-tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/zeee0404/zeee0404-tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/zeee0404/zeee0404-tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in 3,978 bytes of Python: full legal move generation (verified against known perft counts), negamax alpha-beta search with a transposition table, principal-variation search, null-move pruning, late-move reductions, check extensions, and killer moves, over a quiescence search on captures and promotions with delta pruning. The evaluation is a tapered PeSTO-style score — separate midgame and endgame piece values and piece-square tables blended by game phase, with a bishop-pair bonus and explicit insufficient-material draws — and the search also scores positions already seen earlier in the game as draws, so won positions are converted instead of repeated. Move ordering runs transposition-table move, MVV-LVA captures and promotions, killers, then a history heuristic for quiet moves. It searches to a fixed budget of 30,000 nodes a move, spent cumulatively across iterative-deepening iterations — never a clock — so every move is a pure function of the position and a replay reproduces the game exactly. The source travels as an lzma payload that the file's first lines unpack and run.

## Run it

```sh
MAKEFUN_ENV_URL=<env origin> python3 -P -s main.py
```

The engine plays each game the env serves: it answers `POST /game`, picks its move from the position's FEN, and posts it to `POST /move` until the env reports every game is done.

## Revision

An earlier revision added a history heuristic for quiet-move ordering (depth² bonus on a beta cutoff, cleared every move alongside the transposition table).

The strength revision rewrote the evaluation and search. The old evaluation — flat material values, one set of piece-square tables, and a king table that flipped wholesale once non-pawn material dropped under a threshold — became a tapered PeSTO-style evaluation: distinct midgame/endgame piece values (pawn 82/94, knight 337/281, bishop 365/297, rook 477/512, queen 1025/936) and tables for every piece, blended by a phase count capped at 24, plus an insufficient-material draw rule. The search gained principal-variation search (null-window probes with re-search, at the root as well), null-move pruning, late-move reductions, check extensions, and two killer moves per ply; quiescence now searches captures and promotions only (all evasions when in check) with delta pruning and a ply cap. The 30,000-node budget is now spent cumulatively over all iterative-deepening iterations instead of resetting per depth, and the engine rebuilds the game's position history from the env's move list so that a position already played earlier in the game scores as a draw inside the search — the previous version repeated its way out of won positions. Mate handling stays fastest-mate (mate scores are distance-adjusted).

Measured results: in self-play against the previous engine at an equal 6,000-node budget over 32 games from varied book openings, the new engine scored 20 wins, 12 draws, 0 losses (81%, ≈ +255 Elo). In local runs of the competition's own practice gauntlet (six games against the depth and node-budget anchor engines), it scored Elo 1654, 1191 and 1279 on the three practice instances — the previous engine scored 876 on the first of them — with the packed entrypoint itself playing the third run end to end. Perft was re-verified exact after the rewrite on the five standard positions (start position, Kiwipete, and the three classic en-passant/pin/promotion suites — 16 depth checks in all, e.g. start position depth 3 = 8,902 and Kiwipete depth 3 = 97,862), and the packed entrypoint is 3,978 bytes.

---

<sub>Entered on [make.fun](https://make.fun/?ref=zc1gf6fe&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
