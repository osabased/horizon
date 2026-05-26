# Horizon Full Vision Stack

Horizon is a local-first intelligence substrate for agentic development.

It observes the repo, workspace, host-agent session, validation system, and task outcome; builds a durable project model; compiles task-specific Context Packs through a stable ABI; monitors how agents use or misuse that support; attributes outcomes; and continuously improves retrieval, memory, validation, risk, cost, and host-specific behavior.

This document describes the full vision stack. It is not an MVP checklist, build order, or conservative subset.

Horizon is not an agentic IDE, editor, chat interface, terminal UI, or replacement coding agent. It is the system of record for project intelligence that host coding agents are bad at maintaining across sessions.

## North Star

Every coding-agent session should begin with Horizon already knowing the project, already knowing what is stale or false, already knowing the cheapest useful validation route, already knowing the smallest sufficient task context, and already prepared to learn whether its support helped or hurt.

## Product Boundary

Horizon owns:

- durable repo truth
- workspace and branch reality
- scoped project memory
- negative knowledge
- dependency/version reality
- context compilation
- Context Pack ABI
- validation routing
- policy decisions
- exposure/redaction decisions
- agent waste detection
- outcome attribution
- cost accounting
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

Every Horizon subsystem must improve at least one of:

- Context Pack quality
- attribution quality
- validation efficiency
- memory correctness
- repo/session freshness
- cost/latency reduction
- host-agent reliability
- user trust/auditability

If a subsystem does not improve one of these, it belongs outside Horizon.

## Core Flow

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
16. Explainability + User Surface Plane
17. Contract Governance Plane
```

## 1. Host Integration Plane

The Host Integration Plane is the boundary between Horizon and existing coding agents.

Horizon integrates underneath host tools without owning their UI, editor, terminal, patch generation, or interactive debugging loop.

### Responsibilities

- Host Adapter ABI
- opencode plugin
- MCP server
- host capability matrix
- context injection
- Context Pack rendering
- context usage tracking
- session observation hooks
- file-touch observation hooks
- tool-call observation hooks
- permission brokering
- validation brokering
- task result reporting
- adapter degradation behavior
- adapter contract tests

### Host Capability Levels

```text
Level 0: Manual pack rendering
Level 1: Context injection
Level 2: File-touch observation
Level 3: Tool-call and validation observation
Level 4: Context-usage attribution
Level 5: Permission and validation brokering
```

Horizon should degrade from measured host capability, not optimistic assumptions.

### Adapter Contract Tests

Each adapter should prove whether it can:

- observe sessions
- inject context
- render Context Packs
- read tool calls
- observe file touches
- attach validation results
- broker permissions
- detect patch intent
- report task outcomes
- report context usage signals
- preserve Context Pack references through compaction
- fall back safely when a capability is missing

Every host capability should have contract tests and a defined degradation behavior. Partial capability should be represented explicitly, not treated as full support.

## 2. Local Runtime + Event Ledger Plane

The Local Runtime + Event Ledger Plane is Horizon's local kernel.

It makes Horizon warm, local-first, durable, recoverable, inspectable, and fail-open.

### Responsibilities

- `horizond`
- repo watcher
- background scheduler
- immutable event ledger
- normalized event stream
- SQLite canonical state
- index manager
- cache manager
- artifact store
- resource governor
- cost ledger
- migration runner
- corruption recovery
- fail-open passthrough

### Runtime Guarantees

- Every observation is recorded.
- Every derived signal is recomputable.
- Every pack is reproducible.
- Every decision has provenance.
- Every index is rebuildable.
- Every cache has a fingerprint.
- Every mutation is migratable.
- Every risky runtime failure can fail open.

### Canonical Storage Posture

SQLite should remain the canonical local source of truth.

Vector stores, graph stores, large-scale code search, and workflow engines should be treated as replaceable capability modules or rebuildable projections.

### Background Work Rule

Every background task should declare:

- cost budget
- expected payoff
- invalidation trigger
- freshness requirement
- eviction policy
- safe interruption behavior

A background optimizer that costs more than it saves is a product failure.

## 3. Trust Boundary + Exposure Plane

The Trust Boundary + Exposure Plane decides what Horizon may observe, store, expose, redact, send to a host, send to an external docs provider, or include in a replay/audit artifact.

Context Packs are powerful because they compress repo truth. That also makes them sensitive.

### Responsibilities

- local-only data classification
- secret detection and redaction
- network egress policy
- per-host exposure policy
- per-pack exposure manifest
- transcript retention policy
- artifact retention policy
- docs-provider exposure policy
- eval-provider exposure policy
- audit export redaction
- privacy-preserving cost reporting
- privacy-preserving attribution reporting

### Exposure Manifest

No Context Pack should leave the local boundary or enter a host adapter without an `ExposureManifest`.

An `ExposureManifest` should describe:

- included sensitive categories
- redacted fields
- external exposure targets
- host adapter exposure level
- docs-provider exposure level
- retention policy
- audit export policy
- user-visible warnings when needed

### Trust Boundary Rule

Local-first is not enough. Horizon must know and record what crosses each boundary.

## 4. Config + Policy Plane

The Config + Policy Plane lets repos configure Horizon without changing Horizon internals or relying on prompts.

Policy should compile into typed decisions, not prompt text.

### Responsibilities

- `horizon.yaml`
- repo policy schema
- adapter capability schema
- Context Pack requirements
- validation rule schema
- memory promotion rules
- attribution policies
- context budget profiles
- background task schedules
- per-host overrides
- generated-file rules
- architecture/import boundary rules
- destructive-command policy
- exposure/redaction policy
- fail-open/fail-closed preferences by risk class

### Policy Behavior Types

```text
context rules:
  what must be included, excluded, warned, marked uncertain, or redacted

validation rules:
  what must be checked before or after a class of change

intervention rules:
  when Horizon stays silent, warns, challenges, blocks, or fails open

exposure rules:
  what may be shown to a host, sent externally, retained, exported, or replayed
```

## 5. Ingestion + Extraction Plane

The Ingestion + Extraction Plane converts raw repo/docs/session/artifact material into candidate evidence and candidate facts.

Extraction does not write memory directly. Extraction proposes evidence. Fact lifecycle decides whether evidence becomes durable knowledge.

### Inputs

- source files
- tests
- local docs
- external docs
- changelogs
- migration guides
- API docs
- screenshots
- diagrams
- PDFs
- architecture notes
- validation output
- session traces
- generated reference material

### Outputs

- `EvidenceItem`
- candidate `Fact`
- candidate `NegativeFact`
- provenance anchor
- freshness metadata
- invalidator
- extraction confidence
- artifact-derived context item
- claim-type classification

### Responsibilities

- source chunking
- symbol extraction
- source/test affinity extraction
- docs ingestion
- changelog/migration-guide ingestion
- dependency/API fact extraction
- claim extraction
- artifact extraction
- provenance anchoring
- invalidator generation
- extraction confidence scoring

## 6. Workspace + Repo Intelligence Plane

The Workspace + Repo Intelligence Plane turns a raw repo into a compact, queryable project model.

It answers what exists, how it relates, what owns it, what depends on it, what tests it, what constraints govern it, and how reliable that knowledge is.

### Workspace State

- branch
- commit
- dirty files
- staged files
- untracked files
- detached HEAD state
- merge conflict state
- stash state
- lockfile drift
- installed dependency state
- generated-file state
- repo snapshot fingerprint
- worktree identity
- workspace root identity

### Repo Understanding

- file index
- symbol index
- imports
- call relationships
- package graph
- dependency graph
- source/test affinity
- ownership boundaries
- architectural boundaries
- service boundaries
- API provider/consumer map
- generated-code source/output map
- vendored and build-artifact detection

### Multi-Repo and Workspace Graph

- monorepo support
- polyrepo support
- linked package support
- local dependency workspaces
- multi-root repo support
- shared config resolution
- cross-package dependencies
- service boundary maps
- local provider/consumer relationships

### Branch, Worktree, and Concurrency Semantics

Horizon should scope facts, indexes, and attribution across:

- multiple worktrees
- multiple active branches
- rebases
- rewritten history
- force-pushes
- detached HEAD
- stash state
- merge conflict state
- generated state per branch
- dependency version per workspace
- memory scoped to branch, commit range, dependency version, and workspace root

### Repo Contracts

Repo contracts encode durable rules that host agents should respect.

- architectural boundaries
- import rules
- ownership rules
- naming conventions
- framework conventions
- test conventions
- migration policies
- generated-code policies
- forbidden-pattern rules
- source-controlled Horizon checks

## 7. Fact + Memory + Negative Knowledge Plane

The Fact + Memory + Negative Knowledge Plane stores durable project knowledge without letting hallucinations become permanent.

Memory is not a transcript cache. It is scoped truth management.

### Fact Lifecycle

Facts should move through explicit states.

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

Every durable fact should include:

- claim
- claim type
- scope
- source
- evidence
- confidence
- validity window
- branch/worktree scope
- dependency-version scope
- contradictions
- last validation
- invalidators
- attribution history

Session claims are evidence, not facts. Agent claims are weak evidence unless validated.

### Memory Types

- approved facts
- rejected facts
- stale facts
- repo conventions
- architectural contracts
- dependency/API facts
- validation memory
- user-confirmed project facts
- external documentation facts
- artifact-derived facts
- attribution-derived facts

### Negative Knowledge

Negative knowledge is first-class. It prevents repeated agent waste.

Horizon should remember:

- nonexistent symbols
- nonexistent files
- deprecated APIs
- unavailable APIs for the repo's dependency version
- rejected approaches
- failed fixes
- misleading docs
- generated files that should not be edited
- flaky validations
- expensive validations that rarely help
- user-rejected suggestions
- patterns that caused reverted patches

Every `NegativeFact` must have invalidators.

Common invalidators:

- dependency version changed
- file appeared
- symbol appeared
- test added
- user approved a new approach
- branch changed
- framework upgraded
- generated output changed
- repo contract changed

### Evidence Priority Model

When evidence conflicts, Horizon should resolve it with deterministic source priority.

A single global order is not enough. Evidence priority should depend on claim type.

#### Default Fallback Priority

1. live worktree
2. staged changes
3. current branch
4. lockfile / installed dependency state
5. repo config
6. tests
7. source code
8. local repo docs
9. external versioned docs
10. approved memory
11. model prior knowledge

#### Implementation Truth

1. live worktree
2. staged changes
3. source code
4. generated source map
5. repo docs
6. memory

#### Intended Behavior Truth

1. explicit user instruction
2. tests
3. acceptance criteria
4. repo docs
5. source behavior
6. memory

#### Dependency/API Availability Truth

1. lockfile / installed dependency state
2. package manager metadata
3. local API surface
4. versioned external docs
5. changelog / migration guide
6. model prior knowledge

#### Architecture Policy Truth

1. source-controlled Horizon policy
2. repo config
3. architecture docs
4. existing import/dependency graph
5. user-confirmed project facts

#### Historical Outcome Truth

1. replayable session trace
2. validation run
3. patch snapshot
4. user correction
5. attribution event
6. memory summary

The evidence priority model should govern retrieval ranking, fact conflict resolution, memory promotion, Context Pack inclusion, stale warning generation, validation routing, agent warning generation, and attribution confidence.

## 8. Change Intent Plane

The Change Intent Plane converts vague user tasks into bounded implementation intent before retrieval, validation, and risk policy.

### Responsibilities

- task intent extraction
- affected capability resolution
- repo area inference
- non-goal detection
- expected output classification
- risk classification
- blast-radius estimation
- acceptance criteria generation
- validation need estimation
- context budget shaping
- host capability matching

### Outputs

A change-intent object should describe:

- what the user appears to be changing
- what is explicitly out of scope
- which repo areas are likely affected
- which areas are risky
- which facts are required
- which tests or checks are likely useful
- what a successful result should satisfy
- what uncertainty remains

## 9. Evidence Selection Plane

The Evidence Selection Plane chooses what should be considered before a Context Pack is compiled.

It turns task intent into ranked, scoped, conflict-aware candidate evidence.

### Responsibilities

- retrieval planning
- lexical retrieval
- symbol retrieval
- dependency-neighborhood retrieval
- source/test affinity retrieval
- similar-task retrieval
- memory retrieval
- negative-knowledge retrieval
- external-doc retrieval
- evidence ranking
- conflict detection
- typed evidence priority
- inclusion/exclusion rationale
- context budget preallocation
- risk-aware omission detection

### Output

Evidence Selection should produce a ranked evidence set with:

- evidence IDs
- claim types
- provenance anchors
- freshness metadata
- scope
- confidence
- conflict markers
- token/cost estimate
- inclusion rationale
- exclusion rationale when useful

## 10. Context Compilation + Context Pack ABI Plane

The Context Compilation + Context Pack ABI Plane is the product center.

It turns selected evidence into compact, grounded, task-specific intelligence artifacts for host coding agents.

A Context Pack is a compiled artifact, not a prompt template. Rendered markdown is only one host-specific view.

### Responsibilities

- Context Pack assembly
- Context Pack ABI emission
- pack manifest
- provenance handles
- attribution hook IDs
- token budget enforcement
- host-specific rendering
- compaction-safe representation
- redaction/exposure flags
- pack quality gate
- pack cache fingerprint
- pack diff
- pack replay
- pack blame

### Context Pack Contents

A full Context Pack can contain:

- ABI version
- pack schema
- pack manifest
- task identity
- host identity
- repo identity
- branch / commit / worktree fingerprint
- dependency / lockfile fingerprint
- task summary
- change intent
- acceptance criteria
- relevant source snippets
- symbol summaries
- file summaries
- architectural contracts
- repo conventions
- source/test relationships
- validation hints
- dependency/API facts
- external documentation facts
- approved memory
- stale memory warnings
- negative knowledge
- generated-file warnings
- risk notes
- unknowns
- inclusion rationale
- exclusion rationale
- retrieval signals
- confidence score
- risk score
- freshness metadata
- token budget metadata
- provenance anchors
- citation/reference handles
- redaction/exposure flags
- exposure manifest
- expected-use hints
- attribution hook IDs
- cache fingerprint
- pack diff format
- compatibility/migration policy

### Pack Lifecycle

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

### Pack Quality Gate

Before a pack is emitted, Horizon should verify:

- repo snapshot is fresh enough
- task scope is resolved enough
- included facts are supported by evidence
- every included item has provenance
- stale or conflicting facts are marked
- high-risk omissions are explicit
- generated, vendored, minified, and build artifacts are filtered or flagged
- token budget is respected
- host rendering is safe for the target adapter
- redaction rules have been applied
- exposure manifest is attached
- attribution hooks are attached
- pack can be replayed
- pack can be compared with later packs

The quality gate should downgrade, warn, and mark unknowns before blocking. Blocking should be rare and reserved for exposure, secrets, destructive-action, or severe safety issues.

### ABI Goals

- Host agents can consume context without knowing Horizon internals.
- Horizon can change retrieval internals without breaking host adapters.
- Every item in the pack can be traced back to evidence.
- Every item has a cost and a reason for inclusion.
- Every omission can be explained when useful.
- Every pack can be compared against later packs.
- Every pack can participate in outcome attribution.

## 11. Validation Routing Plane

The Validation Routing Plane reduces uncertainty by selecting the cheapest useful check for the current change.

Validation is not "run tests." Validation is risk -> uncertainty -> cheapest discriminating check -> result -> attribution.

### Responsibilities

- command discovery
- impacted-test selection
- typecheck routing
- lint routing
- static-analysis routing
- security scan routing
- architecture-rule checks
- dependency/API drift checks
- validation cache
- failure summarization
- validation escalation
- validation de-escalation
- flaky test registry
- failure signature database
- test ownership map
- test-to-source affinity map
- historical failure correlation
- minimal reproduction extraction
- validation usefulness scoring
- wasted-validation detection
- runtime cost estimation
- safe background execution classification
- attribution-informed validation routing

### Command Discovery Sources

- `package.json` scripts
- `justfile`
- `mise.toml`
- `Makefile`
- `Taskfile.yml`
- `tox.ini`
- `noxfile.py`
- language-native test configs
- CI workflow files

Validation routing should minimize uncertainty, not maximize command execution.

## 12. Intervention + Patch Recovery Plane

The Intervention + Patch Recovery Plane interrupts only when Horizon has enough confidence that silence would be harmful.

Horizon should be invisible by default.

### Intervention Ladder

```text
silent context injection
silent validation routing
passive status note
soft warning
stale-context warning
permission challenge
destructive-action block
fail-open passthrough
```

### Responsibilities

- risky-edit detection
- unexpected-file-change detection
- destructive-action detection
- stale-context warning
- permission challenge
- intervention thresholding
- pre-task checkpoint
- patch snapshot
- rollback plan
- partial rollback support
- recovery bundle
- audit bundle
- fail-open passthrough

Interventions should be attributed like context and validation. A warning can help, hurt, or annoy.

## 13. Session Observation Plane

The Session Observation Plane observes the host agent without becoming the host agent.

It tracks how the agent used, ignored, or fought Horizon's support.

Raw events should be immutable. Derived signals should be recomputable.

### Raw Session Events

- transcript events
- host tool calls
- file reads
- file writes
- terminal commands
- validation runs
- permission prompts
- compaction events
- final task result

### Derived Session Signals

- repeated scan
- context miss
- context ignored
- stale context reliance
- hallucinated file
- hallucinated symbol
- hallucinated API
- validation skipped then needed
- user correction
- reverted patch
- task drift
- repeated failed patch attempts
- host compaction losing important references

### Responsibilities

- transcript parsing
- command/tool-call observation
- file-touch observation
- patch-intent extraction
- context-reference resolution
- host compaction observation
- agent confusion detection
- user-correction capture
- reverted-patch capture
- host behavior profile updates

## 14. Attribution + Self-Optimization Plane

The Attribution + Self-Optimization Plane is Horizon's learning mechanism.

It learns whether Context Packs, retrieval choices, memory facts, validations, and interventions helped, hurt, or were irrelevant.

### Attribution Model

Attribution should be conservative, evidence-weighted, replayable, and reversible.

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

### Attribution Inputs

- Context Pack manifest
- context item IDs
- session transcript
- host tool calls
- file reads
- file edits
- terminal commands
- validation runs
- failed validations
- repeated scans
- hallucinated-symbol incidents
- user corrections
- reverted patches
- final task outcome
- follow-up fixes
- host capability profile
- host behavior profile

### Attribution Labels

- Horizon helped
- Horizon hurt
- Horizon was neutral
- context missing
- context over-included
- context unused
- context stale
- validation saved time
- validation wasted time
- memory prevented repeat failure
- memory caused confusion
- intervention useful
- intervention annoying
- host adapter limited attribution

### Attribution Scopes

- individual snippet
- symbol summary
- file summary
- Context Pack
- retrieval query
- retrieval strategy
- memory fact
- negative fact
- validation route
- validation gate
- policy decision
- host adapter
- host model / agent profile
- task type
- repo area
- branch / commit state

### Self-Optimization Outputs

- retrieval weight updates
- Context Pack template updates
- context budget changes
- memory confidence changes
- stale fact invalidation
- negative knowledge writes
- validation policy changes
- host-specific rendering changes
- intervention threshold changes
- cost/savings estimates
- regression cases for evals
- host behavior profile updates

Attribution updates must be conservative, confidence-scored, reversible, and replay-tested before they affect durable policy.

## 15. Evaluation + Replay Plane

The Evaluation + Replay Plane proves Horizon is actually improving agentic development instead of adding ceremony.

Horizon's claims are empirical: fewer tokens, lower latency, fewer repeated scans, fewer hallucinations, cheaper validation, better patches, and less agent confusion.

Benchmarks are replay fixtures, not product truth.

### Responsibilities

- replayable sessions
- Horizon-on / Horizon-off ablations
- golden task suites
- SWE-bench-style task adapters
- mini-agent baseline runner
- benchmark leakage warnings
- weak-test warnings
- patch outcome verifier
- context precision scoring
- context recall scoring
- context usefulness scoring
- attribution accuracy scoring
- validation usefulness scoring
- hallucination incident tracking
- regression benchmarks
- audit bundles
- provenance diffs
- cost / latency / token comparison

### Evaluation Questions

- Did Horizon reduce total tokens?
- Did Horizon reduce repeated repo scanning?
- Did Horizon reduce time-to-useful-edit?
- Did Horizon reduce hallucinated files, symbols, or APIs?
- Did Horizon select the smallest sufficient context?
- Did Horizon over-include context?
- Did Horizon miss important context?
- Did validation routing catch real issues cheaply?
- Did validation routing waste time?
- Did memory help or mislead?
- Did an intervention prevent harm or just annoy the user?

## 16. Explainability + User Surface Plane

The Explainability + User Surface Plane makes Horizon inspectable without making it intrusive.

Normal usage should stay inside the user's chosen coding agent.

### Surfaces

- minimal status indicator
- diagnostics command
- Context Pack inspector
- attribution inspector
- memory review
- stale/conflict review
- audit explorer
- cost/savings report

### Explainability Commands

- `horizon pack build`
- `horizon pack inspect`
- `horizon pack diff`
- `horizon pack replay`
- `horizon pack explain`
- `horizon pack blame`
- `horizon why-this-context`
- `horizon why-not-this-context`
- `horizon why-this-validation`
- `horizon why-this-memory`
- `horizon why-this-warning`
- `horizon why-this-file`
- `horizon show-context-attribution`
- `horizon show-unused-context`
- `horizon show-missed-context`
- `horizon show-wasted-validation`
- `horizon show-agent-waste`
- `horizon show-savings`
- `horizon replay-attribution`

## 17. Contract Governance Plane

The Contract Governance Plane keeps Horizon's internal and external contracts stable as the system evolves.

ABIs are not documentation. They are enforceable compatibility surfaces.

### Responsibilities

- ABI registry
- schema versioning
- compatibility rules
- migration rules
- deprecation policy
- fixture corpus
- contract tests
- adapter conformance tests
- replay compatibility checks
- forward/backward compatibility reports
- golden Context Pack fixtures
- golden attribution fixtures
- golden exposure fixtures
- golden validation route fixtures

### Contract Rule

Every plane should consume and emit typed objects. No plane should depend on another plane's internal storage.

## Sibling ABIs

The Context Pack ABI is the spinal ABI, but the full system should have several durable contracts.

### Context Pack ABI

What Horizon gives to host agents.

### Host Adapter ABI

What Horizon expects from host integrations.

### Attribution ABI

How session events become learning signals.

### Memory Fact ABI

How durable project facts are stored, scoped, validated, challenged, promoted, and retired.

### Validation Route ABI

How checks are selected, run, cached, summarized, and attributed.

### Policy Decision ABI

How warnings, blocks, permissions, redactions, and fail-open behavior are represented.

### Audit Bundle ABI

How a session can be replayed, inspected, exported, or compared.

### Exposure Manifest ABI

How sensitive data, redaction, egress, retention, and audit exposure are represented.

### Contract Version ABI

How ABI versions, migrations, compatibility windows, and fixture conformance are represented.

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

Level 11: Policy Improvement
  update retrieval, memory, validation, budgets, host profiles, and intervention thresholds

Level 12: Replay and Eval
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

### Isolation

- Firecracker for isolated validation and risky command execution

### Reproducible Execution

- Dagger
- Nix
- devcontainers

### Workflow Durability

- DBOS
- Temporal

### Orchestration Graphs

- LangGraph for explicit complex background reasoning graphs

### Large-Scale Code Search

- Sourcebot
- Zoekt
- livegrep

### Graph Projections

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

Autonomous background work is allowed only when it improves future packs, validation, attribution, freshness, cost, auditability, or host reliability.

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
-> self-optimization system
-> eval, replay, audit, explainability, and contract governance
```

The full vision is not a larger coding agent. It is the durable intelligence layer underneath coding agents: the system that makes them cheaper, less confused, less repetitive, less hallucination-prone, and more aligned with the actual project.
