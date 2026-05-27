# Horizon Experience Optimization Addendum

This document extends `stack.md` with an experience-optimization layer for Horizon.

The point is not to turn Horizon into a patch-writing coding agent. Horizon should still sit underneath opencode, Codex, Claude Code, and other host agents. The point is to make Horizon better at what it owns: evidence selection, Context Pack compilation, validation routing, memory policy, negative knowledge, host adaptation, intervention policy, attribution, replay, audit, and leverage accounting.

## Core Thesis

Horizon should treat its own support logic as an optimizable harness.

```text
repo/session evidence
-> Context Pack
-> host-agent behavior
-> validation and patch outcome
-> attribution
-> replay/eval result
-> candidate Horizon policy improvement
-> replay/ablation/leverage gate
-> durable Horizon behavior
```

The improvement loop should optimize Horizon's harness around the host agent, not replace the host agent.

A harness candidate may change how Horizon retrieves evidence, budgets context, formats a Context Pack, routes validation, promotes memory, emits negative knowledge, renders for a specific host, or decides when to warn. It should not directly generate user product patches as a core responsibility.

## Why This Belongs in Horizon

Horizon's existing stack already has the necessary ingredients:

- replayable sessions
- audit bundles
- Horizon-on/off ablations
- golden task suites
- SWE-bench-style adapters
- patch outcome verification
- context precision/recall/usefulness scoring
- attribution accuracy scoring
- validation usefulness scoring
- hallucination incident tracking
- subsystem leverage scoring
- regression benchmarks
- cost/latency/token comparison

The missing piece is how those results become practical improvement pressure on Horizon itself.

Eval should not only answer:

```text
Did this run pass?
```

It should also answer:

```text
Which part of Horizon helped, hurt, wasted budget, or missed evidence?
What candidate policy would improve the next run?
Can that candidate survive replay, ablation, regression, and leverage gates?
```

## Experience Optimization Plane

Add this plane after the Evaluation + Replay Plane and before Leverage Accounting.

### Experience Optimization Plane

Turns replay history into better Horizon behavior.

This plane treats retrieval strategies, pack recipes, validation routers, memory policies, negative-knowledge rules, host renderers, adapter behaviors, context budgets, and intervention thresholds as candidate harnesses.

Candidates are proposed from full prior experience, replayed against benchmarks and product traces, compared against baselines and ablations, scored on a Pareto frontier, and either rejected, quarantined, or promoted.

```text
Experience Store
-> candidate generation
-> cheap contract checks
-> replay evaluation
-> causal ablation
-> regression suite
-> leverage accounting
-> durable promotion or rejection
```

The optimizer must inspect raw experience, not only summaries. Summaries are for humans. Raw traces are the optimization substrate.

## Experience Store

The Experience Store is a queryable filesystem/database of prior runs, candidates, outcomes, failures, and rejected hypotheses.

It should preserve:

- task prompt
- task intent
- repo snapshot
- workspace state
- branch/worktree identity
- dependency fingerprint
- host capability profile
- host behavior profile
- Context Pack
- Pack Quality Report
- exposure manifest
- retrieval queries
- included context
- excluded context
- stale/conflict warnings
- negative knowledge used
- file reads
- file writes
- host-agent tool calls
- terminal commands
- validation routes
- validation output
- patch snapshot
- patch verdict
- attribution labels
- cost records
- latency records
- token records
- hallucination incidents
- repeated-scan signals
- candidate policy/config/code
- scorecards
- replay diffs
- ablation deltas
- rejected hypotheses
- promotion/rejection decisions

The store should be inspectable through normal tools:

```text
horizon experience list
horizon experience show <run>
horizon experience grep <pattern>
horizon experience diff <run-a> <run-b>
horizon experience trace <run>
horizon experience frontier
horizon experience rejected
```

A human-facing dashboard can summarize this, but the optimizer should be able to inspect full artifacts.

## Harness Candidates

A `HarnessCandidate` is a proposed change to Horizon's support behavior.

Examples:

- retrieval strategy candidate
- evidence-ranking candidate
- Context Pack recipe candidate
- context-budget candidate
- pack-rendering candidate
- validation-route candidate
- memory-promotion candidate
- negative-knowledge candidate
- stale/conflict policy candidate
- intervention-threshold candidate
- host-adapter behavior candidate
- host-specific rendering candidate
- background prewarm candidate
- exposure/redaction policy candidate

A candidate should declare:

- candidate ID
- parent candidate or baseline
- changed policy/config/code
- intended task classes
- expected metric improvement
- expected cost
- affected Horizon objects/ABIs
- invalidation triggers
- rollback behavior
- required replay suites
- required contract checks
- allowed hosts
- allowed repos/scopes
- safety/exposure implications

A candidate must be explicit enough to replay and diff. It should not be an informal note like "retrieve better context."

## Candidate Lifecycle

```text
proposed
-> contract-checked
-> replayed
-> ablated
-> regression-tested
-> leverage-scored
-> quarantined
-> promoted
-> monitored
-> reverted or retired
```

### Proposed

The optimizer proposes a candidate from prior runs, failures, traces, and scorecards.

### Contract-checked

Cheap checks run before expensive replay:

- schema validity
- ABI compatibility
- config parse
- missing-field checks
- host-adapter compatibility
- exposure-policy compatibility
- fixture conformance

### Replayed

The candidate runs against relevant ReplayCases and benchmark provider tasks.

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

### Regression-tested

The candidate must not regress unrelated task classes, hosts, repos, or safety/exposure behavior.

### Leverage-scored

The candidate is scored by net contribution, not only pass rate.

### Quarantined

A candidate that helps only a narrow benchmark or task class can be quarantined behind a scope rule instead of promoted globally.

### Promoted

Durable promotion requires replay evidence, causal ablation, regression checks, and leverage accounting.

### Monitored

A promoted candidate remains reversible. Product-session evidence can downgrade, narrow, or retire it.

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

## Pareto Frontier, Not One Score

Horizon should not optimize one scalar leaderboard score.

A candidate that improves patch success but doubles token cost may be bad. A candidate that cuts context in half but misses critical evidence may be bad. A candidate that passes benchmarks by exploiting weak tests is bad.

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
- attribution confidence
- host-agent waste reduction
- exposure risk
- replay reproducibility
- benchmark leakage risk

Promotion should require a net improvement profile appropriate to the task class and host profile.

## Benchmark Provider ABI

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

## Product-Session Evidence

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
- did Horizon remain invisible when it should?

Product evidence is messier than benchmarks, but it is closer to Horizon's product truth.

## Attribution Requirements

Experience optimization depends on attribution quality.

A task succeeding does not prove Horizon helped. A file being edited does not prove it should have been included. A snippet being ignored does not prove it was useless. A validation passing does not prove the validation route was useful.

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
- benchmark-specific
- host-specific
- repo-specific
- task-class-specific

Durable policy updates should require enough evidence to separate Horizon helping from the host agent succeeding anyway.

## Practical Output

Experience optimization should compile results into concrete Horizon updates:

| Signal | Practical output |
|---|---|
| Useful context included | Promote retrieval rule or pack recipe |
| Necessary context missed | Add retrieval edge, source/test affinity, or evidence requirement |
| Irrelevant context included | Tighten budget/ranking/filter |
| Context ignored | Reformat, shrink, or lower priority |
| Context contradicted | Create conflict and stale warning |
| Hallucinated API/file | Add negative fact or dependency-version fact |
| Validation caught bug | Promote validation route for similar task class |
| Validation wasted time | Downgrade route or increase threshold |
| User corrected missing fact | Add candidate evidence, not durable memory yet |
| Reverted patch | Add negative outcome evidence |
| Candidate helps only benchmark | Quarantine as benchmark-specific |
| Candidate adds cost without payoff | Downgrade/remove via leverage accounting |

The output should not be just a report. It should be:

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
-> RegressionGateResult
-> AuditBundle
```

## Sibling ABIs to Add

Add these to the Sibling ABIs section of `stack.md`:

- **Benchmark Provider ABI**: how external and local benchmark suites normalize into Horizon replay cases.
- **Harness Candidate ABI**: how candidate retrieval, pack, validation, memory, intervention, host-rendering, and adapter strategies are represented.
- **Optimization Run ABI**: how candidate policies, traces, scorecards, ablations, regressions, and promotion decisions are stored.
- **Experience Store ABI**: how full replay history is preserved and queried without compressing away diagnostic evidence.
- **Benchmark Verdict ABI**: how benchmark outcomes, verifier confidence, leakage risk, and product relevance are represented.

## Core Objects to Add

Add these to the Core Object Model:

- `BenchmarkProvider`
- `BenchmarkVerdict`
- `ExperienceStore`
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
- `RegressionGateResult`
- `RejectedHypothesis`
- `PromotionDecision`

## Commands to Add

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
horizon candidate promote <candidate>
horizon candidate reject <candidate>
horizon benchmark providers
horizon benchmark run <provider>
horizon benchmark import <provider>
horizon optimization report
```

These commands are inspection and governance surfaces. Normal usage should remain in the background.

## Guardrails

### Horizon becomes a coding agent

Hardening rule: Experience optimization may propose Horizon policy, retrieval, validation, memory, rendering, and adapter changes. It must not make patch generation a core Horizon responsibility.

### Optimization overfits to benchmark artifacts

Hardening rule: Candidate policies may inspect benchmark traces, but durable promotion requires held-out replay, leakage checks, causal ablation, regression checks, and product-session evidence. Benchmark-only wins must be marked benchmark-specific.

### Raw traces become unsafe memory

Hardening rule: Experience Store artifacts are evidence, not durable facts. Memory promotion still goes through fact lifecycle, scopes, invalidators, and exposure policy.

### Summaries hide important failure evidence

Hardening rule: Human summaries may exist, but optimization must preserve raw traces, scorecards, Context Packs, validation logs, replay diffs, and rejected hypotheses.

### A candidate wins by increasing cost

Hardening rule: Every candidate must be scored against cost, latency, token use, validation cost, and subsystem leverage. A correctness gain can still fail promotion if the cost profile is bad.

### Agentic judges become ground truth

Hardening rule: Agentic judges may produce diagnostic labels and review hints. They do not replace executable replay, patch verification, causal ablation, or leverage gates.

### Product sessions leak sensitive information

Hardening rule: Experience Store writes and exports must obey the Trust Boundary + Exposure Plane. No trace should be exposed to a host, provider, benchmark, or audit export without an ExposureManifest.

### Candidate policies become irreversible

Hardening rule: Every promoted candidate needs rollback behavior, invalidation triggers, and monitoring. Promotion is reversible.

## How It Maps Back to `stack.md`

This addendum changes the stack as follows:

1. Add an **Experience Optimization Plane** after Evaluation + Replay and before Leverage Accounting.
2. Extend the **Evaluation + Replay Plane** so evals produce queryable experience, not just reports.
3. Extend the **Attribution + Self-Optimization Plane** so attribution labels feed HarnessCandidates and PolicyUpdateCandidates.
4. Extend the **Leverage Accounting Plane** so every candidate is scored by net leverage before promotion.
5. Extend the **Contract Governance Plane** with Benchmark Provider, Harness Candidate, Optimization Run, Experience Store, and Benchmark Verdict ABIs.
6. Extend the **Explainability + User Surface Plane** with experience and candidate inspection commands.
7. Extend the **Failure Modes and Adversarial Guardrails** with benchmark overfitting, trace compression, unsafe memory, judge overtrust, and irreversible promotion guardrails.

## Final Shape

Experience optimization makes Horizon a self-improving support substrate:

```text
host-agent session
-> Horizon support behavior
-> replay/audit bundle
-> attribution and leverage record
-> queryable experience
-> candidate harness improvement
-> replay + ablation + regression + leverage gates
-> scoped durable update
```

The durable product claim becomes stronger:

```text
Horizon does not just remember the repo.
Horizon learns which support behavior actually makes coding agents cheaper, less confused, less repetitive, and less hallucination-prone.
```

## Sources

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
