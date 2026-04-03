# World Cup Fantasy Pool

## Project Overview
Game design for a 2026 FIFA World Cup pick-and-hold fantasy pool (20-50 players, 9 picks across 5 tiers).

## Key Files
- `WORLD_CUP_GAME_DESIGN.md` — The canonical game design document (currently v6). All scoring rules, tier assignments, and proofs live here.
- `docs/superpowers/specs/2026-04-02-scoring-rebalance-design.md` — Spec for the v5→v6 scoring rebalance (merged knockout scoring, compressed multipliers, boosted champion bonus).

## Upcoming Deliverables
- `world_cup_countries.py` — All 48 teams assigned to tiers, structured for Flask integration.
- `WORLD_CUP_PHASE_X_HANDOFF.md` — Handoff doc for Claude Chat to drive technical design and integration into the fantasy-platform repo.

## Design Constraints (locked)
- 5 tiers, country assignments are final — do not propose changes to tier membership
- Multipliers (×1 / ×1.5 / ×2.5 / ×4 / ×7) are final
- Knockout scoring is merged (single value per round, no separate match + milestone)
- Champion bonus = 50 base points — calibrated so T1 undefeated champion (116) outscores T2 best-case runner-up (111)

## Style
- The game design doc is written for casual players — keep language accessible
- All scoring math must be verifiable: show arithmetic in proofs and examples
- Half-point scores (from ×1.5 and ×2.5 on odd base values) are acceptable; players use the lookup table
