# Layered System Development Framework

## Overview

A system development framework built on the principle that each layer depends on the integrity of the layers above it. Skipping or underdeveloping an upstream layer is the root cause of most system failures. The layers are conceptually sequential — each depends on the one before it — but the discovery process is iterative. New information at any layer may refine upstream layers. Iteration is refinement, not discovery of fundamentals. Front-load the upstream work; the cost of learning increases by roughly an order of magnitude at each layer down.

---

## Layers

### Layer 1: Goal

The desired outcome or end state that directs behavior, decision-making, and resource allocation toward a specific result. The goal defines *what you're trying to achieve* without prescribing how.

A goal establishes the gap between where you are now and where you want to be. That gap motivates everything downstream.

The goal stays simple and aspirational. Parameters that feel like goal clarifications — time horizons, risk tolerance, cost thresholds — are actually SS (Soft Subjectivity) declarations that belong in the model's scope. The goal says *what you want*. The model says *under what terms and constraints you're pursuing it*.

**Boundary with Layer 2:** The goal says *what outcome you want*. The model says *how the relevant domain actually works*. The goal is aspirational; the model is descriptive.

---

### Layer 2: Model

A structured understanding of the problem domain — the concepts (entities you're reasoning about) and the theory (how they relate to each other). The model is descriptive, testable, and the thing you reason against when deriving requirements.

You need the model because you can't specify what a system must do if you don't understand the forces, entities, and dynamics it operates within.

#### Epistemic Discipline: SKD Integration

The model layer is where claims about reality enter the system. Those claims are a mix of observed facts, framework interpretations, and value judgments — and most people don't distinguish between them. Apply the Structured Knowledge Document (SKD) framework during model construction to pressure-test every claim going into the model.

SKD forces categorization of every model claim:

- **Hard Objectivity (HO):** Observer-independent, verifiable truth. "Orders arrive via Kafka with at-least-once delivery."
- **Framework-Dependent (FD):** True within a cited system. "PCI-DSS requires encryption at rest." Distinguish FD-settled (clear application) from FD-contested (disputed application requiring cited authority).
- **Soft Subjectivity (SS):** Rational value disagreement. "200ms latency is the acceptable threshold." Declare these explicitly as chosen values.
- **Epistemic Junk (EJ):** Invalid epistemic moves. Tribal signaling, motive attribution, unfalsifiable claims. Reject on contact.

A requirement derived from an HO-backed model claim is solid. A requirement derived from an undeclared SS assumption is a landmine.

SKD is a discipline applied *during* model construction, not a replacement for the model itself. It validates the inputs. The model organizes them into an operational structure (entity relationships, state machines, domain maps) that you can derive requirements from.

#### Scaling: Recursive Application

Complex domains contain multiple sub-domains, each with their own claims, frameworks, and unknowns. The framework handles this through recursive application — apply the same layered process to each sub-domain independently, then integrate the results into the parent model. This is a scaling mechanism, not an additional layer. The process is the same at every level of decomposition.

#### Model Complexity Varies

The model layer ranges from "document what we already know" to "run a multi-month research program" depending on the domain. For well-understood domains, the model is mostly existing knowledge organized and validated through SKD. For novel or complex domains (financial markets, distributed systems at new scale, unexplored technical territory), building the model *is* the hard work and requires substantial research and experimentation before downstream layers can proceed.

**Boundary with Layer 3:** The model describes reality; requirements prescribe what the system must do *within* that reality. This is the descriptive-to-prescriptive boundary.

---

### Layer 3: Requirements

The specific conditions, capabilities, and constraints that must be satisfied. Requirements bridge the gap between the model (how the domain works) and the design (how you'll build within it). They take abstract understanding and make it concrete enough to act on and verify.

Requirements answer: *what does this thing need to do, and within what boundaries?*

#### Types

- **Functional:** What the system does. "The system must reconcile out-of-order events within 5 seconds."
- **Non-Functional:** How well it does it. Performance, security, reliability, observability.
- **Constraints:** What you can't change. Budget, timeline, existing infrastructure, team capacity.

#### Quality Test

Good requirements are testable — you can definitively say whether they've been met. "The system must process 10,000 events per second with sub-100ms latency" is a requirement. "Make it fast" is a goal that hasn't been refined yet.

#### Derivation from Model

Requirements are derived from model claims, and the category of those claims determines the strength of the requirement. If the model says "orders can arrive out of sequence due to network partitioning" (HO), the requirement becomes "the system must handle out-of-order delivery and reconcile within N seconds" (where N is an SS threshold, declared explicitly). Model findings that are ignored become unaddressed risks. The crash risk finding in a momentum model directly produces a crash detection requirement; without that model work, the requirement never exists and you discover the gap in production.

**Boundary with Layer 4:** Requirements say *what must be true*. Design says *how you'll make it true*. This is the what-to-how boundary. Requirements are solution-agnostic; design commits to specific trade-offs and structures.

---

### Layer 4: System Design

The architecture, components, interfaces, and interactions needed to satisfy requirements. Design is the act of making trade-off decisions: consistency vs. availability, simplicity vs. flexibility, cost vs. performance. Every design choice closes some doors and opens others.

Design sits between requirements (what needs to be true) and implementation (the actual building).

#### What Good Design Does

- Decomposes a complex problem into manageable parts.
- Defines clear boundaries and contracts between those parts.
- Anticipates behavior under stress, failure, and change over time.
- Addresses the failure path, not just the happy path.

#### The Output

The output isn't code — it's a set of decisions and their rationale. The architecture diagram is the artifact. The real design lives in the reasoning behind *why* you chose this approach over that one, why you split this boundary here, why you accepted this trade-off in this context.

**Boundary with Layer 5:** Design says *what we're building*. The implementation plan says *how we get from here to there*. This is the architecture-to-execution boundary.

---

### Layer 5: Implementation Plan

The concrete sequence of steps, assignments, and timelines that translate a system design into working output. A single design can yield multiple implementation plans depending on team structure, risk tolerance, and sequencing strategy.

#### What It Addresses

- **Sequencing:** What must exist before something else can be built. You don't write the API layer before the data model it depends on.
- **Parallelism:** What can be worked on simultaneously.
- **Risk Ordering:** Tackle the hardest or most uncertain pieces early so you don't discover a fatal flaw late.
- **Milestones:** Intermediate checkpoints where you verify progress and validate the thing actually works so far.

#### Distinction from a Task List

An implementation plan encodes *dependencies and reasoning*, not just items. "Build the ingestion pipeline" is a task. "Build the ingestion pipeline first because the transformation layer and the monitoring system both depend on having real data flowing through it" is planning.

#### Accounting for Reality

Plans must account for integration points, testing, migration, rollback strategies, and the fact that estimates are always wrong. The best plans are structured enough to provide direction but flexible enough to absorb the inevitable surprises.

---

## Supporting Artifacts

These are not additional layers. They feed into the layers without changing the hierarchy.

### Glossary/Definitions

A dependency of the model layer. You can't build a coherent model if people are using the same terms to mean different things. In SKD terms, a good definition is an HO or FD claim about what a word means in this context: "In this system, 'order' means a committed purchase with a payment method attached, not a cart or a quote." This is Chamber 1 (scope declaration) work. Conceptually part of the model, extracted to its own file when domain complexity makes inline definitions unreadable.

### Research/Experiments/POCs

An evidence generation mechanism that serves multiple layers. Research reduces the [Unknown] count in the model by converting unverified claims into HO claims through deliberate investigation.

- At the model layer: "We believe momentum works" is SS until you backtest it; then it becomes HO with data.
- At the design layer: "We think this architecture meets the latency requirement" is SS until you run a POC; then it's verified or refuted.
- At the requirements layer: Research can surface constraints or dynamics that produce new requirements that wouldn't have existed otherwise.

Using SKD terminology, research converts [Unknown-general], [Unknown-source], and [Unknown-contested] claims into HO claims. This is the mechanism by which front-loading the model work pays off — running a targeted experiment at the model layer is cheap compared to discovering the same thing doesn't work at implementation.

---

## Layer Summary

| Layer | Contains | Key Question | Boundary |
|-------|----------|--------------|----------|
| Goal | Desired outcome | What are we trying to achieve? | Aspirational to Descriptive |
| Model | Domain understanding (validated via SKD) | How does the relevant domain actually work? | Descriptive to Prescriptive |
| Requirements | Functional, non-functional, constraints | What must be true? | What to How |
| System Design | Architecture, trade-offs, rationale | How will we make it true? | Architecture to Execution |
| Implementation Plan(s) | Sequence, dependencies, milestones | How do we get from here to there? | — |

| Supporting Artifact | Relationship | Purpose |
|---------------------|-------------|---------|
| Glossary/Definitions | Component of Model, extracted for scale | Shared terminology |
| Research/Experiments/POCs | Evidence generation for Model, Design, Requirements | Convert [Unknown] claims to HO |

---

## Core Principle

The cost of learning increases by roughly an order of magnitude at each layer down. Changing your understanding at the model layer costs almost nothing — it's thinking and research. That same discovery at the requirements layer means rewriting specs. At the design layer it means rearchitecting. At implementation it means throwing away code. Front-load the upstream work. Iterate when new information surfaces, but don't use iteration as an excuse to under-invest in the model.

---

## Walkthrough: Momentum Trading System

A condensed example demonstrating the framework applied to a single sub-domain of an automated trading system.

### Layer 1: Goal

Build an automated momentum-based trading system that outperforms SPY on a risk-adjusted basis over rolling 3-year periods.

### Layer 2: Model

**Chamber 1 (Scope):** Establish whether momentum is a real, exploitable phenomenon in US equities and under what conditions it works and fails.

- Frameworks invoked: Modern portfolio theory for risk-adjusted measurement via Sharpe ratio (FD-settled). SEC and FINRA regulations for trading constraints (FD-settled).
- SS declarations: Risk-adjusted returns over absolute returns. 3-year evaluation window. US equities as the universe.

**Chamber 2 (HO Claims, populated through research):**

- Academic studies (Jegadeesh and Titman, 1993) documented that stocks with high returns over 3-12 months continue to outperform over the next 3-12 months.
- Momentum strategies experienced a significant drawdown in 2009.
- Transaction costs for momentum strategies are higher than buy-and-hold due to portfolio turnover.
- Momentum exhibits crash risk: sharp, sudden reversals that can wipe out years of gains in weeks.

**Chamber 3 (Logic):**

- HO (momentum has historically outperformed) + FD (Sharpe ratio) = momentum has produced a Sharpe ratio of roughly 0.5-0.7, compared to SPY at roughly 0.4.
- HO (momentum crashes happen) + SS (risk tolerance) = how much crash risk is acceptable for average outperformance? This directly drives requirements.
- Critical gap: *Why* does momentum work? Behavioral explanation (underreaction then herding) is SS. Risk-based explanation (compensation for crash risk) is also SS. Mechanism is [Unknown-contested]. Must be declared explicitly.

**Chamber 4 (Interpretation):** Momentum appears to be a real phenomenon with historical evidence of outperformance, carrying severe crash risk. Mechanism is [Unknown-contested]. Exploitability after transaction costs is supported by evidence but sensitive to implementation details.

### Layer 3: Requirements

Derived from model findings:

- **Functional:** Rank US equities by trailing 12-month returns monthly. Rebalance portfolio to hold top N percentile. Execute trades within market hours. Detect momentum crash conditions and execute predefined risk-reduction protocol.
- **Non-Functional:** Rebalancing computation completes within 30 minutes of market open. Backtest engine supports 20+ years of daily data.
- **Constraints:** Operate within pattern day trader rules. Transaction costs below X% of portfolio annually. Initial capital of $Y.

Note: the crash detection requirement exists *only because* the model surfaced crash risk as an HO finding. Without the model work, this requirement would be missing entirely.

### Layer 4: System Design

Data pipeline for price history. Ranking algorithm. Portfolio construction logic. Execution layer integrated with brokerage API. Risk monitoring module. Backtesting framework that shares logic with the live system to ensure parity.

Key design trade-off: crash detection as simple drawdown threshold (simple, explainable, possibly too slow) versus regime detection model (complex, faster, harder to trust). Decision informed by the model's finding that crashes are sudden.

### Layer 5: Implementation Plan

1. Data pipeline — everything depends on it.
2. Backtesting engine — validate strategy logic before building execution.
3. Portfolio construction and ranking.
4. Execution integration with brokerage.
5. Risk monitoring and crash detection.
6. Paper trading for N months before live deployment.

Risk ordering: backtesting early, because if the strategy doesn't survive historical validation, nothing else matters.
