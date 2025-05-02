# Tokenomics

| Element | White-paper spec (rev 2025) | Current UI code |
|---------|-----------------------------|-----------------|
| **Genesis supply** | 1 000 000 000 XYZ | `GENESIS_SUPPLY = 1_000_000_000` |
| **Ideal validator count** | 19 | `V_TARGET = 19` |
| **APR corridor** | 5 % floor, 20 % cap | `APR_MIN = 0.05`, `APR_MAX = 0.20` |
| **APR-vs-validators** | Linear-down to floor when *v* ≥ 19 | `aprRaw = APR_MIN + (APR_MAX–APR_MIN)·max(0, 1 – v/19)` |
| **Stake scaling** | `APR × (REF_STAKE·v / Σ stake)` then clipped | `stakeScale = (REF_STAKE * v) / totalStaked` followed by `clip()` |
| **Reference stake** | 2 500 000 XYZ | `REF_STAKE = 2_500_000` |
| **Stake limits (UI guard-rails)** | 0.5 M – 2 M XYZ | `MIN_STAKE = 500 000`, `MAX_STAKE = 2 000 000` |
| **Burn model** | Max 5 M XYZ / yr at 100 % usage | `BURN_MAX = 5_000_000`; slider gives 0 – 5 M directly |
| **Net-supply calc** | `ΔS = issuance – burn` | `netChange = annualIssuance - annualBurn` |
