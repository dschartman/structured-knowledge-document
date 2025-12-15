# The Structured Knowledge Document (SKD)
**Specification v0.4.1**

**Scope:** Define a document architecture that enforces epistemic hygiene through structural separation of objective observation from subjective interpretation.

---

## Definitions

**Scope**
The declared focus of the document—what it is trying to establish. All chambers must serve the declared scope; content that does not serve the scope does not belong in the document.

**Objective Reality**
That which exists independently of the observer. To qualify as an Objective input, a data point must possess:
* **External Verification:** Measurable or confirmable by a third party. Actions and statements are external; intentions and thoughts are internal and disqualified.
* **Temporal Anchoring:** A specific coordinate in time, absolute or relative to other data points.
* **Atomic Integrity:** Irreducible. Cannot contain relational operators ("but," "therefore," "despite," "which") that link two facts together.

**Subjective Reality**
That which exists through interpretation, meaning-making, or belief. Conclusions, narratives, and opinions derived from objective inputs.

**Epistemic Hygiene**
The practice of scrubbing Objective observations to ensure they are free of Subjective contamination (adjectives, inferred motives) and Logical contamination (premature synthesis).

**Chamber**
A sealed, functional container within a document structure. Unlike a "section," a chamber describes *function and failure modes*:
* **Airlock principle:** Prevents contamination. The Observations chamber prevents subjective and logical content from entering.
* **Heart principle:** One-way flow. Logic flows *from* Observations. The flow cannot reverse.
* **Pressure principle:** Structural integrity. Weak walls cause collapse under logical pressure.

**Contextual Parameter**
A subjective value, rule, or threshold explicitly chosen for the specific context of the document. These are the "game rules" that govern the Logic Chain (e.g., "High Cost" > $10k, "Failure" > 200ms, "Qualified Candidate" ≥ 5 years experience).

**Axiomatic Consent**
The principle that the reader must accept the Contextual Parameters in Chamber 1 to validly process the Logic Chain in Chamber 3. Disagreement with parameters is valid; disagreement that logic followed stated parameters is a different claim.

**External Verification**
The property that an observation can be confirmed by a third party independent of the author.

**Temporal Anchoring**
The property that an observation is fixed to a point in time (date, timestamp, or relative sequence).

**Atomic Integrity**
The property that an observation is a single, irreducible unit. Compound sentences containing relational operators violate atomic integrity because they perform assembly (logic) rather than ingestion (observation).

**The Null State**
The default value for missing information. The system presumes neither regularity (competence) nor malice (conspiracy). Where data is missing, the value is **[Unknown]**.

**Certainty**
A spectrum measuring how verifiable a claim is. Ranges from low (unproven, speculative, contested) to high (verified, documented, independently confirmable).

**Magnitude Matrix**
A framework for classifying logical links by Certainty and Parameter Adherence:
* **Signal:** High Certainty + Meets Parameter Threshold.
* **Noise:** High Certainty + Below Parameter Threshold.
* **Speculation:** Low Certainty + Meets Parameter Threshold.
* **Void:** Low Certainty + Below Parameter Threshold.

**Transparency Engine**
The principle that subjectivity must be isolated in the setup (Parameters) so that processing (Logic) remains objective. Not all truths are equal; the SKD filters for significance by requiring that interpretations rest on Signal.

**Proportionality**
The principle that evidence strength must match claim weight. Boulder claims need boulder evidence.

**Adjective Ban**
The constraint that narrative descriptors (e.g., "massive," "struggled," "unfortunate") are prohibited in the Observations chamber. These words encode subjective judgment.

**Contextual Locking**
The constraint that definitions and parameters, once declared in Chamber 1, cannot be modified mid-argument.

**Chronological Causality**
The constraint that logic links must respect the timeline established in Chamber 2. An event at T2 cannot be listed as the cause of an event at T1.

**Gap Acknowledgment**
The constraint that interpretations must explicitly flag when they depend on [Unknown] values. Gaps cannot be filled with assumed intent.

**Chamber Collapse**
A failure mode where the pressure of the argument exceeds the structural integrity of the evidence.

**Contamination**
A failure mode where Subjective Reality leaks into Objective chambers.

**Pre-Mature Synthesis**
A failure mode where relational logic (comparisons, contradictions) leaks into Chamber 2, contaminating raw data with narrative structure.

**Achronological Causality**
A failure mode where the Logic Chain connects events in a way that violates their Temporal Anchors.

**Definition Drift**
A failure mode where a term or parameter defined in Chamber 1 is applied inconsistently in Chamber 3.

**False Equivalence**
A failure mode where Noise is treated with the same weight as Signal.

---

## Observations

An SKD contains exactly four chambers:
1. Definitions & Parameters
2. Observations
3. Logic Chain
4. Interpretations

Content flows sequentially: Definitions/Parameters → Observations → Logic → Interpretation.

**Chamber 1 (Definitions & Parameters) Constraints:**
* **Scope Declaration:** The document must explicitly state its scope—what it is trying to establish. Common examples include a question to answer, a claim to evaluate, or a problem to analyze.
* All ambiguous terms must be defined before use.
* **Parameter Declaration:** The document must explicitly define the Contextual Parameters that govern the Logic Chain. Common examples include severity thresholds, budget caps, success criteria, or qualification standards.
* **Contextual Locking:** Definitions and parameters, once declared, cannot be modified mid-argument.
* A threshold cannot be used in Chamber 3 that was not declared in Chamber 1.

**Chamber 2 (Observations) Constraints:**
* Contains only verifiable, objective content.
* **External Verification:** Each observation must be confirmable by a third party.
* **Temporal Anchoring:** Each observation must have a time coordinate.
* **Atomic Integrity:** Each observation must be a single, irreducible unit. Compound sentences with relational operators are prohibited.
* **Adjective Ban:** Narrative descriptors are prohibited.
* **Parameter-Free Zone:** Observations must be recorded raw, independent of the Parameters defined in Chamber 1. Record "$15,000 cost," not "Expensive cost."
* **The Null State:** Missing information is recorded as [Unknown], not inferred.

**Chamber 3 (Logic Chain) Constraints:**
* **Runtime Execution:** Logic is the application of Chamber 1 Parameters to Chamber 2 Observations.
* **The Equation:** Observation + Parameter → Classification. (e.g., "$15k cost" + "High Cost > $10k" → Signal)
* **Chronological Causality:** Logic links must respect the timeline. T2 cannot cause T1.
* Each logical link is assessed via the **Magnitude Matrix** (Certainty × Parameter Adherence).
* Interpretations must trace to **Signal** (High Certainty + Meets Parameter Threshold).
* **Proportionality:** Claim weight must match evidence strength.

**Chamber 4 (Interpretations) Constraints:**
* Contains subjective conclusions derived from Signal logic.
* **Gap Acknowledgment:** Dependencies on [Unknown] values must be explicitly flagged.
* Noise (True but Below Threshold) must be identified as such.

---

## Logic Chain

**Scope Declaration constrains relevance.**
Because the document must state what it's trying to establish, all content must serve that focus. Observations and logic that don't advance the scope are excluded.

**External Verification enables third-party audit.**
Because observations must be confirmable by others, the factual foundation is not dependent on trusting the author.

**Temporal Anchoring establishes sequence.**
Because every observation is time-fixed, the Logic Chain can verify causality. Events can only cause subsequent events.

**Atomic Integrity prevents premature synthesis.**
Because observations cannot contain relational operators, the author cannot smuggle narrative structure into the raw data. "A said X, but B shows Y" is logic, not observation. Separating "A said X [T1]" and "B shows Y [T2]" forces the comparison to happen in Chamber 3 where it is visible.

**Parameters contain subjectivity.**
By forcing value judgments into Chamber 1 as explicit Parameters, we prevent them from contaminating Chamber 2. The reader can disagree with the Parameter (the rule), but they cannot disagree that the Logic followed the rule.

**Runtime execution makes arguments debuggable.**
Logic is the application of a pre-defined parameter to observed data. If the conclusion is wrong, was the Observation wrong, or was the Parameter poorly calibrated? These are different failure modes with different fixes.

**The Null State prevents gap-filling.**
Because missing data defaults to [Unknown], the system resists speculation. Gaps are preserved rather than papered over with assumed intent.

**Contextual Locking prevents definition drift.**
Because terms and parameters are locked in Chamber 1, the argument cannot shift definitions mid-stream.

**The Adjective Ban isolates raw data.**
By prohibiting narrative descriptors, the framework forces separation between what happened and how it felt.

**Chronological Causality prevents backwards reasoning.**
Because logic must respect the timeline, an author cannot claim a later event caused an earlier one.

**The Magnitude Matrix prevents false equivalence.**
By classifying each logical link on Certainty and Parameter Adherence, the framework distinguishes between facts that meet the threshold (Signal) and facts that don't (Noise).

**Proportionality prevents overreach.**
Requiring that claim weight match evidence strength stops boulder claims from resting on pebble evidence.

**Gap Acknowledgment preserves intellectual honesty.**
Because interpretations must flag [Unknown] dependencies, conclusions cannot rest on hidden assumptions.

---

## Interpretation

The Structured Knowledge Document is a document architecture designed to enforce epistemic hygiene. It separates Objective Reality from Subjective Reality through a four-chamber structure and filters for significance through the Transparency Engine.

The architecture is a container format. It does not prescribe what you argue about—severity, money, time, ethics—only that you:
1. Define the terms (Definitions).
2. Set the thresholds (Parameters).
3. Measure the inputs (Observations).
4. Run the function (Logic).

This makes any argument debuggable. A budget proposal, hiring decision, incident report, or performance review can all be structured, validated, and critiqued using the same framework.

The architecture addresses failures in human reasoning:
1. **Fact-opinion bleed** (addressed by the Airlock, Adjective Ban).
2. **Hidden value judgments** (addressed by Contextual Parameters, Axiomatic Consent).
3. **Premature synthesis** (addressed by Atomic Integrity).
4. **False equivalence** (addressed by the Magnitude Matrix).
5. **Overreach** (addressed by Proportionality).
6. **Gap-filling** (addressed by The Null State and Gap Acknowledgment).
7. **Timeline violations** (addressed by Temporal Anchoring and Chronological Causality).
8. **Definition shifting** (addressed by Contextual Locking).

The specification is itself an SKD, demonstrating the recursive utility of the format.

---

**Authorship**
Created by Don Schartman, 2025.

**License**
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — Share and adapt with attribution.
