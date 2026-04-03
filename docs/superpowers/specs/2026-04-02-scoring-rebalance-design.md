# Scoring Rebalance Design Spec

**Date:** 2026-04-02
**Status:** Approved
**Applies to:** WORLD_CUP_GAME_DESIGN.md (v5)

---

## Problem

Two scoring issues in the current design:

1. **Redundant knockout scoring categories.** Knockout rounds award both "match points" (3 for a win) and a "stage milestone" separately, but they always co-occur. A win in R16 is always 3 + 8 = 11. There's no reason to present these as two separate line items.

2. **Cross-tier champion vs. runner-up imbalance.** An undefeated Tier 1 World Cup champion (99 pts) gets outscored by a Tier 2 runner-up (142 pts) by 43 points. Picking the world champion should put you near the top of the leaderboard, not 43 points behind someone whose team lost the Final.

## Changes

### Change 1: Merge Knockout Match Points + Milestones

Knockout rounds now have a single point value per result. No separate "match points" and "milestone" categories.

| Knockout Result | Base Points | Derivation |
|---|---|---|
| Win R32 | **8** | was 3 (match) + 5 (milestone) |
| Win R16 | **11** | was 3 + 8 |
| Win QF | **15** | was 3 + 12 |
| Win SF | **19** | was 3 + 16 |
| **Champion** | **50** | was 3 + 30, boosted to 50 |
| Runner-Up | **8** | unchanged (0 match + 8 milestone) |
| Win 3rd Place | **8** | was 3 + 5 |

Group stage scoring is unchanged: Win 3, Draw 1, Loss 0. Group advancement milestones unchanged: Win Group 4, Runner-Up 3, Best 3rd 1.

The old "Knockout Advance" match points row (3 base points per KO win) is eliminated as a separate category. Those points are now baked into each knockout stage value above.

### Change 2: Compress Multipliers

| Tier | Name | Old Multiplier | New Multiplier |
|---|---|---|---|
| 1 | Favorites | x1 | **x1** |
| 2 | Contenders | x2 | **x1.5** |
| 3 | Dark Horses | x3 | **x2.5** |
| 4 | Underdogs | x5 | **x4** |
| 5 | Wildcards | x8 | **x7** |

Tier assignments and number of picks per tier are unchanged.

---

## Updated Points Per Achievement by Tier

| Achievement | Base | T1 (x1) | T2 (x1.5) | T3 (x2.5) | T4 (x4) | T5 (x7) |
|---|---|---|---|---|---|---|
| Group Win | 3 | 3 | 4.5 | 7.5 | 12 | 21 |
| Group Draw | 1 | 1 | 1.5 | 2.5 | 4 | 7 |
| Win Group | 4 | 4 | 6 | 10 | 16 | 28 |
| Advance Runner-Up | 3 | 3 | 4.5 | 7.5 | 12 | 21 |
| Advance Best 3rd | 1 | 1 | 1.5 | 2.5 | 4 | 7 |
| Win R32 | 8 | 8 | 12 | 20 | 32 | 56 |
| Win R16 | 11 | 11 | 16.5 | 27.5 | 44 | 77 |
| Win QF | 15 | 15 | 22.5 | 37.5 | 60 | 105 |
| Win SF | 19 | 19 | 28.5 | 47.5 | 76 | 133 |
| **Champion** | **50** | **50** | **75** | **125** | **200** | **350** |
| Runner-Up | 8 | 8 | 12 | 20 | 32 | 56 |
| Win 3rd Place | 8 | 8 | 12 | 20 | 32 | 56 |

**Note on half-points:** x1.5 and x2.5 produce .5 values on odd base points. Players use this table as a lookup; nobody needs to multiply manually.

---

## Podium Proof

The Champion always outscores the Runner-Up, and the Runner-Up always outscores Third Place, within the same tier regardless of scenario.

| Finish | Scenario | Raw Total |
|---|---|---|
| **Champion** (worst case) | 0W 3D group, best 3rd, win R32-Final | 3+1+8+11+15+19+50 = **107** |
| **Runner-Up** (best case) | 3W group, win group, win R32-SF, lose Final | 9+4+8+11+15+19+8 = **74** |
| **Runner-Up** (worst case) | 0W 3D group, best 3rd, win R32-SF, lose Final | 3+1+8+11+15+19+8 = **65** |
| **3rd Place** (best case) | 3W group, win group, win R32-QF, lose SF, win 3rd | 9+4+8+11+15+8 = **55** |
| **4th Place** (best case) | 3W group, win group, win R32-QF, lose SF, lose 3rd | 9+4+8+11+15 = **47** |

Champion (107) > Runner-Up (74) > 3rd Place (55) > 4th Place (47). Apply any multiplier and the ordering holds.

---

## Cross-Tier Validation

The primary design goal: an undefeated Tier 1 champion should outscore a best-case Tier 2 runner-up in all realistic scenarios.

### Best case vs. best case

| Scenario | Points |
|---|---|
| T1 undefeated champion | 116 x 1 = **116** |
| T2 best-case runner-up | 74 x 1.5 = **111** |
| **T1 wins by 5** | |

### Realistic worst case

A T1 champion that lost a group game but still advanced as runner-up:

| Scenario | Points |
|---|---|
| T1 champion (2W 1L, runner-up advance) | 112 x 1 = **112** |
| T2 best-case runner-up | 74 x 1.5 = **111** |
| **T1 wins by 1** | |

### Absolute worst case

A T1 champion that drew every group game and squeaked through as best 3rd:

| Scenario | Points |
|---|---|
| T1 worst-case champion (0W 3D, best 3rd) | 107 x 1 = **107** |
| T2 best-case runner-up | 74 x 1.5 = **111** |
| **T2 wins by 4** | |

This requires the champion to draw all 3 group games, advance as best 3rd, then win 5 straight knockout matches. A team that limps into the bracket like this winning the entire tournament is an extreme outlier. In any realistic scenario where the champion won at least 2 group games, they outscore the Tier 2 runner-up.

### Tier 3 runner-up vs. Tier 1 champion

| Scenario | Points |
|---|---|
| T1 undefeated champion | 116 x 1 = **116** |
| T3 best-case runner-up | 74 x 2.5 = **185** |
| **T3 wins by 69** | |

A Tier 3 team making the Final is a significant upset (2022 Morocco-level run). The x2.5 multiplier rewards this appropriately. This cross-tier gap is by design.

---

## Updated Example Scenarios

### Spain (T1) wins World Cup undefeated + Argentina (T1) loses in QF

**Spain** — 3 group wins, wins every knockout round, champion:

| Source | Points |
|---|---|
| Group (3W) | 9 |
| Win Group | 4 |
| Win R32 | 8 |
| Win R16 | 11 |
| Win QF | 15 |
| Win SF | 19 |
| Champion | 50 |
| **Raw total** | **116** |
| **x Tier 1 (x1)** | **116 pts** |

**Argentina** — 2 group wins, 1 draw, wins R32 and R16, loses in QF:

| Source | Points |
|---|---|
| Group (2W, 1D) | 7 |
| Win Group | 4 |
| Win R32 | 8 |
| Win R16 | 11 |
| **Raw total** | **30** |
| **x Tier 1 (x1)** | **30 pts** |

**Combined Tier 1 total: 146 pts**

### Norway (T2) reaches the Final, loses — 2W 1D group

| Source | Points |
|---|---|
| Group (2W, 1D) | 7 |
| Advance Runner-Up | 3 |
| Win R32 | 8 |
| Win R16 | 11 |
| Win QF | 15 |
| Win SF | 19 |
| Runner-Up | 8 |
| **Raw total** | **71** |
| **x Tier 2 (x1.5)** | **106.5 pts** |

Spain's championship (116) outscores Norway's runner-up finish (106.5) by 9.5 points.

### Senegal (T3) wins group, reaches QF, loses

| Source | Points |
|---|---|
| Group (2W, 1D) | 7 |
| Win Group | 4 |
| Win R32 | 8 |
| Win R16 | 11 |
| **Raw total** | **30** |
| **x Tier 3 (x2.5)** | **75 pts** |

### Canada (T4) surprises with an R16 run

| Source | Points |
|---|---|
| Group (1W, 2D) | 5 |
| Advance Runner-Up | 3 |
| Win R32 | 8 |
| **Raw total** | **16** |
| **x Tier 4 (x4)** | **64 pts** |

### Iran (T5) Cinderella run to QF

| Source | Points |
|---|---|
| Group (1W, 1D, 1L) | 4 |
| Advance Best 3rd | 1 |
| Win R32 | 8 |
| Win R16 | 11 |
| **Raw total** | **24** |
| **x Tier 5 (x7)** | **168 pts** |

Iran's miracle QF run produces 168 points. Still the highest single-team score in these examples, but the gap to an undefeated T1 champion (116) is narrower than before (was 192 vs 99; now 168 vs 116).

### Curacao (T5) exits in groups — 3 losses

**0 pts.** That's the risk of a Wildcard pick.

---

## Multiplier Rationale (updated)

| Tier | Picks | Approx Raw EV | Mult | EV/Pick | Tier EV | % of Portfolio |
|---|---|---|---|---|---|---|
| 1 | 2 | ~46 | x1 | ~46 | ~92 | ~31% |
| 2 | 1 | ~24 | x1.5 | ~36 | ~36 | ~12% |
| 3 | 2 | ~13 | x2.5 | ~33 | ~65 | ~22% |
| 4 | 2 | ~7 | x4 | ~28 | ~56 | ~19% |
| 5 | 2 | ~4 | x7 | ~25 | ~50 | ~17% |
| | **9** | | | | **~299** | |

EV per pick ranges from ~25 (T5) to ~46 (T1), a max/min ratio of 1.84x. This is slightly wider than the previous 1.58x, which is intentional: Tier 1 and Tier 2 selections and late-tournament knockout results should determine the winner, not a lucky Wildcard flyer. Tier 5 trades expected value for variance.

---

## What Did NOT Change

- Tier assignments and country placements
- Number of picks per tier (2/1/2/2/2)
- Group stage match scoring (3/1/0)
- Group advancement milestones (4/3/1)
- Tiebreaker rules
- Edge cases and admin rules
- Tournament format reference
