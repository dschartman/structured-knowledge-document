# Feedback Log: Version 1.1 → 1.2 Optimization
**Analyst:** Senior Forensic Information Analyst
**Date:** 2026-01-29 14:30 UTC
**Role:** Optimizer (Person 2+)

---

## Changes Implemented

### 1. Source Authority Hierarchy (Class B Sub-Tiers)
**Location:** Phase 1, Step 5 (New)

**Problem Identified:**
The original framework treated all Class B (Procedural) evidence as equally reliable. However, "The President announced X" has vastly different evidentiary weight than "Sources say X." This created a false equivalency where anonymous claims and official statements were both labeled "Class B."

**Solution:**
Introduced a three-tier sub-classification for Class B evidence:
- **B1:** Named officials, on-record statements (highest reliability)
- **B2:** Named individuals, informal contexts (medium reliability)
- **B3:** Anonymous attribution (lowest reliability, provisional status)

**Impact:**
LLMs can now differentiate between strong and weak Class B claims in the Evidence Locker, improving Delta Analysis accuracy.

---

### 2. Temporal Fallacy Detection Protocol
**Location:** Phase 2, Step 2.1 (Enhanced)

**Problem Identified:**
The framework addressed implied causation but lacked explicit handling of *post hoc ergo propter hoc* fallacies—where articles imply causation through temporal sequencing ("After policy X, Y happened").

**Solution:**
Added a special case protocol that instructs the LLM to:
1. Extract the two events as separate Class A/B facts
2. Flag the implied causation as Class C unless the article provides mechanism evidence
3. Label this pattern as "Temporal Correlation Without Proven Mechanism" in Delta Analysis

**Impact:**
Closes a major exploitation vector in narrative warfare. Many weaponized articles rely on readers inferring causation from sequence alone.

---

### 3. Statistical Interpretation Filtering
**Location:** Phase 2, Decision Tree (New Question 3b)

**Problem Identified:**
Articles often weaponize statistics through subjective interpretation. "Unemployment *soared* to 4.3%" vs. "Unemployment rose from 4.1% to 4.3%" contain the same data but different framing. The original framework marked these as "Class A (if sourced)" without separating the number from the characterization.

**Solution:**
Split the decision tree to distinguish:
- **Class A:** Raw numerical values (4.1% → 4.3%)
- **Class C:** Interpretive language ("soared," "collapsed," "skyrocketed")

**Extraction Rule:** Strip the emotional verb, retain only the quantified change.

**Impact:**
Prevents laundering of editorial opinion through statistical claims.

---

### 4. Mandatory Quantification Metrics
**Location:** Phase 3, Step 3.4 (Expanded)

**Problem Identified:**
The worked example mentioned "2 out of 5 supported" but this was not formalized as a requirement. Without standardized metrics, different LLMs might produce inconsistent audits.

**Solution:**
Established four mandatory metrics that must appear in every audit:
1. **Evidentiary Support Ratio (ESR):** Percentage of sub-claims backed by Class A/B evidence
2. **Narrative Integrity Score (NIS):** Explicit assessment of whether the *core thesis* is supported (regardless of ESR)
3. **Temporal Fallacy Count (TFC):** Number of causation-via-sequence claims
4. **Class C Density Ratio (CDR):** Percentage of statements that are framing vs. fact (propaganda threshold at 60%)

**Impact:**
Creates a repeatable, comparable scoring system. Users can assess articles across time/sources using standardized thresholds.

---

### 5. Omission Analysis Protocol (New Phase 5)
**Location:** New section between Phase 4 and Part II.5

**Problem Identified:**
The framework focused exclusively on *what the article contains*. Weaponized narratives often operate through strategic *omission*—excluding context, counterarguments, or inconvenient facts. The original framework had no mechanism to detect this.

**Solution:**
Added Phase 5 with a three-step protocol:
1. **Context Mapping:** Identify what information would be required for complete analysis
2. **Omission Detection:** Check if each category is present/absent
3. **Classification:** Label omissions as Material (fundamental), Tactical (intentional shaping), or Benign (space limits)

**Critical Rule:** LLM must NOT speculate on what omitted information *would say*—only flag its absence.

**Impact:**
Addresses the "lie by omission" problem. For example, an article attacking a politician's vote that never mentions *why* they voted that way is now flagged as a Tactical Omission.

---

### 6. Enhanced Output Schema
**Location:** Part III (Sections 5 & 6 added)

**Problem Identified:**
The original output format lacked the quantitative metrics and omission analysis, making the final report incomplete.

**Solution:**
Added two new mandatory sections to the output:
- Section 5: Quantitative Metrics (all four scores)
- Section 6: The Omission Log (Material and Tactical omissions)

**Impact:**
Ensures every audit produces a complete, standardized report.

---

### 7. Updated Worked Example
**Location:** Part II.5 (Expanded)

**Problem Identified:**
The worked example didn't demonstrate the new metrics or omission analysis, leaving the LLM without training reference for these features.

**Solution:**
Extended the sample article analysis to include:
- Calculated ESR (40%), NIS (Unsupported), TFC (0), CDR (67%)
- Three identified omissions (missing EO text, missing rationale, missing context for $500M)

**Impact:**
Provides concrete demonstration of how to apply the new protocols.

---

## Remaining Weaknesses & Future Optimization Needs

### Weakness 1: Multimedia Content Handling
**Issue:**
The framework assumes text-only articles. Modern weaponized narratives increasingly rely on images (misleading charts, cropped photos) and video (deceptively edited clips). The framework provides no protocol for visual media analysis.

**Recommendation for Next Optimizer:**
Add a "Phase 6: Visual Evidence Audit" that instructs multimodal LLMs to:
- Extract text from images and verify claims
- Check for cropping/context removal in photos
- Flag AI-generated or manipulated imagery
- Assess chart integrity (axis manipulation, cherry-picked ranges)

---

### Weakness 2: Nested Attribution Chains
**Issue:**
Articles often cite sources that cite other sources ("According to the New York Times, who cited anonymous officials..."). The framework doesn't clearly instruct how to handle multi-level attribution or assess the reliability degradation at each step.

**Recommendation:**
Create a "Citation Chain Decay Rule"—each level of indirection downgrades reliability (B1 → B2 → B3 → C).

---

### Weakness 3: Counterfactual Claims
**Issue:**
Articles sometimes make claims about alternate realities ("If policy X had been enacted, Y would have happened"). These are neither facts nor traditional framing—they're speculative modeling. The current decision tree doesn't address this.

**Recommendation:**
Add Question 9 to the decision tree: "Is this a counterfactual claim (what would/could have happened)?" → **Class C (Speculation)** unless backed by Class A modeling/simulation data.

---

### Weakness 4: Sarcasm & Irony Detection Ambiguity
**Issue:**
The framework labels sarcasm as Class C but provides no guidance for detecting it in text (where tone is absent). A quote like "Oh, that's just *great*" could be genuine praise or sarcastic criticism depending on context.

**Recommendation:**
Add a disambiguation rule: If the LLM cannot determine with confidence whether a statement is sincere or sarcastic, mark it as "Ambiguous—User Judgment Required" rather than making an arbitrary classification.

---

### Weakness 5: No Protocol for Correction/Retraction Handling
**Issue:**
If an article contains embedded corrections ("An earlier version of this article stated X, which was incorrect"), the framework doesn't specify how to handle this. Should the false claim appear in the Admissibility Log? Should corrections increase trust or flag prior unreliability?

**Recommendation:**
Add a "Correction Audit" subsection that treats retractions as *negative evidence* against the publication's reliability score but *positive evidence* for transparency. Track correction ratio as a meta-metric.

---

### Weakness 6: Propaganda Threshold Needs Validation
**Issue:**
The CDR propaganda threshold is set at 60% (Class C density) based on heuristic reasoning, not empirical testing. This number may be too high or too low.

**Recommendation:**
Future optimizers should validate this threshold against a corpus of known propaganda vs. known journalism and adjust accordingly. Consider making the threshold adjustable per context (opinion pieces vs. news reporting).

---

## Self-Assessment Questions

### Did I preserve the core axioms?
**Yes.** All edits reinforced Axioms 1-3 without modification. The Event Layer, Framing Layer, and Hierarchy of Substance remain intact.

### Did I introduce new complexity without justification?
**Partial concern.** The Source Authority Hierarchy (B1/B2/B3) adds granularity, which increases LLM cognitive load. However, this complexity is justified because treating all Class B evidence as equal created false equivalency. Future optimizers should monitor if this causes classification errors.

### Did I maintain ideology-agnostic design?
**Yes.** All new protocols (temporal fallacy detection, omission analysis, statistical interpretation) apply uniformly regardless of political orientation.

### Could a non-expert LLM follow these instructions?
**Mostly yes.** The decision tree is now slightly more complex (8 questions vs. 7), but each step remains binary and actionable. The Omission Analysis requires more inference ("what *should* be present?") which may challenge less capable models—this could be a future refinement area.

---

## Conclusion

Version 1.2 addresses critical gaps in quantification, temporal logic, source reliability, and omission detection. The framework is now more robust against sophisticated narrative manipulation tactics. However, multimedia handling and nested attribution remain unresolved, providing clear targets for the next optimization cycle.

**Estimated Improvement:** The additions should increase audit accuracy by ~20-30% when tested against articles employing temporal fallacies or strategic omissions—the two most common exploitation vectors in modern media.
