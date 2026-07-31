


# Project Vision: A Diagnostic Reasoning System
 
## The problem, in one sentence
 
Every business runs on commitments, and when a commitment is missed, finding out *why* is manual archaeology — someone digs through systems, emails, and chat logs to reconstruct what happened.
 
## The scenario that started this
 
An electronics assembly house takes orders from a production control center. The control center knows things the assembly house doesn't: customer priority (a bulk Microsoft order outranks a retail one), resource updates that arrive through informal channels, internal notes that never make it into a system of record.
 
Weeks later, someone asks: *"We submitted this order a while ago. It still hasn't been assembled. Why?"*
 
Today, answering that means a person opening four or five tools and reconstructing a story from fragments. The information exists. It's just scattered, and nothing connects it.
 
## What we are building
 
A system that answers "why didn't X happen as expected?" by reasoning over all those fragments at once — and shows its work.
 
The output is not a chat response. It's a **causal chain with citations**:
 
> Order 1043 slipped because Line 2 went down on the 14th, which happened because the capacitor reel arrived short, which we knew from a supplier email logged on the 12th — but the schedule wasn't re-run until the 16th, and by then a higher-priority order had taken the line.
 
Every link in that chain points to a specific fact, with a timestamp and a source. You can check it. That's the difference between a diagnostic tool and a chatbot that sounds confident.
 
## The core insight: information nuggets
 
The system treats every fact as a **nugget** — a small, self-describing piece of information:
 
- What it says
- Which entities it's about (this order, that resource)
- **When it was true** (the reel was short from the 10th onward)
- **When we learned it** (the email arrived on the 12th)
- Where it came from, and how much to trust it
Those two separate timestamps matter more than they might appear. The gap between *when something was true* and *when we knew it* is where a large share of real business failures live. Nothing went wrong in the world; the information arrived late, or arrived stale, and someone made a reasonable decision on the wrong picture.
 
A dashboard cannot find that. It only shows current state. A system that keeps both timelines can say: *the scheduler acted correctly on what it knew — and what it knew was four days out of date.*
 
## Why this generalizes
 
Strip away the assembly house and the pattern is:
 
- Something was **supposed to happen** (a commitment, a target, a normal state)
- Facts arrived from **multiple channels** with different reliability
- Something **deviated**
- Someone needs to know **why**, with evidence
That describes a manufacturer, a logistics operation, a hospital, an engineering team responding to an outage, a loan processing desk. The domain changes. The reasoning does not.
 
So the system is built as a **generic core with domain adapters**. The core knows about nuggets, time, evidence, and causality. The adapter knows what an "order" is, or a "service," or a "claim."
 
## The two reference domains
 
We are building two, deliberately different, to prove the abstraction is real rather than hypothetical.
 
**Assembly house** — built to depth. Commitments are scheduled dates. Causality runs through resource contention: your order didn't run because something else took the line. Time is measured in days.
 
**Incident root cause analysis** — built thin. Commitments are service level objectives, not dates. Causality runs through service dependencies: search broke because checkout exhausted a shared connection pool. Time is measured in seconds.
 
If one core handles both without special-casing, the generic claim holds. If it doesn't, we find out early — which is the point of building the second one.
 
## Where this could go as a product
 
The natural shape is a B2B SaaS platform:
 
- **One reasoning engine**, many domains
- **Domain packs** — an adapter, an ontology, worker agents, and a UI skin, per industry
- **The same experience everywhere** — ask a question, watch the investigation, get a chain you can verify
A customer in manufacturing and a customer in healthcare would see different vocabulary and different data sources, but the same product underneath.
 
Multi-tenancy, isolation, audit trails, role-based access, retention policy — all necessary eventually, none of it necessary now. Building them early would mean building infrastructure around a reasoning engine that hasn't been proven yet.
 
## What we are deliberately not doing yet
 
- No production data. Synthesized scenarios only.
- No multi-tenancy, no auth, no persistence layer.
- No real ingestion connectors. Simulated sources.
- No cloud deployment decision. That is a deployment concern, not an architecture one, and locking it in early is how you end up vendor-locked by accident.
## How we will know it works
 
Not "it produced an answer." The bar is:
 
1. It finds a **multi-hop cause** — not just the last thing that went wrong, but the chain behind it.
2. It **rules things out** visibly. Discarding a plausible-but-wrong hypothesis is more convincing than finding the right one.
3. It says **"I don't know"** when the evidence doesn't support a conclusion — and names precisely what's missing.
4. Changing **one fact** changes the answer correctly.
The third is the one that earns trust. A diagnostic system that confabulates is worse than no diagnostic system, because someone will act on it.