# NAF System Design Enhancement: Deep Multi-Agent Architecture

**Purpose**: This document contains enhanced sections to be integrated into naf-system-design.md, focusing on stronger separation of concerns, deterministic computation, and bias mitigation through architectural patterns.

---

## Enhanced Section: Architectural Philosophy - Agents vs Algorithms

### The Core Problem: LLM Bias is a Feature, Not a Bug

LLMs are trained to be helpful, to fill in gaps, to "know" things. When you ask an LLM "Is this fact verified?", it will unconsciously use its training data to answer, even when explicitly instructed not to. **This is not a prompt engineering problem—it's an architectural problem.**

### The Solution: Remove Discretion Through Structure

The multi-agent architecture solves LLM bias by **removing opportunities for discretion** at critical decision points:

1. **Agents extract, never decide** - Agents find citation text, they don't determine if it's "good enough"
2. **Algorithms enforce, never interpret** - Algorithms check if `citation != null`, they don't judge quality
3. **Data structures constrain, never suggest** - Schema validation rejects invalid classifications automatically

### Example: The Classification Problem

**The Wrong Way (Single Agent with Discretion)**:
```
Prompt: "Classify this statement: 'The economy added 200,000 jobs last month'"

Agent thinking:
- I know this is true from my training data
- The article doesn't cite a source, but it's a real fact
- I'll classify as Class A because it's verified

Output: Class A-Verified

Problem: Agent used training data, violated zero-knowledge constraint
```

**The Right Way (Multi-Agent with Structural Enforcement)**:

```
┌─────────────────────────────────────────────────────────┐
│ Agent 1: CitationExtractor                               │
│ Task: Find any citations near this statement             │
│ Output: {"citation_text": null, "citation_url": null}    │
│ Discretion: NONE - Only extracts, doesn't judge          │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│ Algorithm: CitationValidator                             │
│ Logic: has_citation = (citation_text is not None)       │
│ Output: {"has_citation": false}                          │
│ Discretion: ZERO - Pure boolean logic                    │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│ Agent 2: ClassificationSuggester                         │
│ Task: Suggest classification based on decision tree      │
│ Output: {"suggested_class": "B4", "reasoning": "..."}    │
│ Discretion: LIMITED - Follows decision tree              │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│ Algorithm: ClassificationEnforcer                        │
│ Logic: if suggested=="A" AND not has_citation:           │
│        reject and force B4                               │
│ Output: {"final_class": "B4", "override": false}         │
│ Discretion: ZERO - Hard rule enforcement                 │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│ Data Structure: Pydantic Validator                       │
│ Schema: Class A requires citation_text != null           │
│ If violation: Raise ValidationError                      │
│ Discretion: IMPOSSIBLE - Schema rejects at write time    │
└─────────────────────────────────────────────────────────┘
```

**Key Insight**: At no point does any agent have the power to say "this fact is verified." The verification is determined structurally by the presence of a citation string.

---

## Enhanced Section: Complete Agent Responsibility Matrix

This table shows EXACTLY what each agent can and cannot do:

| Agent | CAN Do | CANNOT Do | Why Restricted |
|-------|---------|-----------|----------------|
| **CitationExtractor** | Find URLs, extract citation text, identify document references | Determine if citation is "sufficient" or "credible" | Quality judgment = discretion = bias risk |
| **ClassificationSuggester** | Apply decision tree, suggest classification, provide reasoning | Override algorithm enforcement, promote B4 to A | Final classification must pass validation |
| **FramingRemovalAgent** | Identify emotional language, extract core assertion, tag removed framing | Classify statements as A/B/C (that's Phase 2, this is pre-processing) | Mixing tasks creates cognitive overload |
| **StatementParser** | Break article into discrete statements, assign paragraph numbers, preserve verbatim text | Classify statements, remove framing, add interpretation | Single responsibility only |
| **NarrativePitchExtractor** | Synthesize headline + opening + closing into one-sentence thesis | Determine if thesis is supported (that's Delta Analysis) | Separation of extraction from evaluation |
| **DeltaAnalysisAgent** | Match sub-claims to evidence, identify gaps, determine support status | Calculate ESR percentage (that's algorithmic) | Agents interpret, algorithms compute |
| **MetricCalculator** | NOTHING - This is pure algorithm | Use judgment, interpret edge cases, "estimate" if data missing | Metrics must be deterministic |

### The Principle: Minimize Agent Judgment Surface Area

Every decision point where an agent has discretion is a potential bias injection point. Therefore:

- **Maximize algorithmic enforcement** (citation checking, counting, percentage calculation)
- **Minimize agent interpretation** (only when semantic understanding is required)
- **Eliminate agent discretion** (use validation schemas to reject invalid outputs)

---

## Enhanced Section: Deterministic Computation Patterns

### Pattern 1: Count, Don't Estimate

**Bad (Agent Discretion)**:
```python
# Agent prompt: "Approximately how many Class C statements are there?"
# Agent output: "Around 40-50% are Class C"
# Problem: "Around" is not deterministic
```

**Good (Algorithmic)**:
```python
def calculate_class_c_percentage(statements: List[Statement]) -> float:
    """Pure deterministic calculation - no LLM involved"""
    total = len(statements)
    class_c = sum(1 for s in statements if s.classification == "C")
    return (class_c / total * 100) if total > 0 else 0.0
```

### Pattern 2: Validate, Don't Trust

**Bad (Agent Self-Reporting)**:
```python
# Agent prompt: "Calculate CDI and report the result"
# Agent output: {"CDI": 42.5}
# Problem: No way to verify agent did the math correctly
```

**Good (Algorithm Validates Agent Work)**:
```python
def validate_cdi_calculation(agent_reported_cdi: float,
                             classified_statements: List[Statement]) -> dict:
    """
    Agent reports CDI, algorithm recalculates and validates.
    If mismatch > 1%, reject and use algorithmic calculation.
    """
    class_a = [s for s in classified_statements if s.classification == "A-Verified"]
    class_b4 = [s for s in classified_statements if s.classification == "B4"]

    denominator = len(class_a) + len(class_b4)
    if denominator == 0:
        calculated_cdi = None
    else:
        calculated_cdi = (len(class_b4) / denominator) * 100

    if agent_reported_cdi is None and calculated_cdi is None:
        return {"status": "VALID", "cdi": None}

    if calculated_cdi is None or agent_reported_cdi is None:
        return {
            "status": "MISMATCH",
            "agent_reported": agent_reported_cdi,
            "calculated": calculated_cdi,
            "override": calculated_cdi
        }

    error_margin = abs(agent_reported_cdi - calculated_cdi)
    if error_margin > 1.0:
        return {
            "status": "INVALID",
            "agent_reported": agent_reported_cdi,
            "calculated": calculated_cdi,
            "error_margin": error_margin,
            "override": calculated_cdi,
            "reason": "Agent calculation differs from algorithmic calculation by >1%"
        }

    return {"status": "VALID", "cdi": calculated_cdi}
```

### Pattern 3: Schema as Gatekeeper

**Bad (No Validation)**:
```python
class Statement(BaseModel):
    classification: str  # Any string accepted
    citation: str = ""   # Empty string allowed for Class A
```

**Good (Schema Enforces Rules)**:
```python
from pydantic import BaseModel, validator, Field
from typing import Optional, Literal

class Statement(BaseModel):
    statement_id: str
    text: str
    classification: Literal["A-Verified", "B1", "B2", "B3", "B4", "C"]
    citation_text: Optional[str] = None
    citation_url: Optional[str] = None

    @validator('classification')
    def class_a_requires_citation(cls, v, values):
        """Enforces: Class A MUST have citation"""
        if v == "A-Verified":
            has_citation = (
                values.get('citation_text') or
                values.get('citation_url')
            )
            if not has_citation:
                raise ValueError(
                    f"Class A-Verified requires citation. "
                    f"Statement: {values.get('text', '')[:50]}... "
                    f"If no citation exists, classify as B4."
                )
        return v

    @validator('classification')
    def class_b4_prohibits_citation(cls, v, values):
        """Enforces: Class B4 means NO citation"""
        if v == "B4":
            has_citation = (
                values.get('citation_text') or
                values.get('citation_url')
            )
            if has_citation:
                raise ValueError(
                    f"Class B4 (Journalist Assertion) cannot have citation. "
                    f"If citation exists, classify as A-Verified."
                )
        return v

    @validator('citation_text', 'citation_url')
    def citation_not_empty_string(cls, v):
        """Enforces: null is different from empty string"""
        if v == "":
            return None  # Convert empty strings to null
        return v
```

**Result**: Agent cannot create invalid classifications. Schema rejects at write time.

---

## Enhanced Section: Bias Mitigation Through Architectural Patterns

### Anti-Pattern 1: The Oracle Agent

**Problem**: Asking an agent "Is this claim supported?" allows the agent to use training data.

**Solution**: Break into non-oracle sub-tasks.

```python
# ❌ BAD: Oracle Agent
prompt = """
Is this claim supported by the article?
Claim: "GDP increased 2.3%"
Article: {article_text}
"""

# ✅ GOOD: Non-Oracle Sub-Tasks
# Step 1: Agent extracts candidate evidence
prompt_1 = """
Find all statements in the article that mention GDP, economy, or growth.
Return verbatim text only, no interpretation.
"""

# Step 2: Algorithm checks for exact match
def contains_claim(evidence_list: List[str], target_claim: str) -> bool:
    return any(target_claim.lower() in evidence.lower() for evidence in evidence_list)

# Step 3: Agent determines semantic similarity (if no exact match)
prompt_2 = """
Do any of these evidence statements convey the same factual claim as the target?
Target: "GDP increased 2.3%"
Evidence: {evidence_list}
Answer: Yes/No with reasoning
"""
```

### Anti-Pattern 2: The Psychic Agent

**Problem**: Asking agent "What is the article's narrative pitch?" allows agent to infer unstated intent.

**Solution**: Extract only from specific locations.

```python
# ❌ BAD: Psychic Agent
prompt = """
What is the article trying to make you believe?
[Article text]
"""

# ✅ GOOD: Structural Extraction
prompt = """
Extract the following and ONLY the following:
1. Headline (verbatim)
2. First paragraph (verbatim)
3. Last paragraph (verbatim)

Then, synthesize these THREE elements into one sentence:
"Article wants you to believe [X] because [Y]"

Do NOT read between the lines. Only use information from headline + first + last paragraph.
"""
```

### Anti-Pattern 3: The Judge Agent

**Problem**: Asking agent "Is this source credible?" allows ideological bias.

**Solution**: Extract credentials, algorithm determines tier.

```python
# ❌ BAD: Judge Agent
prompt = """
Is Dr. Smith a credible expert on economics?
"""

# ✅ GOOD: Credential Extraction + Tiered Algorithm
# Agent extracts structured data
prompt = """
Extract expert information:
- Name:
- Credentials:
- Institutional affiliation:
- Field of expertise:
- Is this field relevant to the claim? (yes/no)
"""

# Algorithm determines credibility tier
def determine_credibility_tier(expert_info: dict) -> str:
    has_name = expert_info["name"] is not None
    has_credentials = expert_info["credentials"] is not None
    has_institution = expert_info["institutional_affiliation"] is not None
    is_relevant = expert_info["relevant_to_claim"]

    if not has_name:
        return "Tier4_Anonymous"  # Class C
    if has_name and has_credentials and has_institution and is_relevant:
        return "Tier1_VerifiedDomainAuthority"  # Class B1
    if has_name and has_credentials:
        return "Tier2_NamedCredentialed"  # Class B2
    if has_name:
        return "Tier3_NamedOnly"  # Class B3
    return "Tier4_Insufficient"
```

---

## Enhanced Section: Data Flow Architecture

### The Immutable Pipeline

Each phase produces an immutable artifact (JSON file). Later phases read but never modify earlier artifacts.

```
Phase 1 Output: statement_registry.json (IMMUTABLE)
    ↓ (read-only)
Phase 2 Output: classified_statements.json (IMMUTABLE)
    ↓ (read-only)
Phase 3 Output: delta_analysis.json (IMMUTABLE)
    ↓ (read-only)
Phase 5 Output: omission_analysis.json (IMMUTABLE)
    ↓ (read-only)
Final Output: audit_report.json (IMMUTABLE)
```

**Why Immutable?**
- Enables debugging (can inspect intermediate state)
- Allows re-running phases without re-doing earlier work
- Creates audit trail (can see exactly what each agent produced)
- Prevents agents from "fixing" earlier mistakes (which would hide errors)

### The Validation Sandwich Pattern

Every agent output is sandwiched between pre-validation and post-validation:

```
┌──────────────────────────────────┐
│  Pre-Validation (Schema Check)   │  ← Ensures input is well-formed
└──────────────────────────────────┘
                ↓
┌──────────────────────────────────┐
│      Agent Execution (LLM)       │  ← Does semantic work
└──────────────────────────────────┘
                ↓
┌──────────────────────────────────┐
│ Post-Validation (Logic Check)    │  ← Ensures output follows rules
└──────────────────────────────────┘
                ↓
┌──────────────────────────────────┐
│  Algorithmic Override (if needed) │  ← Fixes violations automatically
└──────────────────────────────────┘
```

**Example: Classification Agent**

```python
def execute_classification_agent(
    statements: List[Statement],
    validation_mode: str = "strict"
) -> ClassificationResult:

    # PRE-VALIDATION
    validate_statement_schema(statements)  # Pydantic check
    validate_framing_removed(statements)   # Logic check

    # AGENT EXECUTION
    agent_output = classification_agent.invoke(statements)

    # POST-VALIDATION
    validation_result = validate_classifications(agent_output)

    if validation_result["violations"]:
        # ALGORITHMIC OVERRIDE
        corrected = apply_classification_rules(
            agent_output,
            validation_result["violations"]
        )

        return ClassificationResult(
            classifications=corrected,
            original_agent_output=agent_output,  # Preserve for debugging
            corrections_applied=validation_result["violations"],
            validation_status="CORRECTED"
        )

    return ClassificationResult(
        classifications=agent_output,
        validation_status="PASSED"
    )
```

---

## Enhanced Section: Concrete Implementation Example

### Example: CDI Calculation (Complete Flow)

Let's trace a single metric (CDI) through the entire pipeline to show agent-algorithm separation:

#### Step 1: Agent Classifies Statements (Phase 2)

```python
# Agent receives statements with framing removed
agent_prompt = """
Classify each statement as A-Verified, B1, B2, B3, B4, or C.

Statement 1: "200,000 jobs added last month"
- Evidence form: Data
- Citation found: null
- Classification: ?

Statement 2: "Senator X stated: 'This is good news'"
- Evidence form: DirectQuote
- Citation found: Press conference transcript (link)
- Classification: ?
"""

# Agent suggests classifications
agent_output = {
    "stmt_1": {"classification": "B4", "reasoning": "Data claim without citation"},
    "stmt_2": {"classification": "B1", "reasoning": "Named official, on-record quote"}
}
```

#### Step 2: Algorithm Validates Classifications

```python
def validate_classification(statement: dict, suggested_class: str) -> dict:
    """Enforce classification rules"""

    # Rule 1: Class A requires citation
    if suggested_class == "A-Verified":
        if not statement.get("citation_url") and not statement.get("citation_text"):
            return {
                "valid": False,
                "corrected_class": "B4",
                "reason": "Class A requires citation (Axiom 1 violation)"
            }

    # Rule 2: Class B4 prohibits citation
    if suggested_class == "B4":
        if statement.get("citation_url") or statement.get("citation_text"):
            return {
                "valid": False,
                "corrected_class": "A-Verified",
                "reason": "Citation present, must be Class A"
            }

    # Rule 3: Quotes must be B1/B2/B3, not A or B4
    if statement["evidence_form"] == "DirectQuote":
        if suggested_class not in ["B1", "B2", "B3", "C"]:
            return {
                "valid": False,
                "corrected_class": "B1",  # Default to B1, can refine
                "reason": "Direct quotes are speech acts (Class B), not events (Class A)"
            }

    return {"valid": True}
```

#### Step 3: Algorithm Calculates CDI

```python
def calculate_cdi(validated_statements: List[Statement]) -> CDIResult:
    """
    Pure deterministic calculation.
    NO LLM involvement. NO discretion.
    """

    # Filter to factual assertions only
    class_a = [s for s in validated_statements if s.classification == "A-Verified"]
    class_b4 = [s for s in validated_statements if s.classification == "B4"]

    # CDI formula: B4 / (A + B4) * 100
    denominator = len(class_a) + len(class_b4)

    if denominator == 0:
        return CDIResult(
            value=None,
            interpretation="N/A - No factual assertions (only quotes and framing)",
            numerator=0,
            denominator=0,
            class_a_statements=[],
            class_b4_statements=[]
        )

    cdi_value = (len(class_b4) / denominator) * 100

    # Interpretation thresholds (deterministic)
    if cdi_value <= 25:
        interpretation = "Strong Citation (≤25% unsourced)"
    elif cdi_value <= 50:
        interpretation = "Moderate Gap (26-50% unsourced)"
    else:
        interpretation = "Severe Deficit (>50% unsourced)"

    return CDIResult(
        value=round(cdi_value, 2),
        interpretation=interpretation,
        numerator=len(class_b4),
        denominator=denominator,
        class_a_statements=[s.statement_id for s in class_a],
        class_b4_statements=[s.statement_id for s in class_b4],
        calculation_trace={
            "formula": "CDI = (B4_count / (A_count + B4_count)) * 100",
            "a_count": len(class_a),
            "b4_count": len(class_b4),
            "calculation": f"({len(class_b4)} / {denominator}) * 100 = {cdi_value:.2f}"
        }
    )
```

#### Step 4: Traceability Validation

```python
def validate_cdi_traceability(cdi_result: CDIResult,
                               validated_statements: List[Statement]) -> bool:
    """
    Verify that CDI calculation can be traced back to source statements.
    This prevents "metric gaming" where agent outputs numbers without work.
    """

    # Verify every Class A ID exists
    for stmt_id in cdi_result.class_a_statements:
        stmt = next((s for s in validated_statements if s.statement_id == stmt_id), None)
        if not stmt:
            raise ValidationError(f"CDI traces to non-existent statement: {stmt_id}")
        if stmt.classification != "A-Verified":
            raise ValidationError(f"CDI lists {stmt_id} as Class A, but actual class is {stmt.classification}")

    # Verify every Class B4 ID exists
    for stmt_id in cdi_result.class_b4_statements:
        stmt = next((s for s in validated_statements if s.statement_id == stmt_id), None)
        if not stmt:
            raise ValidationError(f"CDI traces to non-existent statement: {stmt_id}")
        if stmt.classification != "B4":
            raise ValidationError(f"CDI lists {stmt_id} as Class B4, but actual class is {stmt.classification}")

    # Recalculate CDI from scratch
    recalculated = calculate_cdi(validated_statements)

    # Allow 0.01% floating point tolerance
    if abs(cdi_result.value - recalculated.value) > 0.01:
        raise ValidationError(
            f"CDI calculation mismatch: "
            f"Reported {cdi_result.value}, recalculated {recalculated.value}"
        )

    return True
```

**Key Insight**: The CDI value is **never determined by an agent**. Agents classify statements, algorithms count and calculate. This ensures determinism.

---

---

## Enhanced Section: Agent Communication Protocol

### The Message Bus Architecture

Agents never call each other directly. All communication goes through a central message bus (orchestrator).

```
Agent A ──┐
Agent B ──┼──→ [Message Bus / Orchestrator] ──→ Agent D
Agent C ──┘                                   └──→ Agent E
```

**Benefits:**
- **Decoupling**: Agents don't know about each other
- **Replay**: Can replay messages for debugging
- **Monitoring**: All communication is observable
- **Testing**: Can inject mock messages

### Message Schema

Every agent communication follows a standard schema:

```python
from pydantic import BaseModel
from typing import Literal, Dict, Any
from datetime import datetime

class AgentMessage(BaseModel):
    message_id: str  # Unique identifier
    phase: Literal["phase_1", "phase_2", "phase_3", "phase_5", "output"]
    source_agent: str  # Which agent sent this
    target_agent: Optional[str]  # Which agent should receive (None = orchestrator decision)
    message_type: Literal["task", "result", "error", "validation_request"]
    payload: Dict[str, Any]  # The actual data
    timestamp: datetime
    parent_message_id: Optional[str]  # For tracing chains
    audit_metadata: Dict[str, Any]  # For observability

    # Validation metadata
    schema_version: str = "1.0"
    validation_status: Optional[Literal["pending", "passed", "failed"]] = None
    validation_errors: List[str] = []
```

### Example Message Flow: Classification Phase

```python
# Message 1: Orchestrator → FramingRemovalAgent
{
    "message_id": "msg_001",
    "phase": "phase_2",
    "source_agent": "orchestrator",
    "target_agent": "framing_removal_agent",
    "message_type": "task",
    "payload": {
        "statement_registry": { /* JSON from Phase 1 */ },
        "task": "remove_framing"
    },
    "timestamp": "2026-01-30T10:15:00Z"
}

# Message 2: FramingRemovalAgent → Orchestrator
{
    "message_id": "msg_002",
    "phase": "phase_2",
    "source_agent": "framing_removal_agent",
    "target_agent": "orchestrator",
    "message_type": "result",
    "payload": {
        "statements_with_framing_removed": [ /* array */ ]
    },
    "parent_message_id": "msg_001",
    "timestamp": "2026-01-30T10:15:45Z"
}

# Message 3: Orchestrator → AlgorithmicValidator
{
    "message_id": "msg_003",
    "phase": "phase_2",
    "source_agent": "orchestrator",
    "target_agent": "algorithmic_validator",
    "message_type": "validation_request",
    "payload": {
        "statements": [ /* from msg_002 */ ],
        "validation_rules": ["schema_check", "framing_completeness"]
    },
    "parent_message_id": "msg_002",
    "timestamp": "2026-01-30T10:15:46Z"
}

# Message 4: AlgorithmicValidator → Orchestrator
{
    "message_id": "msg_004",
    "phase": "phase_2",
    "source_agent": "algorithmic_validator",
    "target_agent": "orchestrator",
    "message_type": "result",
    "payload": {
        "validation_status": "passed",
        "warnings": []
    },
    "parent_message_id": "msg_003",
    "timestamp": "2026-01-30T10:15:47Z"
}
```

---

## Enhanced Section: Gate Implementation as State Machines

### Gate 2: Classification Quality Gate

Gates are implemented as deterministic state machines that inspect agent output and enforce rules.

```python
from enum import Enum
from typing import List, Dict

class GateStatus(Enum):
    PENDING = "pending"
    CLEARED = "cleared"
    CONDITIONAL_PASS = "conditional_pass"
    FAILED = "failed"

class Gate2StateMachine:
    """
    Gate 2 enforces:
    1. Class C ≥ 20% (framing detection threshold)
    2. All Class A items have citations
    3. No Class B4 items have citations
    4. Evidence form consistency
    """

    def __init__(self, classified_statements: List[Statement]):
        self.statements = classified_statements
        self.status = GateStatus.PENDING
        self.violations = []
        self.warnings = []
        self.metrics = {}

    def execute(self) -> Dict[str, Any]:
        """Run all gate checks"""

        # Check 1: Class C Threshold
        self._check_class_c_threshold()

        # Check 2: Class A Citation Requirement
        self._check_class_a_citations()

        # Check 3: Class B4 Citation Prohibition
        self._check_class_b4_no_citations()

        # Check 4: Evidence Form Consistency
        self._check_evidence_consistency()

        # Determine final gate status
        self._determine_status()

        return {
            "gate": "GATE_2",
            "status": self.status.value,
            "violations": self.violations,
            "warnings": self.warnings,
            "metrics": self.metrics,
            "action_required": self._get_action()
        }

    def _check_class_c_threshold(self):
        """Enforce: Class C must be ≥ 20%"""
        total = len(self.statements)
        class_c_count = sum(1 for s in self.statements if s.classification == "C")
        class_c_pct = (class_c_count / total * 100) if total > 0 else 0

        self.metrics["class_c_percentage"] = class_c_pct
        self.metrics["class_c_count"] = class_c_count
        self.metrics["total_statements"] = total

        if class_c_pct < 20:
            self.violations.append({
                "rule": "class_c_minimum_threshold",
                "severity": "critical",
                "message": f"Class C is {class_c_pct:.2f}% (required: ≥20%). "
                           f"This indicates under-filtering of framing/speculation.",
                "affected_count": class_c_count,
                "remediation": "Return to Phase 2, reapply framing removal protocols"
            })
        elif class_c_pct < 30:
            self.warnings.append({
                "rule": "class_c_low_threshold",
                "severity": "moderate",
                "message": f"Class C is {class_c_pct:.2f}% (threshold: 20-30%). "
                           f"Low framing density may indicate edge case article or under-filtering.",
                "action": "Proceed with caution, document in final report"
            })

    def _check_class_a_citations(self):
        """Enforce: Class A requires citation"""
        class_a_without_citation = [
            s for s in self.statements
            if s.classification == "A-Verified"
            and not s.citation_text
            and not s.citation_url
        ]

        if class_a_without_citation:
            self.violations.append({
                "rule": "class_a_citation_requirement",
                "severity": "critical",
                "message": f"{len(class_a_without_citation)} Class A statements lack citations (Axiom 1 violation)",
                "affected_statements": [s.statement_id for s in class_a_without_citation],
                "remediation": "Reclassify these statements as Class B4 (Journalist Assertion)"
            })

    def _check_class_b4_no_citations(self):
        """Enforce: Class B4 prohibits citation"""
        class_b4_with_citation = [
            s for s in self.statements
            if s.classification == "B4"
            and (s.citation_text or s.citation_url)
        ]

        if class_b4_with_citation:
            self.violations.append({
                "rule": "class_b4_citation_prohibition",
                "severity": "critical",
                "message": f"{len(class_b4_with_citation)} Class B4 statements have citations (should be Class A)",
                "affected_statements": [s.statement_id for s in class_b4_with_citation],
                "remediation": "Reclassify these statements as Class A-Verified"
            })

    def _check_evidence_consistency(self):
        """Enforce: Evidence form matches classification"""
        inconsistencies = []

        for stmt in self.statements:
            # Check 1: Direct quotes should be Class B, not A or C
            if stmt.evidence_form == "DirectQuote":
                if stmt.classification not in ["B1", "B2", "B3"]:
                    inconsistencies.append({
                        "statement_id": stmt.statement_id,
                        "issue": f"DirectQuote classified as {stmt.classification} (should be B1/B2/B3)",
                        "text": stmt.text[:100]
                    })

            # Check 2: Editorial framing should be Class C
            if stmt.evidence_form == "EditorialFraming":
                if stmt.classification != "C":
                    inconsistencies.append({
                        "statement_id": stmt.statement_id,
                        "issue": f"EditorialFraming classified as {stmt.classification} (should be C)",
                        "text": stmt.text[:100]
                    })

        if inconsistencies:
            self.violations.append({
                "rule": "evidence_form_consistency",
                "severity": "moderate",
                "message": f"{len(inconsistencies)} statements have inconsistent evidence form vs classification",
                "details": inconsistencies,
                "remediation": "Review classifications for consistency"
            })

    def _determine_status(self):
        """Determine final gate status based on violations/warnings"""
        critical_violations = [v for v in self.violations if v["severity"] == "critical"]

        if critical_violations:
            self.status = GateStatus.FAILED
        elif self.warnings:
            self.status = GateStatus.CONDITIONAL_PASS
        else:
            self.status = GateStatus.CLEARED

    def _get_action(self) -> str:
        """Determine orchestrator action based on status"""
        if self.status == GateStatus.FAILED:
            return "ABORT_PHASE_3_RETRY_PHASE_2"
        elif self.status == GateStatus.CONDITIONAL_PASS:
            return "PROCEED_WITH_WARNING"
        else:
            return "PROCEED_TO_PHASE_3"
```

### Usage in Orchestrator

```python
def orchestrator_phase_2_to_phase_3_transition(classified_statements: List[Statement]):
    """Enforce Gate 2 before proceeding to Phase 3"""

    # Execute Gate 2 state machine
    gate = Gate2StateMachine(classified_statements)
    gate_result = gate.execute()

    # Log gate result
    logger.info(f"Gate 2 Status: {gate_result['status']}")
    save_gate_checkpoint("gate_2_checkpoint.json", gate_result)

    # Enforce gate decision
    if gate_result["status"] == "failed":
        logger.error(f"Gate 2 FAILED with {len(gate_result['violations'])} violations")

        # Attempt automatic remediation
        remediated = auto_remediate_gate_2_violations(
            classified_statements,
            gate_result["violations"]
        )

        # Re-run gate with remediated data
        gate_retry = Gate2StateMachine(remediated)
        gate_result_retry = gate_retry.execute()

        if gate_result_retry["status"] == "failed":
            raise GateFailureError(
                "Gate 2 failed after automatic remediation. Manual review required.",
                gate_result_retry
            )

        # If remediation succeeded, proceed with corrected data
        return remediated

    elif gate_result["status"] == "conditional_pass":
        logger.warning(f"Gate 2 CONDITIONAL PASS with {len(gate_result['warnings'])} warnings")
        # Log warnings but proceed
        return classified_statements

    else:
        logger.info("Gate 2 CLEARED - proceeding to Phase 3")
        return classified_statements
```

---

## Enhanced Section: Automatic Remediation Strategies

### Strategy 1: Citation-Classification Mismatch

**Problem**: Agent classifies statement as Class A but no citation exists.

**Automatic Fix**: Reclassify as Class B4.

```python
def remediate_class_a_without_citation(statements: List[Statement]) -> List[Statement]:
    """
    Automatically fix Class A statements lacking citations.
    This is safe because the rule is unambiguous: Class A requires citation.
    """
    remediated = []

    for stmt in statements:
        if stmt.classification == "A-Verified":
            if not stmt.citation_text and not stmt.citation_url:
                # Log the remediation
                logger.info(
                    f"Auto-remediation: {stmt.statement_id} reclassified from "
                    f"A-Verified to B4 (Axiom 1 enforcement: no citation found)"
                )

                # Create corrected statement
                corrected = stmt.copy(update={
                    "classification": "B4",
                    "classification_reasoning": (
                        f"Originally suggested as Class A-Verified, but no citation found. "
                        f"Automatically reclassified as B4 (Journalist Assertion) per Axiom 1. "
                        f"Original reasoning: {stmt.classification_reasoning}"
                    ),
                    "remediation_applied": True,
                    "original_classification": "A-Verified"
                })
                remediated.append(corrected)
            else:
                remediated.append(stmt)
        else:
            remediated.append(stmt)

    return remediated
```

### Strategy 2: Class C Under-Threshold

**Problem**: Class C < 20% of total statements.

**Automatic Fix**: Scan for commonly missed framing patterns and reclassify.

```python
def remediate_class_c_under_threshold(statements: List[Statement]) -> List[Statement]:
    """
    Automatically detect commonly under-classified framing patterns.
    This is semi-automatic: applies deterministic rules to reclassify obvious cases.
    """

    framing_patterns = [
        # Modal hedging
        (r'\b(could|might|may|would)\b', "modal_hedging"),
        # Anonymous attribution
        (r'\b(sources? say|experts? warn|analysts? believe|observers? note)\b', "anonymous_attribution"),
        # Emotional adjectives
        (r'\b(shocking|unprecedented|controversial|dramatic|devastating|alarming)\b', "emotional_adjective"),
        # Scope inflation
        (r'\b(crisis|collapse|surge|explode|soar|plummet)\b', "scope_inflation_verb")
    ]

    remediated = []
    reclassified_count = 0

    for stmt in statements:
        # Only remediate statements currently classified as A or B
        if stmt.classification in ["A-Verified", "B1", "B2", "B3", "B4"]:

            # Check for framing patterns
            detected_patterns = []
            for pattern, pattern_name in framing_patterns:
                if re.search(pattern, stmt.framing_removed_text or stmt.text, re.IGNORECASE):
                    detected_patterns.append(pattern_name)

            # If framing detected in "framing_removed" text, it wasn't properly removed
            if detected_patterns:
                logger.info(
                    f"Auto-remediation: {stmt.statement_id} reclassified to Class C "
                    f"(framing detected: {', '.join(detected_patterns)})"
                )

                corrected = stmt.copy(update={
                    "classification": "C",
                    "classification_reasoning": (
                        f"Originally classified as {stmt.classification}, but framing patterns detected: "
                        f"{', '.join(detected_patterns)}. Auto-reclassified as Class C (Performative). "
                        f"Original reasoning: {stmt.classification_reasoning}"
                    ),
                    "remediation_applied": True,
                    "original_classification": stmt.classification,
                    "detected_framing_patterns": detected_patterns
                })
                remediated.append(corrected)
                reclassified_count += 1
            else:
                remediated.append(stmt)
        else:
            remediated.append(stmt)

    logger.info(f"Auto-remediation reclassified {reclassified_count} statements to Class C")
    return remediated
```

### Strategy 3: Evidence Form Inconsistency

**Problem**: DirectQuote classified as Class A or C instead of Class B.

**Automatic Fix**: Reclassify to appropriate B tier.

```python
def remediate_evidence_form_inconsistency(statements: List[Statement]) -> List[Statement]:
    """
    Fix statements where evidence form doesn't match classification.
    """
    remediated = []

    for stmt in statements:
        # Rule: DirectQuote must be Class B (B1/B2/B3)
        if stmt.evidence_form == "DirectQuote":
            if stmt.classification not in ["B1", "B2", "B3"]:
                # Determine appropriate B tier
                if stmt.attribution and "official" in stmt.attribution.lower():
                    corrected_class = "B1"
                elif stmt.attribution and stmt.attribution != "Anonymous":
                    corrected_class = "B2"
                else:
                    corrected_class = "B3"

                logger.info(
                    f"Auto-remediation: {stmt.statement_id} is DirectQuote but was "
                    f"classified as {stmt.classification}. Reclassifying to {corrected_class}"
                )

                corrected = stmt.copy(update={
                    "classification": corrected_class,
                    "classification_reasoning": (
                        f"Originally classified as {stmt.classification}, but evidence form is DirectQuote. "
                        f"Quotes are speech acts (Class B), not events (Class A) or framing (Class C). "
                        f"Auto-reclassified to {corrected_class}."
                    ),
                    "remediation_applied": True,
                    "original_classification": stmt.classification
                })
                remediated.append(corrected)
            else:
                remediated.append(stmt)

        # Rule: EditorialFraming must be Class C
        elif stmt.evidence_form == "EditorialFraming":
            if stmt.classification != "C":
                logger.info(
                    f"Auto-remediation: {stmt.statement_id} is EditorialFraming but was "
                    f"classified as {stmt.classification}. Reclassifying to C"
                )

                corrected = stmt.copy(update={
                    "classification": "C",
                    "classification_reasoning": (
                        f"Originally classified as {stmt.classification}, but evidence form is EditorialFraming. "
                        f"Editorial framing is always Class C (Performative). Auto-reclassified."
                    ),
                    "remediation_applied": True,
                    "original_classification": stmt.classification
                })
                remediated.append(corrected)
            else:
                remediated.append(stmt)
        else:
            remediated.append(stmt)

    return remediated
```

---

## Enhanced Section: Testing Strategy for Multi-Agent System

### Unit Testing: Individual Agents

Each agent is tested in isolation with mock inputs:

```python
import pytest
from unittest.mock import Mock

def test_classification_agent_class_a_requires_citation():
    """Test that agent correctly requires citation for Class A"""

    # Mock input: Statement without citation
    statement = Statement(
        statement_id="test_001",
        text="The GDP increased 2.3%",
        evidence_form="Data",
        citation_text=None,
        citation_url=None,
        framing_removed_text="GDP increased 2.3%"
    )

    # Execute agent
    result = ClassificationAgent().classify(statement)

    # Verify: Should NOT classify as Class A
    assert result.classification != "A-Verified", \
        "Agent should not classify uncited data as Class A-Verified"

    # Verify: Should classify as Class B4
    assert result.classification == "B4", \
        "Uncited factual assertion should be Class B4 (Journalist Assertion)"


def test_framing_removal_agent_removes_adjectives():
    """Test that framing removal agent correctly identifies emotional adjectives"""

    statement = Statement(
        statement_id="test_002",
        text="The embattled senator desperately clung to power by making a controversial vote",
        evidence_form="ReportedAction"
    )

    result = FramingRemovalAgent().remove_framing(statement)

    # Verify: Core assertion preserved
    assert "senator" in result.framing_removed_text
    assert "vote" in result.framing_removed_text

    # Verify: Framing removed
    assert "embattled" not in result.framing_removed_text
    assert "desperately" not in result.framing_removed_text
    assert "controversial" not in result.framing_removed_text

    # Verify: Removals documented
    removed_terms = [r["removed_text"] for r in result.framing_removed]
    assert "embattled" in removed_terms
    assert "desperately" in removed_terms
```

### Integration Testing: Phase Transitions

Test that phases correctly hand off data:

```python
def test_phase_1_to_phase_2_integration():
    """Test that Phase 1 output is valid input for Phase 2"""

    # Execute Phase 1
    article_text = load_test_article("sample_article_01.txt")
    phase_1_result = execute_phase_1(article_text)

    # Verify Phase 1 output schema
    validate_statement_registry_schema(phase_1_result)

    # Execute Phase 2 with Phase 1 output
    phase_2_result = execute_phase_2(phase_1_result)

    # Verify Phase 2 output schema
    validate_classified_statements_schema(phase_2_result)

    # Verify all statements from Phase 1 are present in Phase 2
    phase_1_ids = {s["statement_id"] for s in phase_1_result["statements"]}
    phase_2_ids = {s["statement_id"] for s in phase_2_result["statements"]}
    assert phase_1_ids == phase_2_ids, "Phase 2 must process all Phase 1 statements"
```

### Gate Testing: Validation Logic

Test that gates correctly enforce rules:

```python
def test_gate_2_fails_on_low_class_c():
    """Test that Gate 2 fails when Class C < 20%"""

    # Create test data with only 15% Class C
    statements = [
        Statement(statement_id=f"stmt_{i}", classification="A-Verified", citation_text="link")
        for i in range(70)
    ] + [
        Statement(statement_id=f"stmt_{i}", classification="C")
        for i in range(70, 85)
    ]

    # Execute Gate 2
    gate = Gate2StateMachine(statements)
    result = gate.execute()

    # Verify: Gate should FAIL
    assert result["status"] == "failed"
    assert any("class_c_minimum_threshold" in v["rule"] for v in result["violations"])


def test_gate_2_clears_on_valid_classification():
    """Test that Gate 2 passes with valid classifications"""

    statements = [
        Statement(statement_id="stmt_1", classification="A-Verified",
                 citation_text="https://example.com", evidence_form="Data"),
        Statement(statement_id="stmt_2", classification="B1",
                 evidence_form="DirectQuote", attribution="Senator X"),
        Statement(statement_id="stmt_3", classification="C",
                 evidence_form="EditorialFraming"),
        Statement(statement_id="stmt_4", classification="C",
                 evidence_form="EditorialFraming"),
    ]

    gate = Gate2StateMachine(statements)
    result = gate.execute()

    # Verify: Gate should CLEAR (50% Class C)
    assert result["status"] == "cleared"
    assert len(result["violations"]) == 0
```

### End-to-End Testing: Complete Audits

Test full pipeline with known articles:

```python
def test_full_audit_known_propaganda_article():
    """Test that system correctly identifies high-framing article"""

    # Load known propaganda article (high framing, low factual content)
    article = load_test_article("propaganda_example_01.txt")

    # Execute full audit
    result = execute_full_audit(article)

    # Verify: High CDR (Class C Density Ratio)
    assert result["metrics"]["CDR"] > 60, \
        "Propaganda article should have >60% Class C statements"

    # Verify: High CDI (Citation Deficit Index)
    assert result["metrics"]["CDI"] > 50, \
        "Propaganda article should have >50% unsourced factual assertions"

    # Verify: Low ESR (Evidentiary Support Ratio)
    assert result["metrics"]["ESR"] < 40, \
        "Propaganda article should have <40% evidentiary support for claims"

    # Verify: NIS = Unsupported
    assert result["metrics"]["NIS"] == "Unsupported", \
        "Propaganda article core thesis should be unsupported"
```

---

---

## Enhanced Section: Orchestration Patterns & Agent Coordination

### The Orchestrator's Role

The orchestrator is **not an agent**—it's a deterministic state machine that:
1. Sequences phases in correct order
2. Enforces gates between phases
3. Handles errors and retries
4. Maintains audit trail
5. Never makes semantic decisions

```python
class AuditOrchestrator:
    """
    Central coordinator for multi-agent audit pipeline.
    Deterministic state machine - NO LLM calls in orchestrator itself.
    """

    def __init__(self, article_text: str, config: AuditConfig):
        self.article_text = article_text
        self.config = config
        self.state = OrchestrationState()
        self.audit_trail = []

    def execute(self) -> AuditResult:
        """Execute full audit pipeline with gates and error handling"""

        try:
            # Phase 1: Input Processing
            self._log_phase_start("PHASE_1")
            phase_1_result = self._execute_phase_1()
            self._save_artifact("statement_registry.json", phase_1_result)
            self._log_phase_complete("PHASE_1")

            # Phase 2: Classification & Framing Removal
            self._log_phase_start("PHASE_2")
            phase_2_result = self._execute_phase_2(phase_1_result)
            self._save_artifact("classified_statements.json", phase_2_result)

            # GATE 2: Validate classifications
            gate_2_result = self._execute_gate_2(phase_2_result)
            self._save_artifact("gate_2_checkpoint.json", gate_2_result)

            if gate_2_result["status"] == "failed":
                phase_2_result = self._remediate_and_retry_phase_2(phase_2_result, gate_2_result)

            self._log_phase_complete("PHASE_2")

            # Phase 3: Delta Analysis
            self._log_phase_start("PHASE_3")
            phase_3_result = self._execute_phase_3(phase_2_result)
            self._save_artifact("delta_analysis.json", phase_3_result)

            # GATE 3: Validate axiom compliance
            gate_3_result = self._execute_gate_3(phase_3_result, phase_2_result)
            self._save_artifact("gate_3_checkpoint.json", gate_3_result)

            if gate_3_result["status"] == "failed":
                raise GateFailureError("Gate 3 failed - Axiom violations detected", gate_3_result)

            self._log_phase_complete("PHASE_3")

            # Calculate Metrics (Algorithmic - No agent)
            self._log_phase_start("METRICS_CALCULATION")
            metrics = self._calculate_all_metrics(phase_2_result, phase_3_result)
            self._save_artifact("metrics.json", metrics)
            self._log_phase_complete("METRICS_CALCULATION")

            # Phase 5: Omission Analysis
            self._log_phase_start("PHASE_5")
            phase_5_result = self._execute_phase_5(phase_3_result, metrics)
            self._save_artifact("omission_analysis.json", phase_5_result)
            self._log_phase_complete("PHASE_5")

            # Output Generation
            self._log_phase_start("OUTPUT_GENERATION")
            final_report = self._generate_output(
                phase_1_result,
                phase_2_result,
                phase_3_result,
                phase_5_result,
                metrics,
                gate_2_result,
                gate_3_result
            )
            self._save_artifact("audit_report.json", final_report)
            self._log_phase_complete("OUTPUT_GENERATION")

            return AuditResult(
                status="SUCCESS",
                report=final_report,
                audit_trail=self.audit_trail
            )

        except Exception as e:
            self._log_error(str(e))
            return AuditResult(
                status="FAILED",
                error=str(e),
                audit_trail=self.audit_trail
            )

    def _execute_phase_1(self) -> Dict:
        """Execute Phase 1 agents in sequence"""

        # Agent 1: Detect article type
        article_type = self._invoke_agent(
            "ArticleTypeDetector",
            {"article_text": self.article_text}
        )

        # Early exit for satire
        if article_type["article_type"] == "Satire":
            raise SatireDetectedError("Article is satire - not subject to audit")

        # Agent 2: Extract headline metadata
        headline_data = self._invoke_agent(
            "HeadlineExtractor",
            {"article_text": self.article_text}
        )

        # Agent 3: Parse statements
        statements = self._invoke_agent(
            "StatementParser",
            {"article_text": self.article_text}
        )

        # Agent 4: Classify evidence forms
        statements_with_forms = self._invoke_agent(
            "EvidenceFormClassifier",
            {"statements": statements}
        )

        # Validate Phase 1 output
        self._validate_phase_1_output(statements_with_forms)

        return {
            "article_type": article_type,
            "headline": headline_data,
            "statements": statements_with_forms,
            "metadata": {
                "statement_count": len(statements_with_forms),
                "word_count": len(self.article_text.split())
            }
        }

    def _execute_phase_2(self, phase_1_data: Dict) -> Dict:
        """Execute Phase 2 agents with parallel processing where possible"""

        statements = phase_1_data["statements"]

        # PASS 1: Framing Removal (sequential for all statements)
        statements_framing_removed = self._invoke_agent(
            "FramingRemovalAgent",
            {"statements": statements}
        )

        # PASS 1: Extract citations (can run in parallel with framing removal conceptually,
        # but we do sequential for simplicity)
        statements_with_citations = self._invoke_agent(
            "CitationExtractor",
            {"statements": statements_framing_removed, "article_text": self.article_text}
        )

        # PASS 1: Classification (requires framing removal + citation extraction complete)
        classified_statements = self._invoke_agent(
            "ClassificationAgent",
            {"statements": statements_with_citations}
        )

        # ALGORITHMIC: Validate classifications
        validated_statements = self._algorithmic_classification_validation(classified_statements)

        # PASS 2: Domain-specific protocols (conditional based on patterns)

        # Check if statistical claims present
        has_stats = any(s["evidence_form"] == "Data" for s in validated_statements)
        if has_stats:
            stats_analysis = self._invoke_agent(
                "StatisticalManipulationDetector",
                {"statements": validated_statements}
            )
        else:
            stats_analysis = {"detected_manipulations": []}

        # Check if expert claims present
        has_experts = any("expert" in s["text"].lower() for s in validated_statements)
        if has_experts:
            expert_analysis = self._invoke_agent(
                "ExpertCredibilityAgent",
                {"statements": validated_statements}
            )
        else:
            expert_analysis = {"expert_assessments": []}

        return {
            "statements": validated_statements,
            "statistical_analysis": stats_analysis,
            "expert_analysis": expert_analysis,
            "metadata": {
                "total_statements": len(validated_statements),
                "classification_distribution": self._count_classification_distribution(validated_statements)
            }
        }

    def _execute_gate_2(self, phase_2_data: Dict) -> Dict:
        """Execute Gate 2 validation (pure algorithm)"""

        statements = phase_2_data["statements"]

        gate = Gate2StateMachine(statements)
        result = gate.execute()

        self._log_gate_result("GATE_2", result)

        return result

    def _remediate_and_retry_phase_2(self, phase_2_data: Dict, gate_result: Dict) -> Dict:
        """Apply automatic remediation for Gate 2 failures"""

        statements = phase_2_data["statements"]

        # Apply remediation strategies
        remediated = remediate_class_a_without_citation(statements)
        remediated = remediate_class_b4_with_citation(remediated)
        remediated = remediate_class_c_under_threshold(remediated)
        remediated = remediate_evidence_form_inconsistency(remediated)

        # Re-run Gate 2
        gate_retry = Gate2StateMachine(remediated)
        result_retry = gate_retry.execute()

        if result_retry["status"] == "failed":
            raise GateFailureError(
                "Gate 2 failed after automatic remediation. Manual review required.",
                result_retry
            )

        self._log_remediation("GATE_2", len(statements), len(remediated))

        return {
            **phase_2_data,
            "statements": remediated,
            "remediation_applied": True
        }

    def _calculate_all_metrics(self, phase_2_data: Dict, phase_3_data: Dict) -> Dict:
        """Calculate all metrics using pure algorithms (NO agents)"""

        statements = phase_2_data["statements"]
        delta_analysis = phase_3_data

        # Tier 1 Metrics (always calculated)
        esr = self._calculate_esr(delta_analysis["sub_claims"])
        nis = self._determine_nis(delta_analysis["core_thesis_support"])
        cdr = self._calculate_cdr(statements)
        cdi = self._calculate_cdi(statements)

        # Tier 2 Metrics (conditional)
        tier_2 = self._calculate_tier_2_metrics(statements, phase_2_data)

        # Tier 3 Qualitative (from agent analysis)
        ces = delta_analysis.get("counterfactual_engagement_score", "Absent")

        return {
            "tier_1": {
                "ESR": esr,
                "NIS": nis,
                "CDR": cdr,
                "CDI": cdi
            },
            "tier_2": tier_2,
            "tier_3": {
                "CES": ces
            },
            "trust_rating": self._calculate_trust_rating(esr, nis, cdr, cdi)
        }

    def _invoke_agent(self, agent_name: str, input_data: Dict) -> Dict:
        """
        Invoke agent with message bus pattern.
        Records all agent calls in audit trail.
        """

        message_id = str(uuid.uuid4())
        start_time = datetime.now()

        self._log_agent_invocation(agent_name, message_id, input_data)

        try:
            # Get agent from registry
            agent = self.agent_registry.get(agent_name)

            # Execute agent
            result = agent.execute(input_data)

            # Validate output schema
            self._validate_agent_output(agent_name, result)

            end_time = datetime.now()
            duration = (end_time - start_time).total_seconds()

            self._log_agent_completion(agent_name, message_id, duration, result)

            return result

        except Exception as e:
            self._log_agent_error(agent_name, message_id, str(e))
            raise AgentExecutionError(f"Agent {agent_name} failed: {str(e)}")

    def _log_agent_invocation(self, agent_name: str, message_id: str, input_data: Dict):
        """Record agent call in audit trail"""
        self.audit_trail.append({
            "event": "AGENT_INVOCATION",
            "agent": agent_name,
            "message_id": message_id,
            "timestamp": datetime.now().isoformat(),
            "input_size": len(json.dumps(input_data))
        })

    def _log_agent_completion(self, agent_name: str, message_id: str, duration: float, result: Dict):
        """Record agent completion in audit trail"""
        self.audit_trail.append({
            "event": "AGENT_COMPLETION",
            "agent": agent_name,
            "message_id": message_id,
            "timestamp": datetime.now().isoformat(),
            "duration_seconds": duration,
            "output_size": len(json.dumps(result))
        })
```

### Parallel Execution Pattern

Some agents can run in parallel when they don't depend on each other:

```python
def _execute_phase_2_parallel_pass(self, statements: List[Statement]) -> Dict:
    """
    Execute Phase 2 Pass 2 agents in parallel where dependencies allow.
    """

    # Identify which agents are needed
    agents_to_run = []

    if self._has_statistical_claims(statements):
        agents_to_run.append("StatisticalManipulationDetector")

    if self._has_expert_claims(statements):
        agents_to_run.append("ExpertCredibilityAgent")

    if self._has_multiple_sources(statements):
        agents_to_run.append("MultiSourceConflictDetector")

    # Run agents in parallel
    with ThreadPoolExecutor(max_workers=len(agents_to_run)) as executor:
        futures = {
            executor.submit(self._invoke_agent, agent_name, {"statements": statements}): agent_name
            for agent_name in agents_to_run
        }

        results = {}
        for future in as_completed(futures):
            agent_name = futures[future]
            try:
                results[agent_name] = future.result()
            except Exception as e:
                self._log_agent_error(agent_name, "parallel_execution", str(e))
                results[agent_name] = {"error": str(e)}

    return results
```

### Error Handling & Retry Strategy

```python
def _invoke_agent_with_retry(
    self,
    agent_name: str,
    input_data: Dict,
    max_retries: int = 3
) -> Dict:
    """
    Invoke agent with exponential backoff retry strategy.
    """

    for attempt in range(max_retries):
        try:
            result = self._invoke_agent(agent_name, input_data)
            return result

        except AgentExecutionError as e:
            if attempt < max_retries - 1:
                wait_time = 2 ** attempt  # Exponential backoff: 1s, 2s, 4s
                self._log_retry(agent_name, attempt + 1, wait_time)
                time.sleep(wait_time)
            else:
                # Final attempt failed
                self._log_retry_exhausted(agent_name, max_retries)
                raise

        except ValidationError as e:
            # Schema validation errors are not retryable
            self._log_validation_error(agent_name, str(e))
            raise
```

---

## Enhanced Section: Data Structure Specifications

### Complete Schema: Statement Registry (Phase 1 Output)

```python
from pydantic import BaseModel, Field, validator
from typing import Optional, Literal, List
from datetime import datetime

class Attribution(BaseModel):
    type: Literal["Named", "Anonymous", "Journalist", "None"]
    name: Optional[str] = None
    role: Optional[str] = None  # e.g., "Senator", "Professor", "CEO"
    institution: Optional[str] = None

class Statement(BaseModel):
    statement_id: str = Field(..., description="Unique identifier: stmt_<number>")
    text: str = Field(..., description="Verbatim text from article")
    paragraph_number: int = Field(..., ge=1)
    sentence_number: int = Field(..., ge=1)
    evidence_form: Literal["DirectQuote", "Paraphrase", "ReportedAction", "EditorialFraming", "Data"]
    attribution: Attribution
    context: Optional[str] = Field(None, description="Surrounding sentences for context")

    @validator('statement_id')
    def validate_statement_id_format(cls, v):
        if not v.startswith('stmt_'):
            raise ValueError(f"statement_id must start with 'stmt_', got: {v}")
        return v

class StatementRegistry(BaseModel):
    article_id: str
    headline: str
    subheadline: Optional[str] = None
    byline: Optional[str] = None
    publication_date: Optional[str] = None
    word_count: int
    statements: List[Statement]
    metadata: dict = Field(default_factory=dict)

    @validator('statements')
    def validate_minimum_statements(cls, v):
        if len(v) < 10:
            raise ValueError(f"Article must contain at least 10 statements, found: {len(v)}")
        return v
```

### Complete Schema: Classified Statements (Phase 2 Output)

```python
class FramingRemoval(BaseModel):
    removed_text: str
    removal_reason: Literal[
        "Adjective", "Adverb", "Metaphor", "ImpliedCausation",
        "PassiveVoice", "ScopeInflation", "ModalHedging"
    ]
    original_position: int  # Character position in original text

class ClassifiedStatement(BaseModel):
    # Inherited from Phase 1
    statement_id: str
    text: str
    paragraph_number: int
    sentence_number: int
    evidence_form: Literal["DirectQuote", "Paraphrase", "ReportedAction", "EditorialFraming", "Data"]
    attribution: Attribution

    # Added in Phase 2
    framing_removed_text: str = Field(..., description="Core assertion after framing removal")
    framing_removed: List[FramingRemoval] = Field(default_factory=list)

    citation_text: Optional[str] = Field(None, description="Citation or source reference text")
    citation_url: Optional[str] = Field(None, description="URL if hyperlink present")
    citation_type: Optional[Literal["Hyperlink", "InlineReference", "FootnoteReference"]] = None

    classification: Literal["A-Verified", "B1", "B2", "B3", "B4", "C"]
    classification_reasoning: str = Field(..., description="Explanation for classification")

    # Remediation tracking
    remediation_applied: bool = False
    original_classification: Optional[str] = None

    # Validation tracking
    validation_status: Literal["passed", "corrected", "flagged"] = "passed"
    validation_notes: List[str] = Field(default_factory=list)

    @validator('classification')
    def class_a_requires_citation(cls, v, values):
        """Enforces Axiom 1: Class A requires citation"""
        if v == "A-Verified":
            has_citation = values.get('citation_text') or values.get('citation_url')
            if not has_citation:
                raise ValueError(
                    f"Class A-Verified requires citation. Statement: {values.get('statement_id')}. "
                    f"If no citation exists, classify as B4 (Journalist Assertion)."
                )
        return v

    @validator('classification')
    def class_b4_prohibits_citation(cls, v, values):
        """Enforces: Class B4 means NO citation"""
        if v == "B4":
            has_citation = values.get('citation_text') or values.get('citation_url')
            if has_citation:
                raise ValueError(
                    f"Class B4 (Journalist Assertion) cannot have citation. Statement: {values.get('statement_id')}. "
                    f"If citation exists, classify as A-Verified."
                )
        return v

    @validator('evidence_form', 'classification')
    def direct_quote_must_be_class_b(cls, classification, values):
        """Enforces: Direct quotes are speech acts (Class B), not Class A or C"""
        if 'evidence_form' in values and values['evidence_form'] == "DirectQuote":
            if classification not in ["B1", "B2", "B3"]:
                raise ValueError(
                    f"DirectQuote must be classified as B1/B2/B3 (speech acts), not {classification}. "
                    f"Statement: {values.get('statement_id')}"
                )
        return classification

class ClassifiedStatements(BaseModel):
    article_id: str
    statements: List[ClassifiedStatement]
    phase_2_metadata: dict
    statistical_analysis: Optional[dict] = None
    expert_analysis: Optional[dict] = None
```

---

## Summary: Key Architectural Decisions

### Decision 1: No Agent Makes Final Determinations on Metrics

**Rationale**: Metrics must be deterministic and traceable. LLMs are unreliable at arithmetic and counting.

**Implementation**:
- Agents extract and classify
- Algorithms count and calculate
- Validators ensure traceability

### Decision 2: Immutable Phase Outputs

**Rationale**: Later phases should not modify earlier work. This enables debugging and prevents error concealment.

**Implementation**:
- Each phase writes to separate JSON file
- Files are write-once (immutable)
- Audit trail preserves all intermediate states

### Decision 3: Gates as State Machines, Not Agents

**Rationale**: Gates enforce rules deterministically. An agent-based gate could be "persuaded" to pass invalid data.

**Implementation**:
- Gates are pure Python code
- No LLM calls in gate logic
- Violations trigger automatic remediation or abort

### Decision 4: Agent Output Requires Validation

**Rationale**: Agents can violate rules even when explicitly instructed not to. Validation must be structural.

**Implementation**:
- Pydantic schema validation (type safety)
- Logic validators (rule enforcement)
- Algorithmic override (auto-correction)

### Decision 5: Separation of Extraction from Judgment

**Rationale**: Agents are better at finding patterns than making value judgments. Judgment creates bias risk.

**Implementation**:
- CitationExtractor finds text (extraction)
- Algorithm checks if text exists (judgment: null/not-null)
- No agent decides if citation is "good enough"

---

**END OF ENHANCEMENT DOCUMENT**

This document provides enhanced architectural patterns to be integrated into naf-system-design.md. Key additions:
1. Deeper agent-vs-algorithm separation
2. Concrete code examples of deterministic enforcement
3. Message bus and orchestration patterns
4. Complete data structure schemas with validation
5. Testing strategies
6. Error handling and remediation
7. Gate implementation as state machines

These enhancements strengthen the multi-agent architecture by removing discretion at critical decision points and enforcing rules structurally rather than through prompts.
