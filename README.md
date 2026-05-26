# Horizon

Horizon is a local-first background optimization layer for agentic development.

It is not another agentic IDE, editor, chat interface, terminal UI, or replacement for tools like opencode, Codex, Claude Code, or Aider. The user should keep using their existing coding agent normally. Horizon runs underneath those tools to make agentic coding scale better on complex projects.

Current status: paper / theory repo. No implementation yet. Everything is subject to change.

## Product thesis

As codebases grow, coding agents waste more tokens, reread the same files, lose context, hallucinate APIs or project structure, and run expensive validation too broadly. Horizon exists to reduce that waste by maintaining durable local repo intelligence and feeding existing agents the smallest sufficient context for the current task.

Horizon's best-case goal is model leverage: a smaller, cheaper, faster model with Horizon should be able to compete with much larger frontier models operating without Horizon.

## Core loop

```text
Evidence -> Context Pack ABI -> Host Agent -> Attribution -> Better Next Pack
```

Horizon observes the repo, task, and host-agent session; compiles the smallest sufficient Context Pack through a stable ABI; watches whether the host agent used, ignored, or fought that support; attributes the outcome; and improves the next pack.

Context Pack ABI is the handoff contract. The attribution loop is the learning mechanism.

## Product boundary

Host agents own the chat UI, editor UI, terminal UI, patch generation, code editing, interactive debugging, user conversation, and normal developer workflow.

Horizon owns durable repo intelligence, evidence selection, context compilation, Context Pack ABI, validation routing, scoped memory, negative knowledge, provenance, policy/risk decisions, outcome attribution, replay, and cost/latency optimization.

## Full vision

The full system architecture, planes, ABIs, object model, guardrails, and optional capability modules live in [stack.md](./stack.md).

## Current priority

Context Pack ABI + attribution loop.
