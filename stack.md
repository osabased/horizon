# Horizon Stack

Horizon is a local-first intelligence substrate for agentic development.

It does not replace opencode, Claude Code, Codex, Cursor, Aider, or other coding agents. Horizon sits underneath them and makes them cheaper, less repetitive, less confused, and less likely to hallucinate by supplying durable project intelligence.

The stack should be built around one rule:

> Do not rebuild solved infrastructure. Own the meaning layer.

Horizon should wrap existing code-intelligence, memory, harness, validation, and observability systems through stable interfaces. Horizon’s custom value is the control loop: evidence selection, Context Pack compilation, provenance, validation routing, attribution, negative knowledge, and governed self-improvement.

---

## Product Boundary

Horizon owns:

* durable repo truth
* task-scoped evidence selection
* Context Pack ABI
* provenance and exposure manifests
* fact lifecycle
* negative knowledge
* validation routing
* session observation
* attribution
* policy improvement
* replay and audit
* leverage accounting

Horizon does not own:

* chat UI
* editor UI
* terminal UI
* patch generation
* generic coding-agent behavior
* generic CI
* generic long-term memory
* full custom code graph engine
* full custom parser/indexer
* full custom harness ecosystem

---

## Core Loop

```text
Task intent
-> evidence selection
-> Context Pack ABI
-> host-agent use
-> session observation
-> outcome attribution
-> better next pack
```

This is Horizon’s product identity.

Everything in the stack must improve at least one of:

* context quality
* repo truth freshness
* validation efficiency
* attribution accuracy
* agent reliability
* cost reduction
* token reduction
* repeated-scan reduction
* hallucination reduction
* replay/audit confidence

---

## Stack Philosophy

Horizon should be:

* local-first
* adapter-first
* provenance-first
* evidence-backed
* replayable
* replaceable underneath
* strict at the ABI layer
* conservative when learning durable policy
* invisible unless intervention is justified

Horizon should not be:

* another agentic IDE
* another coding agent
* another chat frontend
* another CI platform
* another generic memory product
* another code search product
* another workflow automation platform

---

## Recommended External Substrates

### 1. Repo Intelligence Backend

Primary candidate:

* `codebase-memory-mcp`

Use it for:

* tree-sitter parsing
* symbol graph
* function/class indexing
* call graph
* route graph
* semantic/code search
* impact analysis
* cross-repo/cross-service relationships
* MCP codebase tools
* local repo indexing
* auto-sync/watch behavior

Alternate candidate:

* `codegraph`

Use it for:

* lighter local code graph
* symbol lookup
* call graph
* impact analysis
* full-text search
* coding-agent repo context

Horizon should not build its own parser, code graph, route graph, or symbol index first. It should normalize outputs from these systems into Horizon `EvidenceItem`s.

---

### 2. Harness / Skills / Hooks Layer

Primary reference or dependency:

* `ECC`

Use it for:

* cross-harness architecture
* skill packaging
* hook conventions
* rules distribution
* command packaging
* project/global learning distinction
* continuous-learning mechanics
* Codex / Claude Code / OpenCode / Cursor compatibility ideas

Horizon should not build a full cross-harness skill ecosystem from scratch.

Horizon should own only the Host Adapter ABI and Context Pack rendering contract.

---

### 3. Generic Agent Memory

Optional backend:

* `Hindsight`

Use it for:

* generic retain/recall/reflect memory
* long-term user/project/agent memory
* temporal memory queries
* non-code conversational memory
* experience memory experiments

Do not use it as the source of repo truth.

Repo truth must pass through Horizon’s stricter fact lifecycle, provenance model, invalidators, and negative-knowledge system.

---

### 4. Search and Storage

Canonical local state:

* SQLite

Use:

* SQLite tables for canonical Horizon state
* SQLite FTS5 for lexical search
* sqlite-vec or equivalent for local vector search, if needed
* append-only event tables for replay
* rebuildable projections for graph/vector/search views

Do not build:

* custom vector database
* custom graph database
* custom BM25 engine
* custom distributed code search
* custom persistence engine

Heavy projections may be added later, but they must remain rebuildable.

Possible later modules:

* Kuzu
* DuckDB
* LanceDB
* Qdrant
* Sourcebot
* Zoekt
* livegrep

---

### 5. Validation Engines

Use existing project tools.

Horizon should call, route, cache, and attribute validation. It should not replace validation engines.

Use:

* project test commands
* typecheck commands
* lint commands
* format checks
* Semgrep
* CodeQL
* dependency scanners
* security scanners
* framework-specific test runners

Execution substrates:

* plain local shell first
* devcontainers
* Dagger
* Nix
* Docker
* Firecracker only for high-risk isolation later

Horizon owns:

* validation route selection
* impacted-test selection
* cheapest-useful-check policy
* validation cache
* flaky test registry
* failure signature database
* validation usefulness attribution
* wasted-validation detection

---

### 6. Observability and Replay

Start simple:

* local event ledger
* JSONL export
* SQLite trace tables
* CLI inspection

Optional external systems:

* Langfuse
* LangSmith
* OpenTelemetry
* Inspect AI
* SWE-bench-style harnesses

Horizon should not start with a large observability dashboard.

Horizon’s differentiator is not tracing. It is attribution and policy improvement.

---

## Horizon-Owned Architecture

Horizon is composed of planes. Each plane must consume and emit typed objects. No plane should depend on another plane’s internal storage.

```text
1. Host Integration Plane
2. Local Runtime + Event Ledger Plane
3. Trust Boundary + Exposure Plane
4. Config + Policy Plane
5. Repo Intelligence Adapter Plane
6. Fact + Memory + Negative Knowledge Plane
7. Change Intent Plane
8. Evidence Selection Plane
9. Context Compilation + Context Pack ABI Plane
10. Validation Routing Plane
11. Session Observation Plane
12. Attribution + Self-Optimization Plane
13. Evaluation + Replay Plane
14. Leverage Accounting Plane
15. Explainability + User Surface Plane
16. Contract Governance Plane
```

---

## 1. Host Integration Plane

Owns the boundary between Horizon and coding agents.

Horizon integrates through:

* MCP server
* host adapters
* context rendering
* session hooks
* file-touch observation
* validation brokering
* usage reporting
* capability detection

Host capability levels:

```text
Level 0: manual Context Pack rendering
Level 1: context injection
Level 2: file-touch observation
Level 3: tool-call observation
Level 4: validation observation
Level 5: context-usage attribution
Level 6: permission and validation brokering
```

Horizon must degrade based on measured host capability, not optimistic assumptions.

---

## 2. Local Runtime + Event Ledger Plane

Owns the local Horizon daemon and durable event stream.

Components:

* `horizond`
* SQLite canonical database
* append-only event ledger
* background scheduler
* repo watcher
* artifact store
* cache store
* migration system
* cost ledger
* recovery logic
* fail-open passthrough

Guarantees:

* observations are recorded
* derived state is rebuildable
* packs are reproducible
* decisions have provenance
* caches have fingerprints
* risky runtime failure can fail open

---

## 3. Trust Boundary + Exposure Plane

Owns what Horizon may observe, store, expose, redact, send to hosts, or include in replay.

Every Context Pack must include an `ExposureManifest`.

The manifest should track:

* sensitive categories
* redactions
* host exposure level
* provider exposure level
* local retention policy
* external egress policy
* audit export policy
* warnings

No Context Pack should cross a trust boundary without this manifest.

---

## 4. Config + Policy Plane

Owns typed policy, not prompt instructions.

Primary file:

* `horizon.yaml`

Policy areas:

* repo roots
* host profiles
* context budget profiles
* validation rules
* generated-file rules
* architecture/import rules
* memory promotion rules
* negative-knowledge invalidators
* exposure/redaction rules
* intervention thresholds
* fail-open/fail-closed behavior

Policy compiles into typed decisions.

---

## 5. Repo Intelligence Adapter Plane

Horizon should not build the repo graph from scratch.

It should adapt external backends into Horizon objects.

Primary backend:

* `codebase-memory-mcp`

Alternate backend:

* `codegraph`

Adapter output:

* files
* symbols
* definitions
* references
* call relationships
* routes
* services
* dependencies
* source/test relationships
* ownership hints
* architecture boundaries
* impact neighborhoods
* search results
* stale-index warnings

Horizon converts these into:

* `EvidenceItem`
* `RepoSnapshot`
* `WorkspaceState`
* `RepoRelation`
* `ImpactCandidate`
* `ProvenanceAnchor`
* `Invalidator`

The adapter must be replaceable.

---

## 6. Fact + Memory + Negative Knowledge Plane

Memory is scoped truth management, not transcript storage.

Fact lifecycle:

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

Session claims are evidence, not facts.

Agent claims are weak evidence unless validated.

Negative knowledge is first-class.

Examples:

* nonexistent symbols
* nonexistent files
* deprecated APIs
* unavailable APIs for the repo’s dependency version
* misleading docs
* generated files that should not be edited
* failed fixes
* rejected approaches
* flaky tests
* expensive validations that rarely help
* files the agent repeatedly scans unnecessarily
* patterns that caused reverted patches

Every `NegativeFact` must have:

* scope
* confidence
* evidence
* invalidators
* expiry or refresh policy

---

## 7. Change Intent Plane

Turns vague tasks into bounded implementation intent.

`TaskIntent` should include:

* likely change type
* likely repo areas
* out-of-scope areas
* acceptance criteria
* risk areas
* required facts
* useful validation checks
* uncertainty
* blast radius
* context budget shape

This happens before retrieval.

---

## 8. Evidence Selection Plane

Turns task intent into ranked, scoped, conflict-aware evidence.

Inputs:

* repo intelligence backend results
* previous Context Packs
* approved facts
* negative facts
* validation history
* similar tasks
* local docs
* external docs, if enabled
* session traces
* user corrections

Outputs:

* ranked evidence
* inclusion rationale
* exclusion rationale
* conflict warnings
* stale warnings
* missing-evidence warnings
* context budget allocation

Evidence priority must be typed by claim class.

Implementation truth, architecture intent, dependency/API truth, validation truth, and historical outcome truth need different priority rules.

---

## 9. Context Compilation + Context Pack ABI Plane

This is Horizon’s product center.

A Context Pack is a typed artifact, not a prompt template.

A Context Pack may include:

* ABI version
* pack manifest
* task identity
* host identity
* repo identity
* branch/worktree fingerprint
* dependency fingerprint
* task summary
* change intent
* acceptance criteria
* relevant files
* relevant symbols
* code snippets
* source/test relationships
* architecture constraints
* conventions
* dependency/API facts
* validation hints
* stale warnings
* negative knowledge
* generated-file warnings
* risk notes
* unknowns
* inclusion/exclusion rationale
* confidence scores
* token budget metadata
* provenance anchors
* exposure manifest
* attribution hook IDs
* cache fingerprint
* compatibility policy

Lifecycle:

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

Rendered Markdown is only one view of the pack.

---

## 10. Validation Routing Plane

Validation is not “run tests.”

Validation is:

```text
risk
-> uncertainty
-> cheapest useful check
-> result
-> usefulness attribution
-> route update
```

Horizon owns:

* command discovery
* impacted-test selection
* typecheck/lint/static-analysis routing
* security check routing
* architecture-rule checks
* dependency/API drift checks
* validation cache
* failure summarization
* flaky test registry
* failure signatures
* source/test affinity
* validation cost estimation
* validation usefulness scoring
* wasted-validation detection

Horizon should use existing validation engines.

---

## 11. Session Observation Plane

Observes the host agent without becoming the host agent.

Raw events:

* transcript events
* tool calls
* file reads
* file writes
* terminal commands
* validation runs
* permission prompts
* compaction events
* patch snapshots
* final task result

Derived signals:

* repeated scan
* context ignored
* context missing
* stale context reliance
* hallucinated file/symbol/API
* validation skipped then needed
* user correction
* reverted patch
* task drift
* repeated failed patch attempts
* host compaction lost important context

Observation should be as passive as possible.

---

## 12. Attribution + Self-Optimization Plane

Learns whether Horizon helped, hurt, or was irrelevant.

Attribution loop:

```text
signal
-> hypothesis
-> confidence
-> counterevidence
-> attribution label
-> policy update candidate
-> replay validation
-> durable update or rejection
```

A file being edited does not prove it should have been included.

A snippet being unused does not prove it was bad.

A validation passing does not prove it was useful.

A task succeeding does not prove Horizon helped.

Attribution labels:

* helped
* harmful
* neutral
* ignored
* missing
* stale
* overincluded
* underincluded
* misleading
* wasted
* blocked-usefully
* blocked-unhelpfully

Durable updates must be:

* scoped
* confidence-scored
* reversible
* replay-tested
* invalidatable

---

## 13. Evaluation + Replay Plane

Proves Horizon is actually improving agentic development.

Evaluation targets:

* fewer repeated scans
* fewer hallucinated files/symbols/APIs
* fewer stale-doc mistakes
* lower token use
* lower latency
* cheaper validation
* better patch success
* better context precision
* better context recall
* better attribution accuracy

Use:

* replayable sessions
* Horizon-on/off ablations
* golden task fixtures
* regression suites
* SWE-bench-style adapters only as optional fixtures

Benchmarks are replay fixtures, not product truth.

---

## 14. Leverage Accounting Plane

Prevents Horizon from becoming expensive bloat.

Every subsystem needs a `SubsystemLeverageRecord`.

A leverage record should include:

* subsystem name
* owned objects/events/ABIs
* inputs
* outputs
* cost budget
* observed cost
* expected payoff
* observed payoff
* attribution confidence
* failure mode
* invalidation trigger
* downgrade behavior
* replacement candidate

Possible decisions:

* keep
* tune
* downgrade
* remove
* replace

A subsystem that cannot be attributed cannot safely self-optimize.

---

## 15. Explainability + User Surface Plane

Horizon should be inspectable without being intrusive.

Surfaces:

* CLI
* minimal status output
* Context Pack inspector
* memory review
* stale/conflict review
* attribution inspector
* cost/savings report
* leverage report
* audit bundle export

Commands:

```text
horizon pack build
horizon pack inspect
horizon pack diff
horizon pack explain
horizon pack replay
horizon why-this-context
horizon why-not-this-context
horizon why-this-validation
horizon why-this-memory
horizon show-unused-context
horizon show-missed-context
horizon show-wasted-validation
horizon show-agent-waste
horizon show-savings
horizon show-leverage
horizon replay-attribution
```

Do not start with a large dashboard.

---

## 16. Contract Governance Plane

ABIs are enforceable compatibility surfaces.

Core ABIs:

* Context Pack ABI
* Host Adapter ABI
* Evidence ABI
* Memory Fact ABI
* Negative Fact ABI
* Validation Route ABI
* Attribution ABI
* Policy Decision ABI
* Exposure Manifest ABI
* Audit Bundle ABI
* Leverage Record ABI

Each ABI needs:

* schema version
* migration rules
* compatibility rules
* fixture corpus
* contract tests
* adapter conformance tests

No plane should depend on another plane’s internal storage.

---

## Core Objects

```text
RepoSnapshot
WorkspaceState
WorktreeIdentity
BranchScope
DependencyVersionScope
TaskIntent
ChangeScope
EvidenceItem
EvidenceConflict
EvidencePriorityRule
ProvenanceAnchor
Invalidator
Fact
MemoryFact
NegativeFact
RepoContract
PolicyRule
PolicyDecision
ExposureManifest
ContextItem
ContextPack
PackManifest
PackQualityReport
ValidationRoute
ValidationRun
SessionTrace
AgentAction
PatchSnapshot
AttributionEvent
CostRecord
SubsystemLeverageRecord
AuditBundle
ReplayCase
AbiSchema
ContractVersion
HostCapabilityProfile
HostBehaviorProfile
```

---

## What Not To Build From Scratch

Do not build these first:

```text
custom parser/indexer
custom symbol graph
custom call graph
custom route graph
custom code search engine
custom vector database
custom graph database
custom generic memory system
custom skill/harness ecosystem
custom test runner
custom linter
custom typechecker
custom static analyzer
custom CI replacement
custom observability dashboard
custom coding agent
custom patch generator
custom IDE
```

Use existing systems through adapters.

---

## What Horizon Must Build

Build these directly:

```text
Host Adapter ABI
EvidenceItem schema
Context Pack ABI
ExposureManifest
Fact lifecycle
NegativeFact lifecycle
Evidence normalization
Conflict resolution
TaskIntent model
Pack compiler
Pack quality gate
Validation router
Session observation model
Attribution engine
Policy promotion system
Replay/audit bundle
Leverage accounting
Contract governance
```

These are Horizon’s defensible core.

---

## Recommended MVP Stack

Use:

```text
TypeScript
Node.js
SQLite
SQLite FTS5
MCP SDK
codebase-memory-mcp adapter
optional codegraph adapter
local shell validation adapter
horizon.yaml
JSON Schema or Zod schemas
CLI-first user surface
```

Avoid in MVP:

```text
custom code graph
custom dashboard
custom cloud service
custom workflow engine
custom sandbox
custom vector database
custom model routing layer
custom agent framework
```

---

## MVP Build Order

### Phase 1: Thin Local Spine

Build:

* `horizond`
* SQLite schema
* event ledger
* `horizon.yaml`
* Host Adapter ABI v0
* Context Pack ABI v0
* EvidenceItem schema
* CLI commands

Integrate:

* MCP SDK
* `codebase-memory-mcp` or `codegraph`

Goal:

```text
Given a task, Horizon can query repo intelligence and emit a small evidence-backed Context Pack.
```

---

### Phase 2: Repo Evidence and Negative Knowledge

Build:

* evidence normalization
* provenance anchors
* fact lifecycle
* negative facts
* stale/conflict warnings
* inclusion/exclusion rationale

Goal:

```text
Horizon stops agents from rediscovering false, stale, or irrelevant repo information.
```

---

### Phase 3: Validation Routing

Build:

* validation command discovery
* simple risk classification
* impacted-check selection
* validation result storage
* flaky/wasted validation tracking

Goal:

```text
Horizon recommends or runs the cheapest useful validation for a task.
```

---

### Phase 4: Session Observation and Attribution

Build:

* host event ingestion
* file-touch observation
* context-use signals
* missed-context signals
* attribution labels
* pack usefulness scoring

Borrow ideas from ECC hooks.

Goal:

```text
Horizon learns whether its Context Packs helped, hurt, or were ignored.
```

---

### Phase 5: Governed Self-Optimization

Build:

* policy update candidates
* replay validation
* scoped promotion
* rollback
* leverage reports

Goal:

```text
Horizon improves future packs without turning weak observations into permanent truth.
```

---

## Backend Replacement Policy

Every external backend must sit behind a Horizon-owned adapter.

Backend outputs are never trusted directly.

They become candidate evidence.

```text
external backend result
-> adapter normalization
-> EvidenceItem
-> provenance check
-> conflict/stale check
-> fact or Context Pack candidate
```

Backends can be replaced. Horizon ABIs should not break.

---

## Default Dependency Choices

Recommended defaults:

```text
repo intelligence: codebase-memory-mcp
alternate repo intelligence: codegraph
harness pattern/reference: ECC
generic memory: Hindsight, optional
canonical storage: SQLite
lexical search: SQLite FTS5
vector search: optional sqlite-vec
validation execution: local shell first
schema validation: Zod or JSON Schema
host protocol: MCP
language/runtime: TypeScript + Node.js
```

---

## Final Shape

Horizon should become:

```text
existing repo intelligence
+ existing harness hooks
+ existing validation tools
+ existing optional memory backend
+ Horizon evidence/control layer
```

Not:

```text
a new agent
a new IDE
a new CI system
a new code search engine
a new generic memory platform
```

The winning architecture is a thin, strict, local-first control layer:

```text
repo backend
-> EvidenceItem normalization
-> fact / negative-fact lifecycle
-> task intent
-> evidence selection
-> Context Pack ABI
-> validation routing
-> session observation
-> attribution
-> policy improvement
-> leverage accounting
```

Horizon’s moat is not infrastructure.

Horizon’s moat is knowing what context matters, whether it helped, and how to make the next agent run better.
