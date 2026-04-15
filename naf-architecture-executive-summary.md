# NAF Multi-Agent Architecture - Executive Summary
## Quick Reference Guide

**Version:** 2.0
**Date:** 2026-01-30
**Framework:** NAF v2.1
**Full Documentation:** See `naf-system-design.md` (9K+ lines)

---

## The Core Problem

The Narrative Audit Framework v2.1 is a comprehensive 80+ step protocol for separating facts from framing in news articles. Single-agent implementations fail because:

1. **Zero-Knowledge Violation**: LLMs can't reliably avoid using training data to verify facts
2. **Cognitive Overload**: 15+ sub-protocols overwhelm single agents, causing execution failures
3. **Metric Gaming**: Agents output metrics without doing underlying calculations
4. **Inconsistent Classification**: Variable performance on identical inputs

## The Architectural Solution

**Multi-agent architecture** that decomposes the NAF framework into:
- **31 specialized agents** for reasoning tasks (semantic understanding, pattern recognition)
- **19 deterministic algorithms** for computational tasks (counting, arithmetic, rule enforcement)

**Key Insight**: Agents extract, algorithms enforce. At no point does any agent decide if a fact is "verified"—this is determined structurally.

---

## Architecture in 3 Principles

### 1. Agents for Reasoning, Algorithms for Calculation

**Use LLM Agents For:**
- Semantic parsing (identifying statements, extracting claims)
- Context interpretation (is this sarcasm? is this a formal setting?)
- Pattern recognition (semantic drift, juxtaposition)
- Ambiguity resolution (Class B2 vs B3 distinction)

**Use Algorithms For:**
- All metric calculations (ESR, CDR, CDI, NIS)
- All counting operations
- All percentage calculations
- Classification validation
- Gate enforcement
- Trust rating determination

**Rule**: If task can be expressed as "count X where Y" or "if A then B", it MUST be an algorithm.

### 2. Zero-Knowledge Enforcement Through Structure

**The Problem**: When you ask an LLM "Is this fact verified?", it will unconsciously use training data.

**The Solution**: Never ask the LLM that question.

**How It Works**:
```
Agent 1: CitationExtractor
  Task: Find citation text in article (string extraction only)
  Output: {"citation_text": null}

Algorithm: CitationPresenceChecker
  Task: Boolean check (pure logic, no LLM)
  Logic: has_citation = (citation_text is not None)
  Output: {"has_citation": false}

Algorithm: ClassificationEnforcer
  Task: Apply immutable rule
  Logic: if (class == "A" AND has_citation == false) → class = "B4"
  Output: {"final_class": "B4"}
```

**Result**: Agent never makes verification judgment. Structure enforces it.

### 3. Sequential Processing with Gates

```
Phase 1: Input Processing → StatementRegistry.json
   ↓
Phase 2: Classification → ClassifiedStatements.json
   ↓ [GATE 2: Class C ≥ 20%? All Class A have citations?]
   ↓
Phase 3: Delta Analysis → Metrics.json + DeltaAnalysis.json
   ↓ [GATE 3: Axiom compliance? Metrics traceable?]
   ↓
Phase 5: Omission Analysis → OmissionLog.json
   ↓
Output: FinalReport.md
```

**Gates are algorithms** (no LLM) that validate quality before proceeding.

---

## Complete NAF Mapping: Agent vs. Algorithm

### Phase 1: Input Processing
- **Agents**: ArticleTypeDetector, HeadlineExtractor, StatementParser, EvidenceFormClassifier, CitationExtractionAgent, SourceAuthorityClassifier, ExpertCredibilityAgent, ChainOfCustodyTracker
- **Algorithms**: MetadataValidator, StatementCountValidator, CitationFormatValidator, ExpertTierValidator, ChainLengthCounter
- **Ratio**: 73% Agent / 27% Algorithm

### Phase 2: Classification & Framing Removal
- **Agents**: SemanticDriftDetector, ModifyingLanguageDetector, CoreAssertionExtractor, TemporalFallacyDetector, ResponsibilityObfuscationDetector, ScopeInflationDetector, ModalHedgingDetector, ImplicitPremiseDetector, JuxtapositionDetector, ClassificationAgent, StatisticalManipulationDetector, VisualManipulationDetector, MultiSourceConflictDetector, InternalContradictionDetector
- **Algorithms**: ClassAValidator, ClassB4Validator, ClassDistributionCounter, CDRCalculator, SMICalculator, VDMICalculator, MSCCCalculator, CICalculator, Gate2Validator
- **Ratio**: 60% Agent / 40% Algorithm

### Phase 3: Delta Analysis
- **Agents**: NarrativePitchExtractor, SubClaimDecomposer, CentralityLabeler, QuoteContextVerifier, CounterfactualAnalyzer, EvidenceMatchingAgent, SupportStatusLabeler, InferentialLeapDetector, SyntheticNarrativeDetector
- **Algorithms**: EvidenceLockerBuilder, ESRCalculator, NISCalculator, CDICalculator, SNFCalculator, ImplicitPremiseReconciler, Gate3Validator
- **Ratio**: 54% Agent / 46% Algorithm

### Phase 5: Omission Analysis
- **Agents**: ContextMapper, OmissionDetector, OmissionClassifier
- **Algorithms**: None (requires domain judgment throughout)
- **Ratio**: 100% Agent / 0% Algorithm

### Output Generation
- **Agents**: ReportGenerator
- **Algorithms**: TrustRatingCalculator, ArtifactPackager
- **Ratio**: 33% Agent / 67% Algorithm

**Overall System**: 62% Agent / 38% Algorithm

---

## Key Data Structures

All data structures use JSON with Pydantic validation.

### StatementRegistry (Phase 1 Output)
```json
{
  "article_metadata": {...},
  "statements": [
    {
      "id": "uuid",
      "text": "verbatim statement",
      "evidence_form": "DirectQuote | Paraphrase | ReportedAction | EditorialFraming | Data",
      "attribution": {...},
      "contains_quote": boolean,
      "quote_text": "string | null"
    }
  ]
}
```

### ClassifiedStatements (Phase 2 Output)
```json
{
  "classified_statements": [
    {
      "statement_id": "uuid",
      "classification": "A-Verified | B1 | B2 | B3 | B4 | C",
      "framing_removed_text": "string",
      "citation": {
        "has_citation": boolean,
        "citation_text": "string | null",
        "citation_url": "string | null"
      },
      "manipulation_flags": {...}
    }
  ],
  "class_distribution": {...}
}
```

### Metrics (Phase 3 Output)
```json
{
  "tier_1_primary": {
    "esr": {"percentage": float, "interpretation": "Low | Moderate | High"},
    "nis": {"score": "Supported | Partially Supported | Unsupported"},
    "cdr": {"percentage": float, "propaganda_threshold_exceeded": boolean},
    "cdi": {"percentage": float, "interpretation": "Strong | Moderate Gap | Severe Deficit"}
  },
  "tier_2_manipulation": {
    "cvi": int,
    "smi": int,
    "tfc": int,
    "snf": int,
    "ci": int,
    "ipc": int,
    "vdmi": int,
    "mscc": int,
    "sdc": int
  },
  "tier_3_qualitative": {
    "ces": "Strong | Weak | Absent"
  }
}
```

---

## Critical Algorithms (Deterministic, No LLM)

### Gate 2 Validator
```python
def gate_2_validator(classified_statements):
    total = len(classified_statements)
    class_c_count = count(s for s in statements if s.classification == "C")
    class_c_pct = (class_c_count / total) * 100

    if class_c_pct < 20:
        return {"status": "FAILED", "reason": "Class C below 20% threshold"}

    # Check all Class A have citations
    class_a_no_citation = [s for s in statements
                           if s.classification == "A-Verified"
                           and not s.citation.has_citation]

    if class_a_no_citation:
        return {"status": "FAILED", "reason": "Class A without citations"}

    return {"status": "CLEARED"}
```

### ESR Calculator
```python
def calculate_esr(sub_claims):
    total = len(sub_claims)
    supported = count(c for c in sub_claims if c.support_status == "Supported")
    percentage = (supported / total) * 100

    interpretation = (
        "Low Support" if percentage < 50
        else "Moderate Support" if percentage <= 75
        else "High Support"
    )

    return {
        "supported_claims": supported,
        "total_claims": total,
        "percentage": round(percentage, 2),
        "interpretation": interpretation
    }
```

### CDI Calculator
```python
def calculate_cdi(classified_statements):
    class_a = count(s for s in statements if s.classification == "A-Verified")
    class_b4 = count(s for s in statements if s.classification == "B4")

    denominator = class_a + class_b4

    if denominator == 0:
        return {"percentage": None, "interpretation": "N/A - No Factual Claims"}

    percentage = (class_b4 / denominator) * 100

    interpretation = (
        "Strong Citation" if percentage <= 25
        else "Moderate Gap" if percentage <= 50
        else "Severe Deficit"
    )

    return {
        "percentage": round(percentage, 2),
        "interpretation": interpretation,
        "trace": {
            "class_a_ids": [s.id for s in statements if s.classification == "A-Verified"],
            "class_b4_ids": [s.id for s in statements if s.classification == "B4"]
        }
    }
```

### Trust Rating Calculator
```python
def calculate_trust_rating(metrics):
    cdi = metrics["tier_1_primary"]["cdi"]["percentage"]
    esr = metrics["tier_1_primary"]["esr"]["percentage"]

    # Strict order - first match wins
    if cdi and cdi > 50:
        return "🔴 LOW (Severe citation deficit)"

    if esr < 50:
        return "🔴 LOW (Low evidentiary support)"

    if esr > 75 and (not cdi or cdi < 20):
        return "🟢 HIGH (Strong support + solid citations)"

    return "🟡 MEDIUM (Mixed indicators)"
```

---

## Three Mandatory Gates

### Gate 1: Input Validation (After Phase 1)
**Checks:**
- Article parseable
- Word count ≥ 100
- Statements ≥ 5

**On Failure:** Abort (invalid input)

### Gate 2: Classification Quality (After Phase 2)
**Checks:**
- Class C ≥ 20% (framing detection)
- All Class A have citations (Axiom 1)
- Classification rules valid

**On Failure:** Retry Phase 2 (max 2 retries), then abort

### Gate 3: Axiom Compliance (After Phase 3)
**Checks:**
- All "Supported" claims have Class A/B evidence (Axiom 1)
- Sub-claims stripped of framing (Axiom 2)
- Metrics traceable to source statements (Axiom 3)
- ESR-NIS Paradox detection (flag, not violation)

**On Failure:** Retry Phase 3 (max 2 retries), then abort

---

## Implementation Quickstart

### Technology Stack
- **Language**: Python 3.11+
- **Orchestration**: Prefect or Temporal
- **LLM SDK**: Anthropic Claude SDK
- **Validation**: Pydantic v2
- **Database**: PostgreSQL (state persistence)
- **Message Bus**: Redis Streams
- **Caching**: Redis
- **API**: FastAPI

### Phased Implementation (15-16 weeks)

**Phase 0: Foundation (2 weeks)**
- Define JSON schemas (Pydantic models)
- Build orchestrator framework
- Set up infrastructure (DB, Redis)

**Phase 1: Input Processing (2 weeks)**
- Implement Phase 1 agents
- Implement Gate 1 validation
- Test on sample articles

**Phase 2: Classification (3 weeks)**
- Implement Phase 2 agents + algorithms
- Implement Gate 2 validation
- Test classification quality

**Phase 3: Delta Analysis (3 weeks)**
- Implement Phase 3 agents + algorithms
- Implement Gate 3 validation
- Test end-to-end pipeline

**Phase 4: Integration & Testing (2 weeks)**
- Integrate all phases
- Implement retry logic
- Performance testing

**Phase 5: Omission Analysis & Output (2 weeks)**
- Implement Phase 5 agents
- Implement ReportGenerator
- Test output format

**Phase 6: Deployment & Monitoring (1-2 weeks)**
- Deploy to production
- Set up monitoring & observability
- Create operational runbooks

### Cost Estimates (Using Claude 3 Sonnet)
- **Per Article**: $0.20 - $0.35 (optimized)
- **1K articles/month**: $230 - $300 (including infrastructure)
- **10K articles/month**: $2,000 - $2,700
- **100K articles/month**: $19,000 - $26,000

**Cost Optimization:**
- Use Claude Haiku for simple extraction tasks (50% reduction)
- Cache article content (avoid duplicate audits)
- Batch processing (reduce API overhead)

---

## Key Differentiators from Single-Agent Approach

| Aspect | Single-Agent | Multi-Agent Architecture |
|--------|-------------|--------------------------|
| **Bias Control** | LLMs struggle to ignore training data | Structural enforcement prevents knowledge leakage |
| **Consistency** | Variable performance on identical inputs | Deterministic algorithms + specialized agents = stable output |
| **Debugging** | Black box - hard to identify failure point | Clear agent boundaries + audit trails = easy debugging |
| **Scalability** | Sequential bottleneck | Parallel execution where dependencies allow |
| **Metrics** | Often "gamed" (outputted without calculation) | Algorithms calculate metrics from validated data structures |
| **Error Recovery** | Retry entire audit from scratch | Targeted remediation at phase level |
| **Cost** | ~$0.50+ per article | ~$0.20 per article (strategic model use) |

---

## Success Criteria

### Technical Validation
- ✅ Class C detection rate ≥ 20% (framing detection works)
- ✅ Zero Class A without citations (zero-knowledge enforcement works)
- ✅ Metric calculations 100% traceable (no gaming)
- ✅ Classification consistency ≥ 85% on re-test
- ✅ Gate pass rate ≥ 80% (retries < 20% of audits)

### Production Readiness
- ✅ End-to-end audit completion time < 60 seconds
- ✅ Cost per audit < $0.30
- ✅ 99% uptime
- ✅ Audit results reproducible (same article → same metrics)
- ✅ Complete audit trail (every classification decision traceable)

---

## Quick Reference: Agent Responsibilities

**DO USE AN AGENT WHEN:**
- Task requires semantic understanding
- Context interpretation needed
- Pattern recognition across text
- Ambiguity resolution required
- Judgment about sufficiency/importance needed

**DO USE AN ALGORITHM WHEN:**
- Task is counting or arithmetic
- Task is boolean logic (if/then)
- Task is rule enforcement
- Task is data structure transformation
- Task is schema validation

**HYBRID (Agent + Algorithm) WHEN:**
- Agent extracts/suggests → Algorithm validates/enforces
- Example: Classification (Agent suggests class → Algorithm enforces citation rule)

---

## Common Pitfalls to Avoid

1. **❌ Asking agents to calculate metrics**
   - ✅ Instead: Agents classify → Algorithms calculate from classifications

2. **❌ Combining extraction with evaluation in single agent**
   - ✅ Instead: Agent extracts citation text → Algorithm checks if non-null

3. **❌ Allowing agents to override validation rules**
   - ✅ Instead: Algorithm enforces rules with no exceptions

4. **❌ Sequential processing where parallel is possible**
   - ✅ Instead: Run independent agents (ExpertCredibility, Statistical Flags) in parallel

5. **❌ Skipping gates to "speed up" processing**
   - ✅ Instead: Gates prevent error propagation—skip them at your peril

6. **❌ Using same agent temperature for all tasks**
   - ✅ Instead: Low temp (0.0-0.2) for classification, higher (0.5-0.7) for synthesis

---

## Next Steps

1. **Review Full Architecture**: See `naf-system-design.md` for complete technical specification
2. **Define JSON Schemas**: Start with Pydantic models for all data structures
3. **Build MVP**: Implement Phase 1 + Phase 2 + Gate 2 as proof-of-concept
4. **Test Rigorously**: Use sample articles with known characteristics to validate
5. **Iterate**: Refine agent prompts based on classification quality metrics

---

## Contact & Resources

**Full Documentation**: `naf-system-design.md` (9,000+ lines with complete implementation details)
**Framework Specification**: `narrative-audit-framework.md` (NAF v2.1 protocol)

**Key Sections in Full Doc:**
- Complete NAF-to-System Mapping (every step mapped to agent/algorithm)
- Data Structures (full JSON schemas with Pydantic validation)
- Agent Definitions (all 31 agents with prompts)
- Algorithmic Components (all 19 algorithms with pseudo-code)
- Processing Pipeline (end-to-end flow diagrams)
- Quality Assurance & Gates (validation logic)
- Implementation Roadmap (16-week phased approach)
- Prompt Engineering Best Practices (prompt templates and patterns)

---

**Version History:**
- v2.0 (2026-01-30): Complete architecture with NAF v2.1 mapping
- v1.1 (2026-01-30): Initial multi-agent design
- v1.0 (2026-01-29): Conceptual framework

**Status**: Production-ready architecture specification. Ready for implementation.
