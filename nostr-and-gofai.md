Can Nostr Save Good Old Fashioned AI?
=====

Most of us think “AI” just means ChatGPT-style LLMs, giant neural nets that have become so good at prediction that they appear to simulate reasoning. That’s called **connectionist AI**, and for the last decade it has taken the world by storm. But it’s not the only kind of AI that ever existed. There is an older, completely different school called Good Old-Fashioned AI, or GOFAI for short: expert systems that make use of explicit symbols, logical rules, hand-crafted ontologies to achieve actual reasoning (or some would argue) instead of statistical autocomplete. For decades, GOFAI overshadowed connectionist AI as the dominant school of thought. Even now, some consider GOFAI to be the only path to **real** intelligence. But in recent years, with GOFAI's disappointing results and the recent successes of LLMs, interest in GOFAI has waned. So what killed it?

In one word: **knowledge**.

## The 40-year Knowledge-Acquisition Bottleneck

Ever since its inception, GOFAI has been haunted by a single, unsolved problem: how to acquire, at scale and without ruinous cost, the structured commonsense and causal knowledge that humans effortlessly trade in conversation.

The largest and most comprehensive attempt to encode common-sense knowledge using the methods of Good Old-Fashioned Artificial Intelligence is Cyc.

Douglas Lenat began Cyc in 1984 with the conviction that a sufficiently large, hand-crafted ontology of explicit axioms—encoded by professional ontologists—would eventually let machines reason like bright ten-year-olds. After forty years and several hundred million dollars, Cyc contains roughly 25 million axioms yet still fails at simple tasks that any child (or any two-day-fine-tuned LLM) solves instantly: understanding that a glass will spill if turned upside-down, that “my enemy’s enemy is my friend,” or that promising to come tomorrow usually implies intent to come tomorrow. The reason is not inference; it is acquisition. Central teams of paid experts cannot anticipate the infinite contexts, cultures, and edge cases the world throws at a reasoning system. Cyc proved that the bottleneck is not logical but social and economic.

Many view the Cyc project's outcomes as a cautionary tale regarding the limitations of Good Old-Fashioned AI (GOFAI) and purely symbolic approaches. 

The same bottleneck re-appeared, in almost identical form, in the most sophisticated modern descendant of symbolic AI: Josh Tenenbaum’s program of hierarchical Bayesian program induction (Lake et al., 2015; Ellis et al., 2021). Systems such as Bayesian Program Learning and DreamCoder can, in carefully engineered micro-domains, invent rich symbolic programs—new letters, physics engines, list-processing abstractions—from one or a handful of examples. They do this by performing Bayesian inference over a space of short, compositional programs biased by strong inductive priors (simplicity, reuse, core knowledge). The models are psychologically faithful and computationally elegant. Yet they remain confined to toy worlds because no one has ever supplied them with the vast, messy, culturally transmitted library of reusable abstractions that human children absorb from language and observation. Without that library, the prior is either too weak (explosion of hypotheses) or too strong (hand-engineered by the researcher, defeating the entire point).

Large language models appear to have sidestepped the problem entirely by memorizing statistical correlations across trillions of tokens. They can parrot commonsense remarkably well, but they do not possess executable causal models, cannot reliably chain reasoning beyond a few steps, and hallucinate confidently when the statistical pattern runs out. In Tenenbaum’s terms, they are masters of System 1 intuition without System 2 understanding. Billions of dollars continue to pour into scaling this approach, while the symbolic research program that once promised genuine understanding receives essentially zero industry funding.

So what does this have to do with nostr?

## The Nostr Connection

We claim that the knowledge acquisition bottleneck was never at its heart a technical problem. The missing piece is not a better inference algorithm or a cleverer prior; it is a writable, cryptographically signed, globally replicated substrate on which millions of humans can continuously propose, critique, refine, and weight symbolic structures. 

The nostr protocol provides exactly that substrate. And knowledge acquistion? Web of trust. 

## Draft summary of lesson learned, bring thought process around to social interaction, language

Cyc was indeed a viable company for 40 years, with products like (health care expert systems). But its greatest contribution may be that of a cautionary tale. What is the lesson learned? It failed to take into account social interatcions. The symbols we use to represent stuff, the ontologies and the like, are an emergent process, the result of social interactions. Ontologies (most, at least) would not exist without our need to communicate with each other. Ontologies must change and evolve as learning takes place and understanding expands. Without a model for how ontologies change and evolve over time, a model that must of necessity take into account social intraction, GOFAI will fail. What is the correct word for a hat? It depends on the society in which we are embedded. Attempts to make a single repository of all human knowledge is doomed to fail without taking this into account. 

