# Framework Upgrade Report: v2.0 → v2.1

**Role:** Lead Systems Architect
**Date:** 2026-01-29
**Version:** 2.1 (Executive Verdict Added, CDI Logic Fixed)

---

## Summary of Changes

Three critical upgrades were implemented to improve human readability and fix a metric calculation bug.

---

## Change 1: Executive Verdict Layer (New Section 0)

### Problem Solved
The v2.0 output dove directly into technical data (metadata, admissibility logs, evidence lockers). Human readers had to scroll through dense forensic output before understanding "should I trust this article?"

### Solution Implemented
Added **Section 0: Executive Verdict (The Bottom Line)** as the first section of the output schema. This is marked MANDATORY and must appear at the very top of every audit output.

### Executive Verdict Components

| Component | Purpose | Derivation Logic |
|-----------|---------|------------------|
| **Trust Rating** | Visual trust indicator | Strict order: (1) CDI > 50% → 🔴 LOW, (2) ESR < 50% → 🔴 LOW, (3) ESR > 75% AND CDI < 20% → 🟢 HIGH, (4) Default → 🟡 MEDIUM |
| **"So What?" Summary** | One-sentence claim vs. proof comparison | Compares the article's specific claims to what Class A evidence actually proves |
| **"Real News" (Top 3)** | Verified facts only | Lists up to 3 Class A verified facts; "None identified" if fewer exist |
| **Recommendation** | Actionable guidance | Read for context / Read for facts / Skip - Pure Narrative |

### Trust Rating Logic Explained

The Trust Rating uses a **strict evaluation order** to ensure critical failures are caught first:

1. **CRITICAL FAIL (CDI > 50%)** → 🔴 LOW
   - If the journalist makes more unverified assertions than verified facts, the article automatically fails regardless of ESR.
   - Rationale: An article can't be trusted if most factual claims lack verification paths.

2. **LOGIC FAIL (ESR < 50%)** → 🔴 LOW
   - If the narrative pitch isn't supported by evidence, the article fails.
   - Rationale: The core thesis must have evidentiary backing.

3. **HIGH TRUST (ESR > 75% AND CDI < 20%)** → 🟢 HIGH
   - Only awarded when both metrics are strong.
   - Rationale: Claims are well-supported AND journalist provides verification paths.

4. **DEFAULT** → 🟡 MEDIUM
   - All other combinations indicate mixed quality.
   - Rationale: Article passes minimum thresholds but doesn't excel.

---

## Change 2: CDI Formula Fix

### Bug Identified
The v2.0 CDI formula was ambiguous about whether B1/B2/B3 (speech acts) should be included. Some LLM executions were incorrectly including quotes in the denominator, which skewed the reliability score.

### Problem Example
- Article contains: 2 Class A facts, 3 Class B4 assertions, 10 Class B1 quotes
- **Incorrect calculation (with B1):** 3 / (2 + 3 + 10) = 20% CDI
- **Correct calculation (A + B4 only):** 3 / (2 + 3) = 60% CDI

The bug made articles appear more reliable than they actually were.

### Solution Implemented
Updated CDI formula with explicit constraint:

```
Formula: (Count of Class B4 Items) / (Count of Class A Items + Count of Class B4 Items)

Constraint: Do NOT include Class B1, B2, or B3 (Quotes/Speech Acts) in this calculation.
This metric measures the reliability of the *journalist's* assertions of fact,
not the sources' opinions.

Edge Case: If (Class A + Class B4) = 0, set CDI to "N/A - No Factual Claims".
Do not divide by zero.
```

### Rationale
- **Class B1/B2/B3** = Someone else's words (quotes). We're not assessing whether the quoted person is reliable—we're assessing whether the journalist citing them did so accurately.
- **Class B4** = Journalist's own factual assertions without citation.
- **Class A** = Journalist's factual assertions WITH citation.

CDI answers: "Of the facts this journalist asserts, what percentage lack citations?"

---

## Change 3: Gate Status Clarification

### Problem Solved
The v2.0 Gate Status output was confusing:
- `Gate Status: CLEARED` - Why? What does this mean?
- Users didn't understand that high Class C is *expected* and *good*.

### Solution Implemented
Simplified Gate Status options with clear explanations:

**Phase 2 Gate:**
```
Gate Status: [CLEARED (Framing Detected) / FAILED - Under-filtered]
Note: "CLEARED" means the audit successfully identified Class C framing.
If Class C is low (<20%), the gate FAILS because the auditor likely missed
the rhetorical layer.
```

**Phase 3 Gate:**
```
Gate Status: [CLEARED (Logic Verified) / FAILED - Axiom Violation]
```

The explanatory text clarifies that the gate verifies the *auditor's* rigor, not the article quality.

---

## Verification

All changes have been applied to `narrative-audit-framework.md`:
- Version number updated to "2.1 (Executive Verdict Added, CDI Logic Fixed)"
- Section 0: Executive Verdict inserted before Section 1 in Output Schema
- CDI formula updated with explicit B1/B2/B3 exclusion and division-by-zero edge case
- Gate Status simplified with clarifying notes

---

---

## Change 4: Zero-Knowledge Constraint (Post-Testing Addition)

### Problem Identified
During testing, an LLM agent classified factual claims as Class A because it "knew" from training data that the facts were true—even though the article provided no citations. This defeats the purpose of the audit: we're testing the *article's* evidentiary standards, not whether reality exists.

### Example of the Bug
- Article states: "The shooting occurred at 3pm" (no source citation)
- LLM classifies as Class A because it knows from training data this is true
- **This is wrong.** The article provides no verification path. It should be Class B4.

### Solution Implemented
Added **Zero-Knowledge Constraint** in four locations:

1. **Axiom 1 (line ~143)** - Core principle statement
2. **Verification Priority Protocol (line ~294)** - Class A definition
3. **Common Execution Errors table (line ~53)** - Error pattern entry
4. **Critical Safeguards (Phase 4, item 2)** - Detailed enforcement rules
5. **Self-Check list (line ~56)** - First item in pre-submission checklist

### The Constraint
```
⛔ ZERO-KNOWLEDGE CONSTRAINT: The LLM auditor must NOT use its own training data
to verify facts. If the article does not provide a link, citation, or document
reference, the claim is Class B4—even if the LLM "knows" the fact is true.
The audit evaluates the *article's* evidentiary standards, not external reality.
```

### Test
"Does the article text contain a clickable link, document name, or citation I could follow to verify this?" If no → Class B4.

---

## Recommendations for v2.2

1. **Add worked example for Executive Verdict** - Show sample Trust Rating calculation with real numbers
2. **Visual output option** - Consider a compact visual summary format for quick scanning
3. **Threshold tuning** - Test whether ESR/CDI thresholds need calibration after field use
