# 2026 FIFA World Cup Fantasy Pool

A pick-and-hold country fantasy pool for the 2026 FIFA World Cup — designed for 20–50 players with 9 picks across 5 tiers.

## Overview

Each player selects 9 national teams before the tournament starts. Points accumulate as those teams win matches and advance. Lower-tier (longer-shot) teams carry higher multipliers, rewarding bold picks.

| Tier | Name | Picks | Multiplier |
|------|------|-------|-----------|
| 1 | Favorites | 2 | ×1 |
| 2 | Contenders | 1 | ×1.5 |
| 3 | Dark Horses | 2 | ×2.5 |
| 4 | Underdogs | 2 | ×4 |
| 5 | Wildcards | 2 | ×7 |

All 48 World Cup teams are assigned to tiers based on April 2026 sportsbook odds, validated with clustering analysis.

## Key Files

| File | Description |
|------|-------------|
| `WORLD_CUP_GAME_DESIGN.md` | Canonical game design document (v6) — scoring rules, tier assignments, proofs |
| `world_cup_countries.py` | All 48 teams with tier assignments, structured for Flask integration |
| `World Cup Odds2.xlsx` | Raw sportsbook odds used for tier calibration |
| `docs/superpowers/specs/2026-04-02-scoring-rebalance-design.md` | Spec for v5→v6 scoring rebalance |

## Design Highlights

- **Merged knockout scoring** — single point value per round (no separate match + milestone)
- **Compressed multipliers** — balanced so any tier can win
- **Champion bonus** — 50 base points, calibrated so a T1 undefeated champion (116 pts) outscores a T2 best-case runner-up (111 pts)
- **Half-point scores** are acceptable (from ×1.5 and ×2.5 on odd base values); players use a lookup table

## Status

Active design phase. Game design is locked; next deliverable is technical integration into the fantasy-platform web app.
