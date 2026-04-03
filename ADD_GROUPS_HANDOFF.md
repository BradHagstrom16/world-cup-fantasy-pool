# Add Group Assignments to world_cup_countries.py

**Task:** Add the official 2026 FIFA World Cup group assignments to every team record in `world_cup_countries.py`.

**Source:** The draw took place December 5, 2025 at the Kennedy Center in Washington, D.C. All 48 teams are confirmed as of March 31, 2026. Groups are final.

---

## Group Assignments

| Group | Team 1 | Team 2 | Team 3 | Team 4 |
|-------|--------|--------|--------|--------|
| A | Mexico (MEX) | South Africa (RSA) | South Korea (KOR) | Czechia (CZE) |
| B | Canada (CAN) | Bosnia & Herzegovina (BIH) | Qatar (QAT) | Switzerland (SUI) |
| C | Brazil (BRA) | Morocco (MAR) | Haiti (HAI) | Scotland (SCO) |
| D | USA (USA) | Paraguay (PAR) | Australia (AUS) | Turkey (TUR) |
| E | Germany (GER) | Curacao (CUW) | Ivory Coast (CIV) | Ecuador (ECU) |
| F | Netherlands (NED) | Japan (JPN) | Sweden (SWE) | Tunisia (TUN) |
| G | Belgium (BEL) | Egypt (EGY) | Iran (IRN) | New Zealand (NZL) |
| H | Spain (ESP) | Cape Verde (CPV) | Saudi Arabia (KSA) | Uruguay (URU) |
| I | France (FRA) | Senegal (SEN) | Norway (NOR) | Iraq (IRQ) |
| J | Argentina (ARG) | Algeria (ALG) | Austria (AUT) | Jordan (JOR) |
| K | Portugal (POR) | DR Congo (COD) | Uzbekistan (UZB) | Colombia (COL) |
| L | England (ENG) | Croatia (CRO) | Ghana (GHA) | Panama (PAN) |

---

## Changes Required

### 1. Add `"group"` field to every team dict in `TEAMS`

Add a `"group"` key with the single-letter group assignment (A–L) to each team's dictionary. Place it after `"confederation"` and before `"aliases"`.

Example — Spain's record changes from:

```python
"ESP": {
    "name": "Spain",
    "display_name": "Spain",
    "fifa_code": "ESP",
    "ioc_code": "ESP",
    "tier": 1,
    "multiplier": 1.0,
    "confederation": "UEFA",
    "aliases": [],
},
```

To:

```python
"ESP": {
    "name": "Spain",
    "display_name": "Spain",
    "fifa_code": "ESP",
    "ioc_code": "ESP",
    "tier": 1,
    "multiplier": 1.0,
    "confederation": "UEFA",
    "group": "H",
    "aliases": [],
},
```

Do this for all 48 teams.

### 2. Add a `GROUPS` lookup dict

After the `TIERS` dict and `TOTAL_PICKS` line, add:

```python
# =============================================================================
# GROUP ASSIGNMENTS — 2026 FIFA World Cup Draw (Dec 5, 2025)
# =============================================================================
GROUPS = {
    "A": ["MEX", "RSA", "KOR", "CZE"],
    "B": ["CAN", "BIH", "QAT", "SUI"],
    "C": ["BRA", "MAR", "HAI", "SCO"],
    "D": ["USA", "PAR", "AUS", "TUR"],
    "E": ["GER", "CUW", "CIV", "ECU"],
    "F": ["NED", "JPN", "SWE", "TUN"],
    "G": ["BEL", "EGY", "IRN", "NZL"],
    "H": ["ESP", "CPV", "KSA", "URU"],
    "I": ["FRA", "SEN", "NOR", "IRQ"],
    "J": ["ARG", "ALG", "AUT", "JOR"],
    "K": ["POR", "COD", "UZB", "COL"],
    "L": ["ENG", "CRO", "GHA", "PAN"],
}
```

### 3. Add a `teams_in_group()` helper

After the existing `teams_in_tier()` function, add:

```python
def teams_in_group(group: str) -> list[dict]:
    """Return all teams in the given group (A-L), in draw order."""
    codes = GROUPS.get(group.upper(), [])
    return [TEAMS[code] for code in codes if code in TEAMS]
```

### 4. Add group validation to `_validate()`

Add the following checks inside the `_validate()` function, after the existing tier size assertions:

```python
    # Group assignments
    all_grouped = set()
    for group_letter, codes in GROUPS.items():
        assert len(codes) == 4, f"Group {group_letter}: expected 4 teams, got {len(codes)}"
        for code in codes:
            assert code in TEAMS, f"Group {group_letter}: unknown team code {code}"
            assert TEAMS[code]["group"] == group_letter, (
                f"Group mismatch for {code}: GROUPS says {group_letter}, "
                f"team record says {TEAMS[code].get('group')}"
            )
            assert code not in all_grouped, f"Team {code} appears in multiple groups"
            all_grouped.add(code)
    assert len(all_grouped) == 48, f"Groups contain {len(all_grouped)} teams, expected 48"
    assert len(GROUPS) == 12, f"Expected 12 groups, got {len(GROUPS)}"
```

### 5. Update the module docstring

Add `group` to the Fields list in the module docstring at the top:

```
    group           — Group letter A-L (2026 FIFA World Cup draw, Dec 5, 2025)
```

---

## Verification

After making all changes, run the file to confirm validation passes:

```bash
python world_cup_countries.py
```

The `_validate()` function runs on import. If any group assignment is wrong or missing, it will raise an `AssertionError` with a descriptive message.
