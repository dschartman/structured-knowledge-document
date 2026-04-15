# Structured Knowledge Document (SKD)
**Specification v0.6**

---

## Purpose

A document architecture for epistemic hygiene. Separates observation from interpretation. Enables both construction and deconstruction of arguments.

---

## Categories

All claims belong to one of four categories:

| Category | Definition |
|----------|------------|
| **Hard Objectivity (HO)** | Observer-independent truth. Informed parties would converge. Verifiable by third party. |
| **Framework-Dependent (FD)** | True within a cited system (rulebook, standard, law). |
| **Soft Subjectivity (SS)** | Rational value disagreement. Honest, informed opponents can legitimately hold opposing views. |
| **Epistemic Junk (EJ)** | Invalid epistemic move: tribal signaling, motive attribution, unfalsifiable claims, self-sealing logic. |

### FD Subtypes

| Subtype | Definition |
|---------|------------|
| **FD-settled** | Framework with settled interpretation. Cite the framework. |
| **FD-contested** | Framework exists; application disputed. Cite framework AND interpretive authority. |

**Critical:** FD-contested without cited authority functions as SS — the author's application, not a settled fact.

Apply FD subtypes where framework application is genuinely contested. Where interpretation is settled, omit the distinction.

**Examples:**
- "Exceeds 65 mph speed limit" → FD-settled
- "Breach of contract" → FD-contested; requires "per [authority]"
- "Exceeds EPA emission limits per the 2023 compliance audit" → FD-contested with authority cited

### Verification Status

HO and FD-contested claims carry a verification status: either the claim cites its evidence, or it is marked **[Unverified]**.

| Status | How it looks |
|--------|-------------|
| **Verified** | Claim cites evidence inline. "The system processes 10,000 req/s per load test results on 2024-12-01." |
| **Unverified** | Claim carries an explicit marker. "The system processes 10,000 req/s. [Unverified]" |

**Rule:** Every HO or FD-contested claim must either cite its evidence or carry an [Unverified] marker. No unmarked, uncited claims.

**Why:** Claims without visible verification status are silently treated as established fact — by both humans and LLM agents. The [Unverified] marker creates visible friction that prevents unverified assertions from passing as truth.

Unverified claims are not rejected. They can be used in downstream reasoning, but anything that depends on an [Unverified] claim inherits that uncertainty.

---

## Chambers

An SKD contains four chambers, executed sequentially.

### Chamber 1: Scope

Declare upfront:
- **Scope:** What is this document trying to establish?
- **Frameworks (FD):** What external systems are being invoked? Distinguish FD-settled from FD-contested.
- **Values (SS):** What thresholds or priorities is the author choosing?

**Immutability:** Frameworks and values, once declared, cannot shift mid-argument.

---

### Chamber 2: Hard Objectivity

Only HO claims enter this chamber. Each must satisfy:

| Requirement | Constraint |
|-------------|------------|
| **Externally Verifiable** | Confirmable by third party. Actions and statements qualify; intentions do not. |
| **Time-Anchored** | Fixed to point in time: date, timestamp, or relative sequence. |
| **Atomic** | Single, irreducible unit. No relational operators (but, therefore, despite, which). |
| **Adjective-Free** | No evaluative or interpretive modifiers. Record "$15,000 cost," not "excessive cost." |
| **Evidence-Cited or [Unverified]** | Cite evidence inline, or mark [Unverified]. No unmarked, uncited claims. |

Missing information is recorded as **[Unknown]** with appropriate type.

---

### Chamber 3: Logic

Apply FD and SS from Chamber 1 to HO from Chamber 2.

**The Equation:**
```
HO + FD → Classification
HO + SS → Weighting
```

**Constraints:**
- Chronology: Event at T2 cannot cause event at T1.
- Causation: Requires mechanism, not just correlation.
- Citation: Uncited FD or undeclared SS breaks the logic link.
- Narrative imposition: Causal or transformative claims without mechanism fail.

**Narrative Imposition Examples:**
- "The product launch revolutionized the industry" — transformation claim without mechanism
- "The merger fundamentally changed company culture" — causal claim without evidence
- "This was a turning point for the project" — transformation claim without mechanism

**Legitimate alternative:** Narrative framing explicitly labeled as SS. "I interpret these events as forming an arc of X" declares interpretation, not causation.

---

### Chamber 4: Interpretation

State SS conclusions derived from Chamber 3.

**Constraints:**
- Proportionality: Conclusions must match surviving evidence.
- Flagging: Dependencies on **[Unknown]** must be explicit.
- No gap-filling: Assumed intent is forbidden.

---

## [Unknown] Granularity

Different types of missing information have different implications:

| Type | Definition | Implication |
|------|------------|-------------|
| **[Unknown-source]** | This source doesn't say | May be available elsewhere |
| **[Unknown-general]** | No one knows yet | Genuine uncertainty |
| **[Unknown-contested]** | Sources disagree | Requires adjudication |
| **[Unknown-omitted]** | Knowable but excluded | Potential selection bias |

Flagging [Unknown-omitted] requires external knowledge. Deconstruction tool only.

---

## Failure Modes

| Failure | Definition |
|---------|------------|
| **Chamber 1 Collapse** | Scope undefined, FD uncited, SS undeclared, or mid-argument framework shift |
| **Chamber 2 Contamination** | SS or EJ mixed with HO |
| **Atomicity Violation** | Compound claims with relational operators in Chamber 2 |
| **Narrative Imposition** | Causal or transformative claim without mechanism |
| **Logic Failure** | FD uncited, SS undeclared, or correlation treated as causation |
| **Verification Gap** | HO or FD-contested claim lacks both evidence citation and [Unverified] marker |
| **Chamber 4 Overreach** | Conclusions exceed surviving evidence; gaps unflagged |

---

## Reliability Rating

| Rating | Criteria |
|--------|----------|
| **High** | Chambers 1-4 intact; gaps flagged with appropriate type; conclusions proportional to evidence |
| **Medium** | Minor contamination or unflagged gaps; core logic intact; primary conclusions supported |
| **Low** | Chamber 1 collapse OR Chamber 3 failure on primary claim OR [Unknown-omitted] undermines key conclusion |
| **Unreliable** | Structural failures across multiple chambers; conclusions unsupported by surviving evidence |

---

## Quick Diagnostic Flow

For any claim:

```
Verifiable + informed parties converge?
  → Yes → HO (test four requirements)
  → No ↓

Citable framework?
  → Yes, settled → FD-settled
  → Yes, disputed → FD-contested (cite authority)
  → No ↓

Honest, informed opponent could hold this?
  → Yes → SS (declare as value/threshold)
  → No ↓

→ EJ (reject)
```

---

## EJ Markers

Reject immediately:
- "All [group] are [trait]"
- "You only think that because..."
- "Everyone knows..." (uncited consensus claim)
- Denial interpreted as proof
- Motive substituted for argument
- Unfalsifiable claims

---

## Boundaries

SKD v0.6 answers: *Is this argument epistemically hygienic?*

It does not answer: *Is this SS position well-reasoned?*

Once a claim is categorized as SS, the framework's work is complete. SS quality evaluation is out of scope.

---

## Summary

| Chamber | Contains | Key Question |
|---------|----------|--------------|
| 1 | Scope, FD (settled/contested), SS | What are we establishing and by what rules? |
| 2 | HO only | What actually happened, verifiably? |
| 3 | Logic | Does HO meet FD/SS thresholds? Is mechanism present? |
| 4 | Interpretation | What conclusions are earned? What's [Unknown]? |

---

**Authorship**
Created by Don Schartman, 2025. Incorporating Separation Theory categories (HO/FD/SS/EJ).

**License**
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — Share and adapt with attribution.
