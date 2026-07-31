# Design decisions

A running list of choices we've made and why. Newest at the top.
Not a changelog — Git tracks edits. This tracks decisions.

## 2026-07-31 — planner and naming

- **Call them "claims," not "nuggets."** A claim can be true or false, high or low confidence. "Fact" sounded too much like "true thing," which is wrong for a stale note.
- **The reasoning box is the "Reasoner," not "hypothesis engine."** It does two things: proves what the current claims already settle, and hypothesizes only where they leave a gap. "Hypothesis engine" only named half its job.
- **The checking box is the "Validator," not "Test."** It doesn't just check a boolean — it sends agents out to fetch evidence in the world.
- **Claims are made during verification.** When the Validator asks an agent to check something, the agent looks it up and writes new claims. Those count for this investigation and stay for future ones.
- **Priors stay out of the claims store.** If an agent suggests "look here," that's just a hint in the trace. Only real facts (observations) get stored and cited.
- **The Reasoner reads the stores; the Validator dispatches the agents.** Claims + traces feed the Reasoner. Agents feed the Validator.
- **A proof skips the agents.** If the Reasoner already has enough to conclude, the Validator passes it through without calling anyone.
- **Every finished investigation is recorded** to the traces store.
- **The Clarifier ends with a clear question.** It's a multimodal back-and-forth chat. Once it's done, the investigation objective is settled, and the Reasoner works from that.
- **Agent-assisted hypothesis is phase 2.** When the Reasoner runs out of ideas, agents can propose new ones. Later work.

## Earlier — stores, claims, and agents

- **Two kinds of agents.** Dedicated domain agents (each owns one domain — its own data, KB, RAG, tools, MCP servers) and one Scribe agent for the domain-less corporate signal (email, Slack, floor notes).
- **A claim has two clocks.** When it was true in the world, and when the system learned it. The gap between them is often the whole finding (a note written after the thing it describes had already changed).
- **Structured sources use plain code; unstructured sources use a model.** Reading an ERP row into a claim is field-mapping, no model needed. Reading a floor note needs a model to pull out the fact — and that agent proposes, then gets confirmed.
- **The claims store is one materialized store, filled on demand.** Sources keep their own databases. Claims are normalized copies, written only when an investigation needs them. Nothing is overwritten — it only grows.
- **Agents can reason, but every fact they return must point at a claim.** An agent may use a model, RAG, or a live MCP call to decide what to do — but anything it reports as evidence has to bottom out in a stored claim.
- **A claim has two labels.** How much to trust it (system record / inferred / human-said) and what it's for (an observation about this case vs. general reference material). Trust and role are separate.
- **Derived claims can be written by agents using plain-English rules for now.** We don't force everything into strict code rules yet. If a rule proves stable and gets used a lot, we harden it into code later.
- **Past investigations are kept and reused.** Finished investigations go to the traces store, which the planner can search for precedent. Precedent points the search; it never closes it.
- **The planner never decides domain truth.** It knows how to run an investigation; the domain agents know what things mean. Same rule as: a model decides what to look at, never what's true.
