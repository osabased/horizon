# Horizon Full Vision Stack

Horizon is a local-first intelligence substrate for agentic development.

It observes the repo, workspace, host-agent session, validation system, and task outcome; builds a durable project model; compiles task-specific Context Packs through a stable ABI; monitors how agents use or misuse that support; attributes outcomes; and continuously improves retrieval, memory, validation, risk, cost, and host-specific behavior.

This document describes the full vision stack. It is not an MVP checklist, build order, or conservative subset.

Horizon is not an agentic IDE, editor, chat interface, terminal UI, or replacement coding agent. It is the system of record for project intelligence that host coding agents are bad at maintaining across sessions.

## North Star

Every coding-agent session should begin with Horizon already knowing the project, already knowing what is stale or false, already knowing the cheapest useful validation route, already knowing the smallest sufficient task context, and already prepared to learn whether its support helped or hurt.

Best case: a smaller, cheaper, faster host agent with Horizon should compete with a larger frontier agent without Horizon because Horizon supplies durable project intelligence instead of forcing the model to rediscover the repo every session.

The agents should become smarter with each use.

## Product Boundary

Horizon owns:

- durable repo truth
- workspace and branch reality
- scoped project memory
- negative knowledge
- dependency/version reality
- evidence selection
- context compilation
- Context Pack ABI
- validation routing
- policy decisions
- exposure/redaction decisions
- agent waste detection
- outcome attribution
- cost/leverage accounting
- risk/confidence modeling
- audit/replay
- contract governance for Horizon ABIs

Host agents own:

- chat UI
- editor UI
- terminal UI
- patch generation
- code editing
- interactive debugging
- user conversation
- normal developer workflow

Horizon may influence host behavior through context, validation routing, policy decisions, permission brokering, and warnings, but it should not become the host agent.

## Design Law

Every Horizon subsystem must improve the compounding chain:

```text
Evidence
-> Context Pack ABI
-> Host behavior
-> Outcome attribution
-> Better next run
```

A subsystem is justified when it measurably improves at least one of:

- evidence quality
- Context Pack quality
- attribution quality
- validation efficiency
- memory correctness
- repo/session freshness
- cost/latency reduction
- host-agent reliability
- user trust/auditability
- replay/evaluation confidence

If a subsystem improves the chain, add it. If it does not improve the chain, it belongs outside Horizon or behind a replaceable capability interface.

### Subsystem Admission Rule

A proposed subsystem should answer:

- Which object, event, ABI, or decision does it own?
- Which existing plane consumes its output?
- Which metric should improve because it exists?
- What is its cost budget?
- What invalidates its state?
- How does it fail open?
- How is its help, harm, or neutrality attributed?

A subsystem that cannot be attributed cannot safely self-optimize.

## Core Product Loop

The public spine should stay simple:

```text
Task intent
-> evidence selection
-> Context Pack ABI
-> host use
-> outcome attribution
-> better next pack
```

This is Horizon's product identity: compile the smallest sufficient, evidence-backed context; observe whether it helped; and improve the next run.

## Full Control Loop

```text
Observe locally
-> normalize into the event ledger
-> classify trust boundary and exposure risk
-> update workspace, repo, and fact models
-> infer task and change intent
-> ingest and extract candidate evidence
-> retrieve candidate evidence
-> resolve conflicts by typed evidence priority
-> compile a Context Pack
-> quality-gate the pack
-> attach provenance, exposure manifest, and attribution hooks
-> render it for the host capability profile
-> observe host-agent behavior
-> route validation checks
-> intervene only when risk justifies it
-> capture patch, session, and validation outcome
-> attribute help, hurt, or neutral effects
-> update retrieval, memory, validation, policy, cost, and host profiles
-> replay, evaluate, regress, and prewarm future packs
```

## Nested Loops

Horizon should be built as nested loops rather than one mega-loop.

### Evidence Loop

```text
repo/session/docs/artifacts
-> EvidenceItem
-> conflict resolution
-> Fact / NegativeFact candidates
-> pack-eligible context
```

### Pack Loop

```text
task intent
-> ranked evidence
-> budgeted Context Pack
-> host-specific rendering
-> usage observation
```

### Validation Loop

```text
risk / uncertainty
-> cheapest useful check
-> validation result
-> validation usefulness attribution
-> route update
```

### Waste Loop

```text
repeated scan / hallucination / stale read / ignored context
-> waste signal
-> negative knowledge or retrieval update
-> next-pack improvement
```

### Attribution Loop

```text
session signal
-> attribution hypothesis
-> counterevidence
-> confidence
-> replay validation
-> durable update or rejection
```

### Leverage Loop

```text
subsystem cost
-> measured contribution
-> net leverage score
-> keep / tune / downgrade / remove / replace
```

## Architectural Planes and Control Surfaces

Horizon should be described as planes with durable contracts between them, not as a flat checklist of tools.

```text
1. Host Integration Plane
2. Local Runtime + Event Ledger Plane
3. Trust Boundary + Exposure Plane
4. Config + Policy Plane
5. Ingestion + Extraction Plane
6. Workspace + Repo Intelligence Plane
7. Fact + Memory + Negative Knowledge Plane
8. Change Intent Plane
9. Evidence Selection Plane
10. Context Compilation + Context Pack ABI Plane
11. Validation Routing Plane
12. Intervention + Patch Recovery Plane
13. Session Observation Plane
14. Attribution + Self-Optimization Plane
15. Evaluation + Replay Plane
16. Leverage Accounting Plane
17. Explainability + User Surface Plane
18. Contract Governance Plane
```

## Plane Summaries

### 1. Host Integration Plane

Boundary between Horizon and existing coding agents. Owns the Host Adapter ABI, opencode plugin, MCP server, host capability matrix, context injection, Context Pack rendering, usage tracking, session observation hooks, file-touch observation hooks, tool-call observation hooks, permission brokering, validation brokering, task result reporting, adapter degradation behavior, and adapter contract tests.

Host capability should be explicit:

```text
Level 0: Manual pack rendering
Level 1: Context injection
Level 2: File-touch observation
Level 3: Tool-call and validation observation
Level 4: Context-usage attribution
Level 5: Permission and validation brokering
```

Horizon should degrade from measured capability, not optimistic assumptions.

### 2. Local Runtime + Event Ledger Plane

Horizon's local kernel. Owns `horizond`, repo watching, background scheduling, immutable event ledger, normalized event stream, SQLite canonical state, indexes, caches, artifact store, resource governor, cost ledger, migrations, recovery, and fail-open passthrough.

Guarantees: observations are recorded, derived signals are recomputable, packs are reproducible, decisions have provenance, indexes are rebuildable, caches have fingerprints, mutations are migratable, and risky runtime failure can fail open.

SQLite should remain the canonical local source of truth. Vector stores, graph stores, large-scale code search, and workflow engines should be replaceable capability modules or rebuildable projections.

### 3. Trust Boundary + Exposure Plane

Decides what Horizon may observe, store, expose, redact, send to a host, send to an external provider, or include in replay/audit artifacts.

No Context Pack should leave the local boundary or enter a host adapter without an `ExposureManifest` describing sensitive categories, redactions, exposure targets, host exposure level, provider exposure level, retention, audit export policy, and warnings.

### 4. Config + Policy Plane

Lets repos configure Horizon without changing internals or relying on prompts. Policy compiles into typed decisions, not prompt text.

Owns `horizon.yaml`, repo policy schema, adapter capability schema, Context Pack requirements, validation rules, memory promotion rules, attribution policies, context budget profiles, background task schedules, per-host overrides, generated-file rules, architecture/import rules, sensitive-action policy, exposure/redaction policy, and fail-open/fail-closed preferences by risk class.

### 5. Ingestion + Extraction Plane

Converts raw repo/docs/session/artifact material into candidate evidence and candidate facts. Extraction does not write memory directly. It proposes evidence; fact lifecycle decides whether evidence becomes durable knowledge.

Inputs include source files, tests, local docs, external docs, changelogs, migration guides, API docs, screenshots, diagrams, PDFs, architecture notes, validation output, session traces, and generated reference material.

Outputs include `EvidenceItem`, candidate `Fact`, candidate `NegativeFact`, provenance anchor, freshness metadata, invalidator, extraction confidence, artifact-derived context item, and claim-type classification.

### 6. Workspace + Repo Intelligence Plane

Turns a raw repo into a compact, queryable project model: what exists, how it relates, what owns it, what depends on it, what tests it, what constraints govern it, and how reliable that knowledge is.

Owns workspace state, repo snapshots, file/symbol indexes, imports, call relationships, package/dependency graphs, source/test affinity, ownership boundaries, architecture boundaries, service boundaries, provider/consumer maps, generated-code maps, vendored/build-artifact detection, monorepo/polyrepo support, multi-root support, branch/worktree/concurrency semantics, and repo contracts.

### 7. Fact + Memory + Negative Knowledge Plane

Stores durable project knowledge without letting hallucinations become permanent. Memory is not a transcript cache; it is scoped truth management.

```text
observed
-> proposed
-> verified
-> approved
-> contradicted
-> stale
-> rejected
-> retired
```

Session claims are evidence, not facts. Agent claims are weak evidence unless validated.

Negative knowledge is first-class: nonexistent symbols/files, deprecated APIs, unavailable APIs for the repo's dependency version, rejected approaches, failed fixes, misleading docs, generated files that should not be edited, flaky validations, expensive validations that rarely help, user-rejected suggestions, and patterns that caused reverted patches. Every `NegativeFact` must have invalidators.

Evidence priority must be typed by claim class, not global. Implementation truth, intended behavior truth, dependency/API truth, architecture policy truth, and historical outcome truth should each have distinct priority rules.

### 8. Change Intent Plane

Converts vague user tasks into bounded implementation intent before retrieval, validation, and risk policy.

A `TaskIntent` should describe what appears to be changing, what is out of scope, likely repo areas, risky areas, required facts, useful checks, success criteria, uncertainty, blast radius, and context budget shape.

### 9. Evidence Selection Plane

Turns task intent into ranked, scoped, conflict-aware candidate evidence.

Owns retrieval planning, lexical retrieval, symbol retrieval, dependency-neighborhood retrieval, source/test affinity retrieval, similar-task retrieval, memory retrieval, negative-knowledge retrieval, external-doc retrieval, evidence ranking, conflict detection, typed evidence priority, inclusion/exclusion rationale, context budget preallocation, and risk-aware omission detection.

### 10. Context Compilation + Context Pack ABI Plane

Product center. Turns selected evidence into compact, grounded, task-specific intelligence artifacts for host coding agents.

A Context Pack is a compiled artifact, not a prompt template. Rendered markdown is only one host-specific view.

A full Context Pack can contain ABI version, schema, manifest, task identity, host identity, repo identity, branch/commit/worktree fingerprint, dependency fingerprint, task summary, change intent, acceptance criteria, snippets, symbol summaries, file summaries, architectural contracts, conventions, source/test relationships, validation hints, dependency/API facts, external docs, approved memory, stale warnings, negative knowledge, generated-file warnings, risk notes, unknowns, inclusion/exclusion rationale, retrieval signals, confidence/risk scores, freshness metadata, token budget metadata, provenance anchors, redaction/exposure flags, exposure manifest, expected-use hints, attribution hook IDs, cache fingerprint, pack diff format, and compatibility policy.

Pack lifecycle:

```text
draft
-> retrieved
-> ranked
-> budgeted
-> compiled
-> quality-gated
-> emitted
-> rendered
-> used
-> observed
-> attributed
-> superseded
-> replayed
```

The quality gate should downgrade, warn, and mark unknowns before blocking. Blocking should be rare.

### 11. Validation Routing Plane

Reduces uncertainty by selecting the cheapest useful check for the current change.

Validation is not "run tests." Validation is risk -> uncertainty -> cheapest discriminating check -> result -> attribution.

Owns command discovery, impacted-test selection, typecheck/lint/static-analysis/security routing, architecture-rule checks, dependency/API drift checks, validation cache, failure summarization, escalation/de-escalation, flaky test registry, failure signature database, test ownership map, source/test affinity, historical failure correlation, minimal reproduction extraction, validation usefulness scoring, wasted-validation detection, runtime cost estimation, safe background execution classification, and attribution-informed routing.

### 12. Intervention + Patch Recovery Plane

Interrupts only when Horizon has enough confidence that silence would be harmful. Horizon should be invisible by default.

```text
silent context injection
silent validation routing
passive status note
soft warning
stale-context warning
permission challenge
sensitive-action block
fail-open passthrough
```

Interventions should be attributed like context and validation. A warning can help, hurt, or annoy.

### 13. Session Observation Plane

Observes the host agent without becoming the host agent.

Raw events include transcript events, host tool calls, file reads/writes, terminal commands, validation runs, permission prompts, compaction events, and final task result.

Derived signals include repeated scan, context miss, context ignored, stale context reliance, hallucinated file/symbol/API, validation skipped then needed, user correction, reverted patch, task drift, repeated failed patch attempts, and host compaction losing important references.

### 14. Attribution + Self-Optimization Plane

Learns whether Context Packs, retrieval choices, memory facts, validations, interventions, and subsystems helped, hurt, or were irrelevant.

```text
signal
-> hypothesis
-> confidence
-> counterevidence
-> attribution label
-> policy update candidate
-> replay validation
-> durable update
```

A file being edited does not automatically mean it should have been included. A snippet being unused does not automatically mean it was bad. A validation passing does not automatically mean it was useful. A task succeeding does not automatically mean Horizon helped.

Attribution scopes include snippets, summaries, Context Packs, retrieval queries, retrieval strategies, memory facts, negative facts, validation routes, policy decisions, host adapters, host profiles, subsystems, task types, repo areas, and branch/commit state.

Updates must be conservative, confidence-scored, reversible, and replay-tested before they affect durable policy.

### 15. Evaluation + Replay Plane

Proves Horizon is actually improving agentic development instead of adding ceremony.

Horizon's claims are empirical: fewer tokens, lower latency, fewer repeated scans, fewer hallucinations, cheaper validation, better patches, and less agent confusion. Benchmarks are replay fixtures, not product truth.

Owns replayable sessions, Horizon-on/off ablations, golden task suites, SWE-bench-style adapters, mini-agent baseline runner, benchmark leakage warnings, weak-test warnings, patch outcome verifier, context precision/recall/usefulness scoring, attribution accuracy scoring, validation usefulness scoring, hallucination incident tracking, subsystem leverage scoring, regression benchmarks, audit bundles, provenance diffs, and cost/latency/token comparison.

### 16. Leverage Accounting Plane

Prevents the full vision from becoming random accumulation. It does not make Horizon conservative. It makes ambition falsifiable.

Owns subsystem cost accounting, subsystem contribution accounting, token/latency delta tracking, repeated-scan reduction tracking, hallucination incident reduction tracking, validation waste reduction tracking, context precision/recall delta tracking, attribution confidence delta tracking, background-task payoff tracking, host-specific leverage profiles, and replace/downgrade/remove recommendations for low-leverage modules.

A `SubsystemLeverageRecord` should describe subsystem name, owned objects/events/ABIs, consumed inputs, emitted outputs, cost budget, observed cost, expected payoff, observed payoff, attribution confidence, failure mode, invalidation trigger, downgrade behavior, and replacement candidate.

Horizon should add any subsystem that improves the compounding chain, but every subsystem must remain accountable to cost, attribution, and replay.

### 17. Explainability + User Surface Plane

Makes Horizon inspectable without making it intrusive. Normal usage should stay inside the user's chosen coding agent.

Surfaces include minimal status indicator, diagnostics command, Context Pack inspector, attribution inspector, memory review, stale/conflict review, audit explorer, cost/savings report, and subsystem leverage report.

Commands include `horizon pack build`, `horizon pack inspect`, `horizon pack diff`, `horizon pack replay`, `horizon pack explain`, `horizon pack blame`, `horizon why-this-context`, `horizon why-not-this-context`, `horizon why-this-validation`, `horizon why-this-memory`, `horizon why-this-warning`, `horizon why-this-file`, `horizon show-context-attribution`, `horizon show-unused-context`, `horizon show-missed-context`, `horizon show-wasted-validation`, `horizon show-agent-waste`, `horizon show-savings`, `horizon show-leverage`, and `horizon replay-attribution`.

### 18. Contract Governance Plane

Keeps Horizon's internal and external contracts stable as the system evolves.

ABIs are not documentation. They are enforceable compatibility surfaces.

Owns ABI registry, schema versioning, compatibility rules, migration rules, deprecation policy, fixture corpus, contract tests, adapter conformance tests, replay compatibility checks, forward/backward compatibility reports, golden Context Pack fixtures, golden attribution fixtures, golden exposure fixtures, golden validation route fixtures, and golden leverage fixtures.

Every plane should consume and emit typed objects. No plane should depend on another plane's internal storage.

## Sibling ABIs

The Context Pack ABI is the spinal ABI, but the full system should have several durable contracts.

- Context Pack ABI: what Horizon gives to host agents.
- Host Adapter ABI: what Horizon expects from host integrations.
- Attribution ABI: how session events become learning signals.
- Memory Fact ABI: how durable project facts are stored, scoped, validated, challenged, promoted, and retired.
- Validation Route ABI: how checks are selected, run, cached, summarized, and attributed.
- Policy Decision ABI: how warnings, blocks, permissions, redactions, and fail-open behavior are represented.
- Audit Bundle ABI: how a session can be replayed, inspected, exported, or compared.
- Exposure Manifest ABI: how sensitive data, redaction, egress, retention, and audit exposure are represented.
- Contract Version ABI: how ABI versions, migrations, compatibility windows, and fixture conformance are represented.
- Leverage Record ABI: how subsystem cost, payoff, attribution, and replacement decisions are represented.

## Core Object Model

The full system should revolve around a small set of canonical objects.

- `RepoSnapshot`
- `WorkspaceState`
- `WorktreeIdentity`
- `BranchScope`
- `DependencyVersionScope`
- `TaskIntent`
- `ChangeScope`
- `EvidenceItem`
- `EvidenceConflict`
- `EvidencePriorityRule`
- `ExtractionJob`
- `ExtractionResult`
- `Invalidator`
- `Fact`
- `MemoryFact`
- `NegativeFact`
- `RepoContract`
- `PolicyRule`
- `PolicyDecision`
- `ExposurePolicy`
- `ExposureManifest`
- `ContextItem`
- `ContextPack`
- `PackManifest`
- `PackQualityReport`
- `ValidationRoute`
- `ValidationRun`
- `SessionTrace`
- `AgentAction`
- `PatchSnapshot`
- `AttributionEvent`
- `CostRecord`
- `SubsystemLeverageRecord`
- `AuditBundle`
- `ReplayCase`
- `AbiSchema`
- `ContractVersion`
- `AbiFixture`
- `AdapterConformanceReport`
- `HostCapabilityProfile`
- `HostBehaviorProfile`

## Intelligence Hierarchy

```text
Level 0: Raw Events
  filesystem, git, terminal, host session, validation, patches

Level 1: Normalized State
  workspace snapshot, repo snapshot, session trace, validation trace

Level 2: Structural Understanding
  files, symbols, packages, dependencies, tests, ownership, boundaries

Level 3: Candidate Evidence
  source claims, doc claims, artifact claims, validation claims, session claims

Level 4: Durable Facts
  conventions, contracts, API facts, external docs, negative knowledge, stale/conflict state

Level 5: Task Interpretation
  intent, scope, risk, blast radius, acceptance criteria

Level 6: Evidence Selection
  retrieve, rank, filter, budget, explain inclusion/exclusion

Level 7: Context Compilation
  produce ABI-stable Context Pack with provenance, exposure manifest, and attribution hooks

Level 8: Runtime Guidance
  inject context, route validation, warn or block only when needed

Level 9: Behavioral Observation
  monitor how host agent uses, ignores, or misuses support

Level 10: Outcome Attribution
  assign help, hurt, neutral, missing, stale, overincluded, or wasted labels

Level 11: Leverage Accounting
  measure subsystem cost, payoff, and net contribution

Level 12: Policy Improvement
  update retrieval, memory, validation, budgets, host profiles, and intervention thresholds

Level 13: Replay and Eval
  prove that Horizon is reducing cost, latency, hallucination, and bad changes
```

## Full Vision Capability Map

This is a capability map, not an implementation sequence.

### Always Core

- Context Pack ABI
- event ledger
- repo snapshot
- workspace state
- evidence items
- fact lifecycle
- provenance model
- exposure manifest
- attribution hooks
- pack quality gate
- adapter capability profile
- subsystem leverage records
- replayable audit bundle
- contract governance

### Core but Replaceable Implementation

- lexical search
- symbol extraction
- source/test affinity
- validation route discovery
- memory retrieval
- negative knowledge retrieval
- cost ledger
- evidence ranking
- replay runner
- leverage scorer

### Optional Capability Modules

- vector search
- graph projection
- isolated validation
- workflow engine
- large-scale code search
- external docs providers
- benchmark harness providers

## Optional Heavy Backends as Capability Modules

Heavy backends are part of the full vision, but they should sit behind capability interfaces and remain replaceable.

- Firecracker for isolated validation and risky command execution
- Dagger, Nix, and devcontainers for reproducible execution
- DBOS and Temporal for workflow durability
- LangGraph for explicit complex background reasoning graphs
- Sourcebot, Zoekt, and livegrep for large-scale code search
- Kuzu or SQLite edge tables for provenance traversal, conflict graphs, dependency graphs, file-to-test relationships, and symbol/fact relationships

The rule is not to avoid heavy backends. The rule is that heavy backends must not leak into Horizon's meaning layer.

## Non-Goals

Horizon does not own:

- chat UI
- editor UI
- terminal UI
- patch generation
- interactive debugging
- agent personality
- autonomous product management
- generic task execution
- generic CI replacement
- cloud-first repo indexing
- model provider abstraction as a product

Autonomous background work is allowed only when it improves future packs, validation, attribution, freshness, cost, auditability, host reliability, or subsystem leverage.

## Failure Modes and Adversarial Guardrails

### Horizon becomes a second coding agent

Hardening rule: Horizon may recommend context, validation, policy decisions, and warnings. Horizon must not generate patches as a core responsibility.

### Context Pack ABI becomes prompt formatting

Hardening rule: A Context Pack is a typed artifact with schema, manifest, evidence IDs, attribution hooks, compatibility version, redaction flags, exposure manifest, and replay data.

### Attribution learns false lessons

Hardening rule: Attribution updates must be conservative, confidence-scored, reversible, and replay-tested before they affect durable policy.

### Memory becomes a hallucination landfill

Hardening rule: Memory writes must pass through fact lifecycle. Session claims are evidence, not facts. Agent claims are weak evidence unless validated.

### Negative knowledge blocks legitimate future changes

Hardening rule: Every `NegativeFact` must have invalidators and scopes.

### Evidence priority is wrong for the claim type

Hardening rule: Evidence priority must be typed by claim class, with the global order used only as a fallback.

### Host adapters lie by omission

Hardening rule: Every host capability must have contract tests and a degradation behavior.

### Horizon becomes expensive while claiming to reduce cost

Hardening rule: Every background task needs a cost budget, expected payoff, invalidation trigger, and eviction policy.

### Subsystems accumulate without leverage

Hardening rule: Every subsystem needs an owned object/event/ABI, cost budget, output consumer, leverage metric, attribution path, and downgrade behavior.

### Trust boundary is added too late

Hardening rule: No Context Pack should leave the local boundary or enter a host adapter without an `ExposureManifest`.

### Evaluation optimizes for benchmark theater

Hardening rule: Benchmarks are replay fixtures, not product truth.

### Pack quality gate blocks too much

Hardening rule: Pack quality should downgrade, warn, and mark unknowns before blocking. Blocking should be rare.

### Full vision becomes unimplementable spaghetti

Hardening rule: Every plane must consume and emit typed objects. No plane should depend on another plane's internal storage.

## Final Shape

Horizon is a local-first closed-loop intelligence substrate for agentic development.

```text
host adapters
-> local runtime and event ledger
-> trust boundary and policy compiler
-> ingestion and extraction
-> repo/workspace intelligence
-> fact, memory, and negative-knowledge system
-> change-intent reasoner
-> evidence selector
-> Context Pack compiler and ABI
-> validation router
-> intervention and patch recovery
-> session behavior observer
-> attribution engine
-> leverage accounting
-> self-optimization system
-> eval, replay, audit, explainability, and contract governance
```

The full vision is not a larger coding agent. It is the durable intelligence layer underneath coding agents: the system that makes them cheaper, less confused, less repetitive, less hallucination-prone, and more aligned with the actual project.
