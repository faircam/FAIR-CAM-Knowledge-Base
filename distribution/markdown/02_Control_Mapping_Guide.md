# Control Mapping to FAIR-CAM: Practical Guide

## Mapping Procedure (Follow for Every Request)

### Step-by-Step Process

1. Restate the control/objective in your own words (1 sentence)
2. Identify whether its effect on risk is direct (LE) or indirect via VM or DS
3. Leverage the Cybersecurity_Controls_FAIR-CAM_MAPPED.xlsx file to find similar pre-mapped controls
4. When file isn't helpful, make a carefully thought-out mapping of your own
5. Provide the FAIR-CAM path using canonical names (e.g., "Variance Management → Correction → Implementation")
6. Explain why in FAIR-CAM terms: how it changes TEF, Susceptibility, detection timing, containment time, variance frequency/duration, decision alignment, or loss magnitude
7. If multi-part, decompose and map each part separately
8. If uncertain, say so explicitly and state what additional context is needed
9. Include IntEff Example: A concrete, measurable target (e.g., "MFA required for 100% of privileged accounts")
10. Include how IntEff Is Defined: The policy/standard mechanism that establishes the intended efficacy
11. Include Variance Indicators: What to measure to detect when control is in variant state

## Critical Mapping Guardrails

### DO NOT Do These Things:

❌ Revert to generic "prevent/detect/respond" triad unless it maps to canonical functions

❌ Guess or invent labels when uncertain

❌ Map patching/configuration/hardening to Resistance (they're VM → Correction → Implementation)

❌ Assume all controls with "management" in the name go to one function

❌ Trust non-FAIR-CAM terminology from public resources without verification

### DO These Things:

✅ Be explicit about AND dependencies across LE Detection functions

✅ Map planning controls to DS → Misaligned Decision Prevention → Defining Expectations and Objectives

✅ Decompose "management" controls individually - they typically cover many functions

✅ Map vulnerability scanning to VM → Identification → Controls Monitoring (or DS → SA → Data → Control Data when input to analysis)

✅ Think carefully about Avoidance vs. Resistance

## Special Control Type Mappings

### Patching, Configuration, Hardening

**Always map to:** VM → Correction → Implementation

**Reasoning:** They correct weaknesses in LE controls (software), not resist attacks directly

### Vulnerability Scanning

**Typically:** VM → Identification → Controls Monitoring

**Or:** DS → Misaligned Decision Prevention → Situational Awareness → Data → Control Data (when used as input to analysis)

**Not:** LE Resistance

### Planning Controls

**Always map to:** DS → Misaligned Decision Prevention → Defining Expectations and Objectives

### Management Controls

Decompose first, then map each component individually

### Attack Surface Reduction

Be very careful with Avoidance:

✅ **Avoidance:** URL filters (prevent malicious contact from known sources)

❌ **NOT Avoidance:** Disabling local admin accounts (makes compromise harder = Resistance)

**Remember:** Avoidance limits frequency/probability of threat agent coming into contact with attack surface

## Units of Measurement by Function

| Domain | Function | Primary Unit |
|--------|----------|--------------|
| LE Prevention | Avoidance/Deterrence/Resistance | Probability (%) |
| LE Detection | Visibility & Recognition | Probability (%) |
| LE Detection | Monitoring | Time/cadence (frequency) |
| LE Response | Containment, Resilience | Time |
| LE Response | Loss Minimization | Currency |
| VM Prevention | Reduce Change Frequency | Frequency |
| VM Prevention | Reduce Variance Probability | Probability (%) |
| VM Identification | Controls/Threat Monitoring | Frequency |
| VM Correction | Implementation | Time (Variance Duration) |
| DS | Various | Frequency/Timeliness/Quality indicators |

## Output Format

Use this structure for every mapping (each mapping on separate line if multiple functions):

```
Control: [Name and description]
FAIR-CAM Mapping: <Domain → Function [→ Subfunction]>
Reasoning: <1-4 bullet points in FAIR-CAM terms>
Notes (if needed): <ambiguities, decomposition, or data gaps>
```

### Example Mapping Output

```
Control: DASF 56 — Restrict outbound connections from models
FAIR-CAM Mapping: Loss Event → Prevention → Resistance
Reasoning: Prevents data exfiltration/command-and-control. Acts as resistive control 
by blocking unauthorized network communications that could enable data theft or 
remote control of compromised systems.

Control: DASF 56 — Restrict outbound connections from models
FAIR-CAM Mapping: Loss Event → Response → Containment
Reasoning: Limits blast radius during compromise by preventing lateral movement 
and data exfiltration even if initial compromise occurs.
```

## Common Mapping Patterns

### Logging

- For external threats: LE → Detection → Visibility
- For insider threats: LE → Prevention → Deterrence (reduces TEF)
- Can be both depending on scenario context

### Software

- The software itself: LE → Prevention → Resistance
- Patching the software: VM → Correction → Implementation
- Scanning for vulnerabilities: VM → Identification → Controls Monitoring

### Access Controls

- Access privileges/permissions: LE → Prevention → Resistance
- Access management process: VM → Correction → Implementation
- Access review/audit: VM → Identification → Controls Monitoring
- Access policy: DS → Misaligned Decision Prevention → Define Expectations

### Training

- Security awareness training: DS → Misaligned Decision Prevention → Communicate Expectations
- Skills training for specific tasks: DS → Misaligned Decision Prevention → Ensure Capability
- Measuring training completion: VM → Identification → Controls Monitoring (with timeframe)

### Policies

- **Always:** DS → Misaligned Decision Prevention → Define Expectations and Objectives
- **Impact:** Indirect - reduces frequency of misaligned decisions

### Monitoring Solutions (SIEM, EDR, etc.)

Decompose based on capabilities:
- Logging/telemetry → LE → Detection → Visibility
- Automated review → LE → Detection → Monitoring
- Signature/heuristic detection → LE → Detection → Recognition
- Automated blocking → LE → Response → Containment
- Could also include VM → Identification → Controls Monitoring

## Handling Uncertainty

When mapping is uncertain, use this template:

```
Control: [Name]
FAIR-CAM Mapping: Uncertain
Reasoning: This mapping is uncertain within FAIR-CAM without additional context. 
It may decompose into [Option A], [Option B], or [Option C] depending on:
- The specific scenario being analyzed
- The exact control scope and mechanism
- Whether this is a policy, technical control, or process
Notes: Please specify [what you need to know] to complete accurate mapping.
```

## Context-Dependent Mapping Examples

### Encryption

**Data at rest encryption:**
- Context 1 (Preventing unauthorized access): LE → Prevention → Resistance
- Context 2 (Minimizing breach impact): LE → Response → Loss Minimization

**Data in transit encryption:**
- LE → Prevention → Resistance (prevents interception/manipulation)

### Backups

**In availability scenario:**
- LE → Response → Resilience (enables recovery)

**Not relevant in:**
- Confidentiality breach scenarios (data already exposed)

### Network Segmentation

Creates sequential attack surfaces (architectural DiD)
- More complex multi-surface analysis required
- May function as both Resistance (harder to reach targets) and Containment (limits lateral movement)

## Framework Control Translation Challenges

When translating from frameworks like NIST CSF, ISO 27001, CIS, etc.:

1. **Broad definitions map to multiple functions** - decompose first
2. **No standard rating scales** - be careful with quantification
3. **"Mungy" controls** - separate the distinct functions
4. **Context matters** - same control may map differently per scenario

### Example: NIST CSF PR.AA-05

"Access permissions, entitlements, and authorizations are defined in a policy, managed, enforced, and reviewed"

**Decomposes to:**
- "Defined in policy" → DS → Define Expectations
- "Access permissions themselves" → LE → Resistance
- "Managed" → VM → Correction → Implementation
- "Enforced" → DS → Proper Incentives
- "Reviewed" → VM → Identification → Controls Monitoring

## Quick Reference Decision Tree

### "Which Domain Does This Control Belong In?"

1. Does it directly prevent loss events? → LE Prevention (Avoid/Deter/Resist)
2. Does it detect loss events? → LE Detection (Vis/Mon/Recog - remember AND relationship)
3. Does it minimize losses during/after event? → LE Response
4. Does it correct variant conditions? → VM Correction (patching, reconfiguration)
5. Does it identify variance? → VM Identification (scanning, auditing, monitoring)
6. Does it prevent variance? → VM Prevention (reduce change freq/probability)
7. Does it improve decision quality? → DS (policies, training, incentives, SA)
8. Does it identify bad decisions? → DS Identification (RCA, audits)

### "How Do I Measure This Control's Effect?"

1. What function does it serve? (Use domain tree above)
2. What's the unit of measurement for that function? (See reference tables)
3. Is it binary or non-binary?
4. What are IntEff, VarEff, VF, VD, Coverage?
5. Calculate Reliability, then OpEff
6. Apply to appropriate FAIR factor (TEF, Susceptibility, Loss Magnitude)

For Detection controls specifically, use the stage-gated methodology:
1. Which attack stage(s) does this control address?
2. What are the stage-specific V, R, M parameters?
3. What is the Review Independence Factor (ρ) for this detection architecture?
4. Calculate detection probability using: P(Detect) = Cov × V_eff × [1 - (1 - R_eff)^(ρ × λ)]

### "Why Did This Control Fail?"

Use RCA 5 dimensions:
1. Governance - did expectation exist?
2. Awareness - was party aware?
3. Capability - did they have means?
4. Prioritization - was it deprioritized?
5. Intent - was it malicious?

Then map to FAIR-CAM domain for remedy.

## Terminology to Watch

| Public/Framework Term | FAIR-CAM Caution |
|----------------------|------------------|
| "Attack Surface Reduction" | Often means resistance, not avoidance |
| "Compensating Control" | Doesn't map to specific function - analyze what it does |
| "Administrative Control" | Usually DS, but could be VM |
| "Technical Control" | Could be LE, VM, or DS - depends on function |
| "Management" | Almost always needs decomposition |
| "Monitoring" | Could be LE Detection Monitoring OR VM Controls Monitoring |
