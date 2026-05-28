# Horizon MVP

## Purpose

Horizon is a local-first background optimization layer for agentic development.

The MVP must prove one thing:

> A coding agent using Horizon-supplied context should waste less time, read fewer irrelevant files, hallucinate less project structure, and run more targeted validation than the same agent without Horizon.

The MVP should not attempt the full Horizon vision. It should implement the smallest reliable loop that can become the foundation for the rest of Horizon.

```text
Task intent
-> evidence collection
-> Context Pack
-> OpenCode usage
-> validation routing
-> session observation
-> attribution record
-> better next pack
```

## MVP Principle

Build the control layer, not the world around it.

Horizon must own:

* Context Pack ABI
* EvidenceItem schema
* pack compiler
* validation router
* local event ledger
* negative knowledge
* provenance tracking
* minimal attribution
* dashboard/inspection surface

Horizon must not own:

* code editing
* patch generation
* chat UI
* terminal UI
* custom parser
* custom code graph
* custom vector database
* custom CI system
* custom agent
* multi-host integration

## Target Host

OpenCode is the only host target for the MVP.

Do not support Claude Code, Codex CLI, Cursor, Aider, Continue, Cline, Roo Code, or multiple host adapters in the MVP.

OpenCode integration should happen through MCP first.

If deeper OpenCode hooks are unavailable, Horizon must degrade to manual Context Pack generation.

## MVP User Experience

The user should continue using OpenCode normally.

Horizon runs in the background.

The user may interact with Horizon through:

```bash
horizon init
horizon daemon
horizon pack build "<task>"
horizon pack inspect <pack-id>
horizon validate
horizon note "<negative fact>"
horizon dashboard
horizon status
```

The dashboard is for inspection and debugging. It must not become an IDE.

## Core MVP Capabilities

### 1. Local Project Initialization

Command:

```bash
horizon init
```

Creates:

```text
.horizon/
  horizon.yaml
  horizon.db
  artifacts/
  packs/
  logs/
```

`horizon.yaml` defines:

```yaml
repo:
  root: .
  name: auto

host:
  target: opencode
  protocol: mcp

context:
  default_budget_tokens: 8000
  max_pack_items: 40
  include_tests: true
  include_docs: true
  include_negative_facts: true

validation:
  commands:
    typecheck: null
    test: null
    lint: null
    format: null
  auto_detect: true
  max_seconds_default: 120

privacy:
  local_only: true
  retain_raw_events: false
  redact_env_files: true

dashboard:
  enabled: true
  port: 4767
```

Acceptance criteria:

* `horizon init` is idempotent.
* Existing config is not overwritten without confirmation.
* Missing validation commands are allowed.
* Horizon can run with no repo intelligence backend, using lexical fallback only.

---

### 2. Local Daemon

Command:

```bash
horizon daemon
```

Responsibilities:

* maintain SQLite database
* watch repo changes
* invalidate stale packs
* expose local MCP server
* expose local dashboard API
* record append-only events
* manage artifacts
* fail open when broken

The daemon must never block OpenCode from functioning.

If Horizon fails, the host agent should continue without Horizon.

Required behavior:

```text
Horizon healthy -> provide packs, validation routes, memory facts
Horizon degraded -> provide static/partial packs
Horizon failed -> no-op/fail-open
```

Acceptance criteria:

* daemon starts in under 2 seconds on a normal project
* corrupted derived state can be rebuilt
* canonical events remain append-only
* dashboard shows daemon health
* Horizon failure does not corrupt repo files

---

### 3. Storage Layer

Use SQLite as canonical local storage.

Required tables:

```sql
events
repos
repo_snapshots
task_intents
evidence_items
context_packs
context_pack_items
validation_routes
validation_runs
negative_facts
session_observations
attribution_records
cost_records
settings
```

Use append-only `events` as the canonical ledger.

Every important action emits an event:

```text
repo.indexed
task.intent.created
evidence.collected
evidence.rejected
pack.built
pack.served
validation.route.created
validation.run.started
validation.run.finished
negative_fact.created
session.observed
attribution.recorded
```

Derived tables may be rebuilt from events where practical.

Acceptance criteria:

* all Context Packs are reproducible from stored records
* all validation decisions have provenance
* all negative facts have scope and invalidation rules
* deleting derived cache does not destroy event history

---

### 4. Repo Intelligence Adapter

The MVP should use existing tools, not build a parser.

Preferred order:

1. `codebase-memory-mcp` if available
2. `rg`
3. `git grep`
4. `fd`
5. static file tree scan

The adapter normalizes all backend results into `EvidenceItem`.

EvidenceItem schema:

```ts
type EvidenceItem = {
  id: string
  repo_id: string
  source: "codebase-memory" | "rg" | "git-grep" | "fd" | "file-tree" | "manual"
  kind:
    | "file"
    | "symbol"
    | "callsite"
    | "test"
    | "doc"
    | "config"
    | "dependency"
    | "negative_fact"
    | "validation_hint"
  path?: string
  symbol?: string
  summary: string
  relevance: number
  confidence: number
  freshness: "fresh" | "possibly_stale" | "stale" | "unknown"
  provenance: {
    command?: string
    backend?: string
    file_hash?: string
    commit?: string
    line_start?: number
    line_end?: number
  }
  risks?: string[]
}
```

Hard rules:

* backend output is never trusted directly
* every item needs provenance
* stale items are allowed only if clearly marked
* generated files should be flagged
* missing evidence is recorded, not hidden

Acceptance criteria:

* Horizon can answer “why was this file included?”
* Horizon can answer “why was this file excluded?”
* exact path/symbol checks use lexical confirmation when possible
* unavailable repo backend does not break pack generation

---

### 5. Task Intent Model

Command:

```bash
horizon pack build "<task>"
```

Creates a `TaskIntent`.

TaskIntent schema:

```ts
type TaskIntent = {
  id: string
  raw_text: string
  task_class:
    | "bugfix"
    | "feature"
    | "refactor"
    | "test"
    | "docs"
    | "config"
    | "unknown"
  likely_areas: string[]
  risk_level: "low" | "medium" | "high"
  created_at: string
}
```

MVP classification can be heuristic.

Examples:

* contains “fix”, “bug”, “broken” -> `bugfix`
* contains “add”, “implement”, “support” -> `feature`
* contains “test”, “coverage” -> `test`
* contains “docs”, “readme” -> `docs`

Acceptance criteria:

* task classification is visible and editable
* unknown classification is allowed
* Horizon does not pretend high confidence when classification is weak

---

### 6. Context Pack Compiler

Context Pack is the core MVP artifact.

Command:

```bash
horizon pack build "<task>"
```

Output:

```text
.horizon/packs/<pack-id>.md
.horizon/packs/<pack-id>.json
```

Context Pack sections:

```markdown
# Horizon Context Pack

## Task
## Repo Snapshot
## Likely Relevant Files
## Relevant Symbols
## Existing Patterns
## Tests and Validation
## Constraints
## Negative Knowledge
## Risks / Stale Areas
## Suggested First Steps
## Provenance
## Pack Manifest
```

ContextPack schema:

```ts
type ContextPack = {
  id: string
  schema_version: "0.1"
  repo_id: string
  task_intent_id: string
  created_at: string
  budget: {
    target_tokens: number
    estimated_tokens: number
  }
  items: ContextItem[]
  validation_route_id?: string
  negative_fact_ids: string[]
  quality: PackQualityReport
  manifest: PackManifest
}
```

ContextItem schema:

```ts
type ContextItem = {
  evidence_id: string
  section: string
  priority: number
  reason_included: string
  summary: string
  path?: string
  line_start?: number
  line_end?: number
}
```

PackQualityReport:

```ts
type PackQualityReport = {
  precision_estimate: number
  recall_risk: "low" | "medium" | "high"
  stale_risk: "low" | "medium" | "high"
  missing_evidence: string[]
  warnings: string[]
}
```

Pack quality gates:

* must include task intent
* must include repo snapshot
* must include provenance
* must include validation hints when available
* must warn on stale index
* must warn when no tests found
* must warn when confidence is low
* must stay within budget unless explicitly overridden

Acceptance criteria:

* generated markdown is directly usable in OpenCode
* generated JSON is machine-readable
* pack can be inspected later
* pack build is deterministic for unchanged inputs
* pack includes why each item was included

---

### 7. OpenCode MCP Server

Command:

```bash
horizon mcp
```

Expose a small MCP surface.

Tools:

```text
horizon_build_pack(task: string)
horizon_get_pack(pack_id?: string)
horizon_explain_context(pack_id: string, evidence_id: string)
horizon_get_validation_route(pack_id: string)
horizon_record_outcome(pack_id: string, outcome: string)
horizon_add_negative_fact(fact: string, scope?: string)
```

Do not expose dozens of tools in the MVP.

The MCP server should return compact outputs. Large pack bodies should be retrieved explicitly, not injected blindly.

Acceptance criteria:

* OpenCode can request a Context Pack
* OpenCode can ask for validation route
* OpenCode can record basic outcome
* MCP failure does not break local CLI use
* every MCP call is logged as an event

---

### 8. Validation Router

Command:

```bash
horizon validate
```

The MVP validation router does not need perfect impacted-test selection.

It needs useful, conservative routing.

Inputs:

* changed files from git
* pack evidence
* repo config
* detected package manager
* configured commands
* file extensions
* known tests
* previous validation results

ValidationRoute schema:

```ts
type ValidationRoute = {
  id: string
  repo_id: string
  pack_id?: string
  changed_files: string[]
  commands: ValidationCommand[]
  reason: string
  risk_level: "low" | "medium" | "high"
  estimated_cost: "low" | "medium" | "high"
}
```

ValidationCommand schema:

```ts
type ValidationCommand = {
  id: string
  label: string
  command: string
  cwd: string
  required: boolean
  timeout_seconds: number
  reason: string
}
```

Routing rules:

* docs-only change -> no tests required; suggest lint/docs check if configured
* config/package change -> typecheck + relevant tests
* source change with nearby tests -> run nearby tests + typecheck
* test change -> run changed test file
* unknown risk -> run configured safe default
* high-risk files -> escalate to broader validation

Validation run output:

```text
command
exit code
duration
stdout/stderr artifact path
failure signature
status
```

Acceptance criteria:

* user can see why each command was selected
* failed commands preserve logs
* validation can be run without Context Pack
* route generation works even with no test graph
* Horizon never claims validation success without actual command results

---

### 9. Negative Knowledge

Command:

```bash
horizon note "<fact>"
```

Examples:

```bash
horizon note "Do not use legacy auth middleware; auth moved to src/auth/session.ts"
horizon note "Generated API client files must not be edited manually"
horizon note "Tests under packages/web/e2e are flaky in CI"
```

NegativeFact schema:

```ts
type NegativeFact = {
  id: string
  repo_id: string
  fact: string
  scope: {
    paths?: string[]
    symbols?: string[]
    task_classes?: string[]
  }
  source: "manual" | "validation_failure" | "attribution"
  confidence: number
  created_at: string
  invalidation: {
    file_hashes?: Record<string, string>
    expires_at?: string
    condition?: string
  }
}
```

MVP behavior:

* manually added facts are included in relevant packs
* facts can be listed, edited, disabled, or deleted
* facts must be scoped where possible
* stale facts are flagged

Commands:

```bash
horizon notes list
horizon notes disable <id>
horizon notes delete <id>
```

Acceptance criteria:

* negative facts appear in Context Packs
* user can inspect why a fact was included
* negative facts never silently override current repo truth
* disabled facts are not included

---

### 10. Session Observation

The MVP should observe what it can without requiring full host instrumentation.

Observation levels:

```text
L0: Horizon generated a pack, but does not know if OpenCode used it.
L1: OpenCode requested a pack through MCP.
L2: OpenCode requested validation route.
L3: OpenCode recorded outcome manually or through MCP.
```

SessionObservation schema:

```ts
type SessionObservation = {
  id: string
  pack_id?: string
  host: "opencode" | "manual"
  observation_level: "L0" | "L1" | "L2" | "L3"
  events: string[]
  outcome?: "success" | "partial" | "failed" | "unknown"
  notes?: string
}
```

Acceptance criteria:

* Horizon does not infer more than it observed
* dashboard clearly shows observation level
* attribution is disabled or low-confidence for L0/L1
* manual outcome recording is supported

---

### 11. Minimal Attribution

Attribution in the MVP should be humble.

It should answer:

* Did Horizon provide a pack?
* Did OpenCode request it?
* Did validation run?
* Did the user mark the outcome?
* Were included files later changed?
* Were there repeated scans?
* Did negative knowledge prevent a known bad path?

AttributionRecord schema:

```ts
type AttributionRecord = {
  id: string
  pack_id: string
  observation_id: string
  outcome_label: "success" | "partial" | "failed" | "unknown"
  horizon_contribution:
    | "likely_helped"
    | "possibly_helped"
    | "unclear"
    | "possibly_hurt"
    | "likely_hurt"
  confidence: number
  evidence: string[]
  confounders: string[]
}
```

MVP rule:

> Never promote policy automatically.

The MVP may record suggestions, but it must not change durable policy without explicit user action.

Acceptance criteria:

* attribution confidence is visible
* low-observation sessions are marked low confidence
* Horizon can show “possibly helped” or “unclear”
* Horizon never fabricates causal certainty

---

## UI Dashboard

Command:

```bash
horizon dashboard
```

Starts a local dashboard at:

```text
http://localhost:4767
```

The dashboard is read-mostly. It is for inspection, debugging, and trust.

Recommended stack:

```text
Frontend: React + Vite + TypeScript
Backend API: Node.js local server inside horizond
Storage: SQLite
Styling: Tailwind or simple CSS
Charts: lightweight SVG/Recharts
```

Do not build authentication for MVP unless remote access is added. Bind to localhost only.

### Dashboard Pages

#### 1. Overview

Purpose: show whether Horizon is helping or broken.

Cards:

* daemon status
* repo status
* latest Context Pack
* latest validation route
* latest validation result
* observation level
* estimated repeated-scan reduction
* warnings

Table:

```text
Recent sessions
- time
- task
- pack id
- observation level
- validation status
- outcome
- confidence
```

Required actions:

* build new pack
* inspect latest pack
* open validation route
* view warnings

---

#### 2. Context Packs

Purpose: inspect generated packs.

Views:

* pack list
* pack detail
* pack markdown preview
* evidence item list
* provenance view
* quality report
* missing evidence warnings

Required filters:

* task class
* created date
* stale risk
* pack quality
* validation status

Pack detail must answer:

* why this file?
* why this symbol?
* what was excluded?
* what is stale?
* what is missing?
* what validation was suggested?

---

#### 3. Evidence

Purpose: inspect normalized repo evidence.

Views:

* evidence table
* source breakdown
* stale/conflict indicators
* file/symbol/path search
* backend source

Columns:

```text
kind
path
symbol
source
relevance
confidence
freshness
reason
```

Required actions:

* copy path
* open evidence provenance
* mark evidence stale
* create negative fact from evidence

---

#### 4. Validation

Purpose: show validation route decisions and outcomes.

Views:

* latest route
* command list
* command reasons
* run history
* failure signatures
* duration/cost

Command status:

```text
not_run
running
passed
failed
timed_out
skipped
```

Required actions:

* run selected route
* rerun failed command
* copy command
* open logs
* mark flaky

---

#### 5. Negative Knowledge

Purpose: manage durable “do not repeat this mistake” facts.

Views:

* active facts
* disabled facts
* stale facts
* facts by scope/path

Fields shown:

```text
fact
scope
source
confidence
created_at
invalidation
included_in_packs
```

Required actions:

* add fact
* disable fact
* delete fact
* edit scope
* mark stale

---

#### 6. Sessions / Attribution

Purpose: inspect whether Horizon likely helped.

Views:

* session list
* pack used
* validation run
* outcome
* contribution label
* confidence
* confounders

Required actions:

* record manual outcome
* mark pack useful/not useful
* add note
* export session audit bundle

Do not overstate attribution. Use conservative labels.

---

#### 7. Settings

Purpose: edit safe local config.

Editable:

* context budget
* validation commands
* repo backend preference
* dashboard port
* privacy settings
* ignored paths
* generated-file patterns

Non-editable but visible:

* DB path
* repo root
* host target
* schema version
* daemon version

---

## Dashboard API

Local-only HTTP API.

Endpoints:

```text
GET  /api/health
GET  /api/repo
GET  /api/packs
GET  /api/packs/:id
POST /api/packs
GET  /api/evidence
GET  /api/validation/routes
GET  /api/validation/routes/:id
POST /api/validation/run
GET  /api/validation/runs
GET  /api/negative-facts
POST /api/negative-facts
PATCH /api/negative-facts/:id
DELETE /api/negative-facts/:id
GET  /api/sessions
PATCH /api/sessions/:id/outcome
GET  /api/events
GET  /api/settings
PATCH /api/settings
```

MVP API rules:

* bind to localhost
* validate all inputs with Zod or JSON Schema
* never expose raw secrets
* redact `.env*` by default
* no remote telemetry by default

---

## CLI Commands

Required MVP commands:

```bash
horizon init
horizon daemon
horizon status
horizon pack build "<task>"
horizon pack inspect <pack-id>
horizon pack list
horizon validate
horizon validate run <route-id>
horizon note "<fact>"
horizon notes list
horizon dashboard
horizon doctor
```

Nice-to-have, not required:

```bash
horizon pack diff <a> <b>
horizon why-this-context <pack-id> <evidence-id>
horizon why-this-validation <route-id>
horizon export-audit <session-id>
```

---

## Reliability Requirements

### Fail Open

Horizon must not prevent normal development.

If Horizon fails:

* OpenCode still works
* repo files are not modified
* validation commands are not run unless explicitly requested
* failed events are recorded if possible

### Local First

Default behavior:

* no cloud dependency
* no remote telemetry
* no raw transcript retention
* no sending repo contents anywhere except the local host agent flow

### Deterministic Artifacts

For unchanged inputs:

* same task
* same repo state
* same config
* same evidence backend result

Horizon should generate the same Context Pack.

### Provenance Required

Every included item must explain where it came from.

No orphan facts.

No unsourced claims in packs.

### Conservative Learning

The MVP records attribution. It does not self-promote durable policy.

User-approved policy changes may come later.

---

## MVP Milestones

### Milestone 1 — Local Core

Deliver:

* `horizon init`
* SQLite database
* event ledger
* config loading
* repo snapshot
* file tree scan
* `rg` fallback search
* pack artifact writer

Definition of done:

* can build a basic Context Pack with no external backend
* pack has provenance
* events are stored
* repeated run is deterministic

---

### Milestone 2 — Context Pack MVP

Deliver:

* TaskIntent heuristic classifier
* EvidenceItem normalization
* Context Pack JSON + Markdown
* pack quality report
* pack inspect command

Definition of done:

* user can run `horizon pack build "add x"`
* output is useful enough to paste into OpenCode
* dashboard can display the pack
* stale/missing evidence warnings work

---

### Milestone 3 — OpenCode MCP MVP

Deliver:

* MCP server
* `horizon_build_pack`
* `horizon_get_pack`
* `horizon_get_validation_route`
* `horizon_record_outcome`
* MCP event logging

Definition of done:

* OpenCode can request and receive a pack
* MCP failures are logged
* CLI still works without MCP

---

### Milestone 4 — Validation Router

Deliver:

* validation command detection
* changed-file detection
* route generation
* command runner
* run logs
* validation dashboard page

Definition of done:

* Horizon suggests useful validation
* user can run the route
* results are stored
* failed command logs are viewable

---

### Milestone 5 — Negative Knowledge

Deliver:

* manual negative facts
* scoped facts
* fact inclusion in packs
* notes dashboard page

Definition of done:

* user can record a known bad path
* future packs include relevant warnings
* stale/disabled facts are excluded

---

### Milestone 6 — Dashboard

Deliver:

* overview page
* packs page
* evidence page
* validation page
* negative knowledge page
* sessions page
* settings page

Definition of done:

* dashboard explains what Horizon did
* every pack is inspectable
* every validation route is explainable
* every negative fact is manageable
* dashboard remains local-only

---

### Milestone 7 — Minimal Attribution

Deliver:

* session observation records
* manual outcome recording
* simple attribution records
* cost/repeated-scan counters
* session dashboard

Definition of done:

* Horizon can say “likely helped,” “unclear,” or “possibly hurt”
* confidence is shown
* low-observation attribution is labeled low confidence
* no automatic policy promotion exists

---

## MVP Success Metrics

Track these locally:

```text
packs_built
packs_requested_by_host
avg_pack_size
pack_build_latency_ms
validation_routes_created
validation_runs_passed
validation_runs_failed
negative_facts_used
manual_outcomes_recorded
sessions_likely_helped
sessions_unclear
sessions_possibly_hurt
```

Core product metrics:

* fewer repeated file scans
* fewer irrelevant files included
* fewer stale facts
* faster task start
* more targeted validation
* fewer repeated failed approaches

Do not treat token reduction as success unless task quality is preserved.

---

## Explicit Non-Goals

The MVP will not:

* replace OpenCode
* implement a new editor
* implement autonomous coding
* generate patches
* own CI
* build a custom code graph
* build a custom vector DB
* support multiple agents
* auto-promote learned policy
* retain raw transcripts by default
* expose remote dashboard access
* optimize across repos

---

## Recommended Repository Structure

```text
horizon/
  packages/
    cli/
    daemon/
    mcp-server/
    dashboard/
    core/
    adapters/
      repo-intel/
      validation/
      host-opencode/
    storage/
    schemas/
  examples/
  fixtures/
  docs/
    mvp.md
```

Core package boundaries:

```text
core:
  schemas
  pack compiler
  task intent
  evidence selection
  validation routing
  attribution logic

storage:
  SQLite migrations
  repositories
  event ledger

daemon:
  local runtime
  scheduler
  API server
  dashboard server

mcp-server:
  OpenCode MCP tools

dashboard:
  React/Vite UI

adapters:
  codebase-memory
  rg/git/fd
  validation command detection
```

---

## MVP Definition of Done

The MVP is done when this flow works reliably:

```bash
horizon init
horizon daemon
horizon pack build "implement feature X"
horizon dashboard
```

Then inside OpenCode:

```text
OpenCode requests Horizon Context Pack
OpenCode uses pack during task
Horizon suggests validation route
User runs validation
User records outcome
Horizon stores attribution
Next pack includes relevant negative knowledge and fresher evidence
```

Final acceptance test:

1. Pick a real medium-sized repo.
2. Run a task without Horizon.
3. Run the same or comparable task with Horizon.
4. Compare:

   * files opened
   * irrelevant files read
   * validation commands run
   * repeated mistakes
   * time to useful context
   * task outcome
5. Horizon passes MVP only if it measurably improves at least two of those without making task quality worse.

## Final Shape

The MVP should become:

```text
OpenCode
-> Horizon MCP
-> task intent
-> repo evidence
-> Context Pack
-> validation route
-> session observation
-> attribution record
-> better next pack
```

This is enough to build the rest of Horizon on top of.

Do not expand scope until this loop is reliable.
