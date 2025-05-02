# Tokenomics

| Element | White-paper spec (rev 2025) | Current UI code |
|---------|-----------------------------|-----------------|
| **Genesis supply** | 1 000 000 000 INT3 | `GENESIS_SUPPLY = 1_000_000_000` |
| **Ideal validator count** | 19 | `V_TARGET = 19` |
| **APR corridor** | 5 % floor, 20 % cap | `APR_MIN = 0.05`, `APR_MAX = 0.20` |
| **APR-vs-validators** | Linear-down to floor when *v* ≥ 19 | `aprRaw = APR_MIN + (APR_MAX–APR_MIN)·max(0, 1 – v/19)` |
| **Stake scaling** | `APR × (REF_STAKE·v / Σ stake)` then clipped | `stakeScale = (REF_STAKE * v) / totalStaked` followed by `clip()` |
| **Reference stake** | 2 500 000 INT3 | `REF_STAKE = 2_500_000` |
| **Stake limits (UI guard-rails)** | 0.5 M – 2 M INT3 | `MIN_STAKE = 500 000`, `MAX_STAKE = 2 000 000` |
| **Burn model** | Max 5 M INT3 / yr at 100 % usage | `BURN_MAX = 5_000_000`; slider gives 0 – 5 M directly |
| **Net-supply calc** | `ΔS = issuance – burn` | `netChange = annualIssuance - annualBurn` |


# Optimized Tokenomics Framework for **Int3facechain**

**Overview**  
Int3facechain has a genesis supply of **1 billion INT3** and uses a dynamic inflation-deflation schedule that links validator rewards and net issuance to network usage and total staked capital. When validator participation or usage is low the system is mildly inflationary to attract new stakers; at high usage (with fees burned) it turns deflationary, protecting long-term value.

* **Validator- and stake-adaptive APR.** Rewards are highest when the validator set is small or under-staked and taper smoothly to a floor once the ideal set (≈ 19 validators) is reached.  
* **Fee burning for deflation.** A base-fee burn, similar in spirit to EIP-1559, lets usage-driven burn exceed issuance and pull total supply downward.  
* **Smooth transitions.** Linear formulas for APR and burn ensure there are no abrupt jumps in incentives or supply.  
* **Capital-accessibility tuning.** Reference-stake and APR bounds are chosen so that a validator can operate with roughly \$50-200 k worth of INT3 while long-run inflation stays modest or negative.

---

## 1 Adaptive Staking Rewards (APR)

Let  

* *v*  = active validator count (1 ≤ *v* ≤ 100)  
* *s*  = stake per validator  
* *S*  = *v × s* (total staked)  
* *v*★ = 19 (ideal validator count)  

The base reward curve is

\[
APR_{\text{base}}(v) = \alpha_{\min} + (\alpha_{\max}-\alpha_{\min})\,
\max\!\Bigl(0,1-\tfrac{v}{v^{★}}\Bigr),
\]

with bounds  
&nbsp;&nbsp;• APR<sub>min</sub> = 5 %  
&nbsp;&nbsp;• APR<sub>max</sub> = 20 %

To reflect stake size, APR is scaled by

\[
\text{stakeScale} = \frac{\text{REF\_STAKE}\times v}{S},
\]

where **REF_STAKE = 2 500 000 INT3** (≈ \$125 k).  
The final reward rate is clipped to the 5–20 % corridor:

\[
APR = \operatorname{clip}\!\bigl(APR_{\text{base}}\times\text{stakeScale},
\;APR_{\min},APR_{\max}\bigr).
\]

Annual issuance is

\[
I_{\text{year}} = S \times APR.
\]

---

## 2 Fee-Burn Mechanism

Set **B<sub>max</sub> = 5 million INT3 / year** (full-usage burn).  
With block-usage fraction *U* (0 – 100 %), yearly burn is

\[
B_{\text{year}} = U \times B_{\max}.
\]

---

## 3 Net-Supply Change

\[
\Delta S = I_{\text{year}} - B_{\text{year}}, \qquad
S_{\text{new}} = S_{\text{old}} + \Delta S.
\]

At low usage, ΔS > 0 (inflation).  
Once burn exceeds issuance (roughly above 60 % usage in the reference scenario) ΔS becomes negative and supply shrinks.

---

## 4 Illustrative Parameter Set

| Parameter | Value |
|-----------|-------|
| Genesis supply | 1 000 000 000 INT3 |
| Ideal validator count | 19 |
| Stake per validator (typical) | 2 – 3 million INT3 |
| APR bounds | 5 % – 20 % |
| Burn at 100 % usage | 5 million INT3 / yr |

---

## 5 Example Scenarios

| Scenario | Validators | Stake / val. | Usage | APR | Supply change |
|----------|------------|--------------|-------|-----|---------------|
| Bootstrap | 5 | 2 M | 20 % | 16 % | +0.6 M (≈ 0.06 %) |
| Steady state | 19 | 2 M | 80 % | 5 % | −2.1 M (≈ 0.21 %) |

---

## 6 Economic Implications

1. **Self-balancing yields.** Additional validators or larger stakes dilute APR, discouraging over-concentration.  
2. **Predictable ceiling.** Even at maximum validator count and stake, inflation stays below 0.5 % of supply.  
3. **Usage-driven scarcity.** Heavy on-chain activity can turn the system deflationary.  
4. **Capital efficiency.** Operators choose between higher stake per node (lower APR, fewer machines) and more nodes (higher APR, more ops overhead).

---

## 7 Conclusion

Int3facechain’s monetary design tightly couples a 5–20 % APR corridor with fee burning. The network inflates only when it needs to attract capital; under healthy usage it naturally trends deflationary, aligning token supply with real demand.
