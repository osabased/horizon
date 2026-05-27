# Horizon Experience Optimization Addendum

This document extends `stack.md` with Horizon's full-vision experience-optimization system.

This is not an MVP checklist or build order. It describes the end-state architecture for how Horizon turns prior repo, session, validation, replay, attribution, and leverage evidence into better future Horizon behavior.

The point is not to turn Horizon into a patch-writing coding agent. Horizon should still sit underneath opencode, Codex, Claude Code, and other host agents. The point is to make Horizon better at what it owns:

- evidence selection
- Context Pack compilation
- validation routing
- memory policy
- negative knowledge
- host adaptation
- intervention policy
- exposure policy
- attribution
- replay
- audit
- leverage accounting
- policy governance

Experience optimization should improve Horizon as a support substrate, not replace the host agent.

---

## Core Thesis

Horizon should treat its own support behavior as an optimizable harness.

```text
repo/session evidence
-> Context Pack
-> host-agent behavior
-> validation and patch outcome
-> attribution
-> replay/eval result
-> candidate Horizon behavior change
-> replay/ablation/regression/leverage gates
-> scoped durable Horizon behavior
```

A harness candidate may change how Horizon retrieves evidence, budgets context, formats a Context Pack, routes validation, promotes memory, emits negative knowledge, renders for a host, handles exposure, or decides when to warn.

It must not make user product patches a core Horizon responsibility.

---

## Architectural Placement

Experience Optimization is not merely another flat plane.

It is the cross-plane self-improvement control loop that consumes outputs from:

- Session Observation
- Attribution + Self-Optimization
- Evaluation + Replay
- Leverage Accounting
- Trust Boundary + Exposure
- Contract Governance
- Explainability + User Surface

And emits scoped updates into:

- Evidence Selection
- Context Compilation
- Validation Routing
- Fact + Memory + Negative Knowledge
- Host Integration
- Intervention + Patch Recovery
- Config + Policy
- Leverage Accounting

In `stack.md`, it can be represented as a dedicated **Experience Optimization Plane** after Evaluation + Replay and before durable policy promotion, but conceptually it is the governing optimization loop across replay, attribution, leverage, and contract governance.

```text
Session Observation
-> Attribution
-> Evaluation + Replay
-> Experience Optimization
-> Leverage Accounting
-> Contract Governance
-> scoped durable policy update
```

---

## Why This Belongs in Horizon

Horizon's full stack already aims to observe repo state, compile task-specific context, watch host-agent behavior, attribute outcomes, replay sessions, and improve retrieval, memory, validation, risk, cost, and host behavior over time.

Experience optimization makes that improvement loop explicit.

Eval should not only answer:

```text
Did this run pass?
```

It should also answer:

```text
Which part of Horizon helped?
Which part hurt?
Which part wasted budget?
Which evidence was missed?
Which context was overincluded?
Which validation route paid off?
Which warning was useful or annoying?
Which candidate Horizon behavior would improve the next run?
Can that candidate survive replay, ablation, regression, exposure, and leverage gates?
```

The output should not be a report alone. It should be a candidate change to Horizon behavior, with evidence for why that change should be accepted, scoped, quarantined, or rejected.

---

# Architecture

Experience optimization has three major layers:

```text
1. Experience Substrate
2. Candidate Optimization System
3. Governance + Promotion System
```

These layers keep the full vision ambitious without turning it into a flat list of machinery.

---

## 1. Experience Substrate

The Experience Substrate preserves the evidence needed to understand whether Horizon helped, hurt, wasted effort, or missed something.

It is not a memory system. It is not a transcript cache. It is the replayable substrate for optimization.

```text
host-agent session
-> normalized trace
-> Context Pack record
-> validation record
-> attribution labels
-> cost/leverage records
-> replayable ExperienceRecord
```

### Experience Store

The Experience Store is a queryable local filesystem/database of prior runs, candidates, outcomes, failures, scorecards, and rejected hypotheses.

It should preserve enough raw evidence to support replay, attribution, ablation, audit, and candidate generation.

The optimizer must inspect raw experience, not only summaries. Human summaries are useful for review, but raw traces are the optimization substrate.

### Experience Store Contents

The full-vision Experience Store should be able to preserve:

- task prompt
- inferred task intent
- acceptance criteria
- repo snapshot
- workspace state
- branch/worktree identity
- dependency fingerprint
- host capability profile
- host behavior profile
- Context Pack
- Pack Quality Report
- exposure manifest
- retrieval plan
- retrieval queries
- included context
- excluded context
- inclusion rationale
- exclusion rationale
- stale/conflict warnings
- negative knowledge used
- memory facts used
- file reads
- file writes
- host-agent tool calls
- terminal commands
- validation routes
- validation output
- validation cost
- validation usefulness labels
- patch snapshot
- patch verdict
- attribution labels
- cost records
- latency records
- token records
- hallucination incidents
- repeated-scan signals
- ignored-context signals
- missed-context signals
- user corrections
- reverted-patch signals
- candidate policy/config/code
- candidate scorecards
- replay diffs
- ablation deltas
- regression results
- rejected hypotheses
- quarantine decisions
- promotion decisions
- rollback decisions

### Experience Record

An `ExperienceRecord` is the canonical unit of optimization.

It should be reproducible, inspectable, and diffable.

It should include:

- record ID
- task ID
- repo identity
- workspace identity
- host identity
- Context Pack ID
- relevant EvidenceItem IDs
- relevant Fact/NegativeFact IDs
- validation run IDs
- session trace ID
- patch snapshot ID
- attribution event IDs
- cost record IDs
- exposure manifest ID
- replay compatibility metadata

The record should point to raw artifacts rather than duplicating every artifact inline.

### Query Surface

The Experience Store should be inspectable through normal local tools:

```text
horizon experience list
horizon experience show <run>
horizon experience grep <pattern>
horizon experience diff <run-a> <run-b>
horizon experience trace <run>
horizon experience frontier
horizon experience rejected
```

A dashboard may summarize this, but the command surface should remain the canonical governance surface.

---

## 2. Candidate Optimization System

The Candidate Optimization System turns experience into proposed changes to Horizon behavior.

It does not directly patch the user's product code. It proposes changes to Horizon's harness: retrieval, pack compilation, validation routing, memory policy, negative knowledge, host rendering, exposure policy, and intervention thresholds.

```text
Experience Store
-> candidate generation
-> cheap contract checks
-> replay evaluation
-> causal ablation
-> regression suite
-> leverage accounting
-> quarantine / promotion / rejection
```

### Harness Candidate

A `HarnessCandidate` is a proposed change to Horizon's support behavior.

Examples:

- retrieval strategy candidate
- evidence-ranking candidate
- Context Pack recipe candidate
- context-budget candidate
- host-specific pack-rendering candidate
- validation-route candidate
- memory-promotion candidate
- negative-knowledge candidate
- stale/conflict policy candidate
- intervention-threshold candidate
- host-adapter behavior candidate
- background prewarm candidate
- exposure/redaction policy candidate
- benchmark-provider adapter candidate
- attribution-labeling candidate
- leverage-accounting candidate

A candidate must be explicit enough to replay and diff. It should never be an informal note like "retrieve better context."

### Candidate Declaration

A `HarnessCandidate` should declare:

- candidate ID
- parent candidate or baseline
- candidate kind
- changed policy/config/code
- intended task classes
- intended repo scopes
- intended host scopes
- expected metric improvement
- expected cost
- affected Horizon objects
- affected Horizon ABIs
- affected host adapters
- required replay suites
- required contract checks
- required regression checks
- invalidation triggers
- rollback behavior
- safety implications
- exposure implications
- leakage risk
- benchmark-specificity risk
- promotion scope
- quarantine scope

### Candidate Kinds

Candidate kinds should map to Horizon's owned behavior:

#### Retrieval Candidates

Change how Horizon finds and ranks evidence.

Examples:

- source/test affinity tuning
- symbol-neighborhood retrieval
- dependency-neighborhood retrieval
- similar-task retrieval
- negative-knowledge retrieval
- stale-doc demotion
- generated-file exclusion
- architecture-boundary weighting
- external-doc retrieval thresholds

#### Pack Recipe Candidates

Change how Horizon compiles, budgets, orders, or renders Context Packs.

Examples:

- smaller pack budget for low-risk tasks
- expanded acceptance criteria section for bugfixes
- host-specific summary-first rendering
- stronger negative-knowledge section for repeated hallucinations
- conflict warnings moved earlier in the pack
- provenance compression rules
- unknowns and risk notes promoted or demoted by task class

#### Validation Route Candidates

Change how Horizon selects, caches, escalates, or suppresses validation.

Examples:

- run impacted tests before full suite
- skip historically useless checks for a task class
- escalate from typecheck to integration test when risk exceeds threshold
- mark flaky checks as lower-confidence
- prefer cheap static checks for API-surface edits
- route generated-file changes differently

#### Memory Policy Candidates

Change when evidence becomes durable fact or negative knowledge.

Examples:

- stricter promotion threshold for agent-derived facts
- shorter invalidation windows for dependency/API facts
- repo-local scope for user-rejected approaches
- negative facts attached to dependency versions
- stronger contradiction handling for stale docs

#### Host Adaptation Candidates

Change how Horizon supports a specific host agent.

Examples:

- different pack rendering for hosts that ignore long sections
- validation hints formatted differently per host
- intervention threshold tuned by host behavior profile
- context ordering adjusted for hosts with poor long-context use
- fallback behavior for lower-capability adapters

#### Intervention Candidates

Change when Horizon stays silent, warns, challenges, or blocks.

Examples:

- lower warning threshold for sensitive file edits
- higher warning threshold for low-risk repeated scans
- stale-context warning only when conflicting evidence exists
- permission challenge for destructive commands
- fail-open behavior for uncertain pack quality

#### Exposure Candidates

Change redaction, retention, export, or provider exposure behavior.

Examples:

- stricter redaction for product-session traces
- different retention for validation logs
- host-specific exposure downgrade
- benchmark export scrubber
- audit bundle minimization rules

---

## Candidate Generation

Candidates may be generated from:

- repeated missed-context incidents
- repeated overincluded-context incidents
- repeated scans by host agents
- hallucinated files, symbols, APIs, or conventions
- stale-context reliance
- user corrections
- reverted patches
- validation routes that caught real defects
- validation routes that repeatedly wasted time
- benchmark failures
- product-session failures
- pack sections that hosts ignored
- pack sections that hosts used successfully
- attribution labels with high confidence
- leverage records showing low net value
- regression clusters
- rejected hypotheses that become relevant again after invalidation

Candidate generation should prefer small, scoped deltas over broad behavior changes.

---

## Candidate Lifecycle

```text
proposed
-> contract-checked
-> replayed
-> ablated
-> regression-tested
-> exposure-checked
-> leverage-scored
-> quarantined / promoted / rejected
-> monitored
-> reverted / narrowed / retired
```

### Proposed

The optimizer proposes a candidate from prior runs, traces, failures, scorecards, and rejected hypotheses.

Candidates should begin with the narrowest scope that explains the evidence:

```text
repo + host + task class
```

Global candidates require stronger evidence.

### Contract-checked

Cheap checks run before expensive replay:

- schema validity
- ABI compatibility
- config parse
- missing-field checks
- host-adapter compatibility
- exposure-policy compatibility
- fixture conformance
- migration compatibility
- rollback availability

### Replayed

The candidate runs against relevant ReplayCases and benchmark provider tasks.

Replay should include both:

- cases the candidate is expected to improve
- nearby cases it might accidentally regress

### Ablated

The candidate is compared against baselines:

```text
host agent alone
host agent + raw retrieved files
host agent + Context Pack only
host agent + validation routing only
host agent + Context Pack + validation routing
host agent + full Horizon loop
host agent + full Horizon loop + candidate change
```

Ablation should answer whether Horizon helped, and whether the candidate helped beyond baseline Horizon.

### Regression-tested

The candidate must not regress unrelated task classes, hosts, repos, exposure behavior, or safety behavior.

Regression testing should include:

- unrelated task classes
- stale-doc tasks
- distractor-context tasks
- generated-file avoidance tasks
- verifier-hacking tasks
- tasks where Horizon should stay silent
- host adapters with lower capability levels
- exposure-sensitive traces

### Exposure-checked

No candidate should be promoted if it weakens exposure policy, stores unsafe traces as memory, or exports sensitive artifacts beyond their allowed boundary.

Every candidate that changes trace handling, benchmark export, host rendering, memory promotion, or audit bundle generation must include an exposure review.

### Leverage-scored

The candidate is scored by net contribution, not only pass rate.

A candidate that improves patch success but doubles token cost may still be bad. A candidate that cuts context in half but misses critical evidence may still be bad. A candidate that wins only by exploiting weak tests must be rejected or quarantined.

### Quarantined

A candidate that helps only a narrow benchmark, repo, task class, host, or dependency version should be quarantined behind an explicit scope rule instead of promoted globally.

Quarantine is not failure. It is scoped usefulness.

### Promoted

Durable promotion requires:

- replay evidence
- causal ablation
- regression checks
- exposure checks
- leverage accounting
- rollback behavior
- invalidation triggers
- promotion scope

Promotion should default to scoped behavior. Global durable behavior should require unusually strong evidence.

### Monitored

A promoted candidate remains reversible.

Product-session evidence can:

- confirm it
- downgrade it
- narrow it
- quarantine it
- retire it
- revert it

---

# Evaluation and Scoring

## Optimization Targets

Experience optimization should improve Horizon's core product claims:

- smaller sufficient context
- fewer repeated scans
- fewer hallucinated files/symbols/APIs
- fewer stale-context errors
- better Context Pack precision
- better Context Pack recall
- better negative knowledge
- better validation routing
- less validation waste
- better attribution accuracy
- lower token cost
- lower latency
- lower host-agent confusion
- better host-specific rendering
- better replay confidence
- better auditability
- safer exposure behavior
- better scoped policy updates
- fewer user corrections
- fewer reverted patches

## Pareto Frontier, Not One Score

Horizon should not optimize one scalar leaderboard score.

Maintain Pareto frontiers across:

- patch success
- acceptance criteria satisfaction
- context precision
- context recall
- context size
- token cost
- latency
- validation cost
- validation usefulness
- hallucination reduction
- repeated-scan reduction
- stale-context reduction
- attribution confidence
- host-agent waste reduction
- exposure risk
- replay reproducibility
- benchmark leakage risk
- product-session relevance
- user interruption rate

Promotion should require a net improvement profile appropriate to the task class, host profile, repo scope, and risk class.

## Leverage Accounting

Experience optimization should feed Leverage Accounting, not bypass it.

Every candidate should produce or update a `LeverageRecord` describing:

- affected subsystem
- baseline cost
- candidate cost
- observed payoff
- expected payoff
- attribution confidence
- failure mode
- invalidation trigger
- downgrade behavior
- replacement candidate
- promotion scope

A candidate should not survive merely because it improves a metric. It should survive because its net contribution justifies its cost and risk.

---

# Benchmark Provider ABI

Horizon should expose a Benchmark Provider ABI rather than depending on one benchmark.

Providers normalize external and local benchmarks into Horizon objects:

```text
external benchmark task
-> BenchmarkProvider
-> ReplayCase
-> TaskIntent
-> RepoSnapshot
-> WorkspaceState
-> ValidationRoute
-> expected verdict
-> BenchmarkVerdict
```

Provider classes should include:

- issue-resolution suites
- SWE-bench-style patch suites
- multilingual code suites
- terminal-agent suites
- real terminal replay suites
- adversarial terminal suites
- generated repo-specific suites
- local golden suites
- stale-doc suites
- distractor-context suites
- generated-file avoidance suites
- verifier-hacking suites
- tasks where Horizon should stay silent

Benchmarks are fixtures, not product truth. They are useful because they create repeatable pressure, not because they fully represent real development.

## Benchmark Verdict

A `BenchmarkVerdict` should distinguish:

- pass/fail result
- verifier confidence
- test weakness risk
- benchmark leakage risk
- product relevance
- overfitting risk
- task-class label
- host compatibility
- Horizon attribution confidence
- validation route usefulness
- exposure constraints

Benchmark wins should not become durable policy without held-out replay, regression checks, causal ablation, leakage checks, and product-session evidence.

---

# Product-Session Evidence

Benchmark wins are not enough.

Horizon should also learn from real host-agent sessions, with user privacy and exposure rules applied.

Product-session evidence should measure:

- did the host agent read fewer irrelevant files?
- did it avoid repeated scans?
- did it avoid hallucinated files or APIs?
- did it use the Context Pack?
- did it ignore useful context?
- did it fight wrong context?
- did validation catch a real issue?
- did validation waste time?
- did user correction indicate missing context?
- did the patch get reverted?
- did Horizon interrupt too often?
- did Horizon remain invisible when it should have warned?
- did Horizon warn when silence would have been better?
- did exposure policy prevent unsafe trace reuse?
- did cost decrease without quality loss?

Product evidence is messier than benchmarks, but it is closer to Horizon's product truth.

---

# Attribution Requirements

Experience optimization depends on attribution quality.

A task succeeding does not prove Horizon helped. A file being edited does not prove it should have been included. A snippet being ignored does not prove it was useless. A validation passing does not prove the validation route was useful. A warning being acknowledged does not prove it should have fired.

Attribution labels should distinguish:

- helped
- hurt
- neutral
- missing
- stale
- overincluded
- underincluded
- ignored
- contradicted
- wasted
- distracting
- benchmark-specific
- host-specific
- repo-specific
- task-class-specific
- exposure-sensitive
- invalidated

Durable policy updates should require enough evidence to separate Horizon helping from the host agent succeeding anyway.

Attribution confidence should affect promotion scope:

```text
low confidence   -> record only
medium confidence -> scoped candidate
high confidence  -> replay candidate
very high confidence + regression safety -> scoped promotion
```

---

# Practical Outputs

Experience optimization should compile results into concrete Horizon updates.

| Signal | Practical output |
|---|---|
| Useful context included | Promote retrieval rule or pack recipe |
| Necessary context missed | Add retrieval edge, source/test affinity, or evidence requirement |
| Irrelevant context included | Tighten budget/ranking/filter |
| Context ignored | Reformat, shrink, reorder, or lower priority |
| Context contradicted | Create conflict and stale warning |
| Hallucinated API/file | Add negative fact or dependency-version fact |
| Validation caught bug | Promote validation route for similar task class |
| Validation wasted time | Downgrade route or increase threshold |
| User corrected missing fact | Add candidate evidence, not durable memory yet |
| Reverted patch | Add negative outcome evidence |
| Candidate helps only benchmark | Quarantine as benchmark-specific |
| Candidate adds cost without payoff | Downgrade/remove via leverage accounting |
| Warning annoyed user | Raise intervention threshold or narrow scope |
| Host ignored pack section | Change host rendering or reduce that section |
| Exposure risk increased | Reject or require stricter redaction |

The output should be concrete objects, not just prose reports:

```text
EvalReport
-> PolicyUpdateCandidate
-> ReplayEvidence
-> LeverageRecord
-> PackRecipeDelta
-> RetrievalStrategyDelta
-> ValidationRouteDelta
-> MemoryPolicyDelta
-> HostProfileDelta
-> InterventionPolicyDelta
-> ExposurePolicyDelta
-> RegressionGateResult
-> PromotionDecision
-> AuditBundle
```

---

# Governance + Promotion System

Experience optimization needs governance because it changes Horizon behavior.

Governance decides whether a candidate is:

- rejected
- retained as evidence
- quarantined to a scope
- promoted to scoped durable policy
- promoted globally
- reverted
- retired

## Promotion Scope

Promotion scope should be explicit.

Possible scopes:

- repo
- workspace
- branch
- dependency version
- task class
- file area
- package/module
- host
- host capability level
- validation environment
- benchmark provider
- exposure class
- user-approved policy scope

Global promotion should be rare and evidence-heavy.

## Rejected Hypotheses

Rejected hypotheses are first-class.

A rejected candidate may become useful later if:

- dependency versions change
- host behavior changes
- repo architecture changes
- validation environment changes
- task class distribution changes
- exposure policy changes
- prior counterevidence is invalidated

The Experience Store should preserve rejected hypotheses with reasons and invalidators.

## Rollback

Every promoted candidate needs:

- rollback behavior
- invalidation triggers
- monitoring rules
- owner subsystem
- compatibility range
- evidence that justified promotion
- evidence that would justify rollback

No candidate should become irreversible.

---

# Sibling ABIs to Add

Add these to the Sibling ABIs section of `stack.md`:

- **Benchmark Provider ABI**: how external and local benchmark suites normalize into Horizon replay cases.
- **Harness Candidate ABI**: how candidate retrieval, pack, validation, memory, intervention, host-rendering, adapter, and exposure strategies are represented.
- **Optimization Run ABI**: how candidate policies, traces, scorecards, ablations, regressions, and promotion decisions are stored.
- **Experience Store ABI**: how full replay history is preserved and queried without compressing away diagnostic evidence.
- **Benchmark Verdict ABI**: how benchmark outcomes, verifier confidence, leakage risk, and product relevance are represented.
- **Promotion Decision ABI**: how accepted, rejected, quarantined, reverted, and retired candidate decisions are represented.
- **Rejected Hypothesis ABI**: how failed candidate ideas are preserved with counterevidence and invalidators.

---

# Core Objects to Add

Add these to the Core Object Model:

- `ExperienceStore`
- `ExperienceRecord`
- `BenchmarkProvider`
- `BenchmarkVerdict`
- `HarnessCandidate`
- `OptimizationRun`
- `OptimizationTrace`
- `CandidateScorecard`
- `ParetoFrontier`
- `PolicyUpdateCandidate`
- `PackRecipeDelta`
- `RetrievalStrategyDelta`
- `ValidationRouteDelta`
- `MemoryPolicyDelta`
- `HostProfileDelta`
- `InterventionPolicyDelta`
- `ExposurePolicyDelta`
- `RegressionGateResult`
- `ReplayEvidence`
- `AblationResult`
- `PromotionDecision`
- `RejectedHypothesis`
- `QuarantineScope`

---

# Commands to Add

Add these commands to the Explainability + User Surface Plane:

```text
horizon experience list
horizon experience show <run>
horizon experience grep <pattern>
horizon experience diff <run-a> <run-b>
horizon experience trace <run>
horizon experience frontier
horizon experience rejected

horizon candidate list
horizon candidate show <candidate>
horizon candidate diff <candidate-a> <candidate-b>
horizon candidate replay <candidate>
horizon candidate ablate <candidate>
horizon candidate score <candidate>
horizon candidate quarantine <candidate>
horizon candidate promote <candidate>
horizon candidate reject <candidate>
horizon candidate rollback <candidate>

horizon benchmark providers
horizon benchmark import <provider>
horizon benchmark run <provider>
horizon benchmark verdict <run>

horizon optimization report
horizon optimization frontier
horizon optimization rejected
horizon optimization leverage
```

These commands are inspection and governance surfaces. Normal usage should remain in the background.

---

# Guardrails

## Horizon becomes a coding agent

Hardening rule: Experience optimization may propose Horizon policy, retrieval, validation, memory, rendering, exposure, adapter, and intervention changes. It must not make patch generation a core Horizon responsibility.

## Experience optimization becomes a flat pile of machinery

Hardening rule: Keep the architecture layered:

```text
Experience Substrate
-> Candidate Optimization System
-> Governance + Promotion System
```

Objects, commands, ABIs, and lifecycle stages should map to one of these layers.

## Optimization overfits to benchmark artifacts

Hardening rule: Candidate policies may inspect benchmark traces, but durable promotion requires held-out replay, leakage checks, causal ablation, regression checks, leverage accounting, and product-session evidence. Benchmark-only wins must be marked benchmark-specific.

## Raw traces become unsafe memory

Hardening rule: Experience Store artifacts are evidence, not durable facts. Memory promotion still goes through fact lifecycle, scopes, invalidators, and exposure policy.

## Summaries hide important failure evidence

Hardening rule: Human summaries may exist, but optimization must preserve raw traces, scorecards, Context Packs, validation logs, replay diffs, attribution labels, and rejected hypotheses.

## A candidate wins by increasing cost

Hardening rule: Every candidate must be scored against cost, latency, token use, validation cost, and subsystem leverage. A correctness gain can still fail promotion if the cost profile is bad.

## Agentic judges become ground truth

Hardening rule: Agentic judges may produce diagnostic labels and review hints. They do not replace executable replay, patch verification, causal ablation, exposure checks, or leverage gates.

## Product sessions leak sensitive information

Hardening rule: Experience Store writes and exports must obey the Trust Boundary + Exposure Plane. No trace should be exposed to a host, provider, benchmark, or audit export without an ExposureManifest.

## Candidate policies become irreversible

Hardening rule: Every promoted candidate needs rollback behavior, invalidation triggers, monitoring, and promotion scope. Promotion is reversible.

## Attribution learns false lessons

Hardening rule: Attribution updates must be confidence-scored and counterevidence-aware. A successful task is not automatically evidence that Horizon helped.

## Global promotion happens too easily

Hardening rule: Candidate promotion should default to narrow scope. Global promotion requires repeated evidence across repos, hosts, task classes, and replay suites.

## Optimization suppresses useful uncertainty

Hardening rule: Horizon should preserve unknowns, conflicts, and uncertainty labels. A cleaner pack is not better if it hides important uncertainty.

## Experience Store becomes expensive by default

Hardening rule: Trace retention, artifact granularity, and indexing depth should be policy-controlled with cost budgets, retention rules, redaction rules, and eviction behavior.

---

# How It Maps Back to `stack.md`

This addendum changes the stack as follows:

1. Add an **Experience Optimization Plane** as the cross-plane optimization loop over attribution, replay, leverage, and governance.
2. Extend the **Evaluation + Replay Plane** so evals produce queryable experience, not just reports.
3. Extend the **Attribution + Self-Optimization Plane** so attribution labels feed HarnessCandidates and PolicyUpdateCandidates.
4. Extend the **Leverage Accounting Plane** so every candidate is scored by net leverage before promotion.
5. Extend the **Trust Boundary + Exposure Plane** so product-session traces, benchmark exports, audit bundles, and candidate policies remain exposure-safe.
6. Extend the **Contract Governance Plane** with Benchmark Provider, Harness Candidate, Optimization Run, Experience Store, Benchmark Verdict, Promotion Decision, and Rejected Hypothesis ABIs.
7. Extend the **Explainability + User Surface Plane** with experience, candidate, benchmark, and optimization inspection commands.
8. Extend the **Failure Modes and Adversarial Guardrails** with benchmark overfitting, unsafe trace memory, trace compression, judge overtrust, irreversible promotion, overbroad global promotion, and optimization cost creep.

---

# Final Shape

Experience optimization makes Horizon a self-improving support substrate:

```text
host-agent session
-> Horizon support behavior
-> replay/audit bundle
-> attribution and leverage record
-> queryable experience
-> candidate harness improvement
-> replay + ablation + regression + exposure + leverage gates
-> scoped durable update
```

The durable product claim becomes stronger:

```text
Horizon does not just remember the repo.
Horizon learns which support behavior actually makes coding agents cheaper, less confused, less repetitive, and less hallucination-prone.
```

Experience optimization should be ambitious in scope but strict in governance.

The end state is not a bigger coding agent. It is a closed-loop intelligence layer that learns how to support coding agents better while preserving Horizon's boundary, local-first trust model, auditability, and leverage discipline.

---

# Sources

These sources motivate the design. They are not dependencies.

- **Meta-Harness: End-to-End Optimization of Model Harnesses**  
  Primary source for treating harness logic as optimizable code/config, preserving full execution traces, using prior candidate history, and optimizing over multi-objective frontiers.  
  https://arxiv.org/abs/2603.28052

- **DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines**  
  Source for treating LM systems as composable programs that can be optimized against explicit metrics instead of hand-tuned prompt strings.  
  https://arxiv.org/abs/2310.03714

- **GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning**  
  Source for trajectory reflection, candidate evolution, and Pareto-style optimization over compound AI systems.  
  https://arxiv.org/abs/2507.19457

- **SWE-bench**  
  Source for issue-resolution benchmark fixtures, reproducible patch evaluation, and SWE-bench-style benchmark adapters.  
  https://www.swebench.com/  
  https://github.com/princeton-nlp/SWE-bench

- **Terminal-Bench**  
  Source for terminal-based agent evaluation, useful for validation routing, tool-use behavior, terminal replay, and benchmark providers.  
  https://www.tbench.ai/

- **Agent-as-a-Judge: Evaluate Agents with Agents**  
  Source for step-level diagnostic evaluation of agent trajectories. Horizon may use this for labels and review hints, but durable policy updates should still require executable replay, causal ablation, and leverage gates.  
  https://arxiv.org/abs/2410.10934

- **No More, No Less: Task Alignment in Terminal Agents**  
  Source for adversarial context/distractor evaluation in terminal-agent settings.  
  https://arxiv.org/abs/2605.12233

- **Terminal Wrench: A Dataset of Reward-Hackable Environments and Exploit Trajectories**  
  Source for verifier-hacking and benchmark-exploit guardrails.  
  https://arxiv.org/abs/2604.17596

- **TerminalWorld: Benchmarking Agents on Real-World Terminal Tasks**  
  Source for real terminal replay benchmark providers and the need for provider diversity.  
  https://arxiv.org/abs/2605.22535
