# Resistance and Susceptibility Measurement

## Purpose

This document addresses a commonly overlooked aspect of FAIR-CAM analysis: **software and system hardening as Loss Event Controls (LECs)** that provide resistance against attacks. Practitioners frequently map all hardening-related activities to Variance Management, missing the direct risk reduction provided by the hardened state itself.

## Foundational Principle: Software IS a Resistive Control

From FAIR-CAM source material:

> "An important example is software, which is a resistive control against code exploitation attacks. Commonly, things associated with software (e.g., patching, scanning, developer training) are thought of as controls, but not the software itself. As we'll see later, it's extremely useful to recognize that software is itself a control."

This distinction is critical:

| Element | FAIR-CAM Domain | Function | Description |
|---------|-----------------|----------|-------------|
| **Software** (the artifact) | LE → Prevention → Resistance | Directly reduces P(successful exploit) | The code itself resists exploitation |
| **Patching** (the process) | VM → Correction → Implementation | Corrects variance in software | Restores software to intended efficacy |
| **Vulnerability scanning** | VM → Identification → Controls Monitoring | Identifies variant conditions | Detects when software has become vulnerable |

This parallels the relationship between personnel and training:

| Element | Domain | Analogy |
|---------|--------|---------|
| Personnel | LE → Prevention → Resistance | The person resisting phishing |
| Security awareness training | VM → Correction | Restores personnel to intended efficacy |
| Phishing tests | VM → Identification | Identifies variant personnel |

## Extrinsic vs. Intrinsic Variance in Software

FAIR-CAM distinguishes two sources of variance that affect software's efficacy as a resistive control:

### Intrinsic Variance

Occurs when actions (or inaction) **within** the organization cause the software to become variant:

- Coding errors introduced during development
- Misconfigurations during deployment
- Failed patches or incomplete updates
- Disabled security features for "compatibility"

**Correction mechanism**: Internal VM controls (code review, testing, configuration management, patching)

### Extrinsic Variance

Occurs when conditions **outside** the organization reduce the software's efficacy:

> "Extrinsic variance occurs when conditions outside an organization cause a control no longer to be as effective as desired. An obvious example is when zero-day exploits are created within the threat landscape. In that case the software control itself hasn't changed, but the threat landscape has, which means the software is no longer operating as a control at its intended level of efficacy."

**Correction mechanism**: 
- VM → Identification → Threat Capability Monitoring (threat intelligence)
- VM → Correction → Implementation (applying vendor patches)

### Variance Frequency for Software

| Variance Type | Typical VF Sources | Example Data Sources |
|---------------|-------------------|---------------------|
| Extrinsic (zero-days) | CVE publication rates for the technology | NVD, vendor advisories |
| Intrinsic (misconfig) | Change frequency × error rate | Change management tickets, audit findings |
| Intrinsic (drift) | Time since baseline × drift rate | Configuration scanning tools |

## Categories of Software Resistance

Software provides resistance through multiple mechanisms. Each can be measured independently.

### 1. Vulnerability Absence (Patched State)

The most basic form of software resistance: the absence of known exploitable vulnerabilities.

**Measurement approach**:
- **Binary view**: For a specific CVE, either the system is patched (resistant) or not (susceptible)
- **Aggregate view**: Percentage of known vulnerabilities that have been remediated

**Intended Efficacy**: 100% (when fully patched against known vulnerabilities)

**Variant Efficacy**: Function of which vulnerabilities remain unpatched and their exploitability

**Variance Parameters**:

| Parameter | Definition | Data Source |
|-----------|------------|-------------|
| VF (extrinsic) | Rate of new CVEs for this software | Historical CVE data |
| VF (intrinsic) | Rate of patch failures or regressions | Patch management system |
| VD | Time to patch (MTTP) | Vulnerability management metrics |

**Reliability Formula Application**:
```
Rel_patch = (1 - (VF_extrinsic + VF_intrinsic)/365)^VD
```

**Example**: Windows Server with:
- VF_extrinsic: 12 critical CVEs/year
- VF_intrinsic: 2 patch failures/year
- VD: 14 days average time-to-patch

```
Rel = (1 - (14/365))^14 = 0.53 (53% reliability)
```

This means the software is in its intended (fully patched) state only 53% of the time.

### 2. Exploit Mitigations (Architectural Hardening)

Modern operating systems and applications include exploit mitigations that make successful exploitation probabilistic rather than deterministic, even when vulnerabilities exist.

| Mitigation | Mechanism | Effect on Attacker |
|------------|-----------|-------------------|
| ASLR (Address Space Layout Randomization) | Randomizes memory addresses | Requires information leak to locate targets |
| DEP/NX (Data Execution Prevention) | Marks data regions non-executable | Prevents simple shellcode injection |
| CFG/CFI (Control Flow Guard/Integrity) | Validates indirect calls | Constrains ROP/JOP gadget chains |
| CET (Control-flow Enforcement Technology) | Hardware shadow stack | Prevents return address overwrites |
| Stack Canaries | Detects stack buffer overflows | Causes crash before exploitation |
| Sandboxing | Process isolation | Limits blast radius of successful exploit |

**Key insight**: These mitigations provide resistance **independent of patch state**. A system with an unpatched vulnerability but full mitigations enabled may still resist exploitation.

**Measurement approach**:

Exploit mitigations are typically **binary controls** at the configuration level:
- Intended Efficacy: The probability that an exploit attempt fails due to the mitigation
- Variant Efficacy: 0% (mitigation disabled provides no protection)

**Challenge**: Intended efficacy varies by:
- Attacker capability tier (commodity vs. nation-state)
- Vulnerability class (memory corruption vs. logic flaw)
- Mitigation bypass availability

**Practical estimation**:

| Mitigation Stack | P(Bypass) - Commodity | P(Bypass) - Targeted | P(Bypass) - Nation-State |
|------------------|----------------------|---------------------|-------------------------|
| None | 1.00 | 1.00 | 1.00 |
| ASLR only | 0.60 | 0.40 | 0.20 |
| ASLR + DEP | 0.35 | 0.20 | 0.10 |
| ASLR + DEP + CFG | 0.15 | 0.10 | 0.05 |
| Full stack + VBS | 0.05 | 0.03 | 0.02 |

These values represent the probability that an attacker can bypass the mitigations given they have a working exploit for a vulnerability.

**Variance considerations**:

Exploit mitigations have very different variance profiles than patch state:
- VF is typically very low (mitigations are usually static once deployed)
- VD can be very long if misconfiguration goes undetected
- Coverage is the critical variable (are mitigations enabled everywhere?)

### 3. Configuration Hardening (Attack Surface Reduction)

Reducing the attack surface by disabling unnecessary functionality.

| Hardening Category | Examples | Resistance Provided |
|-------------------|----------|---------------------|
| Service minimization | Disable unused services, protocols | Eliminates attack vectors entirely |
| Feature restrictions | Disable macros, scripting, USB | Prevents common initial access techniques |
| Network hardening | Host firewall, protocol restrictions | Reduces reachability of vulnerabilities |
| Privilege reduction | Remove local admin, restrict sudo | Limits post-exploitation capability |

**Measurement**: Configuration compliance percentage against a hardening baseline (CIS Benchmarks, STIG, etc.)

**Mapping to FAIR-CAM**:
- The hardened configuration STATE → LE → Prevention → Resistance
- The hardening PROCESS (applying baselines) → VM → Correction → Implementation
- Configuration SCANNING → VM → Identification → Controls Monitoring

### 4. Application Sandboxing

Sandboxes constrain what a compromised process can do, providing resistance at the boundary.

| Sandbox Type | Example | Escape Difficulty |
|--------------|---------|-------------------|
| None | Legacy applications | N/A (no containment) |
| Basic (AppContainer) | Modern Windows apps | Moderate |
| Strong (Browser sandbox) | Chrome, Edge | High |
| Hardware-backed (VM) | Windows Sandbox, gVisor | Very High |

**Measurement**: P(sandbox escape | initial exploit success)

This is multiplicative with vulnerability exploitation:
```
P(full compromise) = P(exploit success) × P(sandbox escape)
```

## Combining Resistance with Detection: The Susceptibility Model

FAIR defines Vulnerability as the probability that a threat event becomes a loss event. In FAIR-CAM terms:

```
Vulnerability = Susceptibility × (1 - Detection_Response_Capability)
```

Where:
- **Susceptibility** = P(threat event succeeds against resistive controls)
- **Detection_Response_Capability** = P(detect AND respond before loss materializes)

### Why This Decomposition Matters

Resistive controls and detective controls have fundamentally different characteristics:

| Characteristic | Resistive Controls | Detective Controls |
|----------------|-------------------|-------------------|
| Mechanism | Technical barrier | Observation + action |
| Dependency | None (works autonomously) | Requires all three: Visibility ∧ Monitoring ∧ Recognition |
| Availability | 24/7/365 (when enabled) | Subject to staffing, alert fatigue |
| Failure mode | Binary (works or doesn't) | Graduated (faster/slower detection) |

**The "3 AM Test"**: Does this control work at 3 AM on a holiday weekend when the SOC is understaffed?
- Resistive controls: **Yes** (ASLR doesn't sleep)
- Detective controls: **Depends** (who's watching the alerts?)

### Combined Susceptibility for Defense-in-Depth

When multiple resistive controls are layered (intrafunctional DiD), susceptibility combines multiplicatively:

```
Combined_Susceptibility = ∏(1 - OpEff_i) for all resistive controls i
```

**Example**: Endpoint with multiple resistance layers

| Control | OpEff | Susceptibility (1 - OpEff) |
|---------|-------|---------------------------|
| Patched OS | 0.53 | 0.47 |
| Exploit mitigations | 0.85 | 0.15 |
| Application sandbox | 0.70 | 0.30 |
| Application allowlisting | 0.60 | 0.40 |

```
Combined_Susceptibility = 0.47 × 0.15 × 0.30 × 0.40 = 0.0085 (0.85%)
```

Compare to relying on patch state alone: 47% susceptibility.

This demonstrates why hardening investments often have better ROI than trying to achieve perfect patch currency.

## Operational Efficacy for Resistance Controls

### Formula Application

The standard OpEff formula applies:

```
OpEff = Cov × [(Rel × IntEff) + ((1 - Rel) × VarEff)]
```

For **binary** resistive controls (e.g., ASLR enabled/disabled):
- IntEff = control's effectiveness when enabled (e.g., 0.85)
- VarEff = 0 (disabled provides no protection)
- OpEff simplifies to: `Cov × Rel × IntEff`

For **non-binary** resistive controls (e.g., patch currency as a continuous variable):
- IntEff = effectiveness when fully patched
- VarEff = effectiveness with typical patch debt
- Full formula applies

### Coverage Considerations

Coverage is often the dominant factor for endpoint resistance:

| Scenario | Coverage Challenge |
|----------|-------------------|
| BYOD environments | Personal devices may lack enterprise hardening |
| Shadow IT | Unmanaged servers under desks |
| Legacy systems | Cannot support modern mitigations |
| Acquired companies | Different baselines, tools, processes |
| Cloud workloads | Different hardening mechanisms than on-prem |

**Measurement**: 
```
Coverage = (Systems with control enabled) / (Total systems in scope)
```

## Integrating Resistance into FAIR Analysis

### Stage-Gated Model Enhancement

When decomposing an attack into stages, resistance factors into the stage transition probabilities:

**Stage N: Exploit Execution**
```
P(Stage N success) = P(vulnerability exists) × P(mitigations bypassed) × P(sandbox escaped) × P(detection missed)
                            ↑                        ↑                        ↑                    ↑
                      Patch currency          Exploit mitigations        Sandboxing           Detective controls
                         (Resistance)            (Resistance)           (Resistance)          (Detection)
```

### Example: Ransomware Initial Access via Exploit

| Factor | Control | OpEff | P(Failure) |
|--------|---------|-------|------------|
| Vulnerability exists | Patch management | 0.53 | 0.47 |
| Mitigations bypassed | OS hardening | 0.85 | 0.15 |
| Payload executes | Application allowlist | 0.60 | 0.40 |
| Detection missed | EDR | 0.75 | 0.25 |

```
P(successful initial access) = 0.47 × 0.15 × 0.40 × 0.25 = 0.007 (0.7%)
```

Without resistance controls (only EDR):
```
P(successful initial access) = 1.0 × 1.0 × 1.0 × 0.25 = 0.25 (25%)
```

**Risk reduction from resistance**: 97% reduction in stage success probability

## Common Mistakes

### Mistake 1: Mapping All Hardening to VM

**Wrong**: "Patching is a VM control, therefore patch state is a VM concept"

**Correct**: 
- Patching (the process) → VM → Correction → Implementation
- Patched state (the result) → LE → Prevention → Resistance (via the software control)

The software is the LEC. Patching corrects variance in the software.

### Mistake 2: Treating Patch Currency as Binary

**Wrong**: Systems are either "patched" or "unpatched"

**Correct**: Susceptibility is a function of:
- Which vulnerabilities remain unpatched
- CVSS/EPSS scores of those vulnerabilities
- Availability of weaponized exploits
- Compensating controls (mitigations, network position)

### Mistake 3: Ignoring Resistance When Calculating Stage-Gate Probabilities

**Wrong**: Only considering detective controls when modeling attack progression

**Correct**: Each stage should account for:
1. Resistive controls that could cause the attack to fail
2. Detective controls that could identify the attack
3. The interaction between them (detection only matters if resistance fails)

### Mistake 4: Assuming Independence

**Caution**: Resistance controls may have correlated failures:
- Same VM controls manage multiple resistance mechanisms
- Same DS controls (policies, training) affect multiple areas
- Systemic issues affect everything

When in doubt, assume some correlation and adjust combined susceptibility upward.

## Measurement Data Sources

| Resistance Category | Measurement Source | Metric |
|--------------------|-------------------|--------|
| Patch currency | Vulnerability scanner, SCCM/Intune | % systems missing critical patches, MTTP |
| Exploit mitigations | Configuration scanner, Defender ATP | % systems with full mitigation stack |
| Configuration hardening | CIS-CAT, compliance scanner | % compliance with hardening baseline |
| Application control | Allowlist solution logs | % endpoints with allowlisting enforced |
| Sandboxing | Application inventory | % applications running sandboxed |

## Summary: Key Principles

1. **Software IS a resistive control** - Not just the things done to software (patching, scanning)

2. **Distinguish STATE from PROCESS**:
   - Hardened state → LE → Resistance (directly reduces risk)
   - Hardening process → VM → Correction (maintains the control)

3. **Resistance works when detection sleeps** - The "3 AM test"

4. **Layer resistance multiplicatively** - Defense-in-depth compounds protection

5. **Measure resistance separately from detection** - Different failure modes, different investments

6. **Coverage often dominates** - 100% OpEff on 50% of systems = 50% protection

## References

- Jones, J. & Freund, J. (2026). *Measuring and Managing Information Risk*, 2nd Edition, Chapter 10: Controls
- FAIR-CAM White Paper (2021), FAIR Institute
- CIS Controls v8, Center for Internet Security
- MITRE ATT&CK, Mitigations

---

*FAIR-CAM created by Jack Jones. This document supplements the core FAIR-CAM methodology.*
