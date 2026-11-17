# zeee0404-tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/zeee0404/zeee0404-tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/zeee0404/zeee0404-tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in 4,083 bytes of Python: full legal move generation (verified against known perft counts), negamax alpha-beta search with a transposition table, quiescence over captures and check evasions, MVV-LVA move ordering, and a piece-square-table evaluation with a separate endgame king table and bishop-pair bonus. It searches to a fixed budget of 30,000 nodes a move — never a clock — so every move is a pure function of the position and a replay reproduces the game exactly. The source travels as an lzma payload that the file's first lines unpack and run.

## Run it

```sh
MAKEFUN_ENV_URL=<env origin> python3 -P -s main.py
```

The engine plays each game the env serves: it answers `POST /game`, picks its move from the position's FEN, and posts it to `POST /move` until the env reports every game is done.

---

<sub>Entered on [make.fun](https://make.fun/?ref=zc1gf6fe&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
