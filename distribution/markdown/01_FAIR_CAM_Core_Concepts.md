# FAIR-CAM: Core Concepts and Model Overview

## What is FAIR-CAM?

The FAIR Controls Analytics Model (FAIR-CAM) is the first rigorous attempt to define control physiology - understanding how controls actually work, both independently and as an interdependent system. It fills the gap between control anatomy (knowing what controls exist) and understanding their actual effectiveness.

## Three Functional Domains

FAIR-CAM organizes all controls into three functional domains:

### 1. Loss Event (LE) Controls

Directly affect risk by reducing frequency or magnitude of loss events.

**Prevention Functions:**
- **Avoidance**: Reduce contact frequency between threat agents and assets (e.g., email/URL filtering)
- **Deterrence**: Reduce probability of malicious action after contact (e.g., logging for insiders)
- **Resistance**: Reduce likelihood threat action results in loss (e.g., authentication, access privileges, anti-malware, software)

**Detection Functions (AND relationship - all three required):**
- **Visibility**: Provide evidence of activity (e.g., logging, EDR)
- **Monitoring**: Review data from visibility controls (frequency-based)
- **Recognition**: Differentiate normal from abnormal activity (e.g., signatures, baselines)

**Response Functions:**
- **Containment**: Terminate threat agent access/activities (time-based)
- **Resilience**: Maintain/restore normal operations (time-based)
- **Loss Minimization**: Reduce realized losses (currency-based, e.g., insurance)

### 2. Variance Management (VM) Controls

Indirectly affect risk by managing frequency and duration of variant controls. Note that variance can occur to any type of control (VM, DS, LE), so VM applies to all types of controls.

**Prevention:**
- Reduce Change Frequency: Limit changes that introduce variance
- Reduce Variance Probability: Lower chance changes create issues

**Identification:**
- Threat Capability Monitoring: Detect threat landscape changes
- Controls Monitoring: Recognize variant control conditions
- Treatment Selection & Prioritization: Appropriately prioritize remediation

**Correction:**
- Implementation: Correct variant conditions (e.g., patching, reconfiguration)

### 3. Decision Support (DS) Controls

Indirectly affect risk by enabling aligned decision-making.

**Misaligned Decision Prevention:**
- Define Expectations & Objectives: Establish policies/standards
- Communicate Expectations: Training, awareness
- Provide Situational Awareness: Data → Analysis → Reporting
- Ensure Capability: Skills, authority, resources, tools
- Proper Incentives: Align motivation with objectives

**Misaligned Decision Identification:**
- Root Cause Analysis, auditing, post-mortems

## Critical Relationships

**Control Dependencies:**
- LE Prevention (OR logic): If ANY prevention control works, some protection exists
- LE Detection (AND logic): ALL three functions (Visibility, Monitoring, Recognition) must work for detection
- Response depends on Detection: No detection = no response
- VM and DS affect LE controls: They manage reliability and decision quality

## Key Control Parameters

### Intended Efficacy (IntEff)

How well control works when operating as intended

- Binary controls: 100% when working, 0% when variant
- Non-binary controls: Represented as distributions

For controls where the unit of measurement isn't a percentage, the intended efficacy should be expressed in terms of the appropriate unit of measurement, often set forth through policies or standards. For example:
- An organization's policy says critical vulnerabilities should be patched within 48 hours, so 48 hours is the intended efficacy for that control
- An organization's policy says that vulnerability scans should be performed daily on Internet-facing systems, so the intended efficacy (as a frequency) for that control is daily
- An organization's policy says that recovery times for crown jewel assets should be < 24 hours
- An organization's policy says that cyber insurance coverage for ransomware should be $20M

### Variant Efficacy (VarEff)

Control's efficacy when in variant state

- Binary controls: 0%
- Non-binary controls: Some residual effectiveness

For controls where the unit of measurement isn't a percentage, variant efficacy should be expressed in terms of the appropriate unit of measurement. For example:
- Critical vulnerabilities are being patched within 7 days instead of 48 hours
- Vulnerability scans are occurring quarterly rather than daily
- Crown jewel assets are being recovered in 72 hours rather than 24 hours
- Cyber insurance coverage is only $10M rather than $20M

### Variance Frequency (VF)

How often (per year) control becomes variant

### Variance Duration (VD)

How long control remains variant (days)

### Reliability (Rel)

Percentage of time control operates in intended state

```
Rel = (1 - VF/365)^VD
```

### Coverage (Cov)

Percentage of landscape where control is applied

### Operational Efficacy (OpEff)

Ground truth of control effectiveness over time and population

```
OpEff = Cov × [(Rel × IntEff) + ((1-Rel) × VarEff)]
```

## Units of Measurement by Function

| Function | Unit |
|----------|------|
| Avoidance, Deterrence, Resistance | Percentage |
| Visibility, Recognition | Percentage |
| Monitoring | Frequency (time) |
| Containment, Resilience | Time |
| Loss Minimization | Currency |
| VM Correction (Implementation) | Time |
| VM Identification (Controls Monitoring) | Frequency |

## Context Sensitivity

The same control can serve different functions depending on scenario:

- Logging for insider threats = Deterrence (reduces TEF)
- Logging for external threats = Visibility (enables Detection)
- Software = Resistive control against code exploitation

## Defense-in-Depth Types

1. **Intra-functional**: Multiple controls serving same function (e.g., WAF + secure code both resist exploits)
2. **Inter-functional**: Controls serving complementary functions (e.g., Resistance + Detection + Response)
3. **Architectural**: Sequential attack surfaces (network segmentation)

## Critical Insights

1. **Binary controls exist**: Access privileges are 100% effective when configured correctly, 0% when not
2. **Operational efficacy ≠ Intended efficacy**: Controls never maintain 100% effectiveness over time
3. **Use natural units**: Avoid forcing everything into percentages
4. **Variance has two dimensions**: Frequency AND duration both matter
5. **Detection is weakest link**: AND dependency across Visibility, Monitoring, Recognition
6. **Prevention is strongest link**: OR relationship means any working control provides protection
7. **Software IS a control**: Treat vulnerabilities as control variance, patching as variance correction
8. **Temporal relationships matter**: VF, VD, and TEF interact over time to determine LEF

## Relationship to FAIR Model

FAIR-CAM complements FAIR by:

- **TEF**: Affected by Avoidance and Deterrence controls
- **Susceptibility**: Affected by Resistance controls (Susceptibility = 1 - OpEff)
- **LEF**: Result of TEF × Susceptibility considering control reliability
- **Loss Magnitude**: Affected by Detection, Response, Loss Minimization controls

## Measuring Detection and Response Effects

Detection and response controls affect Loss Magnitude by influencing how much damage occurs before and after an attack is identified. FAIR-CAM uses a **stage-gated methodology** for measuring these effects (see 04_Detection_Response_Measurement for full details).

### Key Concepts

**Stage-Gated Structure**: Attacks are decomposed into discrete stages aligned with the attack kill chain. Each stage represents a distinct detection opportunity with stage-specific parameters.

**Multi-Review Detection**: When stage duration exceeds monitoring cadence, multiple detection opportunities occur. The formula accounts for this accumulation:

```
λ = τ / (M / Rel_M)                     (number of reviews)
P(Detect) = Cov × V_eff × [1 - (1 - R_eff)^(ρ × λ)]
```

Where:
- V_eff = V × Rel_V (effective visibility)
- R_eff = R × Rel_R (effective recognition)
- ρ = Review Independence Factor (0-1), calibrating assumptions about review independence

**Conditional Magnitude Distributions**: Rather than asserting a specific loss growth curve, outcomes are classified (Early, Mid, Late, Full Impact, Attacker Fails) with associated loss distributions:

```
E[Loss] = Σ P(outcome_class) × E[LM_class]
```

**Response Time with Concurrency**: Response activities often overlap:

```
T_response = T_containment + T_resilience - α × min(T_containment, T_resilience)
```

Where α (0-1) represents the degree of overlap.

### Detection SLO Alignment

To determine if monitoring meets a time threshold (e.g., "detect within 4 hours"):

```
P(Detect within T) ≈ Visibility × P(Monitored within T) × Recognition
```

This probability-based framing connects detection capabilities to operational SLOs.

## Common Pitfalls to Avoid

1. **Don't assume all controls = prevention**: Many are VM or DS
2. **Don't ignore variance**: It's always present and always matters
3. **Don't treat detection as independent**: It requires all three subfunctions
4. **Don't forget Coverage**: Population-level effectiveness differs from individual instances
5. **Don't assume independence**: VM and DS controls are often systemic (but calculate as independent if no data)
6. **Don't use ordinal scores directly**: They require translation with many assumptions

## Key Formulas Reference

**Reliability:**
```
Rel = (1 - VF/365)^VD
```

**Operational Efficacy (with Coverage):**
```
OpEff = Cov × [(Rel × IntEff) + ((1-Rel) × VarEff)]
```

**Combined Susceptibility (layered resistance):**
```
Combined_Susceptibility = (1 - OpEff₁) × (1 - OpEff₂) × ... × (1 - OpEffₙ)
```

**Multi-Review Detection:**
```
P(Detect_i) = Cov_i × V_eff,i × [1 - (1 - R_eff,i)^(ρ_i × λ_i)]
```

**Response Time with Concurrency:**
```
T = T_containment + T_resilience - α × min(T_containment, T_resilience)
```

Where α = concurrency factor (0 = sequential, 1 = parallel, 0-1 = partial overlap)
