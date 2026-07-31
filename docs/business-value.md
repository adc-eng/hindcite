# Business Value
 
*Draft. Deliberately short — this needs real customer conversations before it's worth more.*
 
## The problem, stated commercially
 
When a commitment is missed, somebody spends hours reconstructing why. They open the ERP, scroll back through email, search chat history, and ask two colleagues what they remember. The answer usually exists. It's just distributed across systems that don't talk, plus a few facts that live only in someone's head.
 
That reconstruction happens constantly and nobody measures it, because it's spread thinly across operations managers, schedulers, account managers, and engineers. It shows up as "customer service overhead" or "just part of the job."
 
## Why now
 
Three things that weren't simultaneously true a few years ago:
 
- Language models can read unstructured messages and email well enough to extract structured facts, with confirmation.
- Agent frameworks make multi-step investigation with an audit trail practical to build.
- Businesses have accumulated enough digital exhaust — chat, email, ticket systems — that the facts are captured, just not connected.
## Who feels it most
 
**Small and mid-sized manufacturers and distributors.** They have an ERP and not much else. Scheduling knowledge lives with two or three experienced people, and a lot of coordination happens over phone and messaging apps. When something slips, there's no system that knows why — only people who remember. This is the sharpest fit, because the informal channels aren't a gap in their process, they *are* the process.
 
**Larger enterprises**, differently. They have more systems, which makes it worse: the facts are scattered across more places, each with an owner, and reconstruction means asking three teams. Value here is less about missing data and more about eliminating cross-team archaeology.
 
**Adjacent, same shape:** third-party logistics, field service, clinical operations, claims processing, engineering incident response. Anywhere a commitment exists, informal channels carry real information, and someone has to explain a miss.
 
## What it's worth
 
**Direct:** hours recovered per missed commitment. Order of a few hours each, at operations-manager cost, across however many misses a month.
 
**Larger, harder to price:** the same root cause tends to recur. A system that names causes precisely enough to count them turns anecdote into a fixable pattern. "We lose four days a month to late supplier notification" is a business case; "things slip sometimes" isn't.
 
**Softer, possibly biggest:** answering the customer faster and with evidence. There's a real difference between "we're looking into it" and a chain with dates and sources, within an hour.
 
**A note on honesty:** none of these numbers are validated. The pattern is plausible from experience; the sizing needs customer conversations. Anyone building a plan on this should treat it as a hypothesis.
 
## Why not just a dashboard
 
Dashboards show current state. This reconstructs history — including what was *believed* at each point, which is often the actual finding. "The scheduler acted on a stale note" is invisible to any dashboard, because dashboards don't keep the history of what was known.
 
## Why not just ask a chatbot over the data
 
Two reasons.
 
First, correctness. Semantic search over business records produces confident, wrong answers about dates and quantities. For diagnostics, that's worse than nothing, because people act on it.
 
Second, verifiability. Nobody will act on an unexplained conclusion about a customer commitment. Every claim needs to trace to a specific fact with a timestamp and a source. That's an architectural property, not a prompt.
 
## Shape of the offering
 
A reasoning engine plus **domain packs** — adapter, ontology, workers, and vocabulary, per industry. Customers in manufacturing and clinical operations see different words and different sources, same product underneath.
 
Pricing likely per seat for the operations roles who use it, with domain packs as the expansion path. Land in one function, expand by connecting sources.
 
Multi-tenancy, isolation, audit, retention: table stakes for enterprise, deliberately deferred until the reasoning is proven. Building the enterprise wrapper around an unproven engine is the classic way to waste a year.
 
## The honest risks
 
**The value depends on informal information being captured.** If the reel-shortage phone call never gets written down anywhere, the system can't find it. Adoption of the note-entry surface is the hinge — and it's a behavior change, which is the hardest kind of adoption.
 
**Entity resolution is where deployments die.** Making "Line 2," "the SMT line," and `RES-0042` one thing is unglamorous, entirely customer-specific, and if it's wrong the reasoning is wrong. This is the real integration cost and it should be priced in, not discovered later.
 
**"Good enough" is a strong competitor.** Someone spending three hours a month on reconstruction won't buy anything. The buyer is the operation where this happens weekly and someone is visibly frustrated.
 
**Trust is fragile and asymmetric.** One confidently wrong causal chain undoes twenty correct ones. This is why "I don't know, and here's what's missing" is a product requirement rather than a nicety — it's what makes the confident answers credible.
 
## What would validate this
 
Five conversations with operations managers at small manufacturers, asking one question: *when an order slips and a customer asks why, what do you actually do?*
 
If the answer is "I check the ERP and it's obvious," there's no product. If it's "I ask around and it takes half a day," there is.
 
