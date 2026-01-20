# FAIR-CAM Knowledge Base - Distribution Package

## Quick Start

**Before using these materials with an AI assistant:**

1. Configure your LLM with the behavioral guidance in `markdown/00_LLM_Behavioral_Guidance.md`
   - Copy the "Behavioral Constraints" section into your system prompt, custom instructions, or GPT configuration
   - This ensures the assistant maintains quantitative discipline

2. Load the knowledge base documents (01-08) into your LLM's context

3. Optionally, include `markdown/00_README.md` as a navigation guide

Without the behavioral guidance, LLMs tend to slip into qualitative language ("high/medium/low" risk) and provide single-point estimates without uncertainty ranges—exactly what FAIR-CAM is designed to avoid.

---

## Contents

This package contains the FAIR-CAM Knowledge Base in markdown format, optimized for LLM consumption.

### File Inventory

```
markdown/
├── 00_LLM_Behavioral_Guidance.md    ← START HERE (for LLM configuration)
├── 00_README.md                      ← Navigation guide
├── 01_FAIR_CAM_Core_Concepts.md      ← Foundational concepts
├── 02_Control_Mapping_Guide.md       ← How to map controls to FAIR-CAM
├── 03_RCA_with_FAIR_CAM.md           ← Root cause analysis
├── 04_Detection_Response_Measurement.md  ← Stage-gated detection
├── 05_Risk_Appetite_Metrics.md       ← Board reporting & KRIs
├── 06_Opportunity_Analysis.md        ← Opportunity enablement
├── 07_Key_Insights_Practical_Guidance.md ← Implementation tips
└── 08_Quantitative_Analysis_Quality_Standards.md ← Calculation quality
```

---

## File Integrity Verification

To ensure these files haven't been modified, verify their SHA-256 checksums:

### On Linux/Mac:
```bash
sha256sum -c checksums.sha256
```

### On Windows (PowerShell):
```powershell
Get-Content checksums.sha256 | ForEach-Object {
    $parts = $_ -split "  "
    $expected = $parts[0]
    $file = $parts[1]
    $actual = (Get-FileHash $file -Algorithm SHA256).Hash.ToLower()
    if ($actual -eq $expected) {
        Write-Host "OK: $file"
    } else {
        Write-Host "FAILED: $file"
    }
}
```

---

## Platform-Specific Notes

### OpenAI Custom GPTs
1. In "Configure", paste the behavioral guidance into "Instructions"
2. Upload all markdown files to "Knowledge"

### Claude Projects
1. Add the behavioral guidance to "Project Instructions"
2. Upload markdown files to the project's knowledge base

### Other LLMs
1. Include behavioral guidance in your system prompt
2. Include knowledge base documents in context (or use RAG)

---

## Source Attribution

This knowledge base is distilled from materials created by Jack Jones 
(creator of FAIR and FAIR-CAM) and resources published by the FAIR Institute.

FAIR-CAM is protected under a Creative Commons Attribution-NonCommercial-
NoDerivatives 4.0 International Public License (CC BY-NC-ND 4.0).

Official resources: https://fairinstitute.org/FAIR-CAM

---

## Package Generated

Date: 2026-01-20 15:27 UTC
Files: 11 markdown files
