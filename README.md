# Evidence-Only Agents: Keeping AI Tools Out of the Decision Loop

I run a personal quantitative research system that estimates market regimes from price data and from analysis of how institutions behave. This year I added AI agents to it. The design question that took the most care was what those agents would be allowed to do. My answer was narrow: they gather sourced evidence, and nothing else. They cannot score a signal, propose a hypothesis, or authorize a decision.

This post describes that boundary, the fail-closed check built around it, and the record-keeping that holds me to the same standard. A system this small says nothing about frontier models. It is a useful place to practice the habits AI control research asks for at much larger scale.

## The system

The system has two halves. One half is statistical: two-state hidden Markov models fitted to price series, one on a single asset and one on three assets jointly, with Student-t emissions. The other half is institutional: written analyses of central banks, exchanges and settlement systems, turned into scored signal layers. A convergence step combines the layers into one composite, and a position is authorized only when the composite clears a five-level veto hierarchy.

The institutional half carries the edge. Its job is to narrow the space of hypotheses before any data is touched. Given what an institution is built to do, and what it is constrained from doing, what behavior should we expect? The statistical half then tests whether the data agrees.

## The boundary: evidence only

In the latest phase I added four ingestion agents, covering sovereign reserve flows, physical metal vaults, energy markets and policy, plus a compiler that assembles their output. The rule for all five is the same: return sourced evidence. Every item carries its source. No agent assigns a score. No agent suggests a new signal, a new layer or a new explanation. No agent can authorize anything.

The reason is specific to how research goes wrong. Search has become cheap. An agent permitted to propose hypotheses will propose many, and some will fit the data by chance. Each looks reasonable alone; together they widen the hypothesis space until the system is fitting noise. I call this the solvent: cheap agentic search dissolves the constraint that made the model worth running. Restricting agents to evidence keeps hypothesis generation in the one place I can audit, my own written institutional analysis.

This is least privilege, an old security principle. What I found useful was applying it to epistemic actions. The boundary governs which kinds of claims an agent may introduce, alongside which systems it may touch.

## Failing closed

A boundary on inputs needs a matching rule on outputs. The convergence step counts how many of its layers are verified for the current cycle. Below a threshold, the composite is voided, and a voided composite blocks position authorization regardless of how strong any single signal looks. I call this state DARK.

It fired on its first live use. A signal looked attractive, too few layers were verified, and the system refused to act. That refusal was the test passing. A breaker that never trips proves nothing about whether it works; one that trips on a real case, for a reason you can trace, earns some trust.

A related rule: no single layer is trusted alone, and that includes probability estimates from the AI model I work with. Those estimates enter as one input among several, with the authority their calibration has earned. So far that is too little to stand alone.

## Holding myself to the same rule

The agents are one source of drift. I am another. Two practices guard against that.

The first is preregistration. Before re-estimating a model, I write down the hypothesis and the gate it must clear to be adopted. I suspected the market had a third regime, so I preregistered that hypothesis and ran the fit. It failed: the third state absorbed a single outlier session and described no persistent regime. The two-state model stayed.

The second is a negative-results registry. Confirmed negatives are logged with the same standing as positive findings; there are ten so far. One records a labeling defect. The model's states had been labeled in the order the fitting routine returned them instead of by variance, which silently mislabeled the base regime. Variance-ordered labeling is now a standing rule. The defect was cheap to fix and easy to miss, which is the shape of failure I worry about most.

## Who did what

I designed the protocol, specified the models and adoption gates, and set the rules above. An AI coding assistant wrote nearly all of the code and ran the fits and tests under those rules. I review its output against the preregistered gates. That division of labor is part of the experiment: a person setting constraints and judging results, an AI doing the implementation, and explicit rules about where the AI's contribution stops.

## What carries over

This is one person's system with personal stakes. What I think carries over is a habit of asking three questions of any AI component. What kinds of claims may it introduce? What happens when the inputs it depends on are missing or unverified? How would I find out it had drifted?

In my system the answers are evidence only, fail closed, and a written record of every negative. Each answer came from a mistake, and several of those mistakes sit in the registry. I want to work on these questions where the systems are larger and the stakes are not personal.
