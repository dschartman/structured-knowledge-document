# LSD for Agentic LLMs: Design Notes

**Date**: 2026-02-18
**Context**: Conversation exploring how to encode Layered System Development (LSD) as an executable protocol for LLM agents (Claude Code specifically), rather than a thinking discipline for humans.

---

## The Core Realization

The LSD framework and SKD already function as an interaction protocol between Don and LLM agents. Don dictates high-level thinking, the LLM generates a structured first pass, Don reviews and talks through it, the LLM refines, repeat until accurate. The frameworks constrain the LLM from making the leaps that fluency enables.

The problem: Don is manually enforcing the protocol. He repeatedly says "we're in the model layer," re-references documents, redirects when the agent jumps ahead. His cognitive effort goes to three things:

1. The actual problem (domain thinking, decisions, feedback)
2. Constraining the agent ("we're in the model layer, not implementation")
3. Managing artifacts (deciding when to read/write files, what goes where)

The goal: eliminate #2 and #3. The human only spends effort on #1.

---

## Why LSD Is More Valuable for LLMs Than Humans

LLMs have the same failure modes the framework guards against, but worse:

- **Jumping ahead** — generating solutions before understanding the problem
- **Confident wrongness** — stating things fluently that aren't verified
- **Silent value judgments** — making SS decisions without surfacing them, because they're embedded in phrasing
- **Context loss** — losing track of decisions across long conversations
- **Over-generation** — producing more than what can reasonably be reviewed

The key insight: **for humans, shortcuts have a rational justification — thinking is expensive. For an LLM, there is no cost difference between the shortcut and the proper path.** The same tokens, the same time. There's zero reason for an LLM to make a local edit without propagating its impact through the system. The framework should enforce the proper path every time because there's no cognitive savings from skipping steps.

---

## What "Protocol" Actually Means

"Protocol" is easy to say and hard to make real. Loading a big instruction set into context and hoping the LLM follows it isn't a protocol — it's a wish. Context degrades. Rules get diluted by conversation history.

**The protocol's power comes from files, not from instructions in context.**

The conversation is ephemeral — it degrades, compresses, and the agent drifts. Files on disk are permanent and can be re-read at exactly the right moment. The protocol defines a cycle:

1. **Read** the current authoritative artifact from disk
2. **Generate** a first pass within a defined scope
3. **Write** it to a file
4. **Present** it to the human with decisions surfaced
5. **Listen** to feedback (dictation)
6. **Read** the file again (from disk, not from memory), apply feedback, write the update
7. Repeat until the human approves
8. **Gate**: human says "move on," agent reads the relevant files to begin the next layer

At step 6, the agent reads from disk, not from its memory of what it wrote. Context drift doesn't corrupt the work. Each iteration is grounded in the actual artifact.

The protocol is small — not a 200-line system prompt. A short set of rules for the current stage. When you move to Layer 3, a different small set loads. The agent only holds the rules for what it's doing right now.

---

## Implementation: Claude Code Skills

Claude Code skills are the implementation mechanism. Each skill is a directory with a `SKILL.md` entrypoint and optional supporting files. Skills can control:

- Whether the human or the agent can invoke them (`disable-model-invocation`)
- What tools the agent can use (`allowed-tools`)
- Whether they run in the main context or a forked subagent (`context: fork`)
- Supporting files that load on demand (not all upfront)

### Proposed Skill Structure

```
~/.claude/skills/
  lsd-goal/
    SKILL.md                <- Layer 1 rules: what to read, what to produce, boundaries
  lsd-model/
    SKILL.md                <- Layer 2 rules: SKD discipline, claim categorization
    skd-chambers.md         <- Chamber rules (supporting file, loaded on demand)
    verification.md         <- Self-check rules before presenting
  lsd-requirements/
    SKILL.md                <- Layer 3 rules: derivation from model, traceability
  lsd-design/
    SKILL.md                <- Layer 4 rules: trade-offs, component decomposition
  lsd-implementation/
    SKILL.md                <- Layer 5 rules: sequencing, dependencies, milestones
  lsd-review/
    SKILL.md                <- Review/verification prompt (generalized)
```

### How Skills Solve the Enforcement Problem

Each layer skill has `disable-model-invocation: true`. The human decides when to enter a layer — the agent can't jump to `/lsd-requirements` on its own. Layer transitions are explicit human actions, not instructions the agent might drift from.

Each skill defines:
- **Read**: what files to load as input (goal, model, evidence)
- **Produce**: what the deliverable is
- **Verify**: what to check before presenting (no prescriptive language in model, all claims categorized, etc.)
- **Stop**: when to pause for human input (SS decisions, gate approvals)
- **Don't**: what's out of bounds at this layer

### Key Design Decisions

**SS decisions are a hard stop.** Every time the agent identifies a value judgment that shapes what comes next, it stops and presents it as a decision for the human. The framework has a hard rule: SS decisions block layer transitions until a human confirms them.

**Verification is automated.** The checks from the SKD review process (HO atomicity, boundary violations, missing references, prescriptive language in descriptive sections) run before the agent presents work. Not a separate review step — built into the layer skill.

**Supporting files load on demand.** The full SKD chamber rules are in `skd-chambers.md` — referenced from `SKILL.md` but only loaded when needed. Context stays small.

---

## The Container Problem

LSD applies to a "container" — one goal, one model, one set of requirements, one design, one implementation. For large systems, there may be multiple containers, potentially nested.

### How Containers Map to System Design

Traditional system design already handles this: high-level design identifies components, each component gets a detailed design. LSD adds two layers above traditional system design (Goal and Model) and one below (Implementation Plan). The model layer does heavy lifting, but it shouldn't pressure-test every potential sub-component upfront.

### Scoping Decision

- The framework is **always applied** at the scope you're currently working at
- It's **recursively applied** only when the human decides a sub-component is complex enough to warrant it
- It's **not applied preemptively** to every possible sub-component

The container hierarchy is a project management problem. The skills handle one container at a time. The skill invocation includes the container path. Multi-container patterns will emerge from usage rather than upfront design.

### What's Not Solved Yet

- How a child model inherits from or references a parent model
- How changes in a parent container propagate to child containers
- How the agent knows which container it's working in when containers are nested
- Cross-container dependencies (a requirement in one container depends on a model claim in another)

Decision: **build the single-container skills first, test on a real project (RBAC), let multi-container patterns emerge from friction.**

---

## The Interaction Model

The human is the product owner. The agent is the engineer.

- The human provides direction, business context, value judgments, and quality validation
- The agent does the heavy lifting of structuring, organizing, generating, and verifying
- The agent presents intermediate work products that are reviewable
- The human gives feedback through natural conversation (dictation)
- The agent surfaces decisions and unknowns proactively
- The agent does mechanical work (cross-referencing, verification, propagation) automatically

The human should never need to say "we're in the model layer" more than once. The skill holds that constraint until the human explicitly moves to the next layer.

---

## Next Steps

1. Write the skill files (start with `lsd-model/` since that's the most complex — includes SKD)
2. Test on the RBAC project (single container, already has Layer 1 and Layer 2 artifacts)
3. Iterate based on friction
4. Add remaining layer skills as needed
5. Let multi-container patterns emerge from real usage
