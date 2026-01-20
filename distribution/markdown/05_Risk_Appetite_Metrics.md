# Risk Appetite, Metrics & Board Reporting with FAIR-CAM

## Risk Appetite Definition (Board-Ready)

### Working Definition

Risk appetite is the explicit combination of:

1. Loss magnitude threshold (specific $ amount or impact)
2. Maximum probability of exceeding it
3. Within defined timeframe (typically 12 months)

### Template Examples

- **Breach**: "Less than 5% probability in next 12 months of disclosing >1M customer PII records"
- **Outage**: "Less than 5% probability in next 12 months of >100k lost transactions in 24-hour period"
- **Regulatory**: "Less than 5% probability in next 12 months of cyber-related regulatory action"
- **Financial**: "Less than 5% probability in next 12 months of financial misstatement >$10M from IT/cyber"

### Why This Framing Works

- Clear expectations aligned to specific loss event types
- Actionable thresholds that focus effort and communication
- Seamless integration with FAIR/CRQ and control analytics
- Objective measurement of whether appetite is met

## Translating Appetite into Control Objectives

FAIR-CAM enables setting measurable targets for each control function that collectively meet appetite.

### Example: Breach Appetite (<5% probability, >1M PII records, 12 months)

**VM Controls:**
- Reduce Change Frequency: Limit changes on crown jewel and privileged systems that could introduce exploitable conditions
- Reduce Variance Probability: Pre-deployment validation, secure SDLC gates
- Controls Monitoring: Continuous scanning/auditing for exploitable conditions; threat intel for zero-days
- Implementation (Correction): SLA targets by exposure class:
  - Internet-facing crown jewels: ≤1 exploitable condition per 3 years; remediate within 48 hours
  - Non-internet-facing equivalents: ≤2 exploitable conditions per year; remediate within 7 days

**LE Detection (Stage-Gated Approach):**
- Define detection SLOs for each attack stage
- Ensure Visibility (telemetry exists), Monitoring (review within X hours), Recognition (trained analysts/rules) meet detection time budget
- Apply stage-gated methodology to calculate P(detection by stage)
- Target overall early detection probability to keep realized losses below threshold

**LE Response:**
- Time-to-contain and time-to-restore sized to keep realized losses below threshold if detection triggers
- Account for response concurrency factor (α) in time estimates

## Measurement Units by FAIR-CAM Function

| Domain | Function | Primary Unit |
|--------|----------|--------------|
| LE Prevention | Avoid/Deter/Resist | Probability of contact/attempt/success |
| LE Detection | Visibility & Recognition | Probability |
| LE Detection | Monitoring | Time/cadence |
| LE Response | Containment, Resilience | Time |
| LE Response | Loss Minimization | Currency |
| VM | VF/VD/Cov/VarEff & Implementation | Frequency, Time, % Coverage |
| DS | Communication, Capability, SA, Incentives | Frequency/Timeliness/Quality |

## Metrics That Support Appetite & Decisions

### Two Families (Don't Mix)

**KRIs (Key Risk Indicators):**
- Probability of exceeding appetite thresholds
- Susceptibility of key attack paths
- Count of crown jewels by exposure class
- Scenario loss distribution percentiles (P50, P90)

**KPIs (Key Performance Indicators):**
- Variance & decision quality indicators
- VF, VD, Coverage %, Policy exceptions
- RCA distribution (Governance, Awareness, Capability, Prioritization, Intent)
- On-time corrections, training completion (with timeframe)

### Reliability & Operational Efficacy Metrics

**Reliability (Rel):**
Percentage of time control operates in intended state.

```
Rel = (1 - VF/365)^VD
```

**Operational Efficacy (OpEff):**
Real protective power over time and population.

```
OpEff = Cov × [(Rel × IntEff) + ((1-Rel) × VarEff)]
```

### Detection Pipeline KPIs (Stage-Gated Approach)

Define detection SLO (e.g., "detect within 4 hours") and measure using stage-gated methodology:

**Per-Stage Metrics:**
- Coverage: % of attack surface with detection controls deployed
- Effective Visibility: V × Rel_V
- Effective Recognition: R × Rel_R
- Reviews per stage (λ): τ / (M / Rel_M)
- Review Independence Factor (ρ): Calibrated to detection architecture

**Stage Detection Probability:**
```
P(Detect_i) = Cov_i × V_eff,i × [1 - (1 - R_eff,i)^(ρ_i × λ_i)]
```

**Outcome Class Probabilities:**
Track distribution across Early, Mid, Late, Full Impact, Attacker Fails.

**Mathematical Bound Check:**
P(Detect_i) ≤ Cov_i × V_eff,i (detection cannot exceed visibility ceiling)

## Board Reporting: The 1-Page Roll-Up

### Top Panel - Appetite Alignment (KRIs)

For each defined appetite (Breach, Outage, Regulatory, Financial):
- Statement: Threshold + timeframe
- Current probability of exceedance (with 4-quarter trend)
- Status: Aligned / Near limit / Out of appetite

### Middle Panel - Leading Indicators (KPIs)

**Variance:**
- VF/VD for top LE controls on crown jewels
- Coverage % for critical telemetry (EDR, logging) and backups

**Detection SLO:**
- % detected by stage (Early, Mid, Late)
- P(Early Detection) trend—higher is better
- Comparison to target outcome distribution

**RCA (DS):**
- Mix of root causes for control failures
- Policy exception count for crown jewels

### Bottom Panel - Options & Outlook

- Remediation options framed as FAIR-CAM levers (what knob moves and by how much)
- Forecast: Expected change in probability-of-exceedance next quarter if option is funded

**Rule:** Every board slide maps each proposed action to its FAIR-CAM function(s) and the unit it improves.

## Getting Aligned vs. Staying Aligned

### Getting Aligned (Initial Setup)

1. Identify crown jewels per appetite (assets whose compromise exceeds threshold)
2. Estimate current probability-of-exceedance for each appetite scenario
3. If above appetite: Pick options that reduce:
   - Susceptibility (LE Resistance)
   - Detection probability gaps (address specific stages using stage-gated analysis)
   - Containment/Resilience times
   - VF/VD/Coverage gaps via VM

### Staying Aligned (Ongoing)

**DS → Misaligned Decision Prevention:**
- Define & communicate expectations
- Ensure capability
- Set incentives

**DS → Provide Situational Awareness:**
- Curate Asset/Threat/Control data → Analysis → Reporting tuned to appetites

**VM → Identification & Correction:**
- Controls monitoring + fast implementation SLAs on crown jewels

**VM → Prevention:**
- Limit changes; require security gates on crown jewel paths

## Worked Example: Breach Appetite

**Appetite:** <5% probability in next 12 months of disclosing >1M customer records

### Control Objectives Set

**VM:**
- Prevention: Limit changes; security gates on crown jewels
- Identification: Continuous scanning; zero-day alerts to owners within 1 hour
- Correction: 48h remediation for internet-facing exploitable conditions; 7d for internal

**LE Detection (Stage-Gated):**
- Stage 1-2 (Early): Target 80%+ detection probability
- Stage 3-4 (Mid): Target 70%+ detection probability
- Overall target: P(Early Detection) ≥ 85%
- Visibility: ≥95% coverage on crown jewels
- Monitoring: Review within 1 hour (M = 0.042 days)
- Recognition: ≥60% effective recognition rate

**LE Response:**
- Containment: Within 2 hours
- Resilience: RTO ≤8 hours for customer-facing apps

### Quarterly Scorecard Snippet

| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| Prob of exceeding threshold | 3.2% | <5% | ✓ Green |
| P(Early Detection) | 82% | 85% | ⚠ Yellow |
| P(Full Impact) | 0.08% | <0.5% | ✓ Green |
| VF (internet-facing exploitable) | 0.3/yr | ≤0.33/yr | ✓ Green |
| Median remediation | 36h | ≤48h | ✓ Green |
| P90 remediation | 60h | ≤72h | ✓ Green |
| EDR Coverage | 98% | 100% | ⚠ Yellow |
| Critical logging coverage | 96% | 100% | ⚠ Yellow |

**Stage-Gated Detection Analysis:**

| Stage | P(Detect\|Reach) | Contribution to E[Loss] |
|-------|-----------------|------------------------|
| 1 | 38% | $3,920 |
| 2 | 78% | $3,510 |
| 3 | 72% | $1,240 |
| 4 | 79% | $890 |
| 5 | 61% | $620 |
| 6 | 28% | $85 |
| Full Impact | - | $1,560 |

**Action:** Stage 1 detection improvement (V: 70%→85%) would shift $2,400 of expected loss from later stages to Early outcome.

**RCA last 6 control failures:**
- Prioritization: 2
- Capability: 2
- Awareness: 1
- Intent: 1

**Action:** Targeted DS interventions for Prioritization and Capability root causes.

## Common Governance Items Mapped to FAIR-CAM

| Governance Item | FAIR-CAM Mapping |
|-----------------|------------------|
| "No crown jewels in dev/test" | DS → Define Expectations + VM → Prevention (reduce variance probability) + VM → Identification (spot policy exceptions) |
| "Crown jewel exceptions require CXO sponsor" | DS → Incentives (decision stakes) + DS → SA → Reporting (visibility of exceptions) |
| "100% accurate asset data for crown jewels" | DS → SA → Data → Asset Data (enables LE Detection/Response and VM accuracy) |

## Guardrails for Metrics

### Avoid Flaw of Averages

- Pair Median with Mean
- Add P90 where useful (time-based SLOs, remediation windows)
- Averages hide crucial outliers in high-variance processes

### Beware Goodhart's Law

"When a metric becomes a target, it ceases to be a good metric"

**Examples of Goodhart's Law:**
- Healthcare: Time limit for ER treatment → hospitals leave patients in ambulances so clock doesn't start
- IT: Mean Time to Patch target → teams fix simple low-impact issues, ignore critical vulnerabilities

**Solution:** Use Median + P90 + tie to actual risk reduction, not just metric achievement

- Promote metrics to "key" sparingly
- Rotate audit checks periodically to prevent gaming
- Use multiple indicators, not single metric

## Key Takeaways

1. **Appetite must be explicit**: Magnitude + probability + timeframe, not vague "low/medium/high"
2. **Control objectives flow from appetite**: Use FAIR-CAM to set measurable targets for each function
3. **KRIs ≠ KPIs**: Risk indicators vs. performance indicators serve different purposes
4. **Detection is AND relationship**: All three subfunctions must work; use stage-gated analysis to find bottlenecks
5. **Reliability matters more than Intended Efficacy**: Controls that work 90% of time at 80% effectiveness > controls that work 50% of time at 95% effectiveness
6. **Stage-gated analysis reveals leverage**: Early-stage detection investments often provide 5-10× more risk reduction than late-stage investments
7. **RCA distribution tells story**: Chronic Prioritization issues = systemic DS problem
8. **Board wants options**: Not just status, but "if we invest $X in Y function, probability drops Z%"
9. **Use natural units**: Don't force everything into percentages; time, frequency, currency are often more meaningful
10. **Crown jewels are priority**: If you can't focus on everything, focus on what matters most
11. **Variance is everywhere**: No control maintains 100% OpEff; plan for variance management
