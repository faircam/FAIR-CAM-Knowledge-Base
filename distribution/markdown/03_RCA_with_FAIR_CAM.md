# Root Cause Analysis (RCA) with FAIR-CAM

## Overview

RCA is a systematic approach to identifying underlying causes of problems or non-compliance. Rather than treating symptoms, RCA uncovers true drivers, enabling effective and lasting corrective actions.

## The Five RCA Dimensions

### 1. Governance
**Question:** Does a formal expectation exist?

### 2. Awareness
**Question:** Was the responsible party aware of the expectation?

### 3. Capability
**Question:** Did they have the skills, resources, or tools to comply?

### 4. Prioritization
**Question:** Was the expectation deprioritized due to business needs?

### 5. Intent
**Question:** Was the non-compliance malicious, self-serving, or error-based?

## Quick Reference Table

| RCA Dimension | Typical Root Cause | FAIR-CAM Control Lens | Remedy Example |
|---------------|-------------------|----------------------|----------------|
| Governance | No formal expectation | DS → Define Expectations (Preventive Admin) | Create/update policy, set SLAs |
| Awareness | Unaware of expectation | DS → Communicate Expectations | Communications, onboarding refresh |
| Capability | Lacked skills/tools | DS → Ensure Capability (Performance) | Provide training, job aids, tools |
| Prioritization | Business overrides control | DS → Incentives, Situational Awareness | Reframe risk, escalate exposure |
| Intent | Malicious/self-serving/error | VM Identification + DS Incentives | Enhance monitoring, enforce consequences |

## Integrating FAIR-CAM into RCA

FAIR-CAM provides structured taxonomy of how controls function to reduce risk. Combined with RCA:

**If NO (Governance):** Create or adjust policy/process
- FAIR-CAM Link: Weak preventive administrative controls (DS → Define Expectations)

**If NO (Awareness):** Improve communications, onboarding, or attestation
- FAIR-CAM Link: Communication and training controls (DS → Communicate Expectations)

**If NO (Capability):** Provide training, aids, or better tools
- FAIR-CAM Link: Performance controls - human or technical (DS → Ensure Capability)

**If YES (Prioritization):** Reframe risk in business terms, escalate exposure, or set SLAs
- FAIR-CAM Link: Misaligned decision-support controls (DS → Incentives, SA)

**If Intent:** Tailor detection, monitoring, enforcement, or simplification accordingly
- FAIR-CAM Link: Stronger detective and enforcement controls (VM Identification + DS Incentives)

## Integration Benefits

### Better targeting:
1. Pinpoint root causes
2. Map to specific control functions (preventive, detective, corrective)
3. Improve clarity and prioritization in remediation

### Systemic improvement:
- Governance gaps → DS preventive controls
- Awareness gaps → DS communication/training
- Capability gaps → DS capability + potentially VM/LE controls
- Prioritization gaps → DS decision-support (SA, incentives)
- Intent gaps → VM Identification + DS enforcement

## Applying RCA + FAIR-CAM in Practice

### 4-Step Process

1. Tag each finding or incident with its RCA root cause category
2. Map the root cause to the appropriate FAIR-CAM control function
3. Select a remedy aligned to both RCA dimension and control function
4. Track outcomes: recurrence reduction, risk exposure change, control effectiveness

### Tracking and Governance

**Metrics to monitor:**
- Recurrence rate by RCA category
- Time to remediation by category
- Distribution of root causes (which dimension appears most?)
- Risk exposure change after remediation

**Review forums:**
- Governance councils
- Incident review boards
- Regular control effectiveness reviews

## Example RCA Analyses

### Example 1: Orphaned Access Privileges

**Finding:** User who changed roles 6 months ago still has admin access to production database

**RCA Analysis:**
1. Governance: Policy exists ✓ (access must be updated within 24 hours of role change)
2. Awareness: Manager was aware ✓ (has been trained, policy communicated)
3. Capability: Manager has process and tools ✓ (can submit access change request easily)
4. Prioritization: ✗ Manager deprioritized this task (too busy with project deadline)
5. Intent: Not malicious, simply deprioritized

**Root Cause:** Prioritization

**FAIR-CAM Mapping:** DS → Misaligned Decision Prevention → Incentives (weak) + Situational Awareness (manager doesn't understand risk)

**Remedy:**
- Implement escalating notifications (first to manager, then to VP, then to CEO if not resolved)
- Provide dashboard showing manager's outstanding access management tasks
- Include access management compliance in performance reviews
- Quantify and communicate risk exposure from stale access

**FAIR-CAM Impact:** Strengthens DS controls (incentives), which improves reliability of VM control (access management process), which improves OpEff of LE control (access privileges)

### Example 2: Unpatched Critical Vulnerability

**Finding:** Internet-facing web server has critical vulnerability unpatched for 45 days, SLA is 7 days

**RCA Analysis:**
1. Governance: Policy exists ✓ (7-day SLA for critical internet-facing systems)
2. Awareness: Team was aware ✓ (vulnerability scanner flagged it, appeared in reports)
3. Capability: ✗ Team lacks automated patching for this custom application; manual process is complex
4. Prioritization: N/A (would have done it if capable)
5. Intent: Not malicious

**Root Cause:** Capability

**FAIR-CAM Mapping:**
- Primary: DS → Ensure Capability (lack of tools/processes)
- Secondary: VM → Correction → Implementation (the patching process itself)

**Remedy:**
- Develop/acquire tooling to simplify patching for custom apps
- Create step-by-step playbooks for manual patching
- Provide additional training on patching procedures
- Consider staffing augmentation or MSP support

**FAIR-CAM Impact:** Strengthens DS control (capability), which reduces Variance Duration of VM Correction, which improves Reliability of LE control (software), which reduces Susceptibility

### Example 3: Repeated Policy Violations

**Finding:** Same manager has 4 audit findings in 12 months for policy violations (various types)

**RCA Analysis:**
1. Governance: Policies exist ✓
2. Awareness: Manager claims unaware of some policies ✗
3. Capability: Manager has capability ✓
4. Prioritization: Possible issue (manager focuses on revenue, not compliance)
5. Intent: Not malicious, but pattern suggests systematic deprioritization

**Root Cause:** Mixed - Awareness + Prioritization

**FAIR-CAM Mapping:**
- DS → Communicate Expectations (awareness issue)
- DS → Incentives (prioritization issue)

**Remedy:**
- Mandatory policy attestation with testing (ensure real awareness)
- Add compliance metrics to manager's performance goals
- Executive escalation for pattern of violations
- One-on-one with manager to understand barriers and reset expectations

**FAIR-CAM Impact:** Strengthens multiple DS controls, which affects Operational Efficacy of many LE and VM controls under that manager's responsibility

### Example 4: Detection Failure

**Finding:** Ransomware attack progressed to Stage 4 (Lateral Movement) before detection despite deployed controls

**RCA Analysis:**
1. Governance: Detection SLOs exist ✓ (detect initial access within 4 hours)
2. Awareness: SOC team aware ✓
3. Capability: ✗ Recognition rules for initial access TTPs were outdated
4. Prioritization: N/A
5. Intent: N/A

**Root Cause:** Capability (technical)

**FAIR-CAM Mapping:**
- DS → Ensure Capability (SOC needs updated rule sets)
- VM → Correction → Implementation (rule update process too slow)
- LE → Detection → Recognition (the failing function)

**Stage-Gated Analysis:**
Using the stage-gated detection methodology reveals:
- Stage 1 P(Detect) was only 28% instead of expected 55%
- Recognition (R) was 30% vs. intended 65% due to stale signatures
- This single failure allowed 72% of attacks to progress to Stage 2+

**Remedy:**
- Automated threat intel integration for detection rules
- Weekly rule effectiveness review
- Detection rule variance monitoring (VM → Identification)

**FAIR-CAM Impact:** Strengthens DS control (capability), which improves Reliability of LE Detection → Recognition, which improves stage-gated detection probability

## Common Patterns and Systemic Issues

### When You See Chronic "Awareness" Issues:

**Likely Systemic Problem:**
- DS → Communicate Expectations is weak across organization
- Policies not effectively communicated
- Training programs superficial or not targeted
- No attestation/testing to verify understanding

**Systemic Remedy:**
- Overhaul communication strategy
- Implement policy acknowledgment with testing
- Create role-specific training programs
- Regular "lunch and learn" sessions

### When You See Chronic "Capability" Issues:

**Likely Systemic Problem:**
- DS → Ensure Capability is weak
- Insufficient training budget/time
- Tools inadequate for tasks
- Staffing levels insufficient

**Systemic Remedy:**
- Increase training budget and mandate time for training
- Tool assessment and procurement
- Staffing analysis and augmentation
- Create better job aids and documentation

### When You See Chronic "Prioritization" Issues:

**Likely Systemic Problem:**
- DS → Incentives misaligned across organization
- DS → Situational Awareness poor (decision-makers don't understand risk)
- Competing priorities not resolved at leadership level
- Risk culture weak

**Systemic Remedy:**
- Executive-level risk appetite and prioritization framework
- Better risk quantification and communication to business leaders
- Align incentives (performance goals, compensation) with risk management
- Escalation procedures for priority conflicts

### When You See Chronic "Governance" Issues:

**Likely Systemic Problem:**
- DS → Define Expectations systematically weak
- Policy development process broken
- Lack of ownership for policy areas
- Policies not maintained/updated

**Systemic Remedy:**
- Establish policy governance framework
- Assign clear ownership for policy domains
- Regular policy review and update cycles
- Make policy development responsive to emerging risks

## RCA Distribution Analysis

Track the distribution of root causes over time to identify trends:

### Healthy Distribution Example:
- Governance: 10%
- Awareness: 15%
- Capability: 20%
- Prioritization: 40%
- Intent: 15%

### Unhealthy Pattern Example:
- Governance: 50% ← Major red flag: basic expectations not set
- Awareness: 30% ← Red flag: communication is broken
- Capability: 10%
- Prioritization: 5%
- Intent: 5%

## Integration with FAIR Risk Analysis

RCA + FAIR-CAM enables you to:

### 1. Quantify impact of root causes:
- Estimate Variance Frequency increase from each RCA category
- Estimate Variance Duration increase from each RCA category
- Calculate impact on Reliability and OpEff
- Calculate impact on LEF and Loss Magnitude

### 2. Prioritize remediation by risk reduction:
- Calculate current risk with observed VF/VD
- Calculate residual risk if root cause addressed
- Prioritize highest risk reduction opportunities

### 3. Cost-benefit remediation decisions:
- Compare cost of systemic remedies (fix DS/VM controls)
- Compare to cost of repeated firefighting (fixing LE controls repeatedly)
- Calculate ROI of addressing root causes

## Key Takeaways

1. **Symptoms vs. causes**: "Policy noncompliance" is a symptom, not a root cause. Use the 5 RCA dimensions to find the real cause.

2. **FAIR-CAM makes it actionable**: Mapping to FAIR-CAM functions tells you what type of control to strengthen.

3. **Track patterns**: Chronic issues in one RCA dimension = systemic problem needing systemic remedy.

4. **Prioritization is common**: Most issues aren't malicious; they're competing priorities. Use DS controls (especially Incentives and SA) to address.

5. **Intent is rare**: True malicious intent is uncommon. When it appears repeatedly, likely a screening/hiring issue.

6. **Governance first**: If policies don't exist, nothing else matters. Start there.

7. **Awareness requires proof**: Don't assume training = awareness. Test and verify.

8. **Measure outcomes**: Track whether remedies actually reduce recurrence and improve control reliability.

9. **Detection failures need stage-gated analysis**: When detection fails, use the stage-gated methodology to identify which stage and which function (V, M, or R) failed.
