# Horizon Experience Optimization Addendum

This document extends `stack.md` with Horizon's full-vision experience-optimization system.

It is not an MVP checklist. It is also not permission to build an unbounded self-modifying machine. It defines the end-state loop by which Horizon turns prior repo, session, validation, replay, attribution, exposure, and leverage evidence into better future Horizon behavior.

Horizon remains a support substrate underneath opencode, Codex, Claude Code, and other host agents. Experience optimization improves what Horizon owns:

- evidence selection
- Context Pack compilation
- validation routing
- fact, memory, and negative-knowledge policy
- host adaptation
- intervention policy
- exposure and retention policy
- attribution
- replay and audit
- leverage accounting
- policy governance

It must not make patch generation, chat UI, editor UI, or autonomous product work a core Horizon responsibility.

---

## Core Thesis

Horizon should treat its own support behavior as an optimizable harness.

```text
repo/session evidence
-> Context Pack
-> host-agent behavior
-> validation and patch outcome
-> attribution
-> replay / audit / leverage result
-> candidate Horizon behavior change
-> capability / replay / ablation / regression / exposure / leverage gates
-> scoped reversible Horizon behavior
```

A candidate may change how Horizon retrieves evidence, budgets context, formats a Context Pack, routes validation, promotes memory, emits negative knowledge, renders for a host, handles exposure, or decides when to warn.

The optimization target is not "make Horizon more agentic." The target is:

```text
smaller sufficient context
+ fewer hallucinations
+ fewer repeated scans
+ cheaper useful validation
+ better host behavior
+ safer memory
+ lower cost
+ stronger auditability
```

---

## Design Laws

### 1. Optimize the harness, not the user's product code

Experience optimization may propose changes to Horizon policy, retrieval, pack recipes, validation routes, memory rules, host rendering, exposure handling, and intervention thresholds.

It must not make product patches a core Horizon responsibility.

### 2. Learn only from observable evidence

Every optimization must be gated by what Horizon actually observed.

A host adapter that only injects context cannot support the same attribution claims as one that observes file touches, tool calls, validation runs, context usage, and permission events.

Weak traces may produce diagnostics. They should not produce durable policy.

### 3. Store experience as ledger-backed manifests

The canonical source remains the local event ledger and artifact store. An `ExperienceRecord` points to raw or redacted artifacts; it does not duplicate the world into a separate warehouse.

Experience optimization is a projection over durable events, not a parallel memory system.

### 4. Retention and exposure are first-class

Raw traces are valuable, but they are also costly and sensitive. Every experience artifact must have retention, exposure, redaction, and eviction policy.

No trace, Context Pack, replay case, benchmark export, audit bundle, or candidate payload should cross a boundary without an `ExposureManifest`.

### 5. Prefer small scoped deltas

Candidates should start with the narrowest scope that explains the evidence:

```text
repo + host + task class + dependency scope + risk class
```

Global promotion should be rare.

### 6. Promote slower than you diagnose

Horizon should freely record, explain, and suggest. It should be conservative about automatic behavior changes.

Candidate maturity should move through explicit modes:

```text
record-only
-> diagnostic
-> suggested
-> shadow
-> canary
-> scoped promoted
-> global promoted
-> retired / reverted
```

---

## Architectural Placement

Experience Optimization is the cross-plane control loop over attribution, replay, leverage, exposure, and governance.

It consumes outputs from:

- Session Observation
- Attribution + Self-Optimization
- Evaluation + Replay
- Leverage Accounting
- Trust Boundary + Exposure
- Contract Governance
- Explainability + User Surface

It emits scoped updates into:

- Evidence Selection
- Context Compilation
- Validation Routing
- Fact + Memory + Negative Knowledge
- Host Integration
- Intervention + Patch Recovery
- Config + Policy
- Leverage Accounting

In `stack.md`, represent it as an **Experience Optimization Plane** after Evaluation + Replay and before durable policy promotion.

```text
Session Observation
-> Attribution
-> Evaluation + Replay
-> Experience Optimization
-> Leverage Accounting
-> Contract Governance
-> scoped reversible policy update
```

---

# Architecture

Experience optimization has four layers:

```text
1. Experience Substrate
2. Candidate Optimization System
3. Evaluation + Attribution Gates
4. Governance + Promotion System
```

The layers prevent the system from becoming a flat pile of machinery.

---

## 1. Experience Substrate

The Experience Substrate preserves the evidence needed to know whether Horizon helped, hurt, wasted effort, missed something, or should have stayed silent.

It is not a transcript cache. It is not durable memory. It is the replayable substrate for optimization.

```text
host-agent session
-> normalized events
-> Context Pack record
-> validation record
-> attribution labels
-> cost/leverage records
-> replayable ExperienceRecord
```

### Event ledger as source of truth

The local event ledger remains canonical.

Experience records are manifests over existing objects:

- `RepoSnapshot`
- `WorkspaceState`
- `TaskIntent`
- `ContextPack`
- `PackQualityReport`
- `ExposureManifest`
- `ValidationRun`
- `SessionTrace`
- `PatchSnapshot`
- `AttributionEvent`
- `CostRecord`
- `LeverageRecord`
- `ReplayCase`
- `AuditBundle`

The optimizer may inspect raw artifacts when policy allows, but durable memory promotion still goes through the Fact + Memory + Negative Knowledge lifecycle.

### ExperienceRecord

An `ExperienceRecord` is the canonical unit of optimization.

It should be reproducible, inspectable, diffable, and policy-scoped.

It includes:

- record ID
- task ID
- repo identity
- workspace identity
- branch/worktree identity
- dependency fingerprint
- host identity
- host capability profile
- observation capability record
- Context Pack ID
- relevant EvidenceItem IDs
- relevant Fact/NegativeFact IDs
- validation run IDs
- session trace ID
- patch snapshot ID
- attribution event IDs
- cost record IDs
- exposure manifest ID
- retention class
- replay compatibility class
- replay artifact pointers
- candidate links
- promotion/rejection/rollback links

The record points to artifacts. It should not inline everything.

### Observation Capability Contract

Every host adapter must declare what was actually observable for the session.

```text
L0: Manual pack rendering only
L1: Context injection observed
L2: File-touch observation
L3: Tool-call and validation observation
L4: Context-usage attribution signals
L5: Permission and validation brokering
```

Optimization rules:

- L0-L1 can support pack diagnostics and user-visible reports.
- L2 can support retrieval and missed/overincluded-context hypotheses.
- L3 can support validation-route and tool-use hypotheses.
- L4 can support host-specific rendering and context-usage attribution.
- L5 can support intervention, permission, and validation-brokering policy.

A candidate must declare the minimum observation level required to justify it.

### Retention classes

Every experience artifact gets a retention class.

```text
raw-retained
redacted-retained
summary-only
scorecard-only
discard-after-window
```

Retention policy should consider:

- sensitivity
- replay value
- attribution value
- cost
- artifact size
- exposure scope
- invalidation triggers
- user/repo policy

Raw retention is useful but should not be the default for every artifact.

### Replay compatibility classes

Replay is not always exact. Every record should say what kind of replay it supports.

```text
exact        same repo snapshot, same dependencies, same validation route
near         comparable repo/dependency state with known drift
synthetic    generated or normalized from a benchmark/provider
diagnostic   useful for inspection, not promotion
invalid      insufficient or unsafe for replay
```

Promotion gates must treat these classes differently.

### Experience Store contents

The full-vision store should preserve or point to:

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
- inclusion/exclusion rationale
- stale/conflict warnings
- negative knowledge used
- memory facts used
- observed file reads/writes where available
- observed host tool calls where available
- terminal commands where available
- validation routes
- validation output
- validation cost
- validation usefulness labels
- patch snapshot
- patch verdict
- attribution labels
- cost, latency, and token records
- hallucination incidents
- repeated-scan signals
- ignored-context signals
- missed-context signals
- user corrections
- reverted-patch signals
- candidate declarations
- candidate scorecards
- replay diffs
- ablation deltas
- regression results
- rejected hypotheses
- quarantine decisions
- promotion decisions
- rollback decisions

### Query surface

Keep the first command surface small.

```text
horizon experience list
horizon experience show <record>
horizon experience diff <record-a> <record-b>
horizon experience trace <record>
horizon optimization report
```

Add richer commands only after the underlying objects exist.

---

## 2. Candidate Optimization System

The Candidate Optimization System turns experience into proposed changes to Horizon behavior.

It does not patch product code. It proposes changes to Horizon's harness.

```text
ExperienceRecord set
-> signal cluster
-> attribution hypothesis
-> HarnessCandidate
-> cheap contract checks
-> replay / ablation / regression / exposure / leverage gates
-> quarantine / promotion / rejection
```

### Candidate primitive types

Keep the primitive model small.

```text
RetrievalDelta
PackDelta
ValidationDelta
PolicyDelta
```

These four primitives cover the broader behavior space without creating an object taxonomy too early.

#### RetrievalDelta

Changes how Horizon finds, ranks, filters, or budgets evidence.

Examples:

- source/test affinity tuning
- symbol-neighborhood retrieval
- dependency-neighborhood retrieval
- similar-task retrieval
- negative-knowledge retrieval
- stale-doc demotion
- generated-file exclusion
- architecture-boundary weighting
- external-doc retrieval threshold changes

#### PackDelta

Changes how Horizon compiles, budgets, orders, compresses, or renders Context Packs.

Examples:

- smaller pack budget for low-risk tasks
- expanded acceptance criteria for bugfixes
- host-specific summary-first rendering
- stronger negative-knowledge section for repeated hallucinations
- conflict warnings moved earlier
- provenance compression rules
- unknown/risk section promoted or demoted by task class

#### ValidationDelta

Changes how Horizon selects, caches, escalates, suppresses, or summarizes validation.

Examples:

- impacted tests before full suite
- skip historically useless checks for a task class
- escalate from typecheck to integration test when risk exceeds threshold
- mark flaky checks as lower-confidence
- prefer cheap static checks for API-surface edits
- route generated-file changes differently

#### PolicyDelta

Changes Horizon policy around memory, negative knowledge, exposure, host adaptation, intervention, permissions, or retention.

Examples:

- stricter promotion threshold for agent-derived facts
- shorter invalidation windows for dependency/API facts
- repo-local scope for user-rejected approaches
- host-specific exposure downgrade
- stale-context warning only when conflicting evidence exists
- permission challenge for destructive commands
- shorter raw-trace retention for sensitive sessions

### HarnessCandidate declaration

A `HarnessCandidate` must be explicit enough to replay, diff, scope, and roll back.

It declares:

- candidate ID
- parent candidate or baseline
- primitive type
- changed policy/config/code
- intended task classes
- intended repo scopes
- intended host scopes
- required host observation capability
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
- maturity mode

A candidate should never be an informal note like "retrieve better context."

### Candidate sources

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
- rejected hypotheses that become relevant after invalidation

Candidate generation should prefer narrow deltas over broad behavior changes.

### Candidate maturity

Candidates move through explicit maturity modes.

```text
record-only
-> diagnostic
-> suggested
-> shadow
-> canary
-> scoped promoted
-> global promoted
-> retired / reverted
```

#### Record-only

The signal is stored but not acted on.

Use this for low-confidence or low-observability evidence.

#### Diagnostic

Horizon can show the issue in reports, but it does not propose a behavior change yet.

#### Suggested

Horizon proposes a candidate for human or governance review.

#### Shadow

The candidate runs in parallel against future sessions or replay cases without affecting host behavior.

#### Canary

The candidate affects a narrow scope with rollback monitoring.

#### Scoped promoted

The candidate becomes durable only inside an explicit scope.

#### Global promoted

The candidate becomes general Horizon behavior.

This should be rare and evidence-heavy.

---

## 3. Evaluation + Attribution Gates

Experience optimization must prove that a candidate helps beyond baseline Horizon and does not merely exploit weak tests, benchmarks, or noisy attribution.

### Required gates

Minimum promotion gates:

```text
contract check
observation capability check
replay check
baseline ablation
nearby regression check
exposure check
cost/leverage check
rollback check
```

### Contract check

Cheap checks run before expensive replay:

- schema validity
- ABI compatibility
- config parse
- missing-field checks
- host-adapter compatibility
- exposure-policy compatibility
- rollback availability

### Observation capability check

The candidate's evidence must be supported by the host/session capability level.

Examples:

- Do not promote context-usage rules from sessions where context usage was not observable.
- Do not promote validation-route rules from sessions where validation was run outside Horizon's observation boundary.
- Do not infer host-specific pack formatting from a host adapter that only supports manual rendering.

### Replay check

Replay should include:

- cases the candidate is expected to improve
- nearby cases it might regress
- stale-doc cases
- distractor-context cases
- generated-file avoidance cases
- verifier-hacking cases
- sessions where Horizon should stay silent
- lower-capability host adapters
- exposure-sensitive traces

### Baseline ablation

A candidate should be compared against practical baselines:

```text
host agent alone
host agent + raw retrieved files
host agent + current Context Pack
host agent + current validation routing
host agent + full Horizon loop
host agent + full Horizon loop + candidate
```

Ablation should answer two questions:

```text
Did Horizon help?
Did the candidate help beyond current Horizon?
```

### Regression check

A candidate must not silently degrade unrelated task classes, hosts, repos, exposure behavior, or safety behavior.

Regression checks should include:

- unrelated task classes
- stale-doc tasks
- distractor-context tasks
- generated-file avoidance tasks
- validation-heavy tasks
- tasks where warnings should not fire
- low-capability host adapters
- exposure-sensitive traces

### Exposure check

No candidate should be promoted if it weakens exposure policy, stores unsafe traces as memory, or exports sensitive artifacts beyond their allowed boundary.

Every candidate that changes trace handling, benchmark export, host rendering, memory promotion, or audit bundle generation requires exposure review.

### Cost and leverage check

A candidate is scored by net contribution, not only pass rate.

A candidate can fail if it:

- improves patch success while doubling token cost
- reduces context size while missing critical evidence
- improves benchmark score while hurting product sessions
- increases warning frequency without strong harm reduction
- depends on expensive background work with weak payoff

### Attribution requirements

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

A task succeeding does not prove Horizon helped.
A file being edited does not prove it should have been included.
A snippet being ignored does not prove it was useless.
A validation passing does not prove the validation route was useful.
A warning being acknowledged does not prove it should have fired.

Durable policy updates require enough evidence to separate Horizon helping from the host agent succeeding anyway.

### Metric set

Start with a small product-relevant metric set:

- Context Pack size delta
- context precision proxy
- context recall proxy
- missed-context incidents
- overincluded-context incidents
- repeated-scan delta
- hallucinated file/symbol/API incidents
- stale-context incidents
- validation yield
- validation waste
- token cost delta
- latency delta
- user correction signals
- reverted-patch signals
- interruption regret
- exposure risk delta
- replay reproducibility

Do not optimize one scalar leaderboard score.

Pareto/frontier analysis may be useful later, but early governance should prefer explicit decisions:

```text
better
worse
mixed
inconclusive
benchmark-specific
unsafe
too expensive
```

### Benchmark evidence

Benchmarks are fixtures, not product truth.

Benchmark providers are useful for repeatable pressure, but benchmark wins should not become durable policy without:

- held-out replay
- leakage checks
- causal ablation
- regression checks
- exposure checks
- leverage accounting
- product-session evidence when available

Benchmark-only wins should be quarantined as benchmark-specific.

### Product-session evidence

Product-session evidence is messier than benchmarks but closer to Horizon's actual claim.

It should measure:

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
- did Horizon remain silent when it should have warned?
- did Horizon warn when silence would have been better?
- did exposure policy prevent unsafe trace reuse?
- did cost decrease without quality loss?

### Agentic judges

Agentic judges may produce diagnostic labels, cluster failures, and suggest candidate explanations.

They do not replace executable replay, patch verification, causal ablation, exposure checks, or leverage gates.

---

## 4. Governance + Promotion System

Experience optimization changes Horizon behavior, so governance must be explicit.

Governance decides whether a candidate is:

- rejected
- retained as evidence
- diagnostic only
- shadowed
- canaried
- quarantined to a scope
- promoted to scoped durable policy
- promoted globally
- reverted
- retired

### Promotion scope

Promotion scope must be explicit.

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

Default to narrow scope.

### Promotion record

Every promoted candidate needs:

- candidate ID
- owner subsystem
- promotion scope
- evidence bundle
- replay results
- ablation results
- regression results
- exposure result
- leverage result
- rollback behavior
- invalidation triggers
- monitoring rule
- review/expiry policy

No candidate should become irreversible.

### Rejected hypotheses

Rejected hypotheses are first-class.

A rejected candidate may become useful later if:

- dependency versions change
- host behavior changes
- repo architecture changes
- validation environment changes
- task class distribution changes
- exposure policy changes
- prior counterevidence is invalidated

Preserve rejected hypotheses with reasons and invalidators.

### Rollback

A candidate should be rolled back, narrowed, or quarantined when product evidence shows:

- higher cost without payoff
- increased hallucinations
- increased missed context
- validation waste
- user correction clusters
- reverted-patch clusters
- exposure risk
- host-specific degradation
- benchmark overfitting

Rollback must be a normal path, not an exceptional failure.

---

# Practical Outputs

Experience optimization should produce concrete Horizon objects, not just prose reports.

```text
ExperienceRecord
-> AttributionHypothesis
-> HarnessCandidate
-> CandidateScorecard
-> ReplayEvidence
-> AblationResult
-> RegressionGateResult
-> LeverageRecord
-> PromotionDecision
-> AuditBundle
```

Signal-to-output mapping:

| Signal | Practical output |
|---|---|
| Useful context included | Promote or shadow a RetrievalDelta / PackDelta |
| Necessary context missed | Add retrieval edge, evidence requirement, or source/test affinity candidate |
| Irrelevant context included | Tighten budget, ranking, or filtering candidate |
| Context ignored | Reformat, shrink, reorder, or lower priority |
| Context contradicted | Create conflict and stale warning candidate |
| Hallucinated API/file | Add negative fact or dependency-version fact candidate |
| Validation caught bug | Promote validation route for similar task class |
| Validation wasted time | Downgrade route or increase threshold |
| User corrected missing fact | Add candidate evidence, not durable memory yet |
| Reverted patch | Add negative outcome evidence |
| Candidate helps only benchmark | Quarantine as benchmark-specific |
| Candidate adds cost without payoff | Downgrade or remove via leverage accounting |
| Warning annoyed user | Raise intervention threshold or narrow scope |
| Host ignored pack section | Change host rendering or reduce that section |
| Exposure risk increased | Reject or require stricter redaction |

---

# ABIs to Add

Add the minimum durable contracts needed for this loop.

- **Experience Record ABI**: how replayable session experience is represented as a ledger-backed manifest.
- **Observation Capability ABI**: what the host/session actually allowed Horizon to observe.
- **Harness Candidate ABI**: how retrieval, pack, validation, and policy deltas are represented.
- **Optimization Run ABI**: how candidate traces, scorecards, ablations, regressions, and decisions are stored.
- **Promotion Decision ABI**: how accepted, rejected, quarantined, reverted, and retired decisions are represented.
- **Rejected Hypothesis ABI**: how failed candidate ideas are preserved with counterevidence and invalidators.

Optional later:

- **Benchmark Provider ABI**: how external and local benchmark suites normalize into Horizon replay cases.
- **Benchmark Verdict ABI**: how benchmark outcomes, verifier confidence, leakage risk, and product relevance are represented.

Do not make benchmark infrastructure the center of the first implementation.

---

# Core Objects to Add

Add these to the Core Object Model:

- `ExperienceRecord`
- `ObservationCapabilityRecord`
- `HarnessCandidate`
- `RetrievalDelta`
- `PackDelta`
- `ValidationDelta`
- `PolicyDelta`
- `OptimizationRun`
- `CandidateScorecard`
- `ReplayEvidence`
- `AblationResult`
- `RegressionGateResult`
- `PromotionDecision`
- `RejectedHypothesis`
- `QuarantineScope`

Optional later:

- `BenchmarkProvider`
- `BenchmarkVerdict`
- `ParetoFrontier`

---

# Commands to Add

Initial command surface:

```text
horizon experience list
horizon experience show <record>
horizon experience diff <record-a> <record-b>
horizon experience trace <record>

horizon candidate list
horizon candidate show <candidate>
horizon candidate replay <candidate>
horizon candidate decide <candidate> <shadow|canary|promote|quarantine|reject|rollback>

horizon optimization report
```

Later command surface, only when justified:

```text
horizon candidate ablate <candidate>
horizon candidate score <candidate>
horizon optimization rejected
horizon optimization leverage
horizon benchmark providers
horizon benchmark import <provider>
horizon benchmark run <provider>
```

Normal usage should remain in the background. Commands exist for inspection, governance, and debugging.

---

# Implementation Sequence

This section is a practical build order. It does not reduce the full vision; it prevents the full vision from becoming unbuildable.

## Phase 0: Preconditions

Required before experience optimization is useful:

- event ledger
- repo/workspace identity
- Context Pack ABI
- Context Pack provenance hooks
- ExposureManifest
- PackQualityReport
- basic validation records
- host capability profile
- cost/token/latency records

Without these, experience optimization becomes guesswork.

## Phase 1: Record-only experience

Build:

- `ExperienceRecord` manifest
- retention classes
- replay compatibility class
- observation capability record
- minimal `horizon experience show`
- minimal `horizon optimization report`

Goal:

```text
Can Horizon explain what happened in a session without changing future behavior?
```

## Phase 2: Diagnostic attribution

Build:

- attribution labels
- missed/overincluded context diagnostics
- repeated-scan diagnostics
- validation usefulness diagnostics
- hallucinated file/API diagnostics
- user correction and revert evidence capture

Goal:

```text
Can Horizon identify likely help, harm, waste, and missing evidence without promoting policy?
```

## Phase 3: Candidate generation

Build:

- `HarnessCandidate`
- four delta primitives
- candidate declarations
- candidate scorecards
- suggested and shadow maturity modes

Goal:

```text
Can Horizon propose small scoped changes that are explicit enough to replay and roll back?
```

## Phase 4: Replay and ablation

Build:

- local replay cases
- baseline comparison
- candidate replay
- candidate ablation
- nearby regression checks
- exposure checks
- leverage records

Goal:

```text
Can Horizon prove a candidate improves over current Horizon, not just over a weak baseline?
```

## Phase 5: Scoped promotion

Build:

- promotion decisions
- quarantine scopes
- canary mode
- rollback behavior
- monitoring rules
- rejected hypothesis store

Goal:

```text
Can Horizon safely change narrow future behavior and undo it when evidence turns?
```

## Phase 6: Benchmark providers and frontier analysis

Build only after product-session replay works:

- benchmark provider adapters
- benchmark verdicts
- benchmark leakage checks
- broader regression suites
- Pareto/frontier analysis

Goal:

```text
Can Horizon use benchmarks as pressure without confusing benchmark wins for product truth?
```

---

# Guardrails

## Horizon becomes a coding agent

Hardening rule: Experience optimization may change Horizon support behavior. It must not make patch generation a core responsibility.

## Experience optimization outruns observability

Hardening rule: A candidate cannot be promoted from evidence the adapter could not observe.

## Experience Store becomes expensive by default

Hardening rule: Trace retention, artifact granularity, and indexing depth are policy-controlled with cost budgets, retention rules, redaction rules, and eviction behavior.

## Raw traces become unsafe memory

Hardening rule: Experience artifacts are evidence, not durable facts. Memory promotion still goes through fact lifecycle, scopes, invalidators, and exposure policy.

## Summaries hide important failure evidence

Hardening rule: Summaries are review surfaces. Replay and attribution depend on preserved artifacts when policy allows.

## Optimization overfits to benchmark artifacts

Hardening rule: Benchmark-only wins are quarantined. Durable promotion requires held-out replay, leakage checks, causal ablation, regression checks, exposure checks, leverage accounting, and product-session evidence where available.

## A candidate wins by increasing cost

Hardening rule: Every candidate is scored against cost, latency, token use, validation cost, and subsystem leverage.

## Agentic judges become ground truth

Hardening rule: Agentic judges provide diagnostic labels, not final authority.

## Candidate policies become irreversible

Hardening rule: Every promoted candidate has rollback behavior, invalidation triggers, monitoring, and scope.

## Attribution learns false lessons

Hardening rule: Attribution updates are confidence-scored and counterevidence-aware. A successful task is not automatically evidence that Horizon helped.

## Global promotion happens too easily

Hardening rule: Promotion defaults to narrow scope. Global promotion requires repeated evidence across repos, hosts, task classes, and replay suites.

## Optimization suppresses useful uncertainty

Hardening rule: Horizon preserves unknowns, conflicts, and uncertainty labels. A cleaner pack is not better if it hides important uncertainty.

---

# How It Maps Back to `stack.md`

This addendum changes the stack as follows:

1. Add an **Experience Optimization Plane** as the cross-plane optimization loop over attribution, replay, leverage, exposure, and governance.
2. Extend the **Evaluation + Replay Plane** so evals produce queryable experience, not just reports.
3. Extend the **Attribution + Self-Optimization Plane** so attribution labels feed `HarnessCandidate` objects.
4. Extend the **Leverage Accounting Plane** so every candidate is scored by net leverage before promotion.
5. Extend the **Trust Boundary + Exposure Plane** so traces, benchmark exports, audit bundles, and candidate payloads remain exposure-safe.
6. Extend the **Contract Governance Plane** with Experience Record, Observation Capability, Harness Candidate, Optimization Run, Promotion Decision, and Rejected Hypothesis ABIs.
7. Extend the **Explainability + User Surface Plane** with minimal experience, candidate, and optimization inspection commands.
8. Keep Benchmark Provider and Pareto Frontier machinery as later capability modules, not initial design centers.

---

# Final Shape

Experience optimization makes Horizon a self-imving support substrate:

```text
host-agent session
-> Horizon support behavior
-> replay/audit bundle
-> attribution and leverage record
-> queryable experience
-> candidate harness improvement
-> capability + replay + ablation + regression + exposure + leverage gates
-> scoped reversible update
```

The durable product claim becomes:

```text
Horizon does not just remember the repo.
Horizon learns which support behavior actually makes coding agents cheaper, less confused, less repetitive, and less hallucination-prone.
```

The end state is not a bigger coding agent. It is a closed-loop intelligence layer that learns how to support coding agents better while preserving Horizon's boundary, local-first trust model, auditability, and leverage discipline.

---

# Sources

These sources motivate the design. They are not dependencies.

- **Meta-Harness: End-to-End Optimization of Model Harnesses**  
  Source for treating harness logic as optimizable code/config, preserving execution traces, and optimizing over prior candidate history.  
  https://arxiv.org/abs/2603.28052

- **DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines**  
  Source for treating LM systems as composable programs optimized against explicit metrics instead of hand-tuned prompt strings.  
  https://arxiv.org/abs/2310.03714

- **GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning**  
  Source for trajectory reflection, candidate evolution, and Pareto-style optimization over compound AI systems.  
  https://arxiv.org/abs/2507.19457

- **SWE-bench**  
  Source for issue-resolution benchmark fixtures and reproducible patch evaluation.  
  https://www.swebench.com/  
  https://github.com/princeton-nlp/SWE-bench

- **Terminal-Bench**  
  Source for terminal-based agent evaluation, useful for validation routing, tool-use behavior, terminal replay, and benchmark providers.  
  https://www.tbench.ai/

- **Agent-as-a-Judge: Evaluate Agents with Agents**  
  Source for step-level diagnostic evaluation of agent trajectories. Horizon may use this for labels and review hints, but durable policy updates still require executable replay, causal ablation, and leverage gates.  
  https://arxiv.org/abs/2410.10934

- **No More, No Less: Task Alignment in Terminal Agents**  
  Source for adversarial context/distractor evaluation in terminal-agent settings.  
  https://arxiv.org/abs/2605.12233

- **Terminal Wrench: A Dataset of Reward-Hackable Environments and Exploit Trajectories**  
  Source for verifier-hacking and benchmark-exploit guardrails.  
  https://arxiv.org/abs/2604.17596

- **TerminalWorld: Benchmarking Agents on Real-World Terminal Tasks**  
  Source for real terminal replay benchmark providers and provider diversity.  
  https://arxiv.org/abs/2605.22535
