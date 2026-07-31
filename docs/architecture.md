# Architecture

![Diagnostic reasoning architecture](architecture-diagram.svg)

The system diagnoses "why didn't X happen as expected?" over heterogeneous business facts, and shows its work as a cited causal chain. It is organized in three bands: **agents** (top), **stores** (middle), and the **planner** (bottom).

## Domain agents

A domain agent owns one slice of a domain and knows how to answer questions about it. Take a **production line** as the running example: a *line-telemetry agent* owns the structured telemetry of the assembly lines — throughput, station cycle times, stoppages, sensor readings.

Each domain agent is self-contained and can carry:

- its own **structured store** (the source system it reads — e.g. the telemetry historian)
- its own **knowledge base** and **RAGs** over domain material
- its own **tools** and **MCP servers** for live lookups and actions
- an **asynchronous ingestion path** — any domain expert can add knowledge (playbooks, past root causes, notes) to the agent's KB at any time, independent of any investigation

Structured line telemetry is *a domain*; orders, schedule, inventory, quality, deploys are other domains, each with its own agent.

## Scribe agent

The **Scribe** is the one agent that is not tied to a domain. It covers the loose corporate information store — email, Slack, floor notes, tickets — the human signal that doesn't belong to any single system of record. Where a domain agent reads structured facts, the Scribe reads prose and turns it into claims.

## Claims

A **claim** is a materialized understanding of a single granular fact, produced by an agent from the domain data, KB, and MCP tools at its disposal.

- Claims are minted **in context**: the Validator asks an agent to verify something, and while verifying, the agent concludes one or more domain-local facts.
- Those facts are written to the **Claims store** and are available to the rest of the investigation.
- They also persist for future investigations — the store accretes; nothing is overwritten.

## Planner

The planner runs one investigation. It has four parts.

### Clarifier

A **multimodal, iterative chat** that makes sure the question is understood. It accepts text and images and goes back and forth with the user as needed. The demarcation is sharp: **when the Clarifier finishes, the investigation objective is clear.** Everything downstream works from that clarified question.

### Reasoner

The Reasoner takes the clarified question and queries the **Claims store**, pulling whatever it judges relevant given its reading of the question and the possible explanations. From those claims it forms either:

- a **hypothesis** — a candidate explanation that the current claims *point at* but do not settle, or
- a **proof** — a conclusion the current claims *already establish*.

### Validator

The Validator takes a hypothesis together with the claims the Reasoner formed it on, and seeks confirmation. It dispatches the domain agents and the Scribe (`verify`) to look in the world; they return **new claims**, and on that enriched evidence the Validator concludes.

- If the Reasoner produced a **proof** rather than a hypothesis, there is nothing to look up — the Validator trivially passes it through without calling any agent.
- The `Done?` decision then either **continues** the loop (form the next link) or ends it.

### Traces store

Every completed investigation — the reasoning and its verdict — is written to the **Traces store** (`always record`). All final investigations enter the Traces store; it is the planner's durable record of what it did.

## Agent-assisted hypothesis *(phase 2)*

When the Reasoner's own hypotheses are exhausted, the planner can ask the domain agents to *propose* hypotheses given the ones already ruled out, then feed those back into the loop (`revise`). This is a later-phase capability and will evolve.
