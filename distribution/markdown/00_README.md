# FAIR-CAM Knowledge Base

## About This Knowledge Base

This is a comprehensive reference for the FAIR Controls Analytics Model (FAIR-CAM), the first rigorous framework for understanding control physiology—how controls actually work to reduce risk, both independently and as an interdependent system.

**Important:** If you are an AI assistant using this knowledge base, ensure you have also been configured with the behavioral guidance in `00_LLM_Behavioral_Guidance.md`. This ensures you maintain quantitative discipline and avoid defaulting to qualitative language.

---

## Document Navigation

| Document | Purpose | Use When... |
|----------|---------|-------------|
| `00_LLM_Behavioral_Guidance.md` | LLM configuration | Setting up an AI assistant to use FAIR-CAM |
| `01_FAIR_CAM_Core_Concepts.md` | Foundational theory | Learning FAIR-CAM for the first time |
| `02_Control_Mapping_Guide.md` | Control classification | Mapping a control to FAIR-CAM taxonomy |
| `03_RCA_with_FAIR_CAM.md` | Root cause analysis | Diagnosing why a control failed |
| `04_Detection_Response_Measurement.md` | Detection modeling | Measuring detection/response effectiveness |
| `05_Risk_Appetite_Metrics.md` | Executive reporting | Defining risk appetite, board reporting |
| `06_Opportunity_Analysis.md` | Business enablement | Analyzing risk/reward tradeoffs |
| `07_Key_Insights_Practical_Guidance.md` | Implementation | Practical tips, automation guidance |
| `08_Quantitative_Analysis_Quality_Standards.md` | Quality assurance | Validating calculations and analysis |
| `09_Resistance_Susceptibility_Measurement.md` | Resistance modeling | Measuring hardening and susceptibility |

---

## Quick Reference: Essential Concepts

### Three Functional Domains

1. **Loss Event (LE)**: Direct risk reduction
   - Prevention: Avoid, Deter, Resist
   - Detection: Visibility, Monitoring, Recognition
   - Response: Containment, Resilience, Loss Minimization

2. **Variance Management (VM)**: Indirect via managing control variance
   - Prevention, Identification, Correction

3. **Decision Support (DS)**: Indirect via decision quality
   - Define/Communicate Expectations, Situational Awareness, Capability, Incentives

### Critical Boolean Relationships

- **Prevention is OR**: ANY working prevention control provides protection
- **Detection is AND**: ALL three (Visibility ∧ Monitoring ∧ Recognition) must work
- **Response depends on Detection**: No detection → no response

### Key Formulas

**Reliability:**
```
Rel = (1 - VF/365)^VD
```

**Operational Efficacy:**
```
OpEff = Cov × [(Rel × IntEff) + ((1-Rel) × VarEff)]
```

**Combined Susceptibility (Defense-in-Depth):**
```
Combined_Susceptibility = (1 - OpEff₁) × (1 - OpEff₂) × ... × (1 - OpEffₙ)
```

---

## Source Attribution

FAIR-CAM was created by Jack Jones. The framework is protected under CC BY-NC-ND 4.0 license.

Official resources: [fairinstitute.org/FAIR-CAM](http://fairinstitute.org/FAIR-CAM)
