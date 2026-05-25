# Horizon

Horizon is a local-first background optimization layer for agentic development.

It is not another agentic IDE, editor, chat interface, terminal UI, or replacement for tools like opencode, Codex, Claude Code, or Aider. The user should keep using their existing coding agent normally. Horizon runs underneath those tools to make agentic coding scale better on complex projects.

Current status: paper / theory repo. No implementation yet.

## Product thesis

As codebases grow, coding agents waste more tokens, reread the same files, lose context, hallucinate APIs or project structure, and run expensive validation too broadly. Horizon exists to reduce that waste by maintaining durable local repo intelligence and feeding existing agents the smallest sufficient context for the current task.

Horizon's role is to reduce:

- token waste
- repeated repo scanning
- stale context
- hallucinated files, symbols, and APIs
- unnecessary validation cost
- agent confusion on large projects
- latency caused by rebuilding project understanding from scratch

## Core loop

Horizon may grow into many local-first agentic development subsystems, but its central loop is:

Observe repo + task + session -> compile Context Pack -> deliver through Context Pack ABI -> observe agent behavior -> attribute outcome -> improve the next pack.

Context Pack ABI is the handoff contract. Attribution loop is the learning mechanism. Everything else in Horizon exists to make that loop more accurate, cheaper, fresher, safer, or more useful across host agents.

## Product spine

Horizon integrates underneath existing agentic coding tools without replacing their UI or workflow.

Core surfaces:

- opencode plugin
- Horizon MCP server
- `horizond` local daemon
- host adapter layer
- background session observer
- context / compaction broker
- permission broker
- validation broker
- local config / policy loader

## Host adapter layer

Horizon should support multiple agentic coding tools through thin adapters.

Initial host targets:

- opencode
- Codex
- Claude Code
- Aider
- other CLI / MCP-compatible coding agents

Adapter components:

- host capability detector
- host capability matrix
- session observer adapter
- context injection adapter
- context-pack rendering adapter
- context-pack usage adapter
- attribution signal adapter
- permission adapter
- validation adapter
- task result adapter
- memory proposal adapter
- degradation strategy per host
- adapter contract tests
- host capability degradation matrix

## Delegated execution model

Horizon does not try to become the coding agent. Existing agents still own the interactive development workflow.

Horizon owns:

- repo understanding
- task scoping
- context packing
- memory retrieval
- provenance capture
- validation routing
- cost / latency optimization
- stale / conflict detection
- post-task learning
- session intelligence
- repo contracts
- risk / confidence scoring

Host agents own:

- user conversation
- code editing
- patch generation
- terminal interaction
- interactive debugging
- normal developer workflow

## Horizon core

The core is the durable intelligence layer for the project.

Components:

- ontology
- event ledger
- capability registry
- task router
- context-pack compiler
- context budgets
- context-pack ABI
- context-pack ABI registry
- context-pack manifest
- context-pack diff engine
- context-pack cache fingerprinting
- context-pack attribution hooks
- context utility ledger
- memory write policy
- fact lifecycle
- provenance model
- evidence priority model
- stale / conflict policy
- validation gate composer
- workflow state
- system state machines
- audit trail
- adapter contracts
- adapter contract tests
- confidence / risk model
- attribution loop
- outcome attribution model
- cost accounting engine
- repo contract engine
- policy DSL engine
- local data model / migrations
- trust boundary contract

## System contracts and lifecycles

Horizon should be explicit about the contracts that connect its subsystems. The capability list describes what Horizon can do; this layer describes how those capabilities stay stable, testable, and safe as background intelligence changes over time.

### State machines

Background behavior should move through named states instead of relying on implicit flags or ad hoc heuristics.

State machines:

- session state machine
- task state machine
- context-pack state machine
- attribution state machine
- memory-fact state machine
- validation-run state machine
- index-freshness state machine
- intervention state machine

### Context Pack ABI

The Context Pack ABI is Horizon's stable handoff contract between repo intelligence and host coding agents. It should make context packs portable across hosts, comparable across runs, cacheable across similar tasks, inspectable by the user, and attributable after the task finishes.

It is not a prompt template. It is the durable interface that lets Horizon change its internal retrieval, memory, indexing, and validation machinery without breaking host adapters.

Contract fields:

- ABI version
- pack schema
- pack manifest
- task identity
- host identity
- repo identity
- branch / commit / worktree fingerprint
- dependency / lockfile fingerprint
- snippet schema
- context item kind
- provenance schema
- source path / symbol / range anchors
- content hash
- freshness metadata
- token budget metadata
- inclusion rationale
- exclusion rationale
- retrieval query / ranking signal
- confidence score
- risk score
- host rendering contract
- compaction-safe representation
- expected-use hints
- citation / reference handles
- attribution hook IDs
- redaction / exposure flags
- cache fingerprint
- pack diff format
- compatibility / migration policy

Context item kinds:

- source snippet
- symbol summary
- file summary
- architectural contract
- repo convention
- test hint
- validation hint
- dependency / API fact
- external documentation fact
- approved memory
- stale memory warning
- negative knowledge
- generated-file warning
- risk note
- task acceptance criterion

ABI goals:

- host agents can consume context without knowing Horizon internals
- Horizon can change retrieval internals without breaking host adapters
- every item in the pack can be traced back to evidence
- every item has a cost and a reason for inclusion
- every omission can be explained when useful
- every pack can be compared against later packs
- every pack can participate in outcome attribution

Possible diagnostics:

- `horizon pack build`
- `horizon pack inspect`
- `horizon pack diff`
- `horizon pack replay`
- `horizon pack explain`
- `horizon pack blame`

### Evidence priority model

When repo facts conflict, Horizon should use a deterministic source-of-truth order instead of trusting whichever source was retrieved first.

Default priority:

- live worktree
- staged changes
- current branch
- lockfile / installed dependency state
- repo config
- tests
- source code
- local repo docs
- external versioned docs
- approved memory
- model prior knowledge

### Fact lifecycle

Memory should not become permanent just because an agent said something once. Facts should be promoted, challenged, and retired through an explicit lifecycle.

Lifecycle states:

- observed
- proposed
- verified
- approved
- contradicted
- stale
- rejected
- retired

### Intervention ladder

Horizon should stay invisible by default and only interrupt when risk, staleness, or cost justifies it.

Intervention levels:

- silent context injection
- silent validation routing
- passive status note
- soft warning
- stale-context warning
- permission challenge
- destructive-action block
- fail-open passthrough

### Adapter contract tests

Each host adapter should prove what it can and cannot support. Horizon should degrade from measured adapter capability, not from optimistic assumptions.

Tests:

- can observe session
- can inject context
- can read tool calls
- can observe file touches
- can broker permissions
- can attach validation results
- can detect patch intent
- can report task outcome
- can report context usage signals
- can preserve context-pack references through compaction
- fallback behavior verified

### Local data model and migrations

Horizon is local-first, so its local state must be durable, recoverable, and evolvable.

Components:

- SQLite schema
- migration system
- index versioning
- cache schema versioning
- context-pack ABI versioning
- attribution schema versioning
- memory schema versioning
- provenance schema versioning
- backward compatibility policy
- corruption recovery policy

### Trust boundary contract

Horizon will touch repo contents, session traces, validation output, and possibly secrets. Data exposure should be explicit per host and per adapter.

Contract fields:

- what never leaves the machine
- what can be sent to host agents
- what can be sent to docs providers
- what can be sent to eval providers
- secret scanning before context injection
- transcript retention policy
- artifact retention policy
- per-host exposure policy

### Attribution loop

The attribution loop is how Horizon learns whether its context packs, retrieval choices, memory facts, validations, and interventions actually improved the agent session.

Self-optimization should distinguish between Horizon helping, hurting, or being irrelevant. Otherwise telemetry becomes noise.

Inputs:

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

Signals:

- context used
- context ignored
- context missing
- context stale
- context over-included
- file edited without prior inclusion
- file read repeatedly despite available context
- memory used correctly
- memory stale or misleading
- validation caught a real issue
- validation was expensive and irrelevant
- intervention prevented a bad action
- intervention interrupted unnecessarily
- hallucinated file / symbol / API detected
- user corrected supplied context or missing context
- patch reverted after relying on bad context

Attribution labels:

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

Attribution scopes:

- individual snippet
- symbol summary
- file summary
- context pack
- retrieval query
- retrieval strategy
- memory fact
- validation gate
- host adapter
- host model / agent profile
- task type
- repo area
- branch / commit state

Outputs:

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

Attribution should avoid naive conclusions. A file being edited does not automatically mean it should have been included. A snippet being unused does not automatically mean it was bad. A validation passing does not automatically mean it was useful. Horizon needs enough event evidence to separate correlation from useful signal.

Possible diagnostics:

- `horizon blame-context`
- `horizon explain-outcome`
- `horizon show-unused-context`
- `horizon show-missed-context`
- `horizon show-wasted-validation`
- `horizon show-context-savings`
- `horizon replay-attribution`

## Background runtime

Horizon should do useful repo preparation continuously without requiring user interaction.

Components:

- repo watcher
- background indexer
- context-pack cache
- memory reconciler
- stale / conflict detector
- validation cache
- daemon health check
- fail-open passthrough mode
- background task scheduler
- resource governor

## Session intelligence layer

Horizon needs to understand active agent sessions without becoming the agent UI.

Components:

- session transcript parser
- command / tool-call observer
- file-touch observer
- patch-intent extractor
- repeated-scan detector
- context-miss detector
- context-usage detector
- context-reference resolver
- agent confusion detector
- hallucinated-symbol detector
- intervention policy

## Workspace state model

Horizon must track the live repo and worktree so it does not reason from stale indexed truth.

Components:

- git state tracker
- branch / commit tracker
- dirty worktree tracker
- staged-change tracker
- untracked-file tracker
- generated-file detector
- dependency / lockfile drift detector
- repo snapshot fingerprint

## Change intent model

Horizon should convert vague user tasks into bounded implementation intent before retrieval and validation.

Components:

- task intent extractor
- affected capability resolver
- non-goals detector
- expected output classifier
- risk class classifier
- blast-radius estimator
- acceptance criteria generator

## Local storage and indexing

Horizon should provide fast local retrieval, indexing, and source lookup.

Uses:

- SQLite
- SQLite FTS5
- sqlite-vec
- Watchman
- ripgrep
- local cache store
- local artifact store

## Memory and storage reuse candidates

Horizon should reuse mature local primitives where possible, but keep its repo-specific fact semantics, provenance rules, negative knowledge, validation memory, and context-pack attribution custom.

Very safe foundation:

- SQLite for canonical local state, event ledger, facts, sessions, validation runs, context-pack metadata, cost ledger, and migrations
- SQLite FTS5 for local full-text search over memory claims, docs chunks, validation failures, command output, session summaries, and context-pack items
- ripgrep for fast source and text lookup outside indexed paths
- Tree-sitter for syntax-aware parsing and source structure extraction

Safe with caveats:

- sqlite-vec for local vector search, semantic memory lookup, similar task capsules, similar validation failures, and docs / code chunk retrieval
- Kuzu as an optional embedded graph projection for provenance traversal, conflict graphs, dependency graphs, file-to-test relationships, and symbol / fact relationships
- SQLite edge tables as the first graph representation before adding a dedicated graph backend

Projects / patterns to investigate or borrow from:

- Graphiti for temporal fact graphs, provenance episodes, validity windows, contradiction / staleness ideas, and hybrid semantic / keyword / graph retrieval patterns
- Mem0 for memory API patterns, user / session / agent memory separation, extraction flows, and CLI-style memory operations
- LangMem for hot-path versus background memory management, memory extraction / consolidation / update patterns, and storage-agnostic memory tooling
- vstash-style hybrid retrieval for a single-file local memory system combining SQLite, FTS, vector search, and reciprocal-rank fusion
- Codebase-Memory / codebase-memory-mcp for persistent Tree-sitter code graphs, MCP code exploration, call-graph traversal, impact analysis, and token-reduction benchmark ideas

Do not outsource to a generic memory system:

- Horizon fact lifecycle
- memory proposal queue
- approved / rejected / stale fact policy
- negative knowledge taxonomy
- evidence priority resolver
- branch / commit / worktree-scoped truth
- context-pack attribution
- validation usefulness memory
- agent-waste ledger
- stale fact invalidation
- why-this-memory diagnostics

Default posture:

- reuse infrastructure aggressively
- treat vector and graph stores as rebuildable indexes / projections
- keep SQLite as the canonical local source of truth
- keep Horizon's meaning layer custom
- make optional backends prove they reduce token cost, latency, hallucination, or validation waste

## Repo understanding

Horizon maintains a compact, queryable model of the codebase.

Primary tools / concepts:

- codebase-memory-mcp
- Serena
- Tree-sitter
- ast-grep
- aider repo-map concepts

Advanced options:

- LSP bridge
- SCIP
- Sourcebot
- Zoekt
- livegrep

## Multi-repo and workspace graph

Horizon should support monorepos, polyrepos, linked packages, and local dependency workspaces.

Components:

- workspace package graph
- cross-package dependency graph
- local dependency resolver
- service boundary map
- API provider / consumer map
- multi-root repo support
- shared config resolver

## Repo contracts

Horizon should encode durable project rules that agents must respect.

Components:

- architectural boundaries
- import rules
- ownership rules
- naming conventions
- framework conventions
- test conventions
- migration policies
- generated-code policies
- forbidden-pattern rules

## Context system

The context system gives agents the smallest sufficient context for the current task through the Context Pack ABI.

Components:

- task classifier
- repo-scope resolver
- symbol / file retriever
- dependency-neighborhood retriever
- context-pack compiler
- context-pack ABI emitter
- context budgeter
- compaction strategy
- provenance-tagged snippets
- attribution-ready context IDs
- context freshness scoring
- context-pack diffing
- context-pack cache fingerprinting
- host-specific context formatting
- generated-file filtering
- context exclusion reasoning

## Memory system

The memory system keeps reusable project knowledge without letting hallucinations become permanent.

Uses:

- Graphiti
- approved-facts store
- rejected-facts store
- stale-facts store
- conflict graph
- provenance graph
- memory proposal queue
- memory approval workflow
- memory confidence scoring
- memory-to-context attribution

## Negative knowledge system

Horizon should remember what is false, obsolete, misleading, or repeatedly harmful.

Components:

- nonexistent-symbol memory
- deprecated-pattern memory
- rejected-approach memory
- failed-fix memory
- bad-context memory
- flaky-validation memory
- misleading-docs memory
- user-rejected-suggestion memory

## External context

Horizon should inject accurate framework, library, and API facts instead of relying on model memory.

Uses:

- Context7
- docs MCP adapters
- local docs index
- version-aware API facts
- changelog / migration facts

## Dependency and API drift system

Horizon should keep agents aligned with the exact dependency versions used by the repo.

Components:

- lockfile parser
- installed-version index
- framework-version detector
- API availability checker
- migration guide retriever
- changelog index
- deprecated API detector
- package upgrade impact analyzer

## Validation system

Horizon should run the right checks for the change instead of everything every time.

Components:

- impacted-test selection
- typecheck / lint routing
- static-analysis routing
- security scan routing
- architecture-rule checks
- validation cache
- failure summarizer
- validation escalation policy
- validation de-escalation policy
- attribution-informed validation routing

## Validation intelligence

Validation selection should be adaptive, explainable, and failure-aware.

Components:

- flaky test registry
- failure signature database
- test ownership map
- test-to-source affinity map
- historical failure correlation
- minimal reproduction extractor
- validation usefulness scoring
- wasted-validation detector

## Build and test impact

Horizon should avoid expensive full-suite validation when the change has a smaller blast radius.

Uses:

- Nx
- Turborepo
- Bazel query
- language-native test selectors
- impacted-test selection
- validation cache

## Policy layers

Horizon should prevent expensive, risky, or invalid agent behavior without blocking normal development.

Agent permissions:

- opencode permission broker
- command allow / deny rules
- file write boundaries
- destructive-command guard

Repo architecture policy:

- import boundaries
- ownership rules
- architectural constraints
- custom repo policies

Security scanning:

- Semgrep
- CodeQL
- Gitleaks
- detect-secrets
- OSV-Scanner

Config validation:

- OPA
- CUE
- JSON Schema

## Patch safety and recovery

Agent edits should be reversible and auditable without Horizon owning code generation.

Components:

- pre-task checkpoint
- patch snapshot
- rollback plan
- partial rollback support
- risky-edit detector
- unexpected-file-change detector
- recovery bundle

## Artifact and generated-code awareness

Horizon should avoid wasting context and validation on derived/generated files.

Components:

- generated-file classifier
- build artifact detector
- vendored dependency detector
- minified file detector
- lockfile semantics detector
- codegen source-to-output mapper
- generated-file edit warning

## Autonomous background tasks

Horizon can handle bounded background work that improves agent sessions without becoming the coding agent.

Tasks:

- background indexing
- dependency graph refresh
- symbol graph refresh
- docs index refresh
- stale memory detection
- conflict detection
- context-pack prewarming
- validation cache warming
- impacted-test map updates
- post-task memory proposal generation
- attribution replay
- audit snapshot generation
- cost / savings snapshot generation

## Self-optimizing system

Horizon should improve retrieval, context, memory, and validation decisions from real task outcomes.

Learns from:

- which context packs led to successful edits
- which retrieved files were actually used
- which files were over-included
- which validations caught real issues
- which validations were unnecessary
- which memories became stale
- which facts caused conflicts
- which task types exceeded token budgets
- which hosts needed different context shapes
- which agent sessions showed confusion or repeated scanning

Optimizes:

- context-pack selection
- retrieval ranking
- context budget sizing
- validation gate selection
- memory promotion / rejection
- stale fact invalidation
- host-specific context formatting
- repo-specific task templates
- intervention thresholds

## Agent feedback loop

Horizon should learn from whether the host agent actually used Horizon's support correctly.

Learns from:

- context snippets referenced by the agent
- context snippets ignored by the agent
- files edited without being included in context
- validations skipped then later needed
- hallucinated files / symbols / APIs
- repeated failed patch attempts
- user corrections
- reverted changes

Outputs:

- retrieval weight updates
- context-pack template updates
- memory confidence changes
- validation policy changes
- host-specific behavior profiles

## Confidence and risk model

Horizon should decide when to intervene, stay silent, ask for review, or fail open.

Components:

- context confidence score
- memory confidence score
- validation confidence score
- stale-index risk score
- blast-radius risk score
- host-agent reliability score
- intervention threshold policy

## Cost accounting engine

Token, latency, validation, and indexing costs should be first-class optimization targets.

Components:

- per-session token ledger
- context-pack cost estimator
- retrieval cost estimator
- validation cost estimator
- background task cost budget
- amortized indexing cost tracker
- cost regression detector
- savings report

## Audit and replay

Horizon should make agent decisions inspectable when debugging, reviewing, or improving the system.

Uses:

- event ledger
- context-pack snapshots
- task replay
- attribution replay
- provenance diff
- audit bundles
- Repomix
- patch snapshots
- session traces

## Observability and evals

Horizon needs to measure whether it is actually reducing cost, latency, hallucinations, and bad changes.

Uses:

- OpenTelemetry
- Langfuse
- Braintrust
- Ragas
- promptfoo

## Evaluation harness

Horizon should continuously prove that it improves agentic development instead of adding ceremony.

Components:

- golden task suite
- replayable agent sessions
- before / after token comparison
- before / after latency comparison
- context precision / recall scoring
- context usefulness scoring
- attribution accuracy scoring
- edit success scoring
- validation usefulness scoring
- hallucination incident tracking
- regression benchmarks

## Grounded research additions

These are the repo-research additions worth keeping because they strengthen the Context Pack ABI and attribution loop instead of expanding Horizon sideways.

### Benchmark and attribution harness

Horizon should use benchmark tasks as replayable eval fixtures, not as leaderboard truth.

Components:

- SWE-bench-style task adapter
- mini-agent baseline runner
- Horizon-on / Horizon-off ablation runs
- benchmark leakage / weak-test warning labels
- patch outcome verifier
- token / latency / repeated-scan comparison
- context precision / recall comparison
- attribution-label scoring
- regression case capture from failed or misleading runs

Goal:

- prove whether Horizon reduces waste and hallucination in controlled runs
- avoid optimizing for benchmark scores that do not reflect real developer value

### Repo-native AI checks

Horizon should support source-controlled checks that express repo-specific guardrails without becoming a CI platform.

Possible layout:

- `.horizon/checks/`
- `.horizon/policies/`

Check types:

- architecture boundary check
- risky-edit check
- generated-file check
- migration safety check
- dependency/API drift check
- validation-routing hint
- context-pack requirement check
- forbidden-pattern check

Checks should be compiled into Horizon policy and validation decisions, not treated as generic prompts.

### Command discovery for validation routing

Horizon should discover how a repo already builds, tests, lints, formats, and typechecks before inventing validation routes.

Sources:

- `package.json` scripts
- `justfile`
- `mise.toml`
- `Makefile`
- `Taskfile.yml`
- `tox.ini`
- `noxfile.py`
- language-native test configs
- CI workflow files

Tracked command metadata:

- command purpose
- expected cost
- historical runtime
- failure signature
- flakiness score
- source/test affinity
- destructive-risk classification
- whether the command is safe for silent background execution

### Docs and artifact ingestion

Horizon should ingest external and local non-code artifacts only when they can produce versioned, provenance-tagged facts for the Context Pack ABI.

Inputs:

- framework docs
- changelogs
- migration guides
- API docs
- local design docs
- PDFs
- screenshots / diagrams
- architecture notes
- generated reference material

Outputs:

- version-aware API facts
- migration facts
- artifact-derived facts
- stale-doc warnings
- provenance anchors back to source artifact, URL, version, page, or section
- context-pack items with freshness and confidence metadata

## Config and policy DSL

Users and repos should be able to configure Horizon without changing Horizon internals.

Components:

- `horizon.yaml`
- repo policy schema
- adapter capability schema
- context-pack ABI schema
- attribution policies
- validation rule schema
- memory promotion rules
- context budget profiles
- background task schedules
- per-host overrides

## Local privacy and resource governor

The background optimizer must not become expensive, invasive, or annoying.

Components:

- CPU / memory budgeter
- battery-aware scheduling
- filesystem ignore policy
- secret redaction boundary
- local-only mode
- network egress policy
- index size limits
- cache eviction policy

## Heavy backends

These should be optional and used only when they directly reduce cost, improve reliability, or support larger repos.

Isolation:

- Firecracker

Reproducible execution:

- Dagger
- Nix
- devcontainers

Workflow durability:

- DBOS
- Temporal

Orchestration graphs:

- LangGraph

Large-scale code search:

- Sourcebot
- Zoekt
- livegrep

## User surfaces

Horizon should appear only when useful. Normal usage should stay inside the user's chosen coding agent.

Surfaces:

- minimal status indicator
- diagnostics command
- context-pack inspector
- attribution inspector
- memory review
- stale / conflict review
- audit explorer
- cost / savings report

## Explainability layer

Horizon's invisible work should be inspectable only when needed.

Commands:

- `why-this-context`
- `why-not-this-context`
- `why-this-validation`
- `why-this-memory`
- `why-this-warning`
- `why-this-file`
- `show-context-attribution`
- `what-changed-since-last-run`
- `show-agent-waste`
- `show-savings`

## Product identity

Horizon is:

- a background optimization layer for agentic development
- a host-agnostic repo intelligence system
- a Context Pack ABI owner
- an attribution loop for agent context
- a context compiler
- a verified memory layer
- a negative-knowledge layer
- a provenance layer
- a validation router
- a repo contract engine
- a cost and latency reducer
- a self-optimizing support system
- an autonomous background maintenance system
- local-first agentic workflow infrastructure

Horizon is not:

- another agentic IDE
- another code editor
- another chat interface
- another terminal UI
- a replacement for opencode
- a replacement for Codex
- a replacement for Claude Code
- a replacement for Aider
- a full autonomous software engineer
