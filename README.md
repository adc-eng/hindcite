# hindcite

> *hindsight you can cite*

A domain-agnostic agentic framework for diagnostic reasoning: it investigates why an expected outcome didn't happen and returns a cited, traceable causal chain.

## What it does

Dashboards tell you *what* happened. hindcite explains *why* it happened — and shows its work.

Given a deviation from an expected outcome ("why didn't order 1052 run on Line 3?", "why did checkout breach its SLO?"), hindcite runs a structured investigation: it forms hypotheses, dispatches domain agents to gather evidence, and assembles the result into a causal chain where **every link cites a checkable fact**. When the evidence doesn't support any explanation, it says so and names the gap — it never fabricates a cause.

The framework is domain-agnostic. You plug in **domain agents** over your own data, tools, and knowledge; hindcite handles the reasoning on top.

## Architecture

hindcite is organized in three bands — agents, stores, and the planner. Domain agents materialize granular **claims** from their own sources; the planner's **Reasoner** forms hypotheses from those claims, the **Validator** dispatches agents to confirm them, and every completed investigation is recorded.

See **[docs/architecture.md](docs/architecture.md)** for the full picture and diagram.

## Repository layout

| Path | Contents |
|------|----------|
| `docs/` | Architecture, vision, business value, design-decision log, glossary |
| `schema/` | The trace/claim contract — the core deliverable |
| `prototype/` | A pre-recorded demo. The trace format, not the HTML, is the real artifact here |
| `backend/` | Not yet built. Target stack: Python, LangGraph, Langfuse, Pydantic |


## Status

Design-stage. The production backend is not yet built.

Start with **[docs/architecture.md](docs/architecture.md)**.