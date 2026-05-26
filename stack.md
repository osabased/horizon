# Horizon Full Vision Stack

Horizon is a local-first intelligence substrate for agentic development.

It observes the repo, workspace, host-agent session, validation system, and task outcome; builds a durable project model; compiles task-specific Context Packs through a stable ABI; monitors how agents use or misuse that support; attributes outcomes; and continuously improves retrieval, memory, validation, risk, cost, and host-specific behavior.

Horizon is not an agentic IDE, editor, chat interface, terminal UI, or replacement coding agent. It is the system of record for project intelligence that host coding agents are bad at maintaining across sessions.

## North Star

Every coding-agent session should begin with Horizon already knowing the project, already knowing what is stale or false, already knowing the cheapest useful validation route, already knowing the smallest sufficient task context, and already prepared to learn whether its support helped or hurt.

## Core Flow

```text
Observe everything locally
-> normalize into the event ledger
-> update workspace, repo, and fact models
-> infer task and change intent
-> retrieve candidate evidence
-> resolve conflicts by evidence priority
-> compile a Context Pack
-> quality-gate the pack
-> render it for the host capability profile
-> observe host-agent behavior
-> route validation and safety checks
-> capture patch, session, and validation outcome
-> attribute help, hurt, or neutral effects
-> update retrieval, memory, validation, policy, cost, and host profiles
-> replay, evaluate, regress, and prewarm future packs
```

## Architectural Planes

Horizon should be described as planes of intelligence with durable contracts between them, not as a flat checklist of tools.

```text
Host Integration Plane
-> Local Runtime Plane
-> Workspace + Repo Intelligence Plane
-> Fact + Memory Plane
-> Change Intent Plane
-> Context Compilation Plane
-> Validation + Safety Plane
-> Session Intelligence Plane
-> Attribution + Self-Optimization Plane
-> Evaluation + Replay Plane
-> Explainability + User Surface Plane
```

## 1. Host Integration Plane

The Host Integration Plane is the boundary between Horizon and existing coding agents.

Horizon integrates underneath host tools without owning their UI, editor, terminal, patch generation, or interactive debugging loop.

### Responsibilities

- Host adapter ABI
- opencode plugin
- MCP server
- host capability matrix
- session observation
- context injection
- context-pack rendering
- context usage tracking
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
- preserve context-pack references through compaction
- fall back safely when a capability is missing

## 2. Local Runtime Plane

The Local Runtime Plane is Horizon's local kernel.

It makes Horizon warm, local-first, durable, recoverable, inspectable, and fail-open.

### Responsibilities

- `horizond`
- repo watcher
- background scheduler
- event ledger
- SQLite canonical state
- index manager
- cache manager
- artifact store
- resource governor
- migrations
- corruption recovery
- local privacy boundary
- fail-open passthrough

### Runtime Guarantees

- Every observation is recorded.
- Every pack is reproducible.
- Every decision has provenance.
- Every index is rebuildable.
- Every cache has a fingerprint.
- Every mutation is migratable.
- Every risky runtime failure can fail open.

### Canonical Storage Posture

SQLite should remain the canonical local source of truth.

Vector stores, graph stores, large-scale code search, and workflow engines should be treated as replaceable capability modules or rebuildable projections.

## 3. Workspace + Repo Intelligence Plane

The Workspace + Repo Intelligence Plane turns a raw repo into a compact, queryable project model.

It answers what exists, how it relates, what owns it, what depends on it, what tests it, what constraints govern it, and how reliable that knowledge is.

### Workspace State

- branch
- commit
- dirty files
- staged files
- untracked files
- lockfile drift
- installed dependency state
- generated-file state
- repo snapshot fingerprint

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

## 4. Fact + Memory Plane

The Fact + Memory Plane stores durable project knowledge without letting hallucinations become permanent.

Memory is not a transcript cache. It is scoped truth management.

### Evidence Priority Model

When facts conflict, Horizon should resolve them with deterministic source priority.

Default priority:

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

The evidence priority model should govern retrieval ranking, fact conflict resolution, memory promotion, Context Pack inclusion, stale warning generation, validation routing, agent warning generation, and attribution confidence.

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
- scope
- source
- evidence
- confidence
- validity window
- branch / worktree scope
- dependency-version scope
- contradictions
- last validation
- invalidators
- attribution history

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

## 5. Change Intent Plane

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

### Outputs

A change-intent object should describe:

- what the user appears to be changing
- what is explicitly out of scope
- which repo areas are likely affected
- which areas are risky
- which facts are required
- which tests or checks are likely useful
- what a successful result should satisfy

## 6. Context Compilation Plane

The Context Compilation Plane is the product center.

It compiles messy project reality into small, grounded, task-specific intelligence artifacts for host coding agents.

A Context Pack is a compiled artifact, not a prompt template.

### Responsibilities

- retrieval planning
- lexical search
- symbol search
- dependency-neighborhood retrieval
- source/test affinity retrieval
- similar task retrieval
- memory retrieval
- negative knowledge retrieval
- external docs retrieval
- context budgeting
- evidence selection
- conflict filtering
- Context Pack compilation
- Context Pack ABI emission
- pack quality gating
- host-specific rendering
- compaction-safe representation
- pack caching
- pack diffing
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
- citation / reference handles
- redaction / exposure flags
- expected-use hints
- attribution hook IDs
- cache fingerprint
- pack diff format
- compatibility / migration policy

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
- attribution hooks are attached
- pack can be replayed
- pack can be compared with later packs

### ABI Goals

- Host agents can consume context without knowing Horizon internals.
- Horizon can change retrieval internals without breaking host adapters.
- Every item in the pack can be traced back to evidence.
- Every item has a cost and a reason for inclusion.
- Every omission can be explained when useful.
- Every pack can be compared against later packs.
- Every pack can participate in outcome attribution.

## 7. Validation + Safety Plane

The Validation + Safety Plane reduces risk without turning Horizon into a CI platform or coding agent.

Validation is not "run tests." Validation is risk -> uncertainty -> cheapest discriminating check -> result -> attribution.

### Validation Routing

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
- attribution-informed validation routing

### Validation Intelligence

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

### Policy Behavior

Policy should compile into three behavior types.

```text
context rules:
  what must be included, excluded, warned, or redacted

validation rules:
  what must be checked before or after a class of change

intervention rules:
  when Horizon stays silent, warns, challenges, blocks, or fails open
```

### Intervention Ladder

Horizon should be invisible by default and interrupt only when risk, staleness, cost, or safety justifies it.

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

### Patch Safety and Recovery

- pre-task checkpoint
- patch snapshot
- rollback plan
- partial rollback support
- risky-edit detector
- unexpected-file-change detector
- recovery bundle
- audit bundle

## 8. Session Intelligence Plane

The Session Intelligence Plane observes the host agent without becoming the host agent.

It tracks how the agent used, ignored, or fought Horizon's support.

### Responsibilities

- transcript parsing
- command/tool-call observation
- file-touch observation
- patch-intent extraction
- repeated-scan detection
- context-miss detection
- context-usage detection
- context-reference resolution
- host compaction observation
- agent confusion detection
- hallucinated-symbol detection
- hallucinated-file detection
- hallucinated-API detection
- user-correction capture
- reverted-patch capture
- host behavior profile updates

### Session Signals

- context snippets referenced by the agent
- context snippets ignored by the agent
- files edited without being included in context
- files repeatedly read despite available context
- validations skipped then later needed
- commands repeatedly failing
- hallucinated files / symbols / APIs
- repeated failed patch attempts
- user corrections
- reverted changes
- task drift during the session
- host compaction losing important references

## 9. Attribution + Self-Optimization Plane

The Attribution + Self-Optimization Plane is Horizon's learning mechanism.

It learns whether context packs, retrieval choices, memory facts, validations, and interventions helped, hurt, or were irrelevant.

### Attribution Model

Attribution should be conservative, evidence-weighted, replayable, and reversible.

```text
signal
-> hypothesis
-> confidence
-> counterevidence
-> attribution label
-> policy update
-> replay validation
```

A file being edited does not automatically mean it should have been included. A snippet being unused does not automatically mean it was bad. A validation passing does not automatically mean it was useful.

### Attribution Inputs

- context-pack manifest
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
- context pack
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
- context-pack template updates
- context budget changes
- memory confidence changes
- stale fact invalidation
- negative knowledge writes
- validation policy changes
- host-specific rendering changes
- intervention threshold changes
- cost / savings estimates
- regression cases for evals
- host behavior profile updates

## 10. Evaluation + Replay Plane

The Evaluation + Replay Plane proves Horizon is actually improving agentic development instead of adding ceremony.

Horizon's claims are empirical: fewer tokens, lower latency, fewer repeated scans, fewer hallucinations, cheaper validation, better patches, and less agent confusion.

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

## 11. Explainability + User Surface Plane

The Explainability + User Surface Plane makes Horizon inspectable without making it intrusive.

Normal usage should stay inside the user's chosen coding agent.

### Surfaces

- minimal status indicator
- diagnostics command
- context-pack inspector
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

## Core Object Model

The full system should revolve around a small set of canonical objects.

- `RepoSnapshot`
- `WorkspaceState`
- `TaskIntent`
- `ChangeScope`
- `EvidenceItem`
- `Fact`
- `MemoryFact`
- `NegativeFact`
- `RepoContract`
- `ContextItem`
- `ContextPack`
- `PackManifest`
- `ValidationRoute`
- `ValidationRun`
- `PolicyDecision`
- `SessionTrace`
- `AgentAction`
- `PatchSnapshot`
- `AttributionEvent`
- `CostRecord`
- `AuditBundle`
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

Level 3: Durable Facts
  conventions, contracts, API facts, external docs, negative knowledge, stale/conflict state

Level 4: Task Interpretation
  intent, scope, risk, blast radius, acceptance criteria

Level 5: Evidence Selection
  retrieve, rank, filter, budget, explain inclusion/exclusion

Level 6: Context Compilation
  produce ABI-stable Context Pack with provenance and attribution hooks

Level 7: Runtime Guidance
  inject context, route validation, warn or block only when needed

Level 8: Behavioral Observation
  monitor how host agent uses, ignores, or misuses support

Level 9: Outcome Attribution
  assign help, hurt, neutral, missing, stale, overincluded, or wasted labels

Level 10: Policy Improvement
  update retrieval, memory, validation, budgets, host profiles, and intervention thresholds

Level 11: Replay and Eval
  prove that Horizon is reducing cost, latency, hallucination, and bad changes
```

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

## Product Boundary

Horizon owns:

- durable repo truth
- scoped project memory
- negative knowledge
- dependency/version reality
- context compilation
- Context Pack ABI
- validation routing
- policy decisions
- agent waste detection
- outcome attribution
- cost accounting
- risk/confidence modeling
- audit/replay

Host agents own:

- chat UI
- editor UI
- terminal UI
- patch generation
- code editing
- interactive debugging
- user conversation
- normal developer workflow

## Design Laws

1. Horizon should compile project intelligence, not generate patches.
2. Context Packs are compiled artifacts, not prompts.
3. Every durable fact needs evidence, scope, freshness, confidence, and invalidators.
4. Negative knowledge is as important as positive memory.
5. The evidence priority model is the source-of-truth law.
6. Host capability must be measured, not assumed.
7. Validation should minimize uncertainty, not maximize command execution.
8. Attribution should be conservative, replayable, and reversible.
9. Heavy backends are allowed only behind clean capability interfaces.
10. User-facing behavior should be invisible by default and inspectable on demand.
11. Every subsystem must improve Context Pack quality, attribution quality, validation efficiency, cost, safety, or host-agent reliability.

## Final Shape

Horizon is a local-first closed-loop intelligence substrate for agentic development.

```text
host adapters
-> local runtime and event ledger
-> repo/workspace intelligence
-> fact, memory, and negative-knowledge system
-> change-intent reasoner
-> evidence selector
-> Context Pack compiler and ABI
-> validation and safety router
-> session behavior observer
-> attribution engine
-> self-optimization system
-> eval, replay, audit, and explainability
```

The full vision is not a larger coding agent. It is the durable intelligence layer underneath coding agents: the system that makes them cheaper, less confused, less repetitive, less hallucination-prone, and more aligned with the actual project.
