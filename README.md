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
- permission adapter
- validation adapter
- task result adapter
- memory proposal adapter
- degradation strategy per host

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
- memory write policy
- provenance model
- stale / conflict policy
- validation gate composer
- workflow state
- audit trail
- adapter contracts
- confidence / risk model
- cost accounting engine
- repo contract engine
- policy DSL engine

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

The context system gives agents the smallest sufficient context for the current task.

Components:

- task classifier
- repo-scope resolver
- symbol / file retriever
- dependency-neighborhood retriever
- context-pack compiler
- context budgeter
- compaction strategy
- provenance-tagged snippets
- context freshness scoring
- context-pack diffing
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
- edit success scoring
- validation usefulness scoring
- hallucination incident tracking
- regression benchmarks

## Config and policy DSL

Users and repos should be able to configure Horizon without changing Horizon internals.

Components:

- `horizon.yaml`
- repo policy schema
- adapter capability schema
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
- memory review
- stale / conflict review
- audit explorer
- cost / savings report

## Explainability layer

Horizon's invisible work should be inspectable only when needed.

Commands:

- `why-this-context`
- `why-this-validation`
- `why-this-memory`
- `why-this-warning`
- `why-this-file`
- `what-changed-since-last-run`
- `show-agent-waste`
- `show-savings`

## Product identity

Horizon is:

- a background optimization layer for agentic development
- a host-agnostic repo intelligence system
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
