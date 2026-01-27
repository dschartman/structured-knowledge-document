# Structured Knowledge Document (SKD)
**Specification v0.5**

---

## Purpose

A document architecture for epistemic hygiene. Separates what we observe from what we interpret. Works for construction (building arguments) and deconstruction (auditing arguments).

---

## Categories

All claims belong to one of four categories:

| Category | Definition | Test |
|----------|------------|------|
| **Hard Objectivity (HO)** | Observer-independent truth | Would informed parties converge? Verifiable by third party? |
| **Framework-Dependent (FD)** | True within a cited system | Is there a nameable rulebook, standard, or law? |
| **Soft Subjectivity (SS)** | Rational value disagreement | Could an honest, informed opponent hold this through legitimate reasoning? |
| **Epistemic Junk (EJ)** | Not a valid epistemic move | Tribal signaling? Motive attribution? Unfalsifiable? Self-sealing logic? |

### FD Subtypes

| Subtype | Definition | Requirement |
|---------|------------|-------------|
| **FD-settled** | Framework + settled interpretation | Cite the framework |
| **FD-contested** | Framework exists, application disputed | Cite the framework AND the interpretive authority being relied on |

If FD-contested does not cite an interpretive authority, it functions as SS — the author's application of the framework, not a settled fact.

**Note:** FD subtypes are most relevant in domains where interpretation is genuinely contested (law, policy, ethics). In domains where frameworks are simply cited or not (technical standards, personal documentation), the distinction may be omitted.

**Examples:**
- "Exceeds 65 mph speed limit" → FD-settled
- "Unconstitutional arrest" → FD-contested; requires "per [authority]"
- "Violates GDPR Art. 6 per the Irish DPC ruling" → FD-contested with authority cited

---

## Chambers

An SKD contains four chambers. Content flows sequentially.

### Chamber 1: Scope + Frameworks + Values

**Required:**
- **Scope:** What is this document trying to establish?
- **Frameworks (FD):** What external systems are being invoked? Distinguish FD-settled from FD-contested.
- **Values (SS):** What thresholds or priorities is the author choosing?

**Constraints:**
- Frameworks and values, once declared, cannot shift mid-argument.
- If you're choosing the threshold, it's SS. If you're citing an external source with settled interpretation, it's FD-settled. If the interpretation is disputed, it's FD-contested and requires an authority.

---

### Chamber 2: Hard Objectivity

**Required:**
- Only HO claims enter this chamber.

**HO Requirements:**

| Requirement | Definition |
|-------------|------------|
| **Externally Verifiable** | Confirmable by a third party. Actions and statements qualify. Intentions and thoughts do not. |
| **Time-Anchored** | Fixed to a point in time (date, timestamp, or relative sequence). |
| **Atomic** | Single, irreducible unit. No relational operators (but, therefore, despite, which) linking two facts. |
| **Adjective-Free** | No narrative descriptors (massive, brutal, unfortunate). Record "$15,000 cost," not "excessive cost." |

**Handling:**
- Missing information is recorded as **[Unknown]** with appropriate granularity (see below).

---

### Chamber 3: Logic

**Function:**
Apply FD and SS from Chamber 1 to HO from Chamber 2.

**The Equation:**
```
HO + FD → Classification (does the fact meet the standard?)
HO + SS → Weighting (does the fact meet the declared threshold?)
```

**Constraints:**
- Logic must respect chronology. Event at T2 cannot cause event at T1.
- Causal claims require mechanism, not just correlation.
- If FD is uncited or SS is undeclared, the logic link fails.
- Narrative imposition (asserting causal or transformative structure without mechanism) is a Chamber 3 failure.

**Narrative Imposition Examples:**
- "The shooting upended the politics of immigration" — causal claim without mechanism
- "Democrats have awakened to a moral moment" — arc imposition without evidence
- "This was a turning point" — transformation claim without mechanism

**Legitimate alternative:** Narrative framing explicitly labeled as SS is not a failure. "I interpret these events as forming an arc of X" is valid — it declares interpretation rather than asserting causation as fact.

---

### Chamber 4: Interpretation

**Function:**
State SS conclusions derived from Chamber 3.

**Constraints:**
- Conclusions must be proportional to surviving evidence.
- Dependencies on **[Unknown]** must be explicitly flagged.
- Gaps cannot be filled with assumed intent.

---

## [Unknown] Granularity

Different types of missing information have different implications:

| Type | Definition | Implication |
|------|------------|-------------|
| **[Unknown-source]** | This source doesn't say | May be available elsewhere |
| **[Unknown-general]** | No one knows yet | Genuine uncertainty |
| **[Unknown-contested]** | Sources disagree | Requires adjudication |
| **[Unknown-omitted]** | Knowable but excluded | Potential selection bias |

**Note on [Unknown-omitted]:** Flagging omission requires external knowledge — you must know the information exists to note its absence. This makes it primarily a deconstruction tool.

---

## Construction Mode

When building a new argument:

1. **Chamber 1:** Declare scope. Cite frameworks (noting FD-settled vs. FD-contested). State values as SS.
2. **Chamber 2:** Gather only HO. Test each claim against the four requirements.
3. **Chamber 3:** Apply FD/SS to HO. Build logic chain. Ensure causal claims have mechanisms.
4. **Chamber 4:** State conclusions. Flag [Unknown] dependencies with appropriate type.

---

## Deconstruction Mode

When auditing an existing argument:

1. **Chamber 1:** Extract their scope (stated or implied), frameworks (cited or missing, settled or contested), values (declared or smuggled).
2. **Chamber 2:** Extract their claims. Test each against HO requirements. Categorize failures as SS contamination, atomic integrity failure, or EJ.
3. **Chamber 3:** Extract their logic links. Test each: Is FD cited (with authority if contested)? Is SS declared? Is mechanism present or just correlation? Flag narrative imposition.
4. **Chamber 4:** Extract their conclusions. Do they exceed what survives Chambers 1-3? Are [Unknown] dependencies flagged or hidden?

---

## Failure Modes

| Failure | Definition |
|---------|------------|
| **Chamber 1 Collapse** | No scope, uncited FD, smuggled SS |
| **Chamber 2 Contamination** | SS or EJ mixed with HO |
| **Atomic Integrity Failure** | Compound claims with relational operators in Chamber 2 |
| **FD-Contested Without Authority** | Framework cited but interpretive authority missing |
| **Narrative Imposition** | Causal or transformative claim without mechanism |
| **Logic Failure** | FD uncited, SS undeclared, or correlation treated as causation |
| **Chamber 4 Overreach** | Conclusions exceed surviving evidence; gaps unflagged |

---

## Reliability Rating

| Rating | Criteria |
|--------|----------|
| **High** | Chambers 1-4 intact; gaps flagged with appropriate type; conclusions proportional to evidence |
| **Medium** | Some contamination or unflagged gaps; core logic holds; central claims supported |
| **Low** | Chamber 1 collapse OR Chamber 3 failure on central claim OR significant [Unknown-omitted] |
| **Unreliable** | Structural failures across multiple chambers; conclusions unsupported by surviving evidence |

---

## Quick Diagnostic Flow

For any claim:

```
Is it verifiable and would informed parties converge?
  → Yes → HO (test the four requirements)
  → No ↓

Is there a citable framework?
  → Yes, settled interpretation → FD-settled
  → Yes, disputed interpretation → FD-contested (cite authority)
  → No ↓

Could an honest, informed opponent hold this?
  → Yes → SS (declare it as a value/threshold)
  → No ↓

→ EJ (reject; does not enter the system)
```

---

## EJ Markers

Reject immediately if you see:
- "All [group] are [trait]"
- "You only think that because..."
- "Everyone knows..." (without citing what)
- Denial interpreted as proof
- Motive substituted for argument
- Unfalsifiable claims about intent

---

## Boundaries of the Framework

SKD v0.5 answers: *Is this argument epistemically hygienic?*

It does not answer: *Is this SS position well-reasoned?*

Once SKD surfaces that a claim is SS, the framework's job is done. Evaluating which SS positions are better reasoned is a separate task.

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
