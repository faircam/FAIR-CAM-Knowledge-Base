# FAIR Model Standard Reference

**Version 3.0 | January 2025 | FAIR Institute**

This document provides the authoritative reference for the Factor Analysis of Information Risk (FAIR) Model—the foundational framework for understanding, measuring, and communicating cyber risk in quantitative terms.

---

## Overview

The FAIR Model is a first-principles approach that decomposes risk into its fundamental components. It serves three purposes:

1. **Analytical Tool**: Enables quantitative risk measurement and management
2. **Framework**: Supports critical thinking about risk factors and relationships
3. **Vocabulary**: Provides clearly defined terms for risk communication

While FAIR typically measures risk in financial terms, it can also measure risk as mission failure probabilities, non-financial impacts, or even qualitative assessments.

---

## FAIR Model Taxonomy

```
                                    ┌─────────────┐
                                    │    RISK     │
                                    └──────┬──────┘
                          ┌─────────────────┴─────────────────┐
                          ▼                                   ▼
               ┌───────────────────┐               ┌───────────────────┐
               │ Loss Event        │               │ Loss Magnitude    │
               │ Frequency (LEF)   │               │ (LM)              │
               └─────────┬─────────┘               └─────────┬─────────┘
            ┌────────────┴────────────┐          ┌───────────┴───────────┐
            ▼                         ▼          ▼                       ▼
  ┌──────────────────┐    ┌──────────────────┐  ┌──────────────┐  ┌──────────────┐
  │ Threat Event     │    │ Susceptibility   │  │ Primary Loss │  │ Secondary    │
  │ Frequency (TEF)  │    │ (Susc)           │  │ (PL)         │  │ Loss (SL)    │
  └────────┬─────────┘    └────────┬─────────┘  └──────────────┘  └──────┬───────┘
    ┌──────┴──────┐         ┌──────┴──────┐                    ┌────────┴────────┐
    ▼             ▼         ▼             ▼                    ▼                 ▼
┌────────┐  ┌─────────┐ ┌────────┐  ┌──────────┐        ┌──────────┐      ┌──────────┐
│Contact │  │Prob. of │ │ Threat │  │Resistance│        │ SLEF     │      │ SLM      │
│Freq.   │  │Action   │ │Capabil.│  │Strength  │        │          │      │          │
│(CF)    │  │(PoA)    │ │(TCap)  │  │(RS)      │        │          │      │          │
└────────┘  └────┬────┘ └────────┘  └──────────┘        └──────────┘      └──────────┘
           ┌─────┴─────┐
           ▼     ▼     ▼
        ┌─────┬─────┬─────┐
        │Value│Effort│Risk │
        └─────┴─────┴─────┘
```

---

## Core Definitions

### Risk

> **"The probable frequency and probable magnitude of future loss"**

**Key Principles:**

| Principle | Implication |
|-----------|-------------|
| Forward-looking | Must be measured probabilistically |
| Uncertainty is inherent | Account for uncertainty in inputs AND outputs |
| Scenario-specific | Must define loss event scenario clearly |

**Derived from:**
- Loss Event Frequency (LEF)
- Loss Magnitude (LM)

**Expression Options:**
- Annualized expected loss (e.g., "$2.5M ALE")
- Exceedance probability (e.g., "50% probability of loss >$100K in 12 months")
- Distribution (e.g., "90th percentile loss of $5M")

**Scenario Definition Example:**
> "The risk associated with compromise of PII by cybercriminals via an email phishing attack"

---

## Frequency Side of the Model

### Loss Event Frequency (LEF)

> **"The probable frequency, within a given timeframe, that a threat agent's actions will inflict harm upon an asset"**

**Key Points:**
- Specific type of harm must be defined in scenario scope (data theft, outage, loss of integrity, destruction, etc.)
- Can be expressed as frequency OR probability within timeframe
- Example equivalence: "LEF of 1 in 20 years" = "5% probability in next 12 months"

**Derived from:**
- Threat Event Frequency (TEF)
- Susceptibility (Susc)

**Relationship:** LEF = TEF × Susceptibility

---

### Threat Event Frequency (TEF)

> **"The probable frequency within a given timeframe that a threat agent will act in a manner that may result in loss"**

**Threat Agent Types:**

| Type | Action Origin | Example |
|------|---------------|---------|
| Human | Intentional (malicious) | External attacker |
| Human | Intentional (non-malicious) | Authorized user error |
| Human | Accidental | Fat-finger mistake |
| Technological | Failure/malfunction | Hardware failure, software bug |
| Natural | Natural processes | Earthquake, flood, pandemic |
| Animal | Various | Rodent damage to cables |

**Derived from:**
- Contact Frequency (CF)
- Probability of Action (PoA)

**Relationship:** TEF = CF × PoA

---

### Contact Frequency (CF)

> **"The frequency within a given timeframe in which a threat agent will come into contact with an asset"**

**"Contact" means:** Threat agent is in a position to act against the asset (physically or logically), given their capabilities and resources.

**Example:** A cybercriminal is in "contact" with a web application if they:
1. Are aware of the web application's existence, AND
2. Have unimpeded network access to it

---

### Probability of Action (PoA)

> **"The probability that a threat agent will act against an asset once contact has occurred"**

**Varies by threat agent type:**

| Threat Type | PoA Behavior |
|-------------|--------------|
| Natural (Mother Nature) | Nearly certain (~100%) |
| Cognitive/Rational (Human, Animal) | Uncertain, depends on factors below |

**For rational threat actors, PoA is derived from:**

| Factor | Description |
|--------|-------------|
| Perceived Value | What the threat agent expects to gain |
| Level of Effort | How much work the threat agent perceives is required |
| Risk to Threat Agent | Perceived consequences if caught/failed |

**Note:** Not all cognitive threat agents are rational.

---

### Susceptibility (Susc)

> **"The probability that a threat event will become a loss event"**

When a threat event occurs, susceptibility determines the probability that actual harm results. The outcome depends on whether the threat agent's capabilities exceed the resistive controls in place.

**Derived from:**
- Threat Capability (TCap)
- Resistance Strength (RS)

**Relationship:** Susceptibility emerges from the comparison of TCap vs. RS  

---

### Threat Capability (TCap)

> **A measurement of a threat agent's ability to defeat resistive controls**

**Characteristics:**
- Abstract measurement (not empirical)
- Accounts for skills AND resources threat agent can apply
- Expressed as percentile relative to overall threat community

**Scale:** 1st percentile to 100th percentile (entire threat population)

**Example:** A specific threat community might fall between 50th-75th percentile—meaning they can defeat controls that stop attackers below this range, but fail against controls designed to stop higher-capability threats.

---

### Resistance Strength (RS)

> **An abstract measurement representing the efficacy of resistive controls against the overall threat population**

**Interpretation:**
- A control with RS of 70%-90% is expected to:
  - Successfully repel attackers below this capability range
  - Fail against attackers above the 90th percentile (always successful)
  - Have variable success against attackers in the 70%-90% range

**Note:** This is analogous to TCap but from the defender's perspective.

---

## Magnitude Side of the Model

### Loss Magnitude (LM)

> **The probable magnitude of loss resulting from a loss event**

**Two primary forms:**

| Form | Nature | Certainty | Typical Magnitude |
|------|--------|-----------|-------------------|
| Primary Loss (PL) | Direct result of event | Higher | Often lower |
| Secondary Loss (SL) | Indirect (stakeholder reactions) | Lower (<100%) | Often higher |

---

### Primary Loss (PL)

> **Loss that occurs directly as a result of the loss event**

**Characteristics:**
- More certain to materialize when a loss event occurs
- Magnitude is typically lower than secondary losses
- Examples: Immediate productivity loss, direct response costs, immediate replacement costs

---

### Secondary Loss (SL)

> **Loss that occurs indirectly from a loss event, typically due to actions or reactions by secondary stakeholders**

**Secondary Stakeholders include:**
- Customers
- Investors
- Employees
- The community
- Business partners
- Regulators

**Key Distinction:** Probability of occurrence is less than 100% because they are indirect ("fallout" from the event).

**Derived from:**
- Secondary Loss Event Frequency (SLEF)
- Secondary Loss Magnitude (SLM)

---

### Secondary Loss Event Frequency (SLEF)

> **The percentage of loss events expected to have secondary effects (fallout)**

**Note:** Although labeled "frequency," it is measured as a percentage.

**Example:** SLEF of 80% means eighty percent of loss events are expected to experience secondary loss.

---

### Secondary Loss Magnitude (SLM)

> **How much loss is expected to materialize from secondary stakeholder reactions**

This is where the forms of loss (next section) are applied.

---

## Six Forms of Loss

FAIR categorizes loss into six forms that provide comprehensive coverage of potential impacts. Each form can occur as primary or secondary loss depending on scenario specifics.

| Form | Abbreviation | Description |
|------|--------------|-------------|
| **Productivity Loss** | ProdL | Reduction in organizational efficiency/effectiveness: reduced revenue from downtime, decreased employee performance, interrupted business processes |
| **Response Costs** | RespC | Expenses for addressing and mitigating the event: incident response, forensic investigations, stakeholder communication, legal consultations |
| **Replacement Costs** | ReplC | Expenditures to restore/replace lost or damaged assets: equipment, software, data recovery services |
| **Fines and Judgments** | FinJu | Financial penalties and legal settlements: regulatory fines, criminal penalties, civil litigation damages |
| **Reputation Damage** | RepuD | Long-term impact on brand and customer trust: decreased loyalty, reduced market share, lower revenue |
| **Competitive Advantage Loss** | CAdvL | Undermined market position or strategic initiatives: lost intellectual property, diminished innovation capacity, weakened market perception |

**Note:** FAIR-MAM (Materiality Assessment Model) provides increased granularity and clarity in loss magnitude analysis.

---

## Common Misperceptions

### Model Depth

**Misperception:** Every analysis must go to the deepest layer of the model.

**Reality:** It's acceptable to estimate at any appropriate layer. For example:
- Estimate at TEF rather than decomposing into CF and PoA
- Time constraints and data availability guide layer selection
- Deeper is not always better—match depth to decision needs

---

### Data Sources

**Misperception:** FAIR requires subject matter expert (SME) estimates.

**Reality:** FAIR stipulates no specific data sources. Valid inputs include:
- Security telemetry
- Industry data
- Historical incident data
- Automated data ingestion via cyber risk management platforms
- Third-party data services
- Yes, also SME estimates when appropriate

---

### Distributions

**Misperception:** FAIR mandates PERT (or Beta PERT) distributions.

**Reality:** No stipulations on statistical methods or distributions exist in the standard. Analysts should:
- Use methods that best fit their needs and constraints
- Select distributions appropriate to the data characteristics
- Be prepared to justify choices based on scenario specifics

---

## Key Relationships Summary

| Relationship | Formula/Concept |
|--------------|-----------------|
| Risk | = f(LEF, LM) |
| LEF | = TEF × Susceptibility |
| TEF | = CF × PoA |
| PoA (rational actors) | = f(Perceived Value, Level of Effort, Risk to Threat Agent) |
| Susceptibility | = P(TCap > RS) |
| LM | = PL + SL |
| SL | = SLEF × SLM |

---

## FAIR Ecosystem

FAIR is the foundational risk model. Extensions include:

| Model | Purpose |
|-------|---------|
| **FAIR-CAM** | Controls Analytics Model—understanding how controls reduce risk |
| **FAIR-MAM** | Materiality Assessment Model—detailed loss magnitude analysis |

---

## Source Attribution

FAIR created by Jack Jones. FAIR Model Standard V3.0 ©2025 FAIR Institute. All Rights Reserved.

Official resources: https://www.fairinstitute.org

Reference text: *Measuring and Managing Information Risk: A FAIR Approach*
