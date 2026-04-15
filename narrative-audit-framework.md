# Narrative Audit Framework

**Current Version:** 2.1

---

## Executive Summary (Start Here)

**What This Framework Does:**
This is an instruction set for AI systems to deconstruct news articles by separating verifiable facts from emotional/rhetorical framing. The output shows what the article wants you to believe versus what evidence actually supports.

**The Core Logic (3 Immutable Axioms):**
1. **Axiom 1 - Truth = Events + Speech Acts.** Only physical actions, data, and direct quotes are "real." Everything else is interpretation.
2. **Axiom 2 - Framing = Noise.** Adjectives, emotional loading, and unproven causation are filtered out as editorial interference.
3. **Axiom 3 - Hierarchy of Substance.** Class A (actions/data) > Class B (statements) > Class C (rhetoric/speculation). Weight evidence accordingly.

**Your Mission as the LLM:**
1. Read the article and catalog every claim
2. Strip away emotional language and adjectives (Axiom 2 enforcement)
3. Classify each claim as A (verifiable), B (speech act), or C (framing) (Axiom 3 enforcement)
4. Compare the article's thesis to the actual evidence (Axiom 1 enforcement)
5. Report the gap: What % of the narrative is supported by Class A/B facts?

**Critical Rule - Ideological Neutrality:**
Apply the same standards to ALL articles, regardless of political orientation. If you would classify a statement differently based on whether it supports your preferred viewpoint, you are injecting bias.

**Mandatory Enforcement Gates:**
At three points in the audit process, you MUST stop and verify compliance with the axioms. These gates cannot be skipped (see Phase 1 Gate, Phase 2 Gate, Phase 3 Gate in full protocol).

**This Framework Prioritizes:** **Consistency over comprehensiveness**. Do not flag every possible issue—flag only what the protocol explicitly instructs.

---

## Common Execution Errors & Corrections

**This section addresses the most frequent mistakes LLMs make when applying the framework. Review before starting.**

| **Error Pattern** | **Symptom** | **Correction** |
|------------------|------------|---------------|
| **Over-Classification** | You mark 80%+ of statements as Class C | You are likely conflating "imperfect sourcing" with "no substance." Re-read Safe-Fail principle: Class C is for speculation, rhetoric, and unverifiable claims—not for every statement that lacks a hyperlink. |
| **Under-Classification** | You mark 80%+ statements as Class A/B | You are likely accepting claims without checking for source attribution. Re-read Verification Priority Protocol: Class A requires actions/data, Class B requires named sources. Anonymous claims = Class C. |
| **Bias Injection** | You classify identical statements differently based on political orientation | You are applying ideological filters. Run the Self-Audit Check: "Would I classify this the same way if it supported the opposite view?" If no, reclassify. |
| **Metric Overload** | You skip calculating metrics or report "Unable to determine" | You are trying to calculate all metrics simultaneously. Follow the tiered structure: Always calculate Tier 1 (ESR, NIS, CDR, CDI). Calculate Tier 2 only when patterns detected. |
| **Skipping Headline Gate** | You complete Phase 2 without checking headline-body consistency | The Headline Gate is MANDATORY after Phase 2 classification. Headlines are the highest-impact framing element. Check before proceeding to Phase 3. |
| **Phantom Implicit Premises** | You flag 5+ implicit premises | You are over-detecting. Re-read Step 2.1.7 constraints: Only flag when ALL THREE criteria met (normative judgment + essential to pitch + no Class A/B support). Max 3 per article. |
| **Visual Protocol Overreach** | You flag Y-axis truncation or aspect ratio issues without visual access | The Visual Protocol (2.3.1) can only be fully applied with image access. If you cannot see the charts, note "Visual content not accessible" and skip detailed visual analysis. |
| **Premature Omission Flagging** | You flag omissions during Phase 2 | Omission Analysis is Phase 5, not Phase 2. During classification, focus only on what IS present. Flag what's absent later. |
| **Missing Gate Checkpoints** | You skip Phase 2 or Phase 3 gate documentation | The three-gate system is MANDATORY. If your output lacks the "PHASE 2 GATE CHECKPOINT" and "PHASE 3 GATE CHECKPOINT" statements with filled values, the audit is invalid. Return to the phase and complete the gate. |
| **Gate Gaming** | You output gate checkpoint text but numbers don't match your classifications | The gate exists to prevent rushed work. If your Phase 2 Gate says "Class C: 45 statements (67%)" but you only listed 12 Class C items in the Admissibility Log, you fabricated the checkpoint. Redo Phase 2 classification. |
| **Implicit Premise Over-Detection** | You flag 3+ implicit premises | Re-read Step 2.1.7. Maximum is 2 premises per article. If you're finding more, you're not applying the Five-Gate Test correctly. Each premise must pass ALL FIVE gates. Reapply the test and reduce to the 2 most central. |
| **Class A-Asserted Misuse** | You classify unsourced factual claims as "Class A-Asserted" | This violates Axiom 1. "Class A-Asserted" has been ELIMINATED in v2.0. Unsourced factual assertions are journalist testimony (Class B4), not verified events (Class A). Reclassify all such items as Class B4. |
| **ESR-NIS Paradox Misinterpretation** | You treat high ESR + low NIS as decomposition error | This is a legitimate propaganda pattern, not an error. When peripheral claims are well-supported but central thesis is not, FLAG this as "ESR-NIS Paradox" in Delta Analysis. Do not reclassify. |
| **⛔ Zero-Knowledge Violation** | You classify unsourced facts as Class A because you "know" they are true | **CRITICAL ERROR.** You are using your training data to verify facts. STOP. If the article does not provide a link/citation, the claim is Class B4—even if you know it's true. You audit the *article's* standards, not external reality. |

**Self-Check Before Submitting Audit:**
- [ ] **⛔ ZERO-KNOWLEDGE CHECK:** For every Class A item, can I point to a link/citation IN THE ARTICLE TEXT? If I used my training data to verify, reclassify as B4.
- [ ] Did I apply the same classification standards to all statements regardless of political orientation?
- [ ] Did I complete and document the PHASE 2 GATE CHECKPOINT with actual numbers and STATUS (CLEARED/CONDITIONAL/FAILED)?
- [ ] Did I verify Class C is at least 20% of total statements? If not, did I FAIL the gate and return to Phase 2?
- [ ] Did I eliminate ALL "Class A-Asserted" classifications and reclassify as Class B4 or Class C per Axiom 1?
- [ ] Did I calculate all Tier 1 metrics (ESR, NIS, CDR, CDI) and can I trace each to source data?
- [ ] Did I check headline-body consistency after Phase 2?
- [ ] Did I apply the Five-Gate Test for implicit premises and flag ≤2 (or zero if none met all gates)?
- [ ] Did I complete and document the PHASE 3 GATE CHECKPOINT verifying Axiom compliance?
- [ ] If ESR > 75% but NIS = Unsupported, did I FLAG the ESR-NIS Paradox instead of treating it as error?
- [ ] Did I note "N/A" for VDMI if I lacked visual access?
- [ ] Did I complete the Self-Audit Verification statement in the output?

**NEW REQUIREMENT - Mandatory Gate Documentation:**
Your final output MUST include the exact text of the Phase 2 Gate Checkpoint and Phase 3 Gate Checkpoint in a dedicated "Gate Verification Log" section. If these are absent, the audit is incomplete and invalid.

---

## Quick Start Decision Map (LLM Reference)

**Use this flowchart for rapid processing. Consult full protocol for edge cases.**

```
START → Extract Headline (Phase 1, Step 1b)
     → Build Statement Registry (Phase 1, Step 2)
     → For EACH statement:
        |
        ├─ Does it describe a physical action with timestamp/location OR specific data point?
        |  YES → Does article provide source citation? YES = Class A (Verified), NO = Class B4 (Journalist Assertion)
        |  NO → Continue
        |
        ├─ Is it a direct quote or official statement from a named source?
        |  YES → Check: Is it a joke/insult/sarcasm?
        |      YES → Class C
        |      NO → Class B (Sub-tier B1/B2/B3 based on source authority)
        |  NO → Continue
        |
        ├─ Does it contain modal verbs (could, might, may) or anonymous attribution?
        |  YES → Class C
        |  NO → Continue
        |
        ├─ Is it an interpretation, judgment, or emotional characterization?
        |  YES → Class C
        |  NO → Continue
        |
        └─ UNCERTAIN? → Class C (Safe-Fail Default)

     → Apply Priority 2 protocols ONLY when specific patterns detected
        (statistical claims → Step 2.3, charts → Step 2.3.1, quote centrality → Step 3.2.1)

     → Build Evidence Locker (Class A/B only)

     → Extract Narrative Pitch from headline + opening + closing

     → Delta Analysis: Match pitch sub-claims to Evidence Locker

     → Calculate metrics + Self-Audit (Phase 3.5)

     → Output report in standard schema
```

**Cognitive Load Management:**
- Process Priority 1 protocols (classification) for ALL statements
- Process Priority 2 protocols (statistical, visual) ONLY when patterns appear
- Process Priority 3 protocols (implicit premises, counterfactual) during Delta Analysis phase
- DO NOT attempt to apply all protocols to all statements simultaneously

---

## Full Documentation

**Objective:** To provide a rigorous, algorithmic step-by-step instruction set for Large Language Models (LLMs) to separate objective reality from weaponized narrative framing, including detection of misleading statistical interpretation, temporal fallacies, narrative omissions, quote mining, responsibility obfuscation, implicit premises, evidence chain-of-custody failures, synthetic narrative construction through claim aggregation, multi-source conflicts, expert credibility assessment, visual data manipulation, absence of counterfactual evidence, and juxtaposition-based false implications.

---

## Part I: The Philosophy & Approach
*(Human-Facing Context)*

### 1. The Core Mission
In an era of high-velocity information, "truth" is often obscured by "framing." The purpose of this framework is not to judge the morality of a news article, but to conduct a **Forensic Information Audit**. We strip away the emotional language (the weapon) to reveal the underlying event data (the reality), allowing the human user to judge the validity of the claims for themselves.

**The Danger of Weaponized Narratives:**
A "weaponized narrative" is an article where the framing layer (adjectives, emotional loading, rhetorical devices) is designed to override critical thinking and trigger a predetermined emotional response. The danger is not the presence of bias—all writers have perspective—but the **ratio of framing to fact**. When an article contains 80% emotional language and 20% verifiable events, it functions as propaganda, not journalism. This framework forces transparency by separating the two layers and making the ratio visible.

### 2. The Grounding Axioms
The logic of this framework relies on three immutable definitions of truth:

* **Axiom 1: The Event Layer.**
    * Truth is defined strictly by **Verifiable Events** (physical actions, timestamps, data) and **Explicit Speech Acts**.
    * *Note:* The fact that a person *said* something is a fact. The *content* of what they said is a claim.
    * **⛔ ZERO-KNOWLEDGE CONSTRAINT (CRITICAL):** The LLM auditor must NOT use its own training data to verify facts. If the article does not provide a link, citation, or document reference, the claim is **Class B4**—even if the LLM "knows" the fact is true. The audit evaluates the *article's* evidentiary standards, not external reality.

* **Axiom 2: The Framing Layer.**
    * "Framing" consists of adjectives, emotional loading, unproven cause-and-effect, and value judgments. This is treated as "noise" or "interference."

* **Axiom 3: The Hierarchy of Substance.**
    * Not all evidence is created equal. To avoid false equivalency, evidence must be weighed:
        * **Class A (Kinetic):** High Value. Physical actions, signed laws, official votes, verifiable raw data.
        * **Class B (Procedural):** Medium Value. Official policy statements, formal declarations, direct commands.
        * **Class C (Performative):** Zero Value. Jokes, rhetoric, insults, "clap-backs," conversational color, and sarcasm. **This must be filtered out.**

---

## Part II: The System Protocol
*(LLM-Facing Instructions)*

### Phase 1: Input Processing & Text Parsing

**Objective:** Ingest the article and establish a clean evidence chain.

**PRELIMINARY CHECK - Article Type Identification:**

Before beginning the audit, identify the article's declared format:
- **News Reporting:** Article presents itself as objective journalism
- **Opinion/Editorial:** Article is explicitly labeled as opinion, editorial, or commentary (typically has "Opinion" tag or byline stating "commentary")
- **Satire:** Article is from known satirical publication (The Onion, Babylon Bee, etc.)
- **Breaking News / Short Brief:** Article is under 500 words and labeled as "breaking" or "developing" news

**If Opinion/Editorial:** Apply framework as normal, but note in final report: "This is explicitly labeled opinion content. High Class C ratio is expected and does not indicate deception if disclosed."

**If Satire:** Do not audit. Output: "This content is satire and not subject to forensic audit."

**If Breaking News / Short Brief (< 500 words):** You may use QUICK AUDIT MODE (see below) ONLY if the article is explicitly labeled "breaking news" or "news brief" and contains primarily factual reporting. If the article contains opinion, analysis, or makes complex claims despite short length, use FULL protocol.

**If Ambiguous:** Proceed with audit and flag in final report: "Article format unclear - presented as [news/opinion]."

---

**QUICK AUDIT MODE (For Breaking News / Short Articles < 500 Words):**

When article length is under 500 words AND labeled as breaking/developing news, you may use this streamlined protocol:

**Streamlined Process:**
1. Extract headline and all factual claims (Phase 1 only)
2. Classify each claim as A/B/C using simplified decision tree (Phase 2 classification only, skip sub-protocols)
3. Calculate Tier 1 metrics only (ESR, NIS, CDR, VDR)
4. Skip: Omission analysis, implicit premise detection, statistical manipulation deep-dive
5. Note in report: "QUICK AUDIT MODE applied due to article brevity. Full protocol analysis not performed."

**When NOT to use Quick Mode (MANDATORY FULL AUDIT):**
- Article makes complex statistical claims (use full Step 2.3)
- Article is labeled "analysis," "explainer," "opinion," or "commentary" (use full protocol)
- Article contains 5+ direct quotes (use full protocol with quote context verification)
- Article makes causal claims (X caused Y) even if brief (use full temporal fallacy protocol)
- Article contains superlatives ("worst," "unprecedented," "catastrophic") even if brief (use full framing removal)
- User explicitly requests full audit
- Article brevity appears intentional to evade scrutiny (if in doubt, use full audit)

**Rationale:** Breaking news briefs (e.g., "Senator X resigned today, effective immediately") often contain minimal framing by design. Applying the full protocol (omission analysis, counterfactual testing) would flag absences that are appropriate for the format. Quick mode preserves classification rigor while acknowledging format constraints.

**Instructions for the LLM:**

1. **Read the entire article once** without analysis. Your goal is to map the structure.

1b. **Extract Headline and Subheadline (Mandatory First Step):**
   - Before processing body text, record:
     - **Headline:** [Exact text]
     - **Subheadline/Deck:** [Exact text, if present]
     - **Byline/Timestamp:** [Author name and publication date, if available]
   - **Purpose:** Headlines are designed for maximum impact and often contain the strongest framing. Extracting them separately allows systematic comparison to body evidence in Phase 3.

2. **Create a Statement Registry.** For each claim, statement, or assertion in the text, log:
   - The exact text (verbatim)
   - The paragraph/sentence number
   - The attribution (Who said/did this?)
   - The evidence form (see classification below)

3. **Evidence Form Classification:**
   - **Direct Quote:** Text enclosed in quotation marks, attributed to a named source.
     - *Example:* Senator X said, "We will vote on this bill."
     - *Rule:* The fact that they *said it* is Class B evidence. The content of the claim must be separately verified.

   - **Paraphrase:** A summary of someone's statement without direct quotation.
     - *Example:* The senator indicated support for the bill.
     - *Rule:* This is a journalist's interpretation. Treat as **unverified claim** unless the article links to the original source material.

   - **Reported Action:** A description of a physical event.
     - *Example:* "The president signed Executive Order 12345 on March 1st."
     - *Rule:* Verify if this is sourced (link, citation) or unsourced (journalist claim).
     - **Special Case - Campaign Promises & Threats as Speech Acts:**
       - *Problem:* Statements like "Candidate X vows to ban Y" or "Official threatens to withdraw from treaty" are BOTH speech acts (Class B - they said it) AND performative rhetoric.
       - **Classification Rule (Three-Tier System):**
         - **Class A (Kinetic Policy):** If the person has ALREADY taken concrete action (signed order, cast vote, submitted bill text).
           - *Example:* "The president signed Executive Order 12345 banning X" → Class A (action occurred)
         - **Class B (Procedural Declaration):** If the statement is a formal policy commitment in an official setting with institutional accountability.
           - *Example:* "In his inaugural address, the president stated he will withdraw from the agreement" → Class B1 (formal setting, on-record, position of authority)
           - *Test for B:* Is there institutional consequence for non-follow-through? (e.g., oath violations, official policy statements that can be fact-checked later)
         - **Class C (Performative Rhetoric):** If the statement is campaign hyperbole, rally rhetoric, informal social media post, or conversational aside.
           - *Example:* "At a rally, candidate shouted 'We're going to ban it all!'" → Class C (rally rhetoric, no formal policy proposal)
           - *Example:* "On Twitter, official posted 'Thinking about banning X'" → Class C (informal, speculative)
       - **Binding Commitment Test (Four-Factor Algorithm):**

         Apply ALL FOUR factors. A statement needs 3 out of 4 "YES" answers to qualify as Class B. Otherwise, it's Class C.

         **Factor 1 - Action Already Taken?**
         - Has the physical action occurred (signed, voted, enacted)?
         - YES = Class A (not B), stop here
         - NO = Continue to Factor 2

         **Factor 2 - Formal Institutional Setting?**
         - Was this stated in: official speech, sworn testimony, signed policy document, legislative debate, press conference as an elected official?
         - YES = +1 point toward Class B
         - NO (rally, social media, campaign ad) = 0 points

         **Factor 3 - Official Capacity?**
         - Was the speaker acting in their official role (President, Senator, Governor) vs. campaign mode (Candidate, Party Member)?
         - Test: Could they be held accountable via impeachment, recall, or institutional consequence for non-follow-through?
         - YES = +1 point toward Class B
         - NO = 0 points

         **Factor 4 - Specific Policy Mechanism?**
         - Does the statement identify HOW it will be done (legislative text, executive order number, specific budget allocation)?
         - "I will ban X by signing Executive Order 12345" = Specific mechanism
         - "I will ban X" = Vague intention
         - YES = +1 point toward Class B
         - NO = 0 points

         **Classification Outcome:**
         - **3-4 points:** Class B (Procedural commitment with institutional weight)
         - **0-2 points:** Class C (Performative rhetoric without binding force)

       - **Edge Case - Repeated Public Commitments:** If a candidate makes the same policy promise in 5+ formal settings (debates, policy speeches, official platforms), this satisfies Factor 2 and Factor 4 even if individual instances are vague. Elevate to Class B2 (demonstrable pattern of commitment).

       - **Warning:** Do not promote campaign promises to Class B simply because they are specific or frequently repeated. The Four-Factor Test must be applied. A stadium rally promise, no matter how specific, typically scores 0-1 points (no official capacity, no formal setting).

   - **Editorial Framing:** Language that attributes motive, emotion, or judgment.
     - *Example:* "The senator's desperate attempt to salvage his reputation..."
     - *Rule:* Flag immediately for Phase 2 filtering.

4. **Source Verification Requirement & Verification Priority Protocol:**
   - If the article cites external documents (bills, orders, studies), note the citation.
   - If a claim lacks attribution (anonymous sources, "sources say"), mark it as **Class C (unverifiable)** unless corroborated by Class A evidence elsewhere.
   - **Special Rule - "According To" Fallacy:** Flag phrases like "according to experts," "analysts say," "observers note" where no specific expert/analyst/observer is named. These are Class C unless names and credentials are provided.

   **NEW: Verification Priority Protocol (AXIOM 1 ENFORCEMENT - REVISED):**
   - **Problem:** Many articles assert Class A events (actions, data) without providing source links or citations.
   - **Axiom 1 Principle:** "Truth = Verifiable Events + Explicit Speech Acts." If an event cannot be verified, it is a CLAIM, not a fact.
   - **Solution - Two-Tier Classification (Class A-Asserted ELIMINATED):**
     - **Class A (Verified):** Claim is sourced with link, citation, or document reference that allows independent verification.
       - *Example:* "The bill passed 60-40" with link to official vote record.
       - **This is the ONLY form of Class A evidence.** No exceptions.
       - **⛔ ZERO-KNOWLEDGE RULE:** Do NOT use your training data to verify. If the link is not in the article text, it is NOT Class A. Your job is to audit the article's standards, not to fact-check against external knowledge.
     - **Class B4 (Journalist Assertion):** Claim describes a physical action or data point but article provides no verification path.
       - *Example:* "The bill passed 60-40" with no citation.
       - **Rationale:** The journalist's claim that an event occurred is itself a speech act. Treat it as testimony (Class B), not verified fact (Class A).
       - **Treatment:** Include in Evidence Locker as **"Class B4 (Journalist Assertion - Unverified Event Claim)."**
       - **Critical Rule:** Do NOT promote to Class A without source citation. Axiom 1 requires verification, not assertion.
     - **Class C (Rejected):** Claim is neither verified nor verifiable in principle (speculation, anonymous rumors).
   - **Practical Instruction:** When processing claims, ask: "If I wanted to independently verify this, does the article give me a path?" If no, this is Class B4 (journalist testimony), not Class A (verified event).
   - **Impact on Metrics:** Class B4 claims contribute to ESR but are weighted lower than Class A in narrative support assessments. VDR metric is eliminated (no longer tracking "asserted" claims, only verified vs. unverified).

5. **Source Authority Hierarchy (Sub-Classification for Class B):**
   - Not all Class B evidence has equal weight. Apply sub-tiers:
     - **B1 (Named Official, On-Record):** Direct quotes from identified officials in formal settings. High reliability.
     - **B2 (Named Individual, Informal):** Direct quotes from identified persons in informal contexts (social media, casual interviews). Medium reliability.
     - **B3 (Anonymous Attribution):** "Officials say," "sources close to." Low reliability. Treat as provisional unless corroborated by Class A or B1 evidence.
     - **B4 (Journalist Assertion of Fact):** NEW. Journalist asserts a physical event or data point occurred but provides no source citation. This is testimony about reality, not verified reality itself.
       - *Example:* "The mayor signed the order yesterday" with no link/citation.
       - **Treatment:** Lower reliability than B1-B3 because journalist credibility is untested. Can support narrative claims but with noted verification gap.
   - **Rule:** In the Evidence Locker, note the sub-tier (e.g., "Class B1" or "Class B4").

6. **Expert Credibility Verification Protocol:**
   - When article cites "experts," "analysts," "researchers," or "studies," apply domain credibility assessment:
   - **Credibility Tier System:**
     - **Tier 1 (Verified Domain Authority):** Named expert with verifiable credentials directly relevant to claim domain + institutional affiliation disclosed + peer-reviewed publication history in field.
       - *Example:* "Dr. Jane Smith, Professor of Epidemiology at Johns Hopkins University, who published 40+ peer-reviewed papers on viral transmission..."
       - *Classification:* Class B1 (High reliability speech act)
     - **Tier 2 (Named Expert, Credentials Disclosed):** Named individual with disclosed credentials but without institutional verification or publication history provided.
       - *Example:* "John Doe, who holds a PhD in Economics, stated..."
       - *Classification:* Class B2 (Medium reliability - credentials cannot be independently verified from article alone)
     - **Tier 3 (Named Expert, No Credentials):** Named individual described as "expert" without disclosed qualifications.
       - *Example:* "Technology expert Mary Jones said..."
       - *Classification:* Class B3 (Low reliability - "expert" status unverifiable)
     - **Tier 4 (Anonymous Expert):** Unnamed "experts," "analysts," "researchers."
       - *Example:* "Experts warn..." or "Studies show..."
       - *Classification:* Class C (Unverifiable - treat as editorial assertion unless study/expert is named)
   - **Study Citation Protocol:**
     - When article references "a study" or "research shows":
       - **Required for Class A:** Study name, publication, date, author(s), link/DOI
       - **Class B (Provisional):** Study author named, publication named, but no link provided
       - **Class C:** "Studies show" without specific citation
   - **Cross-Domain Credibility Trap:**
     - Flag when an expert is cited outside their domain of expertise.
     - *Example:* A Nobel Prize-winning physicist commenting on monetary policy carries less weight than a PhD economist in that specific domain.
     - *Rule:* Note in Evidence Locker: **"Expert cited outside primary domain - [Name] credentials in [Field A], claim regarding [Field B]."**
   - **Conflict of Interest Detection:**
     - Check if article discloses financial/institutional conflicts.
     - *Example:* "Dr. X, who receives funding from pharmaceutical company Y, stated that drug Y is safe."
     - *Rule:* If conflict exists and is undisclosed, flag in Omission Log as **"Undisclosed Conflict of Interest."**
     - If disclosed, note in Evidence Locker: **"Class B - Conflict Disclosed: [details]."**

7. **Attribution Chain Transparency Protocol:**
   - Modern articles often cite other articles, which cite other sources, creating a "citation cascade" where original context degrades.
   - **Chain-of-Custody Tracking:**
     - **Primary Source (Chain Length = 0):** Article directly quotes/cites the original actor or data.
       - *Example:* Article links to official government report. (High reliability)
     - **Secondary Source (Chain Length = 1):** Article cites another news outlet that quotes the original source.
       - *Example:* "According to the New York Times, Senator X said..."
       - *Rule:* Mark as **"Class B - Secondary Attribution."** Reliability depends on intermediary's credibility.
     - **Tertiary+ Source (Chain Length = 2+):** Article cites a source that cites another source.
       - *Example:* "The Daily News reports that the Washington Post stated that Senator X said..."
       - *Rule:* Mark as **"Class B3 - Multi-Hop Attribution, Verification Degraded."** Each hop increases distortion risk.
   - **Critical Rule - The Telephone Game Principle:**
     - When chain length ≥ 2, flag in Delta Analysis as **"Multi-Hop Attribution Chain - Original Context Cannot Be Verified from Article."**
     - Do not reject the claim automatically, but note the degraded verification path.
   - **Social Media Chain-of-Custody:**
     - If article cites a social media post (tweet, Facebook post) without linking to it:
       - Mark as **"Class C - Unverifiable Social Media Claim."**
     - If article links to social media post:
       - Mark as **"Class B2 - Social Media Speech Act (Context: Informal Platform)."**
       - Note: Social media posts can be deleted, edited, or taken out of temporal context. Flag if post is central to Narrative Pitch.

8. **Chain-of-Custody Protocol for Class B Claims:**
   - When a Class B source (quote/statement) makes a factual claim about a Class A event, you must verify the event independently.
   - **Problem Pattern:** Article quotes Senator X saying "The bill passed with 90% support" (Class B speech act).
   - **Required Check:** Did the bill actually pass? Was the vote margin 90%? Verify against Class A sources (voting records).
   - **Extraction Rule:**
     - *Class B (Verified):* Senator X stated the bill passed. (Speech act = fact)
     - *Class A (Required for Full Support):* Independent verification that bill passed with stated margin.
   - **If verification is absent:** Note in Evidence Locker as **"Class B Claim - Class A Verification Pending."** Flag for Delta Analysis as potential unsupported assertion.
   - **Critical Rule:** Never promote a claim to Class A status solely because a credible person said it. Class A requires independent verification.

### Phase 2: The Logic Engine (Framing Removal & Evidence Classification)

**Objective:** Apply Axiom 2 (strip framing) and Axiom 3 (classify by substance).

**COGNITIVE LOAD MANAGEMENT (CRITICAL - READ FIRST):**

This phase contains 15+ sub-protocols. Attempting to apply all protocols to every statement simultaneously will cause execution failure. The framework is designed for SEQUENTIAL PROCESSING, not parallel processing.

**EXECUTION STRATEGY (Mandatory Order):**

**PASS 1 - CLASSIFICATION (Apply to ALL statements):**
- Step 2.1.0: Semantic drift detection (track term changes for same entities)
- Step 2.1.1-6: Remove adjectives, emotional language, detect temporal fallacies, responsibility obfuscation, scope inflation, modal hedging
- Step 2.2: Classify each statement as A/B/C using decision tree
- **Output:** Statement Registry with classification for every claim in article

**PASS 2 - DOMAIN-SPECIFIC PROTOCOLS (Apply ONLY when patterns detected):**
- Step 2.3: Statistical manipulation (trigger: article contains numbers, percentages, or data claims)
- Step 2.3.1: Visual manipulation (trigger: article references charts/images AND you have visual access or detailed text descriptions)
- Step 2.3.2: Multi-source conflicts (trigger: article cites 2+ sources making competing factual claims)
- Step 2.4: Internal contradictions (trigger: you notice conflicting Class A/B facts during classification)
- **Output:** Manipulation flags and conflict logs

**PASS 3 - DELTA ANALYSIS PROTOCOLS (Apply during Phase 3, NOT Phase 2):**
- Step 2.1.7: Implicit premises (identify during narrative pitch decomposition)
- Step 2.1.8: Juxtaposition detection (identify during evidence-to-claim matching)
- Step 3.2.1: Quote context verification (only for quotes central to Narrative Pitch)
- Step 3.2.2: Counterfactual testing
- Step 3.3b: Synthetic narrative detection
- **Output:** Gap analysis and inferential leap identification

**CRITICAL RULE - DO NOT:**
- Attempt to apply implicit premise detection during initial statement classification
- Check quote context for every quote (only those central to the narrative)
- Apply visual manipulation protocol without visual access or detailed descriptions
- Flag omissions during Phase 2 (omissions are Phase 5)

**WHY THIS MATTERS:**
LLMs that try to apply all protocols simultaneously produce incomplete audits with missed classifications and phantom detections. Sequential execution ensures thoroughness.

**Step 2.1: Adjective & Emotional Language Removal**

For each statement in your registry, perform this surgical extraction:

0. **Detect Semantic Drift (Term Substitution Framing):**
   - Articles often shift terminology mid-narrative to reframe events without explicit editorializing.
   - *Example:* Paragraph 1: "Protesters gathered outside City Hall." Paragraph 5: "Rioters blocked access to the building." Paragraph 8: "The mob dispersed after police arrived."
   - **Detection Rule:**
     - Track all nouns/verbs used to describe the same actor, event, or entity throughout the article.
     - If terms shift from neutral → negative (or neutral → positive), flag as **Semantic Drift.**
     - *Example Analysis:*
       - Neutral: "protesters" → Negative: "rioters" → Extreme: "mob"
       - Neutral: "gathering" → Negative: "blockade" → Extreme: "siege"
   - **Extraction Protocol:**
     - Document the term progression in Delta Analysis: **"Semantic Drift Detected: [Entity] described as '[Term 1]' (Para X), then '[Term 2]' (Para Y), then '[Term 3]' (Para Z)."**
     - Check: Does the article provide Class A evidence justifying the escalation? (e.g., Did protesters commit acts of violence/vandalism that would legally classify them as rioters?)
     - If no Class A justification exists, flag as **"Semantic Reframing Without Evidentiary Basis."**
   - **Constraint:** Only flag if BOTH conditions met:
     - (1) The semantic shift carries clear evaluative loading (neutral → negative OR neutral → positive)
     - (2) The shift lacks Class A evidentiary justification
     - *Example of neutral shift (do not flag):* "Protest" → "demonstration" (both neutral)
     - *Example of justified shift (do not flag):* "Gathering" → "riot" when article provides Class A evidence of violence/property damage meeting legal definition of riot
     - *Example of unjustified shift (FLAG):* "Gathering" → "mob" when article provides no Class A evidence of violence or criminal behavior
   - **Frequency Guidance:** Most articles contain 0-2 semantic drift patterns. If you detect 3+, verify each meets BOTH conditions above. Semantic drift is objective when properly constrained—do not artificially limit flagging if shifts are genuinely present and unjustified.

1. **Identify Modifying Language:**
   - Adjectives that convey judgment: "shocking," "unprecedented," "controversial," "widely criticized"
   - Adverbs that imply emotion: "desperately," "callously," "boldly," "angrily"
   - Metaphors/analogies: "declared war on," "threw under the bus," "doubled down"

2. **Extract the Core Assertion:**
   - *Original:* "The embattled senator desperately clung to power by making a controversial vote."
   - *Extracted:* "The senator voted [X] on bill [Y]."
   - *Discarded Framing:* "embattled," "desperately clung," "controversial"

3. **Test for Implied Causation (Temporal Fallacy Detection):**
   - Does the sentence assert causation without evidence?
   - *Example:* "The policy caused widespread harm."
   - *Test:* Is there data showing harm? Is the causal link proven or assumed?
   - *Action:* If unproven, reclassify the causation as editorial opinion (Class C).

   **Special Case: Post Hoc Ergo Propter Hoc (False Temporal Causation)**
   - Articles often imply causation through sequence: "After X did Y, unemployment rose."
   - **Extraction Rule:**
     - *Fact 1:* X did Y on [date]. (Class A/B)
     - *Fact 2:* Unemployment rose from [%] to [%] between [dates]. (Class A, if sourced)
     - *Implied Causation:* Y caused unemployment rise. (Class C unless article provides Class A evidence of mechanism)
   - **Flag for Delta Analysis:** Note this as "Temporal Correlation Without Proven Mechanism."

4. **Detect Responsibility Obfuscation (Passive Voice Evasion):**
   - Passive voice can obscure agency and accountability.
   - *Example:* "Mistakes were made" vs. "Director Smith made mistakes."
   - **Extraction Rule:**
     - If a statement uses passive voice to describe an action without identifying the actor, flag it.
     - *Original:* "The funds were misallocated."
     - *Extracted Question:* "Who misallocated the funds?"
     - If the article does not answer this question, note in Omission Log as **"Agency Omission."**
     - Do not invent an actor, but flag the structural evasion.

5. **Detect Scope Inflation (Generalization Without Data):**
   - A single incident is often inflated into a pattern or trend without supporting evidence.
   - *Example:* "Company X laid off 50 workers. The tech industry is collapsing."
   - **Extraction Rule:**
     - *Fact:* Company X laid off 50 workers on [date]. (Class A, if sourced)
     - *Generalization:* The tech industry is collapsing. (Class C unless supported by Class A data on industry-wide trends)
   - **Flag for Delta Analysis:** Note this as "Scope Inflation - Single Incident → Pattern Claim."

6. **Detect Modal Verb Hedging (Speculative Framing):**
   - Modal verbs ("could," "might," "may") allow speculation to masquerade as analysis while maintaining plausible deniability.
   - *Example:* "The policy could devastate millions" vs. "The policy will devastate millions."
   - **Extraction Rule:**
     - Modal verbs indicate speculation, not fact. Flag immediately as **Class C (Speculative Claim).**
     - *Original:* "Experts warn the bill could cause economic collapse."
     - *Extracted:* Experts issued a warning about the bill. (Class B2/B3 - speech act only)
     - *Rejected:* "Economic collapse" as a fact. (Class C - speculative outcome)
   - **Special Case - "Could" as Threat Framing:**
     - "This policy could harm children" functions to trigger emotional response without making falsifiable claim.
     - **Flag for Delta Analysis:** Mark as **"Speculative Threat - No Class A Evidence of Mechanism or Probability."**

7. **Detect Implicit Premises (Hidden Assumptions) - ALGORITHMIC PROTOCOL WITH HARD GATES:**
   - Articles often rely on unstated assumptions that, if made explicit, would be controversial or unsupported.
   - *Example:* "The mayor's third mansion purchase raises eyebrows."
   - **Implicit Premises:**
     - Premise 1 (Stated): Mayor purchased third mansion. (Class A, if sourced)
     - Premise 2 (Implicit): Public officials should not own multiple mansions. (Value judgment - Class C)
     - Premise 3 (Implicit): The purchase is newsworthy/scandalous. (Editorial framing - Class C)

   **MANDATORY FIVE-GATE TEST - All Gates Must Pass:**

   Only flag as implicit premise if ALL FIVE conditions are met:

   **Gate 1 - Normative Language Test:**
   - Does the statement contain words that express judgment? (scandal, crisis, controversial, alarming, reckless, betrayal, failure)
   - **If NO normative language present → STOP. Not an implicit premise.**

   **Gate 2 - Factual Basis Test:**
   - Is there a Class A/B fact underneath the normative judgment?
   - *Example:* "Mayor purchased mansion" (Class A fact) + "raises eyebrows" (normative judgment)
   - **If NO factual substrate exists → STOP. It's pure opinion, not an implicit premise.**

   **Gate 3 - External Standard Test:**
   - Does the article cite ANY of the following to justify the judgment?
     - Specific law, statute, or regulation (Class A)
     - Official ethics code or policy (Class A)
     - Expert consensus with named sources (Class B1)
     - Precedent with specific examples (Class A)
   - **If ANY external standard is cited → STOP. The premise is argued, not implicit.**

   **Gate 4 - Centrality Test:**
   - Remove this judgment from the article. Does the Narrative Pitch collapse?
   - *Example:* If "scandal" framing is removed from mansion story, does the article still have a point?
   - **If the narrative survives without the judgment → STOP. It's peripheral commentary, not a load-bearing premise.**

   **Gate 5 - Ideological Symmetry Test:**
   - Reverse the political valence. If this judgment appeared in an article supporting the opposite ideology, would you still flag it as an unargued premise?
   - *Example:* "Official's generous donation raises eyebrows" - Would you flag the implied premise that generosity is suspicious?
   - **If you would NOT flag it with reversed politics → STOP. You are injecting bias, not detecting logical gaps.**

   **ONLY IF ALL FIVE GATES PASS:**
   - Document as: "Implicit Premise Detected: Article requires reader agreement that [X normative standard] without providing Class A/B evidence that [X] is the applicable standard in this context."

   - **Frequency Limit:** Maximum 2 implicit premises per article. If you identify 3+, reapply the Five-Gate Test—you are likely over-detecting.

   - **Mandatory Logging:** For each implicit premise flagged, you MUST note which Five Gates were passed. If you cannot articulate how all five were passed, do not flag it.

8. **Detect "Lawyering" (Technically True, Contextually Misleading) - ALGORITHMIC DETECTION:**
   - Individual statements may be factually accurate but arranged to create false impressions.
   - *Example:* "Senator X voted against the Child Safety Act. Child abuse rates are rising."
   - **Analysis:**
     - *Fact 1:* Senator X voted against bill titled "Child Safety Act." (Class A)
     - *Fact 2:* Child abuse rates increased from [X] to [Y]. (Class A, if sourced)
     - **Implied but Unstated:** Senator X opposes child safety / Senator X's vote caused increased abuse.

   **ALGORITHMIC DETECTION PROTOCOL:**
   - **Step 1 - Identify Juxtaposition:** Find pairs of Class A/B facts placed in immediate sequence (same paragraph or consecutive sentences).
   - **Step 2 - Apply Logical Bridge Test:**
     - Ask: "Is there an explicit statement connecting these facts?"
     - *Example (Explicit Bridge):* "Senator X voted against the Child Safety Act, which experts say contributed to rising abuse rates." (The bridge is stated—now test the bridge's Class A/B support)
     - *Example (No Bridge):* "Senator X voted against the Child Safety Act. Child abuse rates are rising." (No explicit connection)
   - **Step 3 - Implication Assessment (Only if No Bridge):**
     - Ask: "What is the only logical reason to place these facts adjacently?"
     - If the implied connection would require Class A evidence of causation but none is provided, flag as juxtaposition implication.
   - **Step 4 - Extraction Rule:**
     - Both facts remain in Evidence Locker separately.
     - In Delta Analysis, note: **"Juxtaposition Implication Detected: Fact A and Fact B placed adjacently without explicit causal link. Reader likely to infer [X], but no Class A/B support provided for connection."**

   **CONSTRAINT - Avoid Over-Flagging:**
   - Only flag when the juxtaposition serves NO other narrative purpose except to imply causation.
   - If facts are part of chronological reporting or context-setting, do not flag.

**Step 2.2: Evidence Classification (Applying Axiom 3)**

For each extracted statement, classify it using this decision tree:

**AXIOM-ANCHORED DECISION TREE (Mandatory Process):**

**BEFORE YOU BEGIN:** Remember Axiom 3 - Only Class A and Class B have evidentiary weight. Class C is the quarantine zone. When uncertain, default to Class C.

Ask these questions in order. Stop at first "Yes."

**CLASS A TESTS (Kinetic Evidence - Axiom 1: Verified Events Only):**
1. **Physical action (signed, voted, arrested, traveled) with timestamp/location AND source citation?** → **Class A (Verified)**
   - If NO source citation provided → **Class B4 (Journalist Assertion)**, not Class A
2. **Verifiable data point (number, percentage, dollar amount) with specific value AND source citation?** → **Class A (Verified)**
   - If NO source citation provided → **Class B4 (Journalist Assertion)**, not Class A

**CLASS B TESTS (Procedural Evidence - Axiom 1: Speech Acts):**
3. **Official statement from named source in formal capacity?** → **Class B (Sub-tier depends on source type)**
4. **Direct quote from named person (not official capacity)?** → **Class B (Sub-tier depends on context)**

**CLASS C FILTERS (Performative/Framing - Axiom 2: Noise Removal):**
5. **Interpretation of data using subjective language (soared, collapsed, crisis)?** → **Class C - Extract underlying number only**
6. **Anonymous attribution (sources say, experts warn, critics claim)?** → **Class C**
7. **Editorial characterization of emotion/motive (angrily, desperately, callously)?** → **Class C**
8. **Joke, sarcasm, insult, or conversational rhetoric?** → **Class C**
9. **Paraphrase without attribution or speculation (may, could, might)?** → **Class C**
10. **Historical comparison without specific data (unprecedented, worst ever)?** → **Class C unless comparative data provided**

**If none of the above:** → **Class C (Safe-Fail Default)**

**MANDATORY PHASE 2 GATE - Classification Verification:**
After completing classification of ALL statements, you MUST perform this check:
- Count your Class A items. For EACH one, verify: (1) Is this an EVENT (physical action) or DATA (specific number)? (2) Does the article provide a SOURCE CITATION allowing independent verification? If answer to either is no, you misclassified it. Reclassify as Class B4 or Class C.
- Count your Class B items. For EACH one, verify: Is this a SPEECH ACT (someone said/stated this) OR a journalist's assertion of fact without citation (B4)? If it's an editorial paraphrase of what someone might think, you misclassified it. Reclassify as Class C.
- Count your Class C items. If Class C is less than 30% of total statements, you are under-filtering. Re-read Axiom 2 and scan for emotional adjectives, unproven causation, and anonymous claims you missed.

**GATE CHECKPOINT DECISION TREE:**
1. **If Class C is less than 20% of total statements:**
   - **GATE STATUS: FAILED**
   - **Required Action:** Return to Step 2.1 and re-apply framing removal protocols. Most articles contain substantial emotional language and editorial framing—if you're not finding it, you're not looking hard enough.
   - **Do not proceed to Phase 3 until Class C ≥ 20%.**

2. **If Class C is 20-30% of total statements:**
   - **GATE STATUS: CONDITIONAL PASS - WARNING ISSUED**
   - **Required Action:** Document in gate checkpoint: "Low framing density detected. Verified this is not under-filtering by spot-checking [X] statements for missed emotional language."
   - **Proceed with caution to Phase 3.**

3. **If Class C is >30% of total statements:**
   - **GATE STATUS: CLEARED**
   - **State explicitly:** "Phase 2 Gate Cleared: Class A items verified as cited events/data. Class B items verified as speech acts or journalist assertions (B4). Class C filtering applied per Axiom 2."
   - **Proceed to Phase 3.**

**CRITICAL SAFE-FAIL PRINCIPLE:**
When classification is uncertain, default to Class C. The framework is designed to be CONSERVATIVE—it is better to exclude a potentially valid claim than to promote an unverified claim to evidentiary status. This is not a flaw; it is a feature. Ambiguous statements belong in Class C until the article provides sufficient clarity for promotion to Class A/B.

**Why Safe-Fail Matters:**
- **Class A/B = High Confidence Zone:** Only includes claims where evidence standard is met
- **Class C = Quarantine Zone:** Holds everything that doesn't meet the bar, including ambiguous cases
- **User Judgment:** The human reader can review Class C items in the Admissibility Log and decide if they want to consider them despite lack of verification

**EXPANDED DECISION TREE (For Complex Cases):**

| **Question** | **Yes → Go To** | **No → Go To** |
|--------------|-----------------|----------------|
| 1. Is this a physical action with a timestamp/location? (e.g., "X signed Y," "Z arrested on [date]") | **Question 1a** | Question 2 |
| 1a. Does the article provide source citation allowing independent verification? | **Class A (Verified)** | **Class B4 (Journalist Assertion)** |
| 2. Is this a formal policy statement or official declaration? (e.g., "The White House announced," "The court ruled") | **Class B** | Question 3 |
| 3. Is this verifiable data with a specific value? (e.g., "Unemployment rate = 5.3%," "Budget = $2.1T") | **Question 3a** | Question 3b |
| 3a. Does the article provide source citation for this data? | **Class A (Verified)** | **Class B4 (Journalist Assertion)** |
| 3b. Is this an *interpretation* of data using subjective language? (e.g., "Unemployment soared," "The economy collapsed") | **Class C (Discard interpretation)** - Extract the raw number only | Question 4 |
| 4. Is this a direct quote of someone speaking? | **Class B** (the speech act) | Question 5 |
| 5. Is this a joke, insult, sarcasm, or conversational rhetoric? | **Class C (Discard)** | Question 6 |
| 6. Is this a paraphrase without source citation? | **Class C (Discard)** | Question 7 |
| 7. Is this an editorial judgment about motive/emotion/intent? | **Class C (Discard)** | Question 8 |
| 8. Is this a historical comparison or precedent claim? (e.g., "unprecedented," "mirrors 2008 crisis") | **Class C (Unverified comparison)** - Flag for fact-checking | **Default: Class C** |

**Step 2.3: Statistical Manipulation Detection Protocol**

**Objective:** Identify common techniques used to distort quantitative evidence while preserving the legitimate use of statistics.

**Statistical Red Flags - The LLM Must Check:**

1. **Base Rate Neglect / Cherry-Picked Percentages:**
   - *Example:* "Crime increased 50% in the city!"
   - **Mandatory Checks:**
     - What is the baseline? (50% increase from 2 incidents to 3 is different from 200 to 300)
     - What is the timeframe? (Yearly vs. monthly vs. daily rates)
     - Is the percentage increase contextualized with absolute numbers?
   - **Extraction Rule:**
     - *Stated Claim:* "Crime increased 50%."
     - *Required Data for Class A:* Baseline number, final number, timeframe, geographic scope.
     - If any are missing, flag as **"Incomplete Statistical Claim"** and mark as Class C.

2. **Timeframe Manipulation:**
   - *Example:* "Unemployment is at a 10-year high!" (But the article doesn't mention it was at a 20-year low last year.)
   - **Mandatory Checks:**
     - Is the chosen timeframe selective or representative?
     - Does the article provide the full trend line, or only a cherry-picked range?
   - **Extraction Rule:**
     - If a superlative claim ("highest," "lowest," "worst," "best") is made with a specific timeframe, note the timeframe in Evidence Locker.
     - Flag for Delta Analysis: "Timeframe Selectivity - Insufficient Context for Trend Assessment."

3. **Percentage vs. Absolute Number Confusion:**
   - *Example:* "90% of experts agree!" (But there were only 10 experts surveyed.)
   - **Mandatory Checks:**
     - If a percentage is cited, is the sample size provided?
     - If not, mark as Class C.
   - **Extraction Rule:**
     - *Stated:* "90% of economists support the policy."
     - *Class A Requirement:* Sample size (e.g., "45 out of 50 economists surveyed").
     - If missing, flag as **"Percentage Without Population Context"** → Class C.

4. **Misleading Averages (Mean vs. Median vs. Mode):**
   - *Example:* "The average salary increased by $10,000." (But if one CEO's salary went up $1M and 99 workers got $100 raises, the "average" is misleading.)
   - **Mandatory Checks:**
     - Is the type of average specified (mean, median)?
     - Is there context about distribution (e.g., "median household income" is more robust than "average income" in skewed distributions)?
   - **Extraction Rule:**
     - If "average" is used without specifying mean/median, flag as **"Ambiguous Statistical Measure."**
     - If distribution context is missing for income/wealth data, note in Omission Log.

5. **Correlation as Causation (Beyond Temporal):**
   - *Example:* "States with higher taxes have better schools." (Implies taxes cause better schools, ignores other variables.)
   - **Extraction Rule:**
     - If a correlation is stated, check: Does the article provide Class A evidence of mechanism?
     - If not, flag as **"Correlation Without Mechanism"** → Class C for causation claim, but preserve the correlation itself if sourced.

6. **Comparative Claims Without Context:**
   - *Example:* "This is the most expensive bill in history!"
   - **Mandatory Checks:**
     - Is this adjusted for inflation?
     - Is it compared to GDP or as a raw number?
     - Is "history" defined (since founding, since 2000, etc.)?
   - **Extraction Rule:**
     - If adjustment method is not stated, flag as **"Unadjusted Comparative Claim"** → Class C.

**Output of Step 2.3:**
- All statistical claims are categorized as either:
  - **Class A** (if complete and sourced with baseline, timeframe, sample size, etc.)
  - **Class C** (if incomplete, cherry-picked, or missing context)
- Flag any statistical manipulation techniques detected for inclusion in Delta Analysis.

**Step 2.3.1: Visual Data Manipulation Detection Protocol**

**Objective:** Identify manipulation in charts, graphs, and infographics when the LLM has direct visual access OR when article text reveals visual manipulation tactics.

**CRITICAL LIMITATION NOTICE:**
This protocol can only be fully applied when:
1. The LLM has direct visual access to images/charts embedded in the article, OR
2. The article's text explicitly describes visual elements (e.g., "The chart shows a dramatic spike from 98 to 100")

**If you do not have visual access and the article does not describe visual elements in detail, skip this protocol and note in the final report: "Visual content present but not accessible for analysis."**

**Visual Manipulation Techniques - Apply When Detectable:**

1. **Unlabeled or Missing Data Points (Text-Detectable):**
   - *Description:* Chart omits axis labels, units, or data sources, making verification impossible.
   - **Detection Rule:**
     - If article describes or references a chart/graph, check article text for: axis labels, units, data sources.
     - If article says "as shown in the chart" but provides no data values, labels, or source → Mark chart as **Class C - "Unverifiable Visual Data, No Descriptive Information."**
     - If article provides data labels/values in text, treat those as potential Class A claims (apply standard verification rules).

2. **Selective Data Windowing (Text-Detectable):**
   - *Description:* Graph shows only a selected time period that supports narrative while omitting contradictory trends.
   - **Detection Rule:**
     - If article text reveals chart timeframe (e.g., "Chart shows unemployment from January to June") and you've already flagged cherry-picked timeframes in Step 2.3, note visual reinforcement: **"Selective Temporal Window Reinforced by Chart."**

3. **Narrative-Visual Conflict (Text-Detectable):**
   - *Description:* Article describes chart as showing "dramatic" or "sharp" change, but the data points mentioned suggest moderate change.
   - **Detection Rule:**
     - If article says "Chart shows dramatic spike in crime" and also mentions "from 100 to 105 incidents," calculate actual change.
     - If emotional description ("dramatic," "catastrophic," "explosive") doesn't match magnitude (5% change), flag as **"Narrative-Visual Inflation - Moderate Change Described as Dramatic."**

4. **Visual Manipulation When LLM Has Image Access:**
   - If you can directly view charts/graphs, apply full protocol:
     - **Y-Axis Truncation:** Check if Y-axis starts at zero (exception: scientific scales where zero is meaningless).
     - **Aspect Ratio Manipulation:** Check if chart dimensions exaggerate or minimize visual slopes.
     - **3D Distortion:** Note if 3D effects distort size perception.
     - **Color-Coding Bias:** Check for emotionally loaded color choices (red/green for political parties, etc.).
     - **Dual-Axis Deception:** If two Y-axes present, verify scale relationship isn't manufacturing false correlation.

**Output of Step 2.3.1:**
- **If Visual Access Available:**
  - Classify each visual as Class A (Evidentiary) or Class C (Framing/Manipulated)
  - Document manipulation techniques detected → VDMI score
- **If No Visual Access:**
  - Note: "Visual content not accessible for analysis"
  - Apply text-detectable rules (unlabeled data, narrative-visual conflicts)
  - VDMI = Not Applicable (N/A)

**Step 2.3.2: Multi-Source Conflict Resolution Protocol**

**Objective:** When article cites multiple sources making conflicting claims, identify discrepancies and assess which (if any) should be elevated to Evidence Locker.

**Instructions:**

1. **Identify Conflicting Source Claims:**
   - As you build the Statement Registry (Phase 1), flag any instance where Source A's claim contradicts Source B's claim.
   - *Example:*
     - Source A (Government agency): "Unemployment is 5.2%"
     - Source B (Opposition economist): "True unemployment is 9.1% when underemployment is included"

2. **Conflict Classification:**
   - **Definitional Conflict:** Sources use different definitions or methodologies, both may be correct within their frameworks.
     - *Example:* U3 unemployment (official) vs. U6 unemployment (includes underemployed). Both are valid but measure different things.
     - **Resolution:** Include both in Evidence Locker as **Class A (with definition noted):** "Official unemployment (U3) = 5.2%. Broader unemployment (U6) = 9.1%."
     - Mark as **"Methodological Difference - Not Contradictory."**

   - **Factual Conflict:** Sources provide incompatible factual claims about same measured reality using same methodology.
     - *Example:*
       - Source A: "The bill passed 60-40"
       - Source B: "The bill passed 58-42"
     - **Resolution:** If article does not provide tie-breaker (official vote record), mark both as **Class B3 - "Conflicting Claims, Verification Required."**
     - Flag in Delta Analysis: **"Unresolved Factual Conflict Between Sources."**
     - *Do not choose which source to believe.* Note the conflict and the absence of Class A verification.

   - **Interpretive Conflict:** Sources agree on facts but disagree on meaning/implications.
     - *Example:*
       - Fact (agreed): "Spending increased by $500B"
       - Source A interpretation: "Reckless overspending"
       - Source B interpretation: "Necessary investment"
     - **Resolution:** The fact ($500B increase) is Class A (if sourced). The interpretations are both Class C. Include the fact, discard both interpretations.

3. **Source Reliability Hierarchy for Conflict Resolution:**
   - When conflict exists, prioritize based on source proximity to ground truth:
     - **Highest Priority:** Official records, primary documents, raw data (Class A if verifiable)
     - **Second Priority:** Named officials in formal capacity (Class B1)
     - **Third Priority:** Named experts with disclosed credentials (Class B2)
     - **Lowest Priority:** Anonymous sources, pundits, editorialists (Class B3/C)
   - *Example:* If official vote tally (Class A) conflicts with politician's claim (Class B1), the official record takes precedence.

4. **The "According to [X]" Disambiguation Protocol:**
   - Articles often present conflicting claims without resolving them: "According to Republicans, the bill will help. According to Democrats, it will hurt."
   - **Extraction Rule:**
     - *Class B Facts:* Republicans made claim X. Democrats made claim Y. (The speech acts are facts)
     - *Class C Claims:* The bill will help/hurt. (Predictive claims, unverifiable at time of publication)
   - Note in Evidence Locker: **"Partisan Claim Conflict - Both Are Speech Acts (Class B), Neither Prediction Is Class A Evidence."**

**Output of Step 2.3.2:**
- Conflicts documented in Delta Analysis
- When resolvable via Class A evidence, resolution noted
- When unresolvable, both claims flagged with **"Unresolved Multi-Source Conflict"** label

**Step 2.4: Internal Contradiction Detection**

**Objective:** Identify when an article contains mutually contradictory claims—often a sign of narrative prioritization over factual consistency.

**Instructions:**

1. **Cross-Reference All Class A/B Claims:**
   - Compare each claim in the Evidence Locker against all others.
   - Check for logical contradictions or inconsistencies.

2. **Common Contradiction Patterns:**
   - **Temporal Contradictions:** "The policy was enacted in March" vs. "The policy has been in effect for six months" (article dated August).
   - **Quantitative Contradictions:** "Unemployment rose 3%" in paragraph 2 vs. "Unemployment rose 5%" in paragraph 7.
   - **Attribution Contradictions:** "The mayor announced the decision" vs. "The city council voted on the decision" without clarifying relationship.

3. **Extraction Rule:**
   - If contradictions exist between Class A/B facts, flag in a **"Contradiction Log"** within Delta Analysis.
   - *Format:* "Claim X (Para 2) states [fact]. Claim Y (Para 8) states [contradictory fact]. Resolution: [Cannot be reconciled / Requires clarification]."
   - **Do not resolve the contradiction** by choosing which claim to believe. Simply document it.

4. **Narrative Implications:**
   - Multiple contradictions (3+) suggest poor editorial quality or intentional obfuscation.
   - Note in Delta Analysis: **"Internal consistency failure detected. Article reliability compromised."**

**Output of Phase 2:**
- A filtered list of statements, each labeled as Class A, B, or C.
- All Class C items are moved to the "Admissibility Log" for documentation but excluded from analysis.
- Statistical manipulation flags are documented for Delta Analysis.
- Chain-of-custody verification status noted for all Class B factual claims.
- Implicit premises extracted and documented.
- Contradictions logged if detected.

**MANDATORY PHASE 2 COMPLETION GATE:**
Before proceeding to Phase 3, the LLM MUST output this exact statement with filled values:

"PHASE 2 GATE CHECKPOINT:
- Total statements processed: [X]
- Class A (Kinetic): [Y] statements ([Z]% of total) - [N] Verified, [M] Asserted
- Class B (Procedural): [Y] statements ([Z]% of total)
- Class C (Performative): [Y] statements ([Z]% of total)
- Axiom 2 Verification: Class C items contain only framing/speculation/rhetoric (no misclassified events or speech acts)
- Axiom 3 Verification: Evidence hierarchy maintained (A > B > C)
- Gate Status: CLEARED - Proceeding to Phase 3"

**If Class C is less than 20% of total statements, the gate is FAILED. You have under-filtered. Return to Step 2.1 and re-apply framing removal protocols. Most articles contain substantial emotional language and editorial framing—if you're not finding it, you're not looking hard enough.**

### Phase 3: The Narrative Audit (Claim vs. Evidence Reconciliation)

**Objective:** Compare the article's thesis (Narrative Pitch) against the Material Evidence Locker to identify gaps, exaggerations, or unsupported leaps.

**Step 3.1: Extract the Narrative Pitch**

1. **Identify the Central Thesis:**
   - Retrieve the headline extracted in Phase 1, Step 1b
   - What is the opening paragraph's primary assertion?
   - What is the closing paragraph's call-to-action or summary?
   - *Synthesize these into one sentence:* "This article wants you to believe that [X] because [Y]."

2. **MANDATORY GATE: Headline-Body Consistency Check (Phase 2 Integration):**
   - **CRITICAL RULE:** This check must occur immediately after Phase 2 classification, not deferred to Phase 3.
   - **Procedure:**
     - Decompose the headline into sub-claims (same process as Step 3.3 but applied to headline only).
     - For EACH headline sub-claim, check Evidence Locker: Is there Class A/B support?
     - **Classification Rules:**
       - **Headline Supported:** All headline sub-claims have Class A/B evidence in body.
       - **Headline Inflation (Mild):** Headline uses stronger emotional language than body evidence supports, but core facts are present.
         - *Example:* Headline: "Senator Slammed for Vote." Body: Contains Class A evidence of vote + Class B quotes criticizing it. ("Slammed" is emotional loading but criticism is evidenced)
       - **Headline Inflation (Severe):** Headline makes factual claim that body does not support with Class A/B evidence.
         - *Example:* Headline: "Mayor Arrested for Fraud." Body: "Mayor under investigation for alleged fraud." (Arrest = Class A claim, Investigation = Class B claim. Not equivalent.)
       - **Headline Contradiction:** Headline claim directly contradicts body evidence.
         - *Example:* Headline: "Bill Passes Senate." Body: "Senate voted down the bill 60-40."
   - **Action:** If Severe Inflation or Contradiction detected, flag immediately as **Critical Structural Failure** and note in Narrative Integrity Score (NIS) assessment.
   - **Rationale:** Headlines are the highest-impact framing element. Many readers only see headlines. Detecting inflation at Phase 2 prevents LLM from wasting Phase 3 effort reconciling unsupported narratives.

3. **Identify the Desired Emotional Response:**
   - Does the article use language designed to provoke fear, anger, hope, outrage, or satisfaction?
   - *Example:* "Readers should feel alarmed about the threat to democracy."

**Step 3.2: Build the Evidence Locker**

- Compile **only** the Class A and Class B items from Phase 2.
- List them in chronological or logical order.
- Each item must include:
  - The fact itself (stripped of framing)
  - The source paragraph/citation
  - The classification (A or B)
  - For quotes (Class B): Include sufficient context

**Step 3.2.1: Quote Context Verification Protocol**

**Objective:** Detect "quote mining"—the practice of extracting quotes out of context to reverse or distort their meaning.

**Instructions:**
1. For each direct quote (Class B) in the Evidence Locker, check:
   - Does the article provide the sentence immediately before and after the quote?
   - Does the quote appear to be part of a longer answer or statement?
   - Is there a transcript, video, or recording linked for verification?

2. **Context Adequacy Assessment:**
   - **Adequate:** Full context provided or linked. Quote meaning is clear and not dependent on missing information.
   - **Questionable:** Quote is presented without surrounding context, making it impossible to verify intent.
   - **Suspicious Indicators:**
     - Use of ellipses (...) without explanation of what was omitted
     - Quote begins mid-sentence without [brackets] indicating modification
     - No link to source material for verification

3. **Extraction Rule:**
   - If a quote lacks adequate context and no source link is provided, note in Evidence Locker: **"Class B - Context Unverifiable."**
   - In Delta Analysis, flag as potential quote mining if the quote is central to the Narrative Pitch.

**Example:**
- *Stated Quote:* Senator X said, "...support the bill..."
- *Problem:* Ellipses indicate omitted content. Could be:
  - "I cannot support the bill in its current form."
  - "I will support the bill."
- *Action:* Flag as **"Quote Fragment - Context Required for Interpretation."** Mark as Class B but note verification limitation.

**Step 3.2.2: Counterfactual Evidence Test (Falsifiability Check)**

**Objective:** Determine whether the article addresses evidence that would contradict or weaken the Narrative Pitch. A robust article presents counterfactual evidence and addresses it; a weaponized narrative ignores it entirely.

**Instructions:**

1. **Identify the Counterfactual Question:**
   - For the Narrative Pitch identified in Step 3.1, ask: "What evidence would *disprove* or *weaken* this claim?"
   - *Example:*
     - Narrative Pitch: "Policy X has failed and harmed the economy."
     - Counterfactual Evidence: Economic indicators showing improvement after Policy X; expert analysis showing positive outcomes; beneficiary testimonials.

2. **Scan Article for Counterfactual Engagement:**
   - Does the article mention opposing viewpoints or contradictory data?
   - **Strong Counterfactual Engagement:**
     - Article presents contrary evidence with Class A/B sourcing and explains why author's conclusion differs.
     - *Example:* "While GDP increased 2%, unemployment rose 1%, indicating mixed results."
   - **Weak Counterfactual Engagement:**
     - Article briefly mentions opposition but dismisses it without Class A/B counter-evidence.
     - *Example:* "Supporters claim the policy worked, but critics disagree." (No data presented for either side)
   - **Absent Counterfactual Engagement:**
     - Article presents only evidence supporting Narrative Pitch, ignoring contradictory evidence entirely.

3. **Falsifiability Assessment:**
   - A strong claim is falsifiable—there exists evidence that could prove it wrong.
   - A weaponized narrative often uses unfalsifiable framing.
   - **Falsifiable:** "Unemployment rose 2% after policy implementation." (Can be disproven with employment data)
   - **Unfalsifiable:** "The policy betrayed American values." (Subjective, no data can disprove)
   - **Extraction Rule:**
     - For each major sub-claim in Narrative Pitch, ask: "Is this falsifiable? If so, does the article acknowledge what would disprove it?"
     - If unfalsifiable claims dominate the Narrative Pitch, flag as **"Unfalsifiable Framing - Immune to Evidence-Based Rebuttal."**

4. **The Steelman Test:**
   - Does the article present the *strongest* version of the opposing argument, or does it strawman?
   - **Strawman Pattern:** "Opponents foolishly claim the policy is perfect and has no flaws."
   - **Steelman Pattern:** "Proponents cite GDP growth of 3% and job creation in manufacturing sector as evidence of success. However, these gains are offset by..."
   - **Extraction Rule:**
     - If article only presents weak or caricatured versions of opposing view, flag as **"Strawman Argumentation - Opposition Position Not Represented Fairly."**

**Output of Step 3.2.2:**
- Counterfactual Engagement Assessment: Strong / Weak / Absent
- List of counterfactual evidence that article fails to address (if applicable)
- Falsifiability assessment for main claims
- Steelman vs. Strawman determination

**Step 3.3: The Delta Analysis (Logic Reconciliation)**

This is the critical comparison. For the LLM, follow this protocol:

**AXIOM 1 ENFORCEMENT - Truth = Events + Speech Acts:**

1. **Decompose the Narrative Pitch into Sub-Claims:**
   - *Example Pitch:* "Senator X's reckless policy destroyed the economy."
   - *Sub-Claims:*
     - Claim 1: Senator X enacted a policy.
     - Claim 2: The policy had economic effects.
     - Claim 3: The effects were negative.
     - Claim 4: The causation is direct (policy → harm).
     - Claim 5 (Implicit): The action was "reckless" (moral judgment).

2. **Match Each Sub-Claim to Evidence (Axiom 1 Application):**
   - For each sub-claim, search the Evidence Locker for supporting Class A or B facts.
   - **Supported:** A Class A event/data point OR Class B speech act directly validates this claim.
     - *Test:* Can you point to a physical action, a number, or a direct quote that proves this?
   - **Unsupported:** No Class A/B fact exists in Evidence Locker.
     - *Test:* The claim is based on editorial interpretation, anonymous sources, or speculation.
   - **Partially Supported:** A Class A/B fact validates part of the claim but not the whole.
     - *Example:* Claim: "Policy destroyed economy." Evidence: "GDP decreased 0.5%." (Partial support - decline is evidenced, "destroyed" is not proportionate to 0.5%.)

   **CRITICAL RULE - Axiom 1 Litmus Test:**
   Before marking any claim as "Supported," ask: "Does the Evidence Locker contain an EVENT (Class A action/data) or SPEECH ACT (Class B quote/statement) that directly corresponds to this claim?" If you're supporting a claim with inference, correlation, or paraphrase, you are violating Axiom 1.

3. **Identify Inferential Leaps:**
   - Does the Narrative Pitch assert causation that is not proven by the Evidence Locker?
   - *Example:* "The policy caused unemployment to rise."
   - *Evidence Locker Check:* Is there Class A data showing unemployment rise? Is there Class A evidence of causation (not just correlation)?
   - *Verdict:* If causation is assumed but not proven, flag as "Inferential Leap."

3b. **Synthetic Narrative Detection (Claim Aggregation Analysis):**
   - **Objective:** Detect when multiple individually unsupported claims are combined to create false sense of certainty.
   - **Pattern Recognition:**
     - Article makes 5+ claims about a topic (e.g., "Policy X is dangerous").
     - Each claim is Class C (unsupported) individually.
     - The sheer volume of claims creates impression of consensus or established fact.
   - *Example:*
     - "Critics slam the policy" (Class C - anonymous)
     - "Experts warn of consequences" (Class C - anonymous)
     - "Analysts predict failure" (Class C - anonymous)
     - "Observers note concerns" (Class C - anonymous)
   - **Extraction Rule:**
     - Count the number of similar unsupported claims (Class C) that point toward the same conclusion.
     - If 4+ similar Class C claims exist without Class A/B support, flag as **"Synthetic Certainty via Claim Aggregation."**
     - **Verdict:** "Article creates impression of widespread agreement through repetition of unverified assertions. No Class A/B evidence supports core claim."
   - **Report in Delta Analysis:** Include Synthetic Narrative Flag count.

3c. **Implicit Premise Reconciliation:**
   - For each implicit premise identified in Step 2.1.7, check:
     - Does the article provide Class A/B evidence to support the unstated assumption?
     - *Example:* If article implies "officials shouldn't own multiple homes," does it cite ethics codes, legal standards, or precedent?
   - **Extraction Rule:**
     - *Unsupported Implicit Premise:* Narrative relies on reader already agreeing with value judgment.
     - List all unsupported implicit premises in Delta Analysis under **"Unargued Assumptions Required for Narrative Acceptance."**

4. **Quantify the Gap (Mandatory Scoring):**
   - Calculate the ratio of supported vs. unsupported sub-claims.
   - *Example:* "3 out of 5 sub-claims are supported by Class A/B evidence. 2 are unsupported."

   **Required Metrics (Consolidated):**

   **TIER 1 - PRIMARY METRICS (Always Calculate):**
   - **Evidentiary Support Ratio (ESR):** (Supported Sub-Claims / Total Sub-Claims) × 100
     - *Example:* 3/5 = 60% ESR
     - *Interpretation:* ESR < 50% = Low Support, 50-75% = Moderate Support, >75% = High Support
   - **Narrative Integrity Score (NIS):** Weight by claim centrality.
     - If the *central thesis* is unsupported but peripheral details are supported, note: "Core claim unsupported despite high ESR."
     - *Format:* "Supported" / "Partially Supported" / "Unsupported"
   - **Class C Density Ratio (CDR):** (Class C Items / Total Statements) × 100
     - A CDR above 60% indicates high framing-to-fact ratio (propaganda threshold).
   - **Citation Deficit Index (CDI):** (Class B4 Items / [Class A + Class B4]) × 100
     - *Numerator:* Count of Class B4 items (Journalist assertions of fact WITHOUT source citation)
     - *Denominator:* Count of Class A items (Verified facts WITH citation) + Count of Class B4 items
     - **CRITICAL: Do NOT include Class B1, B2, or B3 in this calculation.** Those are speech acts (quotes/statements)—we are measuring the reliability of the *journalist's factual assertions*, not the sources' opinions.
     - *Interpretation:* CDI measures what percentage of the journalist's factual claims lack source citations.
       - CDI ≤ 25% = Strong citation practices (most facts are verified)
       - CDI 26-50% = Moderate citation gap (some unverified assertions)
       - CDI > 50% = Severe citation deficit (journalist makes claims without proof, violates Axiom 1)
     - *Replaces:* VDR (Verification Deficit Ratio) - eliminated due to removal of "Class A-Asserted" category

   **TIER 2 - MANIPULATION DETECTION (Calculate When Detected):**
   - **Consolidated Verification Issues (CVI):** Combines CDC + CCF + ACD
     - *Formula:* Sum of: Quote context deficiencies + Chain-of-custody failures + Multi-hop attribution chains (≥2 hops)
     - *Rationale:* All three measure verification quality degradation. Single metric reduces reporting bloat.
     - *Interpretation:* CVI = 0 (No issues), 1-3 (Low), 4-7 (Moderate), 8+ (High)
   - **Statistical Manipulation Index (SMI):** Count of manipulation techniques detected
     - 0 = None, 1-2 = Low, 3-4 = Moderate, 5+ = High
   - **Temporal Fallacy Count (TFC):** Causation claims based solely on temporal sequence without mechanism evidence.
   - **Synthetic Narrative Flag (SNF):** Count of topic areas where 4+ similar unsupported claims create false certainty.
   - **Contradiction Index (CI):** Internal contradictions between Class A/B statements.
   - **Implicit Premise Count (IPC):** Unstated assumptions required for narrative impact without Class A/B support.
   - **Visual Data Manipulation Index (VDMI):** Count of visual manipulation techniques (if visual access available)
     - 0 = None, 1-2 = Low, 3-4 = Moderate, 5+ = High
     - If no visual access: Mark as "N/A - Visual Content Not Accessible"
   - **Multi-Source Conflict Count (MSCC):** Unresolved factual conflicts between cited sources.
   - **Semantic Drift Count (SDC):** Count of term substitution progressions without evidentiary basis.

   **TIER 3 - QUALITATIVE ASSESSMENTS:**
   - **Counterfactual Engagement Score (CES):** Strong / Weak / Absent
     - Strong = Article presents and engages with contrary evidence using Class A/B sourcing
     - Weak = Article mentions opposition without data
     - Absent = Article ignores contradictory evidence entirely

**Output of Phase 3:**
- A completed Delta Analysis section showing which parts of the Narrative Pitch are grounded in material evidence and which are not.
- **Tier 1 Primary Metrics (Always Report):** ESR, NIS, CDR, VDR
- **Tier 2 Manipulation Metrics (Report When Detected):** CVI, SMI, TFC, SNF, CI, IPC, VDMI, MSCC, SDC
- **Tier 3 Qualitative (Always Report):** CES
- Synthetic narrative detection results documented.
- Implicit premises identified and evaluated.
- Contradiction log (if applicable).
- Counterfactual engagement assessment.
- Multi-source conflict documentation.
- Semantic drift analysis (if detected).

**MANDATORY PHASE 3 GATE - Logic Reconciliation Verification:**

Before finalizing your audit, the LLM MUST perform this three-part verification:

**GATE 3A - Axiom 1 Compliance Check:**
- For each claim marked "Supported" in Delta Analysis, verify: Can you cite the specific Class A event/data OR Class B speech act from the Evidence Locker that supports it?
- If you supported a claim using inference, interpretation, or "common sense," you violated Axiom 1. Reclassify as "Unsupported" or "Partially Supported."

**GATE 3B - Axiom 2 Compliance Check:**
- For each claim in the Narrative Pitch, verify: Have you stripped all emotional adjectives and framing language?
- *Example:* If your sub-claim includes words like "reckless," "shocking," "devastating," you failed to remove framing. Rewrite the sub-claim in neutral language.

**GATE 3C - Axiom 3 Compliance Check:**
- Verify your ESR calculation: (Supported Claims / Total Sub-Claims) × 100
- Verify your NIS determination: Is the CORE THESIS supported by Class A/B evidence, or only peripheral details?
- **ESR-NIS PARADOX DETECTION (CRITICAL - DO NOT FLAG AS ERROR):**
  - If ESR > 75% but NIS = "Unsupported," this is NOT necessarily a decomposition flaw.
  - **This is a classic propaganda pattern:** Article provides strong evidence for trivial/peripheral claims while burying lack of support for central thesis.
  - *Example:* Article claims "Senator's corruption destroyed economy" (unsupported core thesis) but includes 20 verified peripheral facts about senator's schedule, office budget, voting record on unrelated bills.
  - **Required Action:** FLAG THIS PATTERN in Delta Analysis as **"ESR-NIS Paradox: High peripheral evidentiary density masks unsupported central claim. Possible obfuscation technique."**
  - **Do NOT reclassify or re-decompose.** Report the paradox as a structural finding.

**GATE CHECKPOINT STATEMENT:**
"PHASE 3 GATE CLEARED:
- Axiom 1 verified: All 'Supported' claims have corresponding Class A/B evidence
- Axiom 2 verified: All sub-claims stripped of emotional framing
- Axiom 3 verified: Evidence hierarchy applied to ESR/NIS calculations
- Gate Status: CLEARED - Proceeding to final output generation"

---

### Phase 3.5: Recursive Self-Audit Protocol

**Objective:** Apply the framework's logic to the LLM's own audit output to prevent bias injection and metric gaming.

**What is Metric Gaming?**
Metric gaming occurs when an LLM outputs numerical scores (ESR, CDR, etc.) without performing the underlying classification work. The framework detects this by requiring TRACEABLE EVIDENCE for every metric. If you cannot show your work, the metric is rejected.

**Critical Requirement:** Before finalizing the report, the LLM must audit its own Delta Analysis using these checks:

1. **Classification Consistency Check:**
   - Review each Class C designation. Ask: "Did I classify this as Class C because of lack of evidence, or because I disagree with its content?"
   - **Self-Audit Question:** "If this same statement appeared in an article supporting the opposite view, would I still classify it as Class C?"
   - If answer is "no," reclassify the statement.

2. **Evidence Symmetry Check:**
   - Review Class A/B items in Evidence Locker. Ask: "Did I apply the same verification standards to all claims, regardless of whether they support or contradict the Narrative Pitch?"
   - **Self-Audit Question:** "Are there claims I accepted as Class A without source verification? Are there claims I rejected that had equal sourcing to accepted claims?"

3. **Framing Detection in Own Language:**
   - Review the Delta Analysis text for emotional language, adjectives, or implicit judgments.
   - *Example of bias injection:* "The article deceptively omits..." (judgmental) vs. "The article omits..." (neutral)
   - **Extraction Rule:** Remove all evaluative language from the audit report itself. State only what is present/absent, supported/unsupported.

4. **Implicit Premise Detection in Own Analysis:**
   - Ask: "Are there unstated assumptions in my Delta Analysis?"
   - *Example of LLM bias:* "The article fails to provide context" (assumes context is obligatory) vs. "The article does not provide context" (neutral observation)

5. **Mandatory Self-Correction Statement:**
   - After completing the self-audit, the LLM must include this statement in the final output:
   - **"Self-Audit Verification: I have reviewed my classifications and analysis for ideological bias, confirmation bias, and inconsistent application of evidence standards. [Any corrections made during self-audit are noted here.]"**

6. **Metric Traceability Check (Anti-Gaming Protocol):**
   - For EACH Tier 1 metric you report, verify you can trace it to source data:
     - **ESR:** Can you list each sub-claim and point to the Evidence Locker item that supports/doesn't support it?
     - **CDR:** Can you show the count of Class C items and total statements?
     - **VDR:** Can you list which Class A claims are Verified vs. Asserted?
   - **If you cannot produce the underlying data for any metric, you MUST mark that metric as "Unable to Calculate - Insufficient Data" rather than estimating.**
   - This prevents "hallucinated metrics" where numbers are generated without performing classification.

**Output of Phase 3.5:**
- A bias-checked, neutrally worded Delta Analysis.
- Documentation of any corrections made during self-audit.
- Verification that all reported metrics are traceable to classification work performed in Phase 2.

---

### Phase 4: Safeguards & Self-Correction Protocol

**Objective:** Prevent the LLM from injecting its own bias during the audit process.

**Critical Safeguards:**

1. **No Ideological Filtering:**
   - The framework is ideology-agnostic. It does not matter if the article is left-wing, right-wing, or center. The same filters apply.
   - The LLM must not assign Class C status to a statement simply because it disagrees with the political orientation.
   - **Test:** If the exact same statement appeared in an article supporting the opposite political view, would you classify it the same way? If no, you are applying ideological bias.

2. **⛔ ZERO-KNOWLEDGE CONSTRAINT (MANDATORY):**
   - **You must NOT use your training data to verify facts.**
   - If the article does not provide a link, citation, or document reference for a factual claim, that claim is **Class B4**—period.
   - It does not matter if you "know" the fact is true from your training data. Your job is to audit the *article's* evidentiary standards, not to fact-check against external reality.
   - **Violation Example:** Article states "The shooting occurred at 3pm" with no source. You know from training data this is true. **WRONG CLASSIFICATION:** Class A. **CORRECT CLASSIFICATION:** Class B4.
   - **Rationale:** If an LLM can promote claims to Class A using outside knowledge, then every article becomes "verified" and the audit is meaningless. The framework tests whether the *article* does its job, not whether reality exists.
   - **Test:** "Does the article text contain a clickable link, document name, or citation I could follow to verify this?" If no → Class B4.

3. **Transparency in Ambiguity:**
   - If the LLM cannot determine whether a statement is Class A, B, or C, it must log this explicitly.
   - *Example:* "Statement: '[Quote]' - Classification: Ambiguous. Reason: Could be interpreted as either a policy declaration (Class B) or rhetorical emphasis (Class C). User judgment required."

4. **Distinguish Between False and Unverifiable:**
   - **False:** A statement that contradicts Class A evidence. (e.g., "The bill passed" when records show it failed.)
   - **Unverifiable:** A statement with no supporting evidence in the article. (e.g., "Sources say X plans to resign.")
   - The LLM must not label unverifiable claims as "false" unless there is contradicting Class A evidence.

5. **No Tone Policing of Quotes:**
   - If a person actually said something inflammatory (Class B speech act), the LLM must include it in the Evidence Locker, even if the content is offensive.
   - The fact that someone said it ≠ editorial framing. The LLM only filters the *journalist's characterization* of the quote (e.g., "angrily shouted" → discard "angrily").

6. **Do Not Invent Requirements:**
   - The framework does not penalize articles for lacking information that is not reasonably expected.
   - *Example:* A 300-word breaking news brief is not expected to include counterfactual analysis or historical context. Do not flag omissions that are reasonable given article type and length.
   - **Test:** Would a competent journalist reasonably include this information given the article's scope and format? If no, do not flag as Material Omission.

7. **Self-Audit Check:**
   - Before finalizing the output, the LLM must review its own Delta Analysis and ask:
     - "Did I strip framing consistently across all sources?"
     - "Did I apply the Class A/B/C decision tree without bias?"
     - "Are my 'unsupported' flags based on lack of evidence, or my disagreement with the claim?"
   - *Note:* See Phase 3.5 for complete Recursive Self-Audit Protocol.

---

### Phase 5: Omission Analysis Protocol

**Objective:** Identify what the article conspicuously avoids mentioning—a common tactic in weaponized narratives.

**Instructions for the LLM:**

1. **Context Mapping:**
   - Based on the article's topic, identify what information a *complete* analysis would require.
   - *Example:* If the article discusses "controversial policy X," a complete analysis would include:
     - The policy's stated goals
     - Historical precedent (has this been done before?)
     - Counterarguments or opposing data
     - Stakeholder perspectives (who benefits/loses?)

2. **Omission Detection:**
   - For each expected information category, check the article:
     - **Present & Substantive:** The article addresses this with Class A/B evidence.
     - **Present but Superficial:** The article mentions this but provides no Class A/B support.
     - **Absent:** The article does not address this at all.

3. **Classification of Omissions:**
   - **Material Omission:** The absence of this information fundamentally alters the reader's understanding.
     - *Example:* An article about a "dangerous new law" that omits the law's actual text or purpose.
   - **Tactical Omission:** The absence suggests intentional narrative shaping.
     - *Example:* An article criticizing a politician's vote that omits their stated rationale.
   - **Agency Omission:** Use of passive voice or vague attribution to obscure who took an action.
     - *Example:* "Mistakes were made" without identifying who made them.
   - **Benign Omission:** Understandable space/scope limitation.
     - *Example:* A breaking news article that lacks long-term impact analysis.

4. **Visual/Multimedia Omission Protocol:**
   - Modern articles use images, charts, videos, and infographics. These have their own framing.
   - **LLM Instructions:**
     - If images are present, note their captions and what they depict.
     - Check: Do images match the article's claims, or do they create additional emotional framing?
     - *Example:* An article about "economic crisis" using a photo of a homeless person (emotional framing) vs. a chart showing actual economic indicators (data).
     - **Classification:**
       - **Evidentiary Visual:** Charts, graphs, documents that provide Class A data.
       - **Framing Visual:** Photos selected for emotional impact without evidentiary value (Class C).
     - Note any visual elements in the Omission Log if they appear to be misleading or if critical visual data is absent.

5. **Output:**
   - In the final report, include an "Omission Log" listing material, tactical, and agency omissions.
   - **Do not speculate** on what the omitted information would say—only note its absence.
   - Include analysis of visual/multimedia framing if present.

---

## Part II.5: Worked Example (Training Reference)

**Purpose:** To demonstrate the complete audit process using a sample article excerpt.

**Sample Article Excerpt:**

> "In a shocking and unprecedented power grab, President Jane Doe signed the controversial Executive Order 9876 yesterday, callously ignoring the millions of Americans who desperately pleaded for reform. Critics say the move is a direct attack on democracy itself. 'This is a disaster,' Senator John Smith angrily declared during a fiery press conference. The order allocates $500 million to infrastructure projects. Sources close to the president suggest she plans to resign by year's end."

**Phase 1 Application (Statement Registry):**

| Statement | Attribution | Form | Para # |
|-----------|-------------|------|--------|
| "President Jane Doe signed Executive Order 9876 yesterday" | Direct report | Reported Action | 1 |
| "shocking and unprecedented power grab" | Author framing | Editorial Framing | 1 |
| "callously ignoring" | Author framing | Editorial Framing | 1 |
| "millions of Americans who desperately pleaded" | Unsourced claim | Paraphrase (unverified) | 1 |
| "Critics say the move is a direct attack on democracy" | Anonymous attribution | Paraphrase (unverified) | 1 |
| "'This is a disaster,' Senator John Smith declared" | Direct Quote | Direct Quote | 1 |
| "angrily declared during a fiery press conference" | Author framing | Editorial Framing | 1 |
| "The order allocates $500 million to infrastructure" | Direct report | Reported Action | 1 |
| "Sources close to the president suggest she plans to resign" | Anonymous sources | Paraphrase (unverified) | 1 |

**Phase 2 Application (Classification):**

| Statement (Framing Removed) | Classification | Reasoning |
|------------------------------|----------------|-----------|
| "President Jane Doe signed Executive Order 9876 on [date]" | **Class B4** | Journalist asserts a physical action occurred, but article provides no link/citation allowing independent verification. Per Axiom 1, unverified event claims are journalist testimony (B4), not verified facts (Class A). |
| "The order allocates $500 million to infrastructure projects" | **Class B4** | Journalist asserts verifiable data point, but article provides no citation to order text. Cannot independently verify, therefore Class B4 (journalist assertion) not Class A (verified fact). |
| "Senator John Smith said, 'This is a disaster'" | **Class B2** | Speech act. The senator did say this. Class B2 (named individual in public context). |
| "shocking," "unprecedented," "power grab" | **Class C** | Editorial adjectives. Discard. |
| "callously ignoring" | **Class C** | Imputed motive. Discard. |
| "millions of Americans who desperately pleaded" | **Class C** | Unverified claim. No source or data. Discard. |
| "Critics say..." | **Class C** | Anonymous, unattributed. Discard. |
| "angrily," "fiery" | **Class C** | Emotional characterization. Discard. |
| "Sources suggest she plans to resign" | **Class C** | Unverified rumor. Discard. |

**Phase 3 Application (Delta Analysis):**

*Narrative Pitch:* "The president committed an illegitimate power grab that harms Americans."

*Sub-Claims:*
1. The president signed Executive Order 9876. → **Supported** (Class B4 - journalist assertion)
2. The signing was a "power grab." → **Unsupported** (Editorial judgment, no Class A/B evidence of illegitimacy)
3. The action harms Americans. → **Unsupported** (No Class A/B data showing harm)
4. The action ignores public will. → **Unsupported** (No Class A/B evidence of public polling or petition data)
5. The order allocates $500M to infrastructure. → **Supported** (Class B4 - journalist assertion without citation)

*Verdict:* 2 out of 5 sub-claims are supported by material evidence. The central thesis (illegitimate power grab) is not supported by Class A or B facts.

**Quantitative Metrics for Sample:**

**Tier 1 (Primary):**
- **ESR:** 2/5 = 40% (Low evidentiary support)
- **NIS:** Unsupported (Core thesis "power grab that harms Americans" has no Class A/B evidence)
- **CDR:** 6/9 statements = 67% (High framing density—exceeds 60% propaganda threshold)
- **CDI:** 2/2 factual claims are Class B4 (journalist assertions without citation) = 100% Citation Deficit (Article provides no verification paths, violates Axiom 1)

**Tier 2 (Manipulation Detection):**
- **CVI (Consolidated Verification Issues):** 0 (Quote is complete, no chain-of-custody failures, no multi-hop attribution)
- **SMI:** 0 (No statistical manipulation in this sample)
- **TFC:** 0 (No temporal fallacies detected)
- **SNF:** 0 (No synthetic narrative aggregation detected)
- **CI:** 0 (No internal contradictions)
- **IPC:** 1 (Implicit premise: "Power grabs" are illegitimate. Article does not provide Class A/B evidence that this action meets definition of power grab)
- **VDMI:** N/A (No visual content in excerpt)
- **MSCC:** 0 (No multi-source conflicts)
- **SDC:** 0 (No semantic drift detected in this brief excerpt)

**Tier 3 (Qualitative):**
- **CES:** Absent (Article ignores potential justifications for order or counterfactual evidence)

**Omission Analysis for Sample:**
- **Material Omission:** The article does not quote or link to the text of Executive Order 9876, preventing readers from assessing its content directly.
- **Tactical Omission:** No explanation of why the president signed the order (stated rationale or policy goal).
- **Tactical Omission:** The infrastructure allocation ($500M) is mentioned but not contextualized (Is this more/less than typical? What projects?).
- **Agency Omission:** The phrase "millions of Americans who desperately pleaded" uses passive construction without identifying specific groups, petitions, or protest movements with verifiable attendance data.

**Structural Integrity Flags for Sample:**
- **Headline Inflation:** Cannot assess (headline not provided in excerpt)
- **Quote Mining Risk:** Low (Senator's quote is complete and context is adequate)
- **Scope Inflation Detected:** None in this sample

---

**GATE VERIFICATION LOG FOR SAMPLE:**

**PHASE 2 GATE CHECKPOINT:**
```
Total statements processed: 9
Class A (Kinetic): 0 statements (0% of total) - No citations provided
Class B (Procedural): 3 statements (33% of total) - 1 quote (B2), 2 journalist assertions (B4)
Class C (Performative): 6 statements (67% of total)
Axiom 2 Verification: Class C items contain only framing/speculation/rhetoric - Confirmed ("shocking," "unprecedented," "power grab," "callously," "angrily," "fiery," "critics say," "sources suggest" - all editorial framing or unverified claims)
Axiom 3 Verification: Evidence hierarchy maintained (A > B > C) - Confirmed
Class C percentage check: 67% (exceeds 30% threshold)
Gate Status: CLEARED - Proceeding to Phase 3
```

**PHASE 3 GATE CHECKPOINT:**
```
Axiom 1 Verification: All 'Supported' claims have corresponding Class A/B evidence
  - Sub-claim 1 "President signed EO 9876" ← Class B4 (Journalist assertion): "President signed EO 9876 on [date]" (no citation provided)
  - Sub-claim 5 "Order allocates $500M" ← Class B4 (Journalist assertion): "$500M to infrastructure" (no citation provided)
  Note: Both claims supported by Class B testimony, not Class A verified facts. CDI = 100% reveals verification gap.
Axiom 2 Verification: All sub-claims stripped of emotional framing - Confirmed (removed "reckless," "power grab," "callously," "desperately")
Axiom 3 Verification: Evidence hierarchy applied to ESR/NIS calculations
  - ESR calculation traceable: 2 supported ÷ 5 total = 40%
  - NIS determination justified: Core thesis "illegitimate power grab that harms Americans" has NO Class A/B support for "illegitimate," "power grab," or "harms Americans" - only the signing action itself has B4 support. Therefore NIS = Unsupported.
Gate Status: CLEARED
```

**METRIC TRACEABILITY VERIFICATION:**
* ESR: Can trace to sub-claim list (5 sub-claims, 2 supported) - YES
* CDR: Can trace to Class C count (6 Class C items ÷ 9 total = 67%) - YES
* CDI: Can trace to Class B4 breakdown (2 B4 assertions ÷ 2 total factual claims = 100%) - YES

---

## Part II.6: Statistical Manipulation Example (Training Reference)

**Purpose:** To demonstrate the Statistical Manipulation Detection Protocol (Step 2.3) in action.

**Sample Article Excerpt:**

> "The governor's reckless policy has caused a catastrophic 300% increase in crime! According to experts, this is the worst public safety crisis in decades. In the past three months, violent incidents have skyrocketed from 2 to 8 cases. A recent poll shows 85% of residents feel unsafe, and economists agree the policy has destroyed the local economy—unemployment is at a staggering 7%, the highest in 5 years."

**Application of Statistical Manipulation Detection:**

| Claim | Detection Protocol Applied | Classification | Reasoning |
|-------|---------------------------|----------------|-----------|
| "300% increase in crime" | **Base Rate Neglect Check** | **Class C** | Baseline = 2 incidents, Final = 8 incidents. While mathematically accurate, the small absolute numbers make the percentage misleading. Raw data should be reported: "Crime incidents increased from 2 to 8 over 3 months." |
| "According to experts" | **"According To" Fallacy** | **Class C** | No experts are named. No credentials provided. Unverifiable attribution. |
| "Worst public safety crisis in decades" | **Timeframe Manipulation & Comparative Claim** | **Class C** | "Decades" is vague (20 years? 40 years?). No Class A data comparing current situation to historical baselines. Superlative without evidence. |
| "Recent poll shows 85% feel unsafe" | **Percentage Without Population** | **Class C** | Sample size not provided. Poll methodology not disclosed. No link to poll. Could be 85% of 20 people or 20,000. Incomplete statistical claim. |
| "Economists agree" | **"According To" Fallacy** | **Class C** | No economists named. No indication of sample size (all economists? 5? 500?). Unverifiable. |
| "Unemployment is at 7%" | **Class A/B4** (depends on sourcing) | **Conditional** | This is a specific, verifiable data point. Classification depends on whether article cites official labor statistics. If sourced with citation, Class A (Verified). If unsourced, Class B4 (Journalist Assertion). |
| "Highest in 5 years" | **Timeframe Selectivity** | **Class C (Flag)** | Cherry-picked timeframe. Why 5 years? What was it 6 years ago or 10 years ago? Without full trend context, this is selective framing. The 7% figure itself may be Class A, but "highest in 5 years" requires historical data citation. |
| "Policy destroyed the economy" | **Correlation as Causation** | **Class C** | No Class A evidence of mechanism. Does the article show *how* the policy caused unemployment? Or is this temporal correlation? |

**Extracted Class A/B4 Facts (Framing Removed):**
- *If article cites Bureau of Labor Statistics with link:* Current unemployment rate is 7% (Class A - Verified)
- *If article provides no citation:* Journalist asserts unemployment rate is 7% (Class B4 - Unverified assertion)
- *If article cites police records with link:* Crime incidents increased from 2 to 8 over 3-month period (Class A - Verified)
- *If article provides no citation:* Journalist asserts crime increased from 2 to 8 incidents (Class B4 - Unverified assertion)

**Statistical Manipulation Index (SMI) for This Sample:** 6 (High)
- Base rate neglect (1)
- Unnamed experts (1)
- Vague comparative claim (1)
- Percentage without population (1)
- Timeframe selectivity (1)
- Correlation as causation (1)

**Delta Analysis Note:**
The article's narrative pitch ("Governor's policy caused catastrophic crisis") relies almost entirely on statistically manipulated or unverifiable claims. The only potentially valid Class A data points (unemployment rate, crime incident count) do not inherently support the catastrophic framing without additional context and mechanism evidence.

---

## Part III: The Output Schema
*(Strict Format Requirement)*

The LLM must produce the final analysis in the following format. Do not deviate.

### 0. Executive Verdict (The Bottom Line)
*(MANDATORY: This section must appear at the very top of the output)*

* **Trust Rating:** [🔴 LOW / 🟡 MEDIUM / 🟢 HIGH]
    * *Logic (Apply in this strict order):*
        1.  **CRITICAL FAIL:** If CDI > 50% → **🔴 LOW** (Automatic fail: Article relies on assertions without verification).
        2.  **Logic Fail:** If ESR < 50% → **🔴 LOW** (Narrative not supported by evidence).
        3.  **High Trust:** If ESR > 75% AND CDI < 20% → **🟢 HIGH**.
        4.  **Default:** All other cases → **🟡 MEDIUM**.
* **The "So What?" Summary:** [One sentence comparing the article's specific claims vs. what is actually proven by Class A evidence.]
* **The "Real News" (Top 3 Verified Facts):**
    * [Fact 1 - Must be Class A verified]
    * [Fact 2 - Must be Class A verified]
    * [Fact 3 - Must be Class A verified]
    * *(If fewer than 3 Class A facts exist, list "None identified")*
* **Recommendation:** [Read for context / Read for facts / Skip - Pure Narrative]

---

### 1. Article Metadata & Narrative Pitch
* **Headline:** [Exact headline text from Phase 1]
* **Subheadline:** [If present]
* **Byline/Date:** [Author and publication date, if available]
* **Article Type:** [News Reporting / Opinion/Editorial / Satire / Ambiguous]
* **Narrative Pitch Summary:** [A concise summary of what the author wants the reader to believe/feel]
* **Intended Emotional Response:** [What is the desired psychological effect?]

### 2. The Admissibility Log (Class C Filtering)
* *List items excluded from evidence because they were identified as Performative/Rhetoric:*
    * "[Quote or Excerpt]" -> **Reason:** [e.g., Sarcasm/Hyperbole]
    * "[Quote or Excerpt]" -> **Reason:** [e.g., Emotional Coloring/No Kinetic Action]

### 3. The Material Evidence Locker
* *List only Class A (Kinetic) and Class B (Procedural) facts:*
    * **[Event/Data Point]** (Class: [A-Verified / A-Asserted / B1 / B2 / B3]) (Source: Paragraph X / [Citation if provided])
    * **[Event/Data Point]** (Class: [A-Verified / A-Asserted / B1 / B2 / B3]) (Source: Paragraph Y / [Citation if provided])

* **Verification Key:**
    * **Class A-Verified:** Physical action or data with source citation/link provided (ONLY form of Class A per Axiom 1)
    * **Class B1:** Named official, on-record statement in formal setting
    * **Class B2:** Named individual, informal context statement
    * **Class B3:** Anonymous attribution
    * **Class B4:** Journalist assertion of factual event/data without source citation

### 4. The Delta Analysis
* **The Verdict:** [Does the Material Evidence support the Narrative Pitch?]
* **Gap Analysis:** [Identify specific claims in the Pitch that are not supported by items in the Evidence Locker.]

### 5. Quantitative Metrics (Mandatory)

**TIER 1 - PRIMARY METRICS (Always Report):**
* **Evidentiary Support Ratio (ESR):** [X/Y = Z%] - Percentage of sub-claims supported by Class A/B evidence
  - *Formula:* (Number of Supported Sub-Claims / Total Sub-Claims in Narrative Pitch) × 100
  - *Interpretation:* <50% = Low Support, 50-75% = Moderate, >75% = High
* **Narrative Integrity Score (NIS):** [Supported / Partially Supported / Unsupported]
  - *Assessment:* Does the CORE THESIS (not peripheral details) have Class A/B support?
* **Class C Density Ratio (CDR):** [X/Y = Z%] - Percentage of total statements that are performative/framing
  - *Formula:* (Number of Class C Statements / Total Statements in Article) × 100
  - *Interpretation:* CDR > 60% = High framing-to-fact ratio (propaganda threshold)
* **Citation Deficit Index (CDI):** [X/Y = Z%] - Percentage of factual assertions lacking source citations
  - *Formula:* (Count of Class B4 Items) / (Count of Class A Items + Count of Class B4 Items)
  - *Constraint:* Do NOT include Class B1, B2, or B3 (Quotes/Speech Acts) in this calculation. This metric measures the reliability of the *journalist's* assertions of fact, not the sources' opinions.
  - *Edge Case:* If (Class A + Class B4) = 0, set CDI to **"N/A - No Factual Claims"**. Do not divide by zero.
  - *Interpretation:* CDI > 50% = Article relies heavily on journalist assertions without providing verification paths (Trustworthy articles have low CDI).

**TIER 2 - MANIPULATION DETECTION (Report When Detected, Otherwise Note "0" or "None"):**
* **Consolidated Verification Issues (CVI):** [Number: 0-8+] - REPLACES CDC + CCF + ACD
  - *Formula:* Sum of quote context deficiencies + chain-of-custody failures + multi-hop attributions (≥2 hops)
  - *Interpretation:* 0 = No issues, 1-3 = Low, 4-7 = Moderate, 8+ = High
* **Statistical Manipulation Index (SMI):** [Number: 0-5+] - Count of statistical manipulation techniques
  - 0 = None, 1-2 = Low, 3-4 = Moderate, 5+ = High
* **Temporal Fallacy Count (TFC):** [Number] - Causation claims based on temporal sequence without mechanism
* **Synthetic Narrative Flag (SNF):** [Number] - Topic areas with 4+ similar unsupported claims aggregated
* **Contradiction Index (CI):** [Number] - Internal contradictions between Class A/B statements
* **Implicit Premise Count (IPC):** [Number] - Unstated assumptions required for narrative impact (Max: 2)
* **Visual Data Manipulation Index (VDMI):** [Number: 0-5+ OR "N/A"]
  - If no visual access: Report "N/A - Visual Content Not Accessible"
  - If visual access: 0 = None, 1-2 = Low, 3-4 = Moderate, 5+ = High
* **Multi-Source Conflict Count (MSCC):** [Number] - Unresolved factual conflicts between sources
* **Semantic Drift Count (SDC):** [Number] - Term substitution progressions without evidentiary basis (Max: 2)

**TIER 3 - QUALITATIVE ASSESSMENTS (Always Report):**
* **Counterfactual Engagement Score (CES):** [Strong / Weak / Absent]
  - Strong = Presents and engages with contrary evidence using Class A/B sourcing
  - Weak = Mentions opposition without data
  - Absent = Ignores contradictory evidence

### 6. The Omission Log
* **Material Omissions:** [List information that, if absent, fundamentally alters understanding]
* **Tactical Omissions:** [List information likely excluded for narrative shaping purposes]
* **Agency Omissions:** [List instances where passive voice or vague attribution obscures responsibility]
* **Visual/Multimedia Analysis:** [Note any images, charts, or media elements and their framing effect - Evidentiary vs. Emotional]

### 7. Structural Integrity Flags
* **Headline Inflation:** [Yes/No] - Does headline claim exceed body evidence?
* **Quote Mining Risk:** [Low/Medium/High] - Based on Context Deficiency Count and centrality of quotes to Narrative Pitch
* **Scope Inflation Detected:** [List any instances where single incidents are generalized to patterns without supporting data]
* **Lawyering Detected:** [List any instances where technically true facts are juxtaposed to create false implications]
* **Synthetic Certainty Construction:** [Yes/No] - Does article use claim aggregation to manufacture consensus?
* **Modal Hedging Count:** [Number] - Count of speculative "could/might/may" framings used to avoid falsifiable claims
* **Counterfactual Omission:** [Yes/No] - Does article fail to address evidence that would contradict the Narrative Pitch?
* **Expert Credibility Issues:** [List any instances where experts are cited outside their domain, credentials are not disclosed, or conflicts of interest are not mentioned]
* **Visual Manipulation Present:** [Yes/No] - Based on VDMI score (Yes if VDMI ≥ 1)

### 8. Self-Audit Verification
* **Statement:** "I have reviewed my classifications and analysis for ideological bias, confirmation bias, and inconsistent application of evidence standards."
* **Corrections Made:** [List any reclassifications or adjustments made during recursive self-audit, or state "None"]
* **Ambiguities Acknowledged:** [List any statements where classification was uncertain and explain reasoning]

### 9. Contradiction Log (If Applicable)
* **Internal Contradictions Detected:** [Number from CI metric]
* **Details:** [List each contradiction with paragraph references: "Claim X (Para 2) states [fact]. Claim Y (Para 8) states [contradictory fact]."]

### 10. Implicit Premises & Unargued Assumptions
* **List:** [Each unstated assumption required for narrative impact]
* **Support Status:** [Whether each premise has Class A/B evidence support]
* **Five-Gate Test Documentation:** [For each premise, note which gates were passed]
* **Example Format:** "Premise: [Implicit assumption]. Support: [Unsupported / The article provides Class A evidence: X]. Gates Passed: [1-Normative language, 2-Factual basis, 3-No external standard, 4-Central to pitch, 5-Ideological symmetry]"

---

### 11. Gate Verification Log (MANDATORY)

**This section verifies that the audit process followed the three-gate system. If this section is absent, the audit is invalid.**

**WHY GATES MATTER (Human-Readable Context):**
The gates ensure the LLM is actually doing forensic work, not rubber-stamping. The Phase 2 Gate specifically checks that the auditor is *catching* emotional language—if Class C is too low, the auditor missed framing that should have been filtered. This is a quality control mechanism.

**PHASE 2 GATE CHECKPOINT:**
```
Total statements processed: [X]
Class A (Kinetic): [Y] statements ([Z]% of total) - All with source citations
Class B (Procedural): [Y] statements ([Z]% of total) - [N] B1, [M] B2, [P] B3, [Q] B4
Class C (Performative): [Y] statements ([Z]% of total)
Axiom 2 Verification: Class C items contain only framing/speculation/rhetoric (no misclassified events or speech acts)
Axiom 3 Verification: Evidence hierarchy maintained (A > B > C)
Class C Threshold Check: [Z]% (Must be ≥20% to pass)
```

Gate Status: [CLEARED (Framing Detected) / FAILED - Under-filtered]
    * *Note:* "CLEARED" means the audit successfully identified Class C framing. If Class C is low (<20%), the gate FAILS because the auditor likely missed the rhetorical layer.

**PHASE 3 GATE CHECKPOINT:**
```
Axiom 1 Verification: All 'Supported' claims have corresponding Class A/B evidence - [List each supported claim and its Evidence Locker citation]
Axiom 2 Verification: All sub-claims stripped of emotional framing - [Confirmed]
Axiom 3 Verification: Evidence hierarchy applied to ESR/NIS calculations
  - ESR calculation traceable: [X supported ÷ Y total = Z%]
  - NIS determination justified: [Core thesis supported/unsupported because...]
```

Gate Status: [CLEARED (Logic Verified) / FAILED - Axiom Violation]

**METRIC TRACEABILITY VERIFICATION:**
* ESR: [Can trace to sub-claim list - YES/NO]
* CDR: [Can trace to Class C count - YES/NO]
* CDI: [Can trace to Class B4 count and Class A count (B1/B2/B3 EXCLUDED) - YES/NO or N/A if no factual claims]
* If any = NO, metric marked as "Unable to Calculate"