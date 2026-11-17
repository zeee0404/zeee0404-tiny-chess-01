# zeee0404-tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/zeee0404/zeee0404-tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/zeee0404/zeee0404-tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in 3,978 bytes of Python (entry cap: 4,096): full legal move generation, negamax alpha-beta search, and a fixed node budget — never a clock — so every move is a pure function of the position and a replay reproduces the game exactly. The source travels as an lzma payload that the file's first lines unpack and run.

## Engine

- **Evaluation:** tapered PeSTO-style — separate midgame/endgame piece values and piece-square tables blended by game phase, a bishop-pair bonus, and explicit insufficient-material draws.
- **Search:** transposition table, principal-variation search, null-move pruning, late-move reductions, check extensions, two killers per ply, MVV-LVA and history ordering, and quiescence on captures and promotions (all evasions when in check) with delta pruning. Mate scores are distance-adjusted, so the fastest mate wins.
- **Repetition:** positions already played earlier in the game score as draws inside the search, so won positions are converted instead of repeated.
- **Budget:** 30,000 nodes per move, spent cumulatively across all iterative-deepening iterations.

## Run it

```sh
MAKEFUN_ENV_URL=<env origin> python3 -P -s main.py
```

The engine answers the env's `POST /game`, picks its move from the position's FEN, and posts it to `POST /move` until every game is done.

## Measured results

- Self-play vs the previous engine (equal 6,000-node budget, 32 games from varied book openings): 20 wins, 12 draws, 0 losses — a score of 81.25%, ≈ +255 Elo.
- Practice gauntlet (the competition's own env and anchor engines): Elo 1654, 1191 and 1279 over three instances, mean ≈ 1,375; the previous engine scored 876 on the first instance.
- Perft re-verified exact after the rewrite: 16/16 depth checks on the five standard positions (e.g. start position depth 3 = 8,902; Kiwipete depth 3 = 97,862).

---

<sub>Entered on [make.fun](https://make.fun/?ref=zc1gf6fe&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
