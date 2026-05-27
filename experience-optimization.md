# Horizon Experience Optimization — Improved Full-Scope Plan

## 1. Purpose

Experience Optimization is Horizon’s evidence-driven feedback loop for improving Horizon-owned support behavior.

It exists to make agentic development cheaper, faster, less context-wasteful, and less hallucination-prone on complex projects without replacing opencode, Codex, Claude Code, or any other host agent.

Experience Optimization improves the harness around agentic work. It does not become the agent.

## 2. Hard Invariants

These are non-negotiable.

- Horizon may optimize Horizon behavior only.
- Horizon may not patch product code.
- Horizon may not replace the host agent, editor, terminal, or chat interface.
- Horizon may not promote behavior changes without observed evidence.
- Horizon may not create a second source of truth outside the canonical event ledger.
- Every promoted change must be scoped, reversible, auditable, and invalidatable.
- Raw traces are not retained by default.
- Benchmark-only wins cannot be globally promoted.
- Token reduction is not a primary success metric unless task quality is preserved.

## 3. What Experience Optimization Owns

Experience Optimization owns the control loop that turns observed development experience into governed Horizon policy improvements.

It owns:

- experience manifests
- observation capability records
- attribution results
- harness candidates
- candidate scorecards
- promotion decisions
- policy registry entries
- rejected hypotheses

It does not own:

- product-code generation
- patch authoring
- host-agent planning
- editor UI
- terminal UI
- chat UI
- raw transcript warehousing
- autonomous repository mutation

## 4. What It Optimizes

Experience Optimization may improve only Horizon-owned support systems:

- retrieval
- codebase evidence selection
- Context Pack assembly
- context budget allocation
- validation routing
- stale/conflict policy
- memory write policy
- negative knowledge policy
- exposure/redaction policy
- host rendering
- intervention thresholds
- cost/leverage policy
- replay and regression policy

## 5. Core Loop

```text
session / repo / validation events
-> ExperienceRecord manifest
-> observation capability classification
-> attribution result + confounders
-> HarnessCandidate with typed delta
-> scorecard
-> replay / ablation / regression / exposure / leverage gates
-> shadow or canary when justified
-> PromotionDecision
-> scoped PolicyRegistryEntry
-> monitored behavior change
-> rollback, expiration, or generalization
```

The durable output is not “new agent behavior” in the abstract. The durable output is a scoped policy entry consumed by existing Horizon subsystems.

## 6. Canonical Objects

Keep the model small. Add new objects only when they have independent lifecycle, storage, and governance needs.

### 6.1 ExperienceRecord

A manifest over canonical event-ledger data.

Stores:

- session id
- repo/workspace id
- host id
- task class
- observation level
- event pointers
- artifact pointers
- outcome summary
- validation summary
- cost summary
- exposure summary
- relevant Context Pack ids
- attribution pointers
- candidate pointers

Does not store by default:

- full transcripts
- raw tool logs
- raw user content
- duplicated event streams

### 6.2 ObservationCapabilityRecord

Describes what Horizon could actually observe.

Fields:

- host
- adapter version
- capability level
- available events
- missing events
- redaction constraints
- replay support
- validation support
- confidence limits

### 6.3 AttributionResult

Explains whether Horizon likely helped, hurt, or had no clear effect.

Fields:

- outcome label
- Horizon contribution label
- evidence pointers
- confidence
- confounders
- alternative explanations
- missing evidence
- allowed downstream use

### 6.4 HarnessCandidate

A proposed Horizon-owned improvement.

Fields:

- id
- owner subsystem
- scope
- delta type
- typed delta
- required observation level
- expected improvement
- expected cost
- affected surfaces
- dependencies
- conflicts
- evidence grade
- rollback rule
- invalidation triggers

### 6.5 CandidateScorecard

The evaluation result for a HarnessCandidate.

Fields:

- replay result
- ablation result
- regression result
- exposure result
- leverage result
- shadow result
- canary result
- cost impact
- task-quality impact
- confidence
- recommendation

### 6.6 PromotionDecision

The governance decision.

Allowed decisions:

- reject
- record only
- diagnostic only
- suggested
- shadow
- canary
- scoped promote
- portable scoped promote
- global promote
- retire
- revert

### 6.7 PolicyRegistryEntry

The only durable behavior-changing output.

Fields:

- policy id
- subsystem
- scoped applicability
- typed delta
- evidence bundle
- evidence grade
- promotion decision
- monitoring rule
- rollback rule
- expiry/review rule
- invalidation triggers
- owner
- audit trail

### 6.8 RejectedHypothesis

Negative knowledge. Prevents Horizon from rediscovering failed ideas.

Fields:

- hypothesis
- scope
- evidence against it
- confounders
- retry-after condition
- invalidation condition
- related candidates

## 7. Evidence Grades

Every candidate receives an evidence grade.

```yaml
evidence_grade:
  G0: observed signal only
  G1: diagnostic correlation
  G2: replay-supported
  G3: ablation-supported
  G4: shadow-supported
  G5: canary-supported
  G6: multi-scope validated
```

Promotion eligibility:

```yaml
promotion_rules:
  record_only:
    allowed: [G0, G1]

  diagnostic:
    allowed: [G1]

  suggested:
    allowed: [G2]

  shadow:
    allowed: [G2, G3]

  canary:
    allowed: [G3, G4]

  scoped_promoted:
    allowed: [G5]

  portable_scoped_promoted:
    allowed: [G5, G6]

  global_promoted:
    allowed: [G6]
    default: discouraged
```

Evidence grade is not cosmetic. It controls what Horizon is allowed to do.

## 8. Observation-to-Action Matrix

Horizon may only learn what its observation level supports.

```yaml
observation_to_action:
  L0_no_host_telemetry:
    allowed:
      - record diagnostics
      - improve static repo indexing
      - identify missing telemetry
    forbidden:
      - promote context-pack policy
      - claim validation effectiveness
      - infer agent behavior

  L1_basic_session_events:
    allowed:
      - correlate task class with pack shape
      - detect repeated missing files
      - record diagnostic candidates
    forbidden:
      - optimize tool-call sequencing
      - infer causal token waste
      - promote validation policy

  L2_tool_and_file_events:
    allowed:
      - replay context-pack usefulness
      - adjust retrieval ranking
      - improve validation routing
    forbidden:
      - global promotion
      - cross-repo generalization without more evidence

  L3_full_trace_with_validation:
    allowed:
      - ablation
      - shadow policies
      - scoped canaries
      - scoped promotion after evidence gates

  L4_cross_session_outcome_data:
    allowed:
      - canary promotion
      - portable scoped promotion
      - cross-repo generalization
      - global-policy consideration
```

Candidate generation should be capability-gated before creation, not only before promotion.

## 9. Typed Policy Deltas

Experience Optimization may emit typed policy/config deltas. Arbitrary optimizer-generated code deltas are forbidden by default.

### 9.1 Retrieval Deltas

```yaml
retrieval_delta:
  - ranking_weight_change
  - source_priority_change
  - freshness_bias_change
  - symbol_graph_bias_change
  - dependency_surface_bias_change
  - negative_cache_rule
  - stale_source_penalty
```

### 9.2 Context Pack Deltas

```yaml
context_pack_delta:
  - budget_reallocation
  - section_order_change
  - evidence_density_change
  - compression_strategy_change
  - dependency_context_expansion
  - omission_threshold_change
  - provenance_requirement_change
```

### 9.3 Validation Deltas

```yaml
validation_delta:
  - required_gate_change
  - risk_trigger_change
  - test_selection_change
  - replay_requirement_change
  - static_analysis_requirement_change
  - validation_escalation_rule
```

### 9.4 Memory Deltas

```yaml
memory_delta:
  - write_threshold_change
  - stale_rule_change
  - conflict_policy_change
  - negative_knowledge_rule
  - decay_rule_change
  - promotion_to_durable_memory_rule
```

### 9.5 Intervention Deltas

```yaml
intervention_delta:
  - warn_before_missing_evidence
  - interrupt_on_stale_context
  - stay_silent_when_low_confidence
  - escalate_when_validation_absent
  - ask_host_for_more_observability
```

### 9.6 Exposure Deltas

```yaml
exposure_delta:
  - retention_change
  - redaction_change
  - provenance_requirement_change
  - artifact_visibility_change
  - raw_trace_access_rule
```

### 9.7 Cost / Leverage Deltas

```yaml
cost_leverage_delta:
  - context_budget_limit_change
  - expensive_gate_threshold_change
  - replay_frequency_change
  - cache_reuse_policy_change
  - validation_cost_cap_change
```

## 10. Candidate Interaction Handling

Candidates can conflict even when individually useful.

Track:

```yaml
candidate_interaction:
  candidate_id:
    depends_on:
      - another_candidate_id

    conflicts_with:
      - another_candidate_id

    amplifies:
      - another_candidate_id

    suppresses:
      - another_candidate_id

    affected_surfaces:
      - retrieval
      - context_pack
      - validation
      - memory
      - exposure
      - cost
```

Example:

```yaml
example:
  retrieval_more_dependency_context:
    conflicts_with:
      - aggressive_context_pack_compression
    reason:
      - one increases included dependency evidence
      - the other removes peripheral context aggressively
```

Do not promote interacting candidates together unless the interaction was evaluated.

## 11. Promotion Model

Default promotion is scoped.

Scopes may include:

- repo
- workspace
- host
- host capability level
- task class
- language/toolchain
- dependency family
- file area
- risk class
- validation availability
- exposure class

### 11.1 Scoped Promotion

Use when evidence supports a policy in one specific environment.

### 11.2 Portable Scoped Promotion

Use instead of broad global promotion.

A policy may become portable when evidence holds across:

- similar host capability levels
- similar repo archetypes
- similar task classes
- similar language/toolchain families
- similar validation availability
- similar exposure constraints

### 11.3 Global Promotion

Global promotion is allowed only for stable, low-risk, broadly validated rules.

Global promotion should be rare.

## 12. Gates

A candidate cannot promote unless it passes all required gates for its target maturity.

Required gates:

- observation capability gate
- provenance gate
- attribution gate
- replay gate
- ablation gate
- regression gate
- exposure gate
- cost/leverage gate
- interaction gate
- rollback gate
- invalidation gate

Gate failure should produce either:

- a rejected hypothesis
- a lower maturity decision
- a narrower scope
- a missing-evidence requirement

## 13. Experience Store

The Experience Store is not a separate warehouse. It is a manifest index over canonical ledger data.

It stores:

- manifests
- pointers
- summaries
- scorecards
- decisions
- redacted artifacts
- rejected hypotheses

It does not store by default:

- full transcripts
- raw user content
- raw event streams
- duplicated host logs
- unbounded trace history

Raw trace retention requires explicit exposure policy.

## 14. Metrics

Optimize product outcomes first, Horizon proxy metrics second.

### 14.1 Product Outcomes

- fewer wrong-file edits
- fewer missed dependencies
- fewer stale-context failures
- higher first-pass validation success
- fewer unnecessary agent turns
- fewer user corrections
- fewer abandoned sessions
- fewer hallucinated assumptions

### 14.2 Horizon Outcomes

- smaller Context Packs at same or better success rate
- higher evidence density
- lower stale-context inclusion
- better validation gate selection
- lower replay disagreement
- fewer promoted-policy reversions
- better cache reuse without quality loss
- better host-specific rendering

### 14.3 User Outcomes

- less manual steering
- fewer interruptions
- better background operation
- less need to explain repo structure repeatedly
- more predictable agent behavior

### 14.4 Anti-Metrics

Do not optimize these alone:

- token count
- number of retrieved files
- benchmark score
- number of validations run
- number of generated candidates
- frequency of interventions

These are only useful when tied to task success and user value.

## 15. Benchmark Policy

Benchmarks are secondary evidence.

Benchmark providers may be useful for regression, replay, and stress testing, but benchmark-only improvements cannot globally promote.

Rules:

- benchmark wins can justify diagnostic or shadow status
- benchmark regressions can block promotion
- benchmark-only wins cannot override product-session evidence
- benchmark suites must declare what they do and do not represent

## 16. Degraded Mode

When observability is weak, Horizon must degrade honestly.

Examples:

```yaml
degraded_mode:
  weak_host_telemetry:
    behavior:
      - record weaker diagnostics
      - avoid causal claims
      - avoid promotion
      - request better adapter capability if useful

  missing_validation_records:
    behavior:
      - do not claim validation improvement
      - limit changes to retrieval/context diagnostics

  missing_cost_records:
    behavior:
      - do not promote cost/leverage policy
      - allow quality-only diagnostics
```

No fake certainty.

## 17. Full-Scope Build Order

This is sequencing for the full vision, not an MVP reduction.

### Phase 1: Preconditions

Build or stabilize:

- event ledger
- repo/workspace identity
- host capability profile
- Context Pack ABI
- provenance hooks
- ExposureManifest
- PackQualityReport
- validation records
- cost/token/latency records
- policy registry skeleton

### Phase 2: Experience Recording

Add:

- ExperienceRecord manifests
- ObservationCapabilityRecord
- manifest pointers to ledger events
- redacted artifact references
- session-level outcome summaries

### Phase 3: Attribution

Add:

- AttributionResult
- confounder register
- evidence confidence
- alternative explanation tracking
- allowed downstream use

### Phase 4: Candidate Generation

Add:

- HarnessCandidate
- typed policy deltas
- candidate scope
- required observation level
- rollback and invalidation rules
- rejected hypotheses

### Phase 5: Evaluation

Add:

- CandidateScorecard
- replay gate
- ablation gate
- regression gate
- exposure gate
- cost/leverage gate
- interaction gate

### Phase 6: Promotion

Add:

- PromotionDecision
- scoped PolicyRegistryEntry
- policy monitoring
- rollback behavior
- expiry/review behavior

### Phase 7: Shadow and Canary

Add:

- shadow evaluation
- canary routing
- portable scoped promotion
- cross-session validation
- cross-repo validation

### Phase 8: Advanced Evaluation

Add later:

- benchmark providers
- Pareto/frontier analysis
- larger replay suites
- multi-host comparative analysis
- long-horizon outcome tracking

## 18. What to Avoid

Avoid:

- arbitrary self-modifying code deltas
- global policies too early
- benchmark theater
- duplicate event warehouses
- raw trace retention by default
- excessive ABIs for objects that are just fields
- optimizing token count over task success
- candidate generation from weak observation
- unscoped promotion
- policy changes without rollback
- conflating Horizon failures with host-agent failures

## 19. Final Architecture Statement

Experience Optimization is not a second agent, not a product-code modifier, and not a benchmark optimizer.

It is Horizon’s governed learning loop:

```text
observe real agentic development work
-> attribute Horizon’s contribution
-> propose typed Horizon policy deltas
-> evaluate with evidence
-> promote only scoped, reversible changes
-> remember both wins and failures
```

The full vision is strongest when it stays strict:

- evidence over vibes
- scoped policy over global magic
- manifests over warehouses
- typed deltas over arbitrary mutation
- product outcomes over proxy metrics
- background support over agent replacement
