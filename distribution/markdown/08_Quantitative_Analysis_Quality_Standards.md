# Quantitative Analysis Quality Standards for FAIR-CAM

## Overview

This document establishes quality standards and error reduction strategies for quantitative analysis work in FAIR-CAM methodology development. These standards emerged from lessons learned during the development of the Stage-Gated Detection and Response methodology.

## Error Categories

### Category 1: Arithmetic Errors

**Examples:**
- Incorrect exponentiation (e.g., 0.64^5.65 calculated as 0.089 instead of 0.080)
- Decimal place errors (e.g., 0.003% instead of 0.03%)
- Rounding errors that propagate through calculations

**Prevention:**
- Use computational tools for all calculations involving exponents, roots, or multi-step operations
- Show intermediate calculation steps explicitly
- Apply reasonableness checks after each step

### Category 2: Mathematical Bound Violations

**Examples:**
- Detection probability exceeding Cov × V_eff (the maximum possible value)
- Probabilities exceeding 100% or falling below 0%
- Susceptibility values outside [0,1]

**Prevention:**
- State mathematical bounds explicitly before calculating
- Verify outputs against bounds before presenting results
- Include bound checks in calculation traces

### Category 3: Formula-Parameter Inconsistencies

**Examples:**
- PERT mean values not matching (min + 4×mode + max)/6
- Stated parameters differing from those used in calculations
- Monte Carlo results inconsistent with analytical expectations

**Prevention:**
- Verify each formula output against stated parameters
- Cross-check Monte Carlo results against analytical bounds
- Document all parameters in a single reference table

### Category 4: Propagation Errors

**Examples:**
- Errors in early calculations cascading through dependent values
- Expected loss calculations using incorrect probabilities
- Cumulative progression errors compounding across stages

**Prevention:**
- Verify intermediate results before proceeding
- Use dependency tracking (which outputs depend on which inputs)
- Recalculate from verified intermediate points when errors found

### Category 5: Unit and Scale Errors

**Examples:**
- Mixing percentages and decimals without conversion
- Time unit mismatches (hours vs. days)
- Currency scaling errors

**Prevention:**
- State units explicitly for all parameters
- Maintain consistent unit conventions throughout
- Include unit verification in calculation traces

## Verification Framework

### Pre-Calculation Phase

**1. Parameter Table Review**
- List all input parameters with units
- Verify parameter values are in expected ranges
- Identify dependencies between parameters

**2. Formula Documentation**
- State all formulas before use
- Identify mathematical bounds imposed by formulas
- Note any assumptions (e.g., independence)

**3. Expected Outcome Ranges**
- Estimate expected output ranges before calculating
- Identify what would constitute an implausible result
- Document reasonableness criteria

### Calculation Phase

**4. Intermediate Step Documentation**

For each multi-step calculation:
```
Step 1: [calculation] = [result]
Verification: [bound check or reasonableness check]

Step 2: [calculation] = [result]
Verification: [bound check or reasonableness check]

Final: [result]
Verification: [final bound check]
```

**5. Bound Verification Template**
```
Calculated value: X
Mathematical bound: X ≤ Y because [reasoning]
Status: ✓ Within bound / ✗ VIOLATION - investigate
```

**6. Cross-Verification**
- Calculate same result via alternative method when possible
- Verify Monte Carlo results against analytical expectations
- Check that probabilities sum to expected totals

### Post-Calculation Phase

**7. Consistency Audit**
- Verify all stated values in tables match calculations
- Check that summary statistics match detailed breakdowns
- Confirm cumulative values equal sums of components

**8. Sensitivity Check**
- Verify small parameter changes produce proportional output changes
- Identify any unexpected discontinuities
- Confirm results are not artifacts of numerical precision

## Stage-Gated Detection Methodology Verification

### Critical Bounds for Detection Calculations

**Detection Probability Ceiling:**
```
P(Detect_i) ≤ Cov_i × V_eff,i
```

Detection probability cannot exceed the visibility ceiling, regardless of how many reviews occur or how high recognition is.

**Multi-Review Formula Verification:**
```
P(Detect_i) = Cov_i × V_eff,i × [1 - (1 - R_eff,i)^(ρ_i × λ_i)]

Where:
- V_eff = V × Rel_V
- R_eff = R × Rel_R
- λ = τ / (M / Rel_M)
- ρ ∈ [0, 1]
```

**Verification steps:**
1. Confirm V_eff = V × Rel_V (effective visibility)
2. Confirm R_eff = R × Rel_R (effective recognition)
3. Confirm λ = τ / (M / Rel_M) (reviews per stage)
4. Confirm ρ × λ ≥ 0 (effective independent reviews)
5. Confirm (1 - R_eff)^(ρ × λ) ∈ [0, 1] (miss probability)
6. Confirm 1 - (1 - R_eff)^(ρ × λ) ∈ [0, 1] (recognition probability)
7. Confirm P(Detect) ≤ Cov × V_eff (ceiling check)

### Cumulative Progression Verification

```
P(Reach_1) = 1.0
P(Reach_i) = P(Reach_{i-1}) × [1 - P(Detect_{i-1})] × P_{i-1}
```

**Verification steps:**
1. Confirm all P(Reach_i) values are decreasing (monotonic)
2. Confirm P(Reach_{final}) > 0 unless P(Detect) = 1 at some stage
3. Confirm Σ(P_detected_at_stage) + P(Full Impact) + P(Attacker Fails) ≈ 1.0

### Outcome Class Probability Verification

```
P(Early) = P(Detect @ Stage 1) + P(Detect @ Stage 2)
P(Mid) = P(Detect @ Stage 3) + P(Detect @ Stage 4)
P(Late) = P(Detect @ Stage 5) + P(Detect @ Stage 6)
P(Full) = P(Reach_final) × [1 - P(Detect_final)] × P_final
P(Fail) = Σ (attacker progression failures across stages)
```

**Verification:** P(Early) + P(Mid) + P(Late) + P(Full) + P(Fail) = 100%

### Expected Loss Verification

```
E[Loss] = Σ P(class) × E[LM_class]
```

**PERT Distribution Mean:**
```
E[X] = (min + 4 × mode + max) / 6
```

**Verification steps:**
1. Verify each PERT mean calculation
2. Verify probability × loss multiplication for each class
3. Verify sum matches stated expected loss

### Review Independence Factor (ρ) Verification

**Valid Range:** ρ ∈ [0, 1]

**Special Cases:**
- When ρ = 0: P(Detect) = Cov × V_eff × R_eff (single-review model)
- When ρ = 1: Full multi-review benefit
- When ρ × λ < 1: Limited benefit from multiple reviews

**Assessment Factors (each 0-1):**
1. Detection method (signature vs. behavior-based)
2. Analyst rotation
3. Evidence evolution (static vs. active attacker)
4. Automation level
5. Rule stochasticity

**Composite ρ:** Average of factor scores, then apply stage-specific adjustments

## Prompting Strategies for AI-Assisted Analysis

### Request Explicit Calculation Traces

**Instead of:**
"Calculate the detection probability for Stage 6"

**Use:**
"Calculate the detection probability for Stage 6 using the multi-review formula. Show:
1. All input parameters used
2. Each intermediate calculation step with result
3. The mathematical bound (Cov × V_eff) and verification that the result is within bounds
4. The final result with appropriate precision"

### Request Bound Checking

Include in prompts:
"After calculating [X], verify that the result respects the following constraints: [list constraints]. If any constraint is violated, investigate and explain."

### Request Internal Consistency Verification

Include in prompts:
"Before finalizing, verify that:
- All means match their stated formula with the stated parameters
- All probabilities sum to 100% (or expected total)
- No calculated value exceeds its mathematical maximum
- Summary tables match detailed calculations"

### Break Complex Analyses into Phases

**Phase 1: Parameter Verification**
"List all input parameters, verify they are within expected ranges, and identify any dependencies."

**Phase 2: Individual Calculations**
"Calculate [specific values] with explicit traces. Do not proceed until these are verified."

**Phase 3: Aggregation**
"Using the verified values from Phase 2, calculate aggregate results. Verify totals."

**Phase 4: Narrative Integration**
"Incorporate verified results into the narrative. Ensure all stated values match calculations."

### Request Adversarial Review

After receiving draft analysis:
"Now review this analysis as a skeptical reviewer looking for:
- Arithmetic errors in any calculations
- Mathematical bound violations
- Inconsistencies between stated parameters and calculated results
- Any values that seem implausible
Report any issues found with specific locations and corrections."

## Documentation Standards

### Calculation Trace Format

```
### [Calculation Name]

**Inputs:**
- Parameter A = [value] [units]
- Parameter B = [value] [units]

**Formula:**
Result = f(A, B) = [formula]

**Calculation:**
Step 1: [intermediate] = [value]
Step 2: [intermediate] = [value]
Result = [final value]

**Verification:**
- Bound: Result ≤ [maximum] because [reason]
- Status: ✓ Verified within bound
```

### Summary Table Format

```
| Parameter | Calculated Value | Verification | Status |
|-----------|------------------|--------------|--------|
| X | 58.1% | ≤ 63.2% | ✓ |
| Y | 79.1% | ≤ 79.2% | ✓ |
```

### Monte Carlo Verification Format

```
**Monte Carlo Results (N = [trials]):**
- Mean: [value] (analytical expectation: [value], difference: [%])
- Median: [value]
- P90: [value]
- Min observed: [value] (theoretical min: [value])
- Max observed: [value] (theoretical max: [value])

**Verification:** Results are consistent with analytical expectations.
```

## Quality Checklist

Before finalizing any quantitative FAIR-CAM analysis:

- [ ] All input parameters documented with units
- [ ] All formulas stated before use
- [ ] Calculation traces shown for complex multi-step operations
- [ ] Mathematical bounds verified for all calculated values
- [ ] Probabilities sum to expected totals
- [ ] Summary tables match detailed calculations
- [ ] PERT/distribution means match formula with stated parameters
- [ ] Monte Carlo results consistent with analytical expectations
- [ ] No propagation of early errors through dependent calculations
- [ ] Sensitivity analysis performed for key parameters
- [ ] Adversarial review completed
- [ ] Detection probabilities ≤ Cov × V_eff for all stages
- [ ] Review Independence Factor (ρ) justified for detection architecture
- [ ] Outcome class probabilities sum to 100%

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | January 2026 | Initial version with stage-gated D&R verification standards |

## References

- FAIR-CAM Core Concepts (01_FAIR_CAM_Core_Concepts.md)
- Detection and Response Measurement (04_Detection_Response_Measurement.md)
- Stage-Gated Detection and Response Methodology with Review Independence Factor
