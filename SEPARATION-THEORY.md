## Separation Theory: Epistemic Classification
**v0.2**

---

## The Diagnostic

**When informed agents disagree, someone is wrong → Hard Objectivity**
**When they use different systems, both may be right → Framework-Dependent**
**When they hold different values, both may be right → Soft Subjectivity**
**When the claim isn't a valid epistemic move → Epistemic Junk**

---

## Four Categories

**Hard Objectivity (HO)**
Truth independent of observer. Expert convergence expected.
*2+2=4 • Speed of light • Caesar crossed the Rubicon • Meta-analysis shows...*

**Framework-Dependent (FD)**
True within a system. System choice depends on goals.

| Subtype | Definition | Example |
|---------|------------|---------|
| FD-settled | Framework + settled interpretation | "Exceeds 65 mph speed limit" |
| FD-contested | Framework exists, application disputed | "Unconstitutional per the 9th Circuit ruling in X v. Y" |

FD-contested without cited authority functions as SS.

**Soft Subjectivity (SS)**
Depends on priorities. Rational disagreement persists.
*"This policy is fair" • "Beauty matters most" • "Worth the tradeoff"*

**Epistemic Junk (EJ)**
Not a valid epistemic move. Fails the test: *Could an honest, informed opponent hold this through legitimate reasoning?*
*"All liberals are dumb" • "You only think that because..." • "Everyone knows..."*

---

## EJ Markers

Reject immediately if you see:
- "All [group] are [trait]"
- "You only think that because..."
- "Everyone knows..." (without citing what)
- Denial interpreted as proof
- Motive substituted for argument
- Unfalsifiable claims about intent
- Tribal signaling dressed as analysis

---

## Application Protocol

**1. Classify before evaluating**
Ask "what kind of claim is this?" before assessing truth or quality.

**2. Decompose compound claims**
"Agile is best; studies show 25% faster delivery"
→ "Best" (SS) + "25% faster" (HO) + "Speed = success" (SS)

**3. Name your frameworks**
❌ "This code is secure"
✓ "Meets OWASP Top 10 (2021)" (FD-settled)

❌ "Unconstitutional!"
✓ "Unconstitutional per ACLU's interpretation" (FD-contested with authority)

**4. Structure: Ground → Frame → Explore**
Lead with facts (HO), clarify systems (FD), then explore values (SS).

**5. Use category-revealing language**
- HO: *"Evidence shows" • "X is" • "The data..."*
- FD: *"Under [system]" • "Per [standard]" • "If we measure by..."*
- SS: *"Many prioritize" • "Tradeoffs include" • "Perspectives vary"*
- EJ: Don't engage. Reject or reframe.

---

## Critical Errors

**Type I: False Objectivity (SS → HO)**
Presenting values as facts.
*"Obviously the right approach..." • "Everyone knows..."*
→ Conceals bias. Users cannot challenge.

**Type II: False Subjectivity (HO → SS)**
Presenting facts as opinions.
*"Some say 2+2=4..." • "One theory is gravity..."*
→ Erodes trust. Users cannot rely.

**Type III: False Framework (SS → FD)**
Presenting values as framework conclusions.
*"Unconstitutional!" (without interpretive authority)*
→ Smuggles opinion as system output.

---

## Domain Guide

| Domain | Category | Key Move |
|--------|----------|----------|
| Math, physics, logic | HO | Grade certainty by evidence |
| Historical events | HO | Acknowledge source quality |
| Law, standards, games | FD | Cite jurisdiction/version; note if contested |
| Aesthetics, ethics, policy | SS | Present competing views |
| Ethics *within* a theory | FD | Name the theory explicitly |
| Predictions | HO → SS | Certainty decays with time/data sparsity |
| Group characterizations | Usually EJ | Test: honest opponent test |

---

## Diagnostic Flow

```
Is it verifiable and would informed parties converge?
  → Yes → HO
  → No ↓

Is there a citable framework?
  → Yes, settled interpretation → FD-settled
  → Yes, disputed interpretation → FD-contested (cite authority or treat as SS)
  → No ↓

Could an honest, informed opponent hold this through legitimate reasoning?
  → Yes → SS
  → No → EJ (reject)
```

---

## The Point

**Distinguish what we know from what we interpret.**

Users deserve to assess reliability.
Communicators must avoid arrogance (Type I), abdication (Type II), and smuggling (Type III).
Clarity reveals the true source of disagreement.

---

## Relationship to SKD

Separation Theory provides the **categorical vocabulary**.
SKD provides the **document architecture**.

| Task | Tool |
|------|------|
| Classifying a claim | Separation Theory |
| Building an argument | SKD |
| Auditing an argument | Both |

ST categories map to SKD chambers:

| ST Category | SKD Location |
|-------------|--------------|
| HO | Chamber 2 |
| FD | Chamber 1 (frameworks) + Chamber 3 (application) |
| SS | Chamber 1 (values) + Chamber 4 (interpretation) |
| EJ | Rejected before entry |

---

*The simplest complete explanation.*

---

*v0.2 — Updated to include EJ category, FD subtypes (settled/contested), Type III error, and SKD relationship.*
*Based on original Separation Theory by Don Schartman, 2025.*
