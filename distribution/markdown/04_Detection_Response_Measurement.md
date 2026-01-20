# Measuring Detection and Response Control Value

## Overview

This method provides quantitative measurement of detection and response control effects on risk. The approach uses a **stage-gated model** that recognizes attacks progress through discrete phases, each representing a distinct detection opportunity with stage-specific parameters. While examples use cybersecurity, the principles apply to other harms (natural disasters, technology failures, etc.).

This methodology replaces continuous loss growth curves with conditional magnitude distributions based on outcome classification, providing more defensible estimates of detection and response value.

## Conceptual Foundation

### Stage-Gated Structure

Rather than modeling loss as a continuous function of time, this methodology decomposes attacks into discrete **stages** aligned with the attack kill chain (e.g., MITRE ATT&CK phases). Each stage represents:

1. A distinct detection opportunity with stage-specific parameters
2. A decision point where the attacker may succeed or fail to progress
3. A classification boundary for conditional loss distributions

This structure captures the reality that detection investments have different value at different points in the attack lifecycle.

### Boolean Relationships Preserved

The methodology preserves FAIR-CAM's critical boolean relationships:

- **Detection (AND relationship)**: Visibility ∧ Monitoring ∧ Recognition must all succeed
- **Prevention (OR relationship)**: Any working prevention control provides some protection
- **Response depends on Detection**: No detection → no response → full loss

### Multi-Review Detection

A critical insight: when stage duration exceeds monitoring cadence, multiple detection opportunities occur within a single stage. The methodology explicitly models this accumulation of detection probability across review cycles.

## Key Parameters

### Stage-Level Parameters

For each stage *i*, define the following parameters:

| Parameter | Symbol | Description | Unit |
|-----------|--------|-------------|------|
| Coverage | Cov_i | Fraction of attack surface where detection controls are deployed | % |
| Visibility | V_i | Probability that evidence of activity is captured | % |
| Visibility Reliability | Rel_V,i | Fraction of time visibility controls operate as intended | % |
| Recognition | R_i | Probability that evidence is correctly classified as malicious | % |
| Recognition Reliability | Rel_R,i | Fraction of time recognition controls operate as intended | % |
| Monitoring Cadence | M_i | Time between evidence reviews | Days |
| Monitoring Reliability | Rel_M,i | Fraction of time monitoring occurs on schedule | % |
| Stage Duration | τ_i | Expected time attacker spends in stage | Days |
| Progression Probability | P_i | Probability attacker successfully advances to next stage | % |
| Review Independence Factor | ρ_i | Degree of independence between successive reviews | 0-1 |

### Response Parameters

| Parameter | Symbol | Description | Unit |
|-----------|--------|-------------|------|
| Containment Time | C | Time to terminate threat actor's ability to continue harm | Days |
| Resilience Time | S | Time to restore normal operating capacity | Days |
| Concurrency Factor | α | Degree of overlap between containment and resilience activities | 0-1 |

### Outcome Classification Parameters

| Outcome Class | Detection Stage | Loss Distribution |
|---------------|-----------------|-------------------|
| Early Detection | 1-2 | LM_early ~ PERT(min, mode, max) |
| Mid Detection | 3-4 | LM_mid ~ PERT(min, mode, max) |
| Late Detection | 5-6 | LM_late ~ PERT(min, mode, max) |
| Full Impact | Not detected | LM_full ~ PERT(min, mode, max) |
| Attacker Fails | N/A | LM_fail ~ PERT(min, mode, max) |

## Core Formulas

### Effective Parameters

Reliability factors degrade intended efficacy to operational efficacy:

```
V_eff,i = V_i × Rel_V,i
R_eff,i = R_i × Rel_R,i
```

### Number of Reviews per Stage

When stage duration exceeds monitoring cadence, multiple review opportunities occur:

```
λ_i = τ_i / (M_i / Rel_M,i)
```

Where:
- τ_i = stage duration (days)
- M_i = monitoring cadence (days)
- Rel_M,i = monitoring reliability (fraction)

### Detection Probability with Multiple Reviews

The multi-review detection formula with the Review Independence Factor (ρ):

```
P(Detect_i) = Cov_i × V_eff,i × [1 - (1 - R_eff,i)^(ρ_i × λ_i)]
```

**Special case when ρ = 0** (fully deterministic detection):
```
P(Detect_i) = Cov_i × V_eff,i × R_eff,i
```

This formula correctly:
- Respects the AND dependency (visibility must exist for any recognition to occur)
- Accumulates detection probability across multiple reviews, discounted by ρ
- Reduces to the single-review model when λ ≤ 1 or ρ = 0
- Converges toward Cov × V_eff as reviews accumulate (capped by visibility)

**Mathematical Bound**: P(Detect_i) ≤ Cov_i × V_eff,i (detection cannot exceed the visibility ceiling)

### The Review Independence Factor (ρ)

The ρ parameter calibrates the multi-review formula based on detection architecture characteristics:

| ρ Value | Interpretation | Typical Scenarios |
|---------|----------------|-------------------|
| 1.0 | Fully Independent | Multiple analysts rotating shifts; behavior-based detection with inherent randomness; active attacker generating new evidence |
| 0.7-0.9 | Mostly Independent | Mixed human/automated review; some analyst rotation; anomaly detection with probabilistic thresholds |
| 0.4-0.6 | Partially Correlated | Limited rotation; mix of signature and behavior rules; attacker intermittently active |
| 0.1-0.3 | Mostly Deterministic | Same analyst reviewing; primarily signature-based; attacker dormant after initial action |
| 0.0 | Fully Deterministic | Pure hash matching; no variability between reviews |

**Assessment Framework for ρ**

Score each factor 0-1, then average:

| Factor | Low (0.0-0.3) | Medium (0.4-0.6) | High (0.7-1.0) |
|--------|---------------|------------------|----------------|
| Detection Method | Pure signature/hash | Mix of signatures + behavior | Anomaly/ML-based |
| Analyst Rotation | Same analyst all passes | 2-3 analysts rotate | Large team, high rotation |
| Evidence Evolution | Static; attacker dormant | Some new evidence over time | Active attacker; new IOCs each window |
| Automation Level | Fully automated, deterministic | Mixed human/automated | Heavy human review |
| Rule Stochasticity | Deterministic thresholds | Some probabilistic elements | Inherently probabilistic |

### Cumulative Progression

The probability of reaching stage *i* undetected:

```
P(Reach_1) = 1.0  (attack begins)
P(Reach_i) = P(Reach_{i-1}) × [1 - P(Detect_{i-1})] × P_{i-1}
```

This recursive formula captures:
- Detection failures at previous stages
- Attacker progression failures (natural attack attrition)

**Key Insight**: Empirical evidence suggests 10-20% of attacks stall due to attacker errors or environmental factors, independent of defender detection. The P_i parameters capture this reality.

### Response Time with Concurrency

Response time accounts for overlap between containment and resilience activities:

```
T_response = T_containment + T_resilience - α × min(T_containment, T_resilience)
```

Where:
- α = 0: Fully sequential (containment completes before recovery starts)
- α = 1: Fully parallel (both execute simultaneously)
- α = 0.3-0.6: Typical partial overlap

### Expected Loss Calculation

```
E[Loss] = Σ P(outcome_class) × E[LM_class]
```

Where outcome classes are: Early, Mid, Late, Full Impact, Attacker Fails

**PERT Distribution Mean**:
```
E[X] = (min + 4 × mode + max) / 6
```

### Loss Minimization

Loss minimization controls (insurance, IR retainers, BC/DR investments) are applied as currency subtraction AFTER determining gross loss:

```
Coverage = min(max(0, Gross_Loss - Deductible), Limit)
Net_Loss = Gross_Loss - Coverage
```

**Critical**: This calculation must be performed per-trial in Monte Carlo simulation, not as post-hoc mean adjustment. The non-linear cap and deductible create asymmetries where E[f(L)] ≠ f(E[L]).

### Risk Appetite Assessment

Probability of exceeding a threshold:

```
P(Exceed Threshold) = Σ P(class) × P(LM_class > Threshold)
```

For PERT distributions, P(X > threshold) requires numerical integration or Monte Carlo sampling.

## Application Method

### 1. Define Attack Stages

Align stages with relevant kill chain framework (MITRE ATT&CK, Cyber Kill Chain, etc.). Typical ransomware stages:

1. Initial Access (phishing, exploit)
2. Persistence (backdoor installation)
3. Privilege Escalation
4. Lateral Movement
5. Data Staging/Exfiltration
6. Execution (encryption, destruction)

### 2. Parameterize Each Stage

For each stage, estimate:
- Coverage: What fraction of attack surface has detection controls?
- Visibility: What's the probability evidence is captured?
- Recognition: What's the probability evidence is correctly classified?
- Reliability factors: What fraction of time do controls operate as intended?
- Monitoring cadence: How often is evidence reviewed?
- Stage duration: How long does attacker typically spend here?
- Progression probability: What fraction of attackers successfully advance?
- Review Independence Factor (ρ): How independent are successive reviews?

### 3. Define Outcome Distributions

Calibrate loss distributions for each outcome class using:
- Historical incident data
- Tabletop exercise estimates
- Insurance claims
- SME judgment

Avoid asserting specific time-to-loss curves—let the conditional distributions represent your uncertainty honestly.

### 4. Calculate Detection Probabilities

Apply the multi-review detection formula to each stage, respecting mathematical bounds.

### 5. Calculate Cumulative Progression

Compute P(Reach_i) recursively, tracking where detections and attacker failures occur.

### 6. Classify Outcomes and Calculate Expected Loss

Sum outcome class probabilities × expected losses to get E[Loss_gross].

### 7. Apply Loss Minimization

Run Monte Carlo simulation to correctly handle insurance/BC-DR non-linearities.

### 8. Assess Risk Appetite Alignment

Calculate P(Exceed Threshold) and compare to stated appetite.

## Worked Example: Ransomware Attack

### Scenario Definition

A financial services organization (10,000 employees, $500M revenue) faces ransomware threat. The organization has deployed detection and response controls across six attack phases.

**Risk Appetite**: Less than 5% probability in 12 months of exceeding $500,000 in cyber losses from a single event.

### Stage Parameters

| Stage | Cov | V | Rel_V | R | Rel_R | M (days) | Rel_M | τ (days) | P | ρ |
|-------|-----|---|-------|---|-------|----------|-------|----------|---|---|
| 1. Initial Access | 95% | 70% | 95% | 40% | 90% | 0.042 | 95% | 0.25 | 90% | 0.40 |
| 2. Persistence | 98% | 85% | 95% | 60% | 92% | 0.042 | 95% | 0.50 | 85% | 0.55 |
| 3. Priv Escalation | 96% | 80% | 94% | 75% | 90% | 0.042 | 94% | 0.50 | 80% | 0.60 |
| 4. Lateral Movement | 92% | 90% | 96% | 65% | 88% | 0.042 | 92% | 1.00 | 75% | 0.65 |
| 5. Data Staging | 88% | 75% | 92% | 55% | 85% | 0.042 | 90% | 0.75 | 70% | 0.50 |
| 6. Execution | 85% | 60% | 90% | 45% | 80% | 0.042 | 88% | 0.25 | 95% | 0.45 |

Note: M = 0.042 days ≈ 1 hour monitoring cadence

### Effective Parameters and Reviews

| Stage | V_eff | R_eff | λ (reviews) | ρ × λ |
|-------|-------|-------|-------------|-------|
| 1 | 66.5% | 36.0% | 5.65 | 2.26 |
| 2 | 80.8% | 55.2% | 11.3 | 6.22 |
| 3 | 75.2% | 67.5% | 11.2 | 6.72 |
| 4 | 86.4% | 57.2% | 21.9 | 14.2 |
| 5 | 69.0% | 46.8% | 16.1 | 8.05 |
| 6 | 54.0% | 36.0% | 5.30 | 2.39 |

### Detection Probability Calculation

Applying P(Detect_i) = Cov_i × V_eff,i × [1 - (1 - R_eff,i)^(ρ × λ)]:

| Stage | Cov × V_eff | [1-(1-R_eff)^(ρλ)] | P(Detect\|Reach) |
|-------|-------------|-------------------|-----------------|
| 1 | 63.2% | 58.9% | 37.2% |
| 2 | 79.2% | 99.7% | 79.0% |
| 3 | 72.2% | 99.9% | 72.1% |
| 4 | 79.5% | ≈100% | 79.5% |
| 5 | 60.7% | 99.9% | 60.6% |
| 6 | 45.9% | 60.2% | 27.6% |

### Cumulative Progression Analysis

| Stage | P(Reach) | P(Detect) | P(Detected Here) | P(Miss & Progress) |
|-------|----------|-----------|------------------|-------------------|
| 1 | 100.0% | 37.2% | 37.2% | 56.5% |
| 2 | 56.5% | 79.0% | 44.6% | 9.99% |
| 3 | 9.99% | 72.1% | 7.20% | 2.23% |
| 4 | 2.23% | 79.5% | 1.77% | 0.34% |
| 5 | 0.34% | 60.6% | 0.21% | 0.094% |
| 6 | 0.094% | 27.6% | 0.026% | 0.065% |

**Outcome Class Summary**:

| Outcome Class | Probability | Stages |
|---------------|-------------|--------|
| Early Detection | 81.8% | 1-2 |
| Mid Detection | 8.97% | 3-4 |
| Late Detection | 0.24% | 5-6 |
| Full Impact | 0.065% | Not detected |
| Attacker Fails | 8.9% | N/A |

### Conditional Loss Distributions

| Outcome | Min | Mode | Max | E[Loss] |
|---------|-----|------|-----|---------|
| Early Detection | $2,000 | $8,000 | $30,000 | $10,667 |
| Mid Detection | $25,000 | $75,000 | $250,000 | $95,833 |
| Late Detection | $200,000 | $500,000 | $2,000,000 | $700,000 |
| Full Impact | $1,000,000 | $3,000,000 | $5,000,000 | $3,000,000 |
| Attacker Fails | $2,000 | $5,000 | $15,000 | $6,167 |

### Expected Loss Calculation

```
E[L_gross] = Σ P(class) × E[LM_class]
           = 0.818 × $10,667 + 0.0897 × $95,833 + 0.0024 × $700,000
           + 0.00065 × $3,000,000 + 0.089 × $6,167
           = $8,726 + $8,596 + $1,680 + $1,950 + $549
           = $21,501
```

### Risk Appetite Assessment

P(Exceed $500K) requires estimating tail probabilities:
- Late Detection: ~62% probability of exceeding $500K (mode is $500K)
- Full Impact: 100% probability of exceeding $500K

```
P(Exceed $500K) = P(Late) × 0.62 + P(Full) × 1.0
                = 0.0024 × 0.62 + 0.00065 × 1.0
                = 0.0015 + 0.00065
                = 0.21%
```

**Result**: The organization meets its risk appetite (0.21% < 5%).

### Investment Analysis

The stage-gated structure reveals WHERE investments matter most:

| Investment Option | Current | Improved | Risk Reduction |
|-------------------|---------|----------|----------------|
| Stage 1 V: 70%→85% | 37.2% detect | 44.8% detect | Reduces all downstream |
| Stage 1 R: 40%→60% | 37.2% detect | 48.1% detect | Highest leverage |
| Stage 5 V: 75%→90% | 60.6% detect | 72.7% detect | Too late in chain |
| Faster monitoring (M/2) | λ doubles | Higher P(Detect) | Improves all stages |

**Key Insight**: Stage 1 improvements have cascading effects—catching attacks earlier reduces the probability of reaching any later stage where losses are higher.

## Important Use Cases

### 1. Control Portfolio Optimization

Compare detection investments by their expected risk reduction. Stage-gated analysis reveals that early-stage improvements often provide 5-10× more risk reduction than late-stage investments of equal cost.

### 2. SLO Definition and Validation

The methodology naturally maps to operational SLOs:
- "Detect initial access within 4 hours" → Stage 1 detection parameters
- "Detect lateral movement within 24 hours" → Stage 4 detection parameters

Validate whether current capabilities can meet stated SLOs.

### 3. Risk Appetite Alignment

Explicitly calculate P(Exceed Threshold) and compare to board-approved appetite. The conditional distribution approach provides defensible estimates without asserting arbitrary loss curves.

### 4. Capital Reserves and Insurance

Use outcome class probabilities and loss distributions to inform:
- Capital reserve requirements
- Cyber insurance coverage levels
- Deductible decisions

## Key Insights

1. **Stage-gated structure reveals investment leverage**: Early-stage detection improvements have cascading benefits that aggregate models miss.

2. **Multi-review math matters**: With hourly monitoring and multi-hour dwell times, detection probability approaches the visibility ceiling—but only if reviews are independent.

3. **The ρ factor prevents overestimation**: Calibrate independence assumptions to your actual detection architecture.

4. **Conditional distributions are more defensible**: Avoid asserting specific time-to-loss curves; let SME-calibrated distributions represent uncertainty honestly.

5. **Attacker failure is real**: 10-20% of attacks stall regardless of detection; capture this in the model.

6. **Response concurrency matters**: Don't assume sequential containment → recovery; model actual overlap.

7. **Loss minimization is non-linear**: Apply insurance/BC-DR inside Monte Carlo, not as post-hoc adjustment.

8. **Mathematical bounds exist**: Detection probability cannot exceed Cov × V_eff regardless of how many reviews occur.

## Limitations and Caveats

1. **Stochastic methods required**: Use Monte Carlo simulation with distributions, not point estimates.

2. **Independence assumptions**: The multi-review formula assumes successive recognition attempts are independent (calibrated by ρ). Validate this assumption for your environment.

3. **Stage definitions matter**: Results are sensitive to how stages are defined and parameterized. Use consistent kill chain taxonomy.

4. **Historical data often limited**: May require SME estimates for many parameters; document assumptions explicitly.

5. **Not all attacks fit neat stages**: Highly automated attacks may collapse multiple stages; adjust model accordingly.

6. **Detection not always relevant**: Some events (power outages, natural disasters) are self-evident; this methodology applies to threats requiring active detection.
