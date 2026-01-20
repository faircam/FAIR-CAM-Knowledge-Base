# FAIR-CAM: Key Insights & Practical Guidance

## Critical FAIR-CAM Insights (The "Less Obvious" Principles)

### 1. Context Flips What a Control "Is"

Same mechanism can be:
- Deterrence (reducing TEF) for insiders
- Visibility (enabling Detection) for outsiders

Function is scenario-dependent, not intrinsic to the tool. Static control catalogs mislead.

### 2. Binary Controls Really Do Have 100% Intended Efficacy

- Access privileges either prevent unauthorized action or don't
- If code-exec bypass occurs, software failed as resistive control - not the privileges
- Clean separation of failure domains clarifies causal attributions

**But:** Operational Efficacy is never 100% (variance always exists)

### 3. Use Natural Units - Avoid "Alchemy"

FAIR-CAM insists on unit that governs function:
- Percentage, time, frequency, or money
- Not forcing everything into % scale
- Reduces hidden assumptions, makes explanations defensible, enables empirical measurement

### 4. Reliability Ties Variance to Time Explicitly

```
Rel = (1 - VF/365)^VD
```

- Couples variance frequency and duration
- Lets you tune VM to tempo of threat events

### 5. Coverage Turns "Attack Surface" into Control Parameter

```
OpEff = Cov × [...]
```

Shadow IT and partial deployments are quantified directly in control efficacy, not hand-waved.

### 6. Prevention is OR; Detection is AND

- **Prevention (Avoid/Deter/Resist):** Protection if ANY are working (OR)
- **Detection (Vis/Mon/Recog):** Requires ALL THREE to operate (AND)

Explains: Why Detection underperforms and why "weakest link" only fits some landscape parts.

### 7. Detection's Unit Mismatch Forces Threshold-Based Math

- Monitoring = cadence/time
- Visibility & Recognition = probabilities

The stage-gated methodology converts "detection within X hours" into probability problem:
- Calculates λ (reviews per stage) from stage duration and monitoring cadence
- Applies multi-review formula: P(Detect) = Cov × V_eff × [1 - (1 - R_eff)^(ρ × λ)]
- ρ (Review Independence Factor) calibrates assumptions

### 8. Response Predicated on Detection (Another AND)

No detection → no response

- Aligns policy thresholds to measurable capability
- Stops teams from modeling response as independent cushion
- Pushes investment toward upstream visibility/cadence

### 9. Concurrency (α) Captures Real-World Overlap

```
T = T_cont + T_res - α × min(T_cont, T_res)
```

- Acknowledges response workstreams can partially overlap
- Sharpens downtime forecasts and SLA design
- α = 0 (sequential), α = 1 (parallel), 0-1 (partial)

### 10. Loss Minimization is Direct Subtraction, Not "Control Efficacy"

- Insurance/penalties modeled in dollars and netted against loss
- Cleanly separated from prevention/detection/response efficacy
- Keeps causal logic straight

### 11. Individual vs. Population Efficacy Are Different Phenomena

- Single person's access may be stable for years
- Population-level OpEff drops due to transfers, turnover, process variance
- Need Coverage and variance parameters at scale

### 12. "Software IS a Control"

By listing software under Resistance:
- Vulnerabilities = control variance
- Patching = variance correction (VM → Implementation)
- Not hygiene abstractions but first-class levers on susceptibility

### 13. VM & DS Are Indirect But Systemic

Applied to LE controls and to themselves:
- Policies affect patching
- Auditing checks policies AND patching
- Training increases personnel resistance

Recursive structure explains why hygiene issues are always VM/DS problems upstream.

### 14. Treat Causes, Not Symptoms - With Incentives as DS Controls

CISO anecdote: Light-touch escalating incentive fixed access-change failures.

FAIR-CAM calls out root-cause analysis as DS control.

### 15. Visibility Can Be Binary or Non-Binary - Scope Matters

- **Binary:** Logging targets specific indicator (SQLi)
- **Non-binary:** Spans broad abuse classes (probabilistic)

Nuance guides how to instrument and measure.

### 16. Detection Isn't Always Relevant

Outages obvious to humans (power loss, natural disasters):
- Detection controls add little value
- Prevents over-fitting SIEM-centric thinking to every scenario

### 17. Quantifying Defense-in-Depth via Susceptibility Multiplication

Layered resistive controls combine by multiplying susceptibilities (complements of OpEff) - if independent.

Then immediately problematizes independence:
- VM/DS drivers are systemic
- Agent-based modeling (ABM) can estimate correlations when data sparse
- Big deal for realistic ROI math

### 18. Framework Ratings Are Multi-Functional and "Mungy"

Single CSF statement (PR.AA-05) maps to multiple FAIR-CAM functions with different units.

Without explicit translation, "2/5" score bakes in unknown assumptions.
Breaks CRQ reliability.

### 19. Policy Thresholds Should Be Evidence-Driven

Detection-in-four-hours example ends with governance fork:
- Improve monitoring cadence/reliability, OR
- Change the policy

FAIR-CAM turns "policy" from aspiration into measurable performance contract.

### 20. VM Knobs Map Cleanly to Math Knobs

Operational levers line up with parameters:
- Reduce VF: Automation, change discipline
- Shorten VD: Faster discovery/correction
- Increase Cov: Deployment reach
- Raise VarEff: Graceful degradation where possible

Roadmap falls directly from:
```
OpEff = Cov × [(Rel × IntEff) + ((1-Rel) × VarEff)]
```

### 21. DS/VM ≠ "Less Important" Than LE - Just Indirect

Stating explicit indirect effect prevents under-investment in capabilities that keep controls healthy over time.

### 22. Evidence-Based Prioritization

Because function, unit, and logic (AND/OR) are explicit:
- Target bottleneck in AND chain (usually Monitoring)
- Instead of over-investing in already-good Visibility

Pick levers (cadence, reliability, staffing, automation) to meet them.
Or change SLO transparently.

## Practical Applications This Unlocks

### 1. Policy-to-Capability Alignment

Express detection/response SLOs in time and probability.
- Pick levers (cadence, reliability, staffing, automation) to meet them
- Or change SLO transparently

### 2. Real DiD ROI

Use susceptibility math for layered resistive controls.
- Be explicit about (likely) dependence
- Consider ABM to estimate correlation where stakes justify it

### 3. Root-Cause Governance

Treat recurring hygiene issues as DS/VM gaps.
- Use incentives, visibility, reconciliation
- Rather than more "awareness emails"

## Automation Considerations

### You Can't Automate All Analyses

One-off, oddball scenarios won't fit cookie-cutter automation.

- **Can automate:** Common, straightforward scenarios
- **Can't automate:** Unique edge cases
- **Should automate:** Most frequent scenarios to free up time for complex ones

### Automation Can Scale Poor Decision-Making

Fundamentally flawed models systematically introduce errors.

**Example: NIST 800-30 Table G-5**
- Derives overall likelihood from attack likelihood × success likelihood
- Logical flaw: ~2/3 of cells contain impossible values
- Output likelihood can't exceed input likelihood (nowhere in universe does this work)
- Manual analysis: Poor measurement affects specific decisions
- Automated analysis: Poor measurement cripples entire landscape

### Critical Automation Challenges

**Scoping is especially challenging:**
- Subtleties identified/adjusted in manual analysis
- Must be assumed and baked into automated solution
- "Out of box" scenarios need pre-definition
- User tweaking introduces new challenges with data sources

**Attack surfaces must be accounted for:**
- Initial, subsequent, and final attack surfaces
- Different controls at different efficacies per surface

**Data interpretation required:**
- Most orgs have poor understanding of asset landscape
- Security telemetry not designed for this analysis
- Data must be parsed and interpreted appropriately
- SME input needed when automated sources unavailable (very common)

**Algorithm must understand control landscape as system:**
- Which controls relevant to each scenario
- Direct vs. indirect effects (LE vs. VM/DS)
- How controls affect one another
- Boolean AND vs. OR relationships

**Framework scores are problematic:**
- Different units of measurement
- How to interpret control-related data
- Mungy definitions (multiple functions combined)
- No standard rating scales
- Qualitative definitions
- Ordinal values require translation with many assumptions

### The Dialog Loss

Manual FAIR analysis conversations provide as much or more value than results:
- Organization learns subtle things about risk posture
- Personnel understand world through FAIR lens
- Knowledge and skills grow organically
- Understanding spreads via osmosis

Automation loses this learning opportunity - significant inherent downside.

## Working with Framework Scores (If You Must)

### Challenges You'll Face

1. Which subcategories are relevant? Many VM/DS affect risk indirectly
2. What does the score represent? All functions at that level? Average? Highest or lowest?
3. Which attack surface? Some controls relevant to one, multiple, or none
4. How to translate scores to quantitative values?
5. What's the rating scale? (3-level, 4-level, 5-level, 10-level?)

### If You Must Use Framework Scores

**Step 1:** Determine what score represents (maturity? reliability? OpEff?)

**Step 2:** Create translation table(s):
- Different table for each unit of measurement
- Different table for each scale (3, 4, 5, 10-level)
- Justify the ranges (why does "2" = 26-50%?)

**Example translation (4-level scale, percentage-based):**

| Score | Min | Most Likely | Max |
|-------|-----|-------------|-----|
| 1 | 0% | 13% | 25% |
| 2 | 26% | 38% | 50% |
| 3 | 51% | 63% | 75% |
| 4 | 76% | 88% | 100% |

**Problems with this:**
- Why would I invest in "4" if it might only be 76% effective?
- At "3" best case is 75%, worst is 51% - huge range
- No empirical basis for these ranges

**Step 3:** Map each subcategory to FAIR-CAM function(s)

**Step 4:** Account for broad definitions that map to multiple functions

**Example:** "Access permissions are defined, managed, enforced, and reviewed"
- Defined = DS
- Permissions themselves = LE
- Managed = VM
- Enforced = DS
- Reviewed = VM

If score is "3": Does that apply to all functions? Is it average? Highest or lowest?
You must make assumption and document it.

## Crown Jewel Management

### Why It Matters

Impossible to protect everything with limited resources.
- Prioritize what matters most
- Crown jewels are where compromise exceeds tolerance

### Four Categories

1. Operationally critical systems (business continuity)
2. Systems containing high-value/sensitive information
3. Endpoints of highly privileged personnel (admins, executives)
4. Security systems themselves
5. (Industry-specific) Payment, medical treatment, etc.

### How to Identify

**Relatively straightforward:**
- Business continuity plans (critical systems)
- Security systems (obvious)

**Challenging:**
- Information concentrations (fluid asset)
- Interview DBAs, PMs, operations personnel
- Use DLP to crawl network and identify concentrations

Surprises are common: Old data dumps, personal laptops, shadow IT

### Crown Jewel Policies

**Must have:**
- Policy describing what qualifies as crown jewel
- Process for standing up new crown jewels (reviews, approvals)
- Specific risk management requirements for crown jewels
- Exception process (executive-level approval only)

**Metrics:**
- Number of crown jewels by category
- Number of non-compliant crown jewels (target: 0)
- Number of internet-facing crown jewels
- Variance Frequency, Duration, Coverage for crown jewel controls
- % of crown jewel deficiencies with RCA completed

## Common Metrics Pitfalls

### Benchmarking (Generally Low Value)

**Problems:**
- No standard scoring scale
- "2" for you ≠ "2" for them
- Assumes "herd has clue" (often doesn't)
- Pre-modern medicine analogy: benchmarking bloodletting physicians

**When it matters:** Gross outliers may indicate serious gaps

### Mean Time to Patch (Problematic)

**Problems:**
- Average hides crucial variance
- Doesn't account for criticality
- Doesn't filter out low-priority vulnerabilities

**Better approach:**
- Filter population first (value, threat landscape, severity)
- Use Median + P90, not just Mean
- FAIR-CAM utility: Informs Variance Duration

### Number of Outstanding Patches (Not Useful)

**Purpose:** Impress/depress executives about challenge

**Better approach:** Focus on critical vulnerabilities in crown jewels

### Phishing Click Rates (Overemphasized)

**Reality:** Humans are always least effective anti-phishing control

**Research shows:** Training has virtually no effect on reducing phishing losses

**FAIR-CAM utility:** Measures OpEff of personnel as Resistive controls

### "Lips on a Chicken" Metrics

Metrics that exist but provide no decision value:
- Number of outstanding patches (unfiltered)
- Failed logins (too noisy unless filtered)
- Raw malware detection counts (without context)

## Best Practices Summary

### For Control Mapping

1. Always decompose "management" controls
2. Check context (insider vs. outsider changes function)
3. Be explicit about AND dependencies
4. Use natural units, don't force percentages
5. When uncertain, say so and specify what's needed

### For Risk Analysis

1. Establish clear scenario scope
2. Account for all three attack surface types (initial, subsequent, final)
3. Identify all relevant controls across LE, VM, and DS
4. Use distributions, not point estimates
5. Apply Monte Carlo or stochastic methods
6. Document assumptions explicitly

### For Metrics

1. Use Median + P90, not just Mean
2. Guard against Goodhart's Law
3. Limit "key" metrics to truly important few
4. Track RCA distribution for systemic insights
5. Measure outcomes (recurrence, risk change), not just activities

### For Communication

1. Use natural language, avoid jargon
2. Map every control to FAIR-CAM function
3. Explain "why" in FAIR-CAM terms
4. Board reports: Options + forecast impact
5. Every recommendation states which knob moves and by how much

### For Automation

1. Start with most common scenarios
2. Account for scenario context sensitivity
3. Build in uncertainty handling
4. Enable SME input where data unavailable
5. Make models transparent, not black boxes
6. Remember: loses the valuable dialog

## Quick Decision Trees

### "Which Domain Does This Control Belong In?"

1. Directly prevents/detects/responds to loss events? → LE
2. Corrects variant conditions? → VM Correction
3. Identifies variance? → VM Identification
4. Prevents variance? → VM Prevention
5. Improves decision quality? → DS Prevention
6. Identifies bad decisions? → DS Identification

### "How Do I Measure This Control's Effect?"

1. What function does it serve? (Use domain tree above)
2. What's the unit of measurement for that function? (See reference tables)
3. Is it binary or non-binary?
4. What are IntEff, VarEff, VF, VD, Coverage?
5. Calculate Reliability, then OpEff
6. Apply to appropriate FAIR factor (TEF, Susceptibility, Loss Magnitude)

### "Why Did This Control Fail?"

Use RCA 5 dimensions:
1. Governance - did expectation exist?
2. Awareness - was party aware?
3. Capability - did they have means?
4. Prioritization - was it deprioritized?
5. Intent - was it malicious?

Then map to FAIR-CAM domain for remedy.

## The One Metric That Might Rule Them All

**Probability of exceeding predefined threshold of loss:**

"The probability of exceeding $X of loss in the next 12 months is Y%"

**Why it matters:**
- Directly addresses fundamental objective: "acceptable level of risk"
- Enables clear board communication
- Informs capital reserves and insurance
- Evaluates relevance of security weaknesses
- Evaluates relevance of risk management improvements

Can be overall or categorical (breach, outage, regulatory, financial).

**Categorical advantages:**
- More explicit about executive concerns
- Easier to measure
- More actionable mitigation options

**Time horizon:**
- 12 months aligns with annualized FAIR inputs (TEF, VF)
- Cyber landscape too dynamic for longer forecasts
- But can forecast farther if needed
