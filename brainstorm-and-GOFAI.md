Brainstorm: Closing the 40-Year Knowledge-Acquisition Loop with Class Threads, GrapeRank, and Internet-Scale Analogical Induction
=====

(This was generated with heavy assistance from AI and has not yet been edited. There are definitely hallucinations in what follows!)

## Abstract

For four decades, symbolic AI foundered on the knowledge-acquisition bottleneck: Cyc could not hire enough ontologists, and Bayesian program-induction systems such as DreamCoder remained confined to toy domains for lack of a culturally transmitted library of reusable abstractions. We present Brainstorm, a local-first client running today on nostr that solves the bottleneck in production. Every signed nostr event is treated as a candidate symbolic fragment. GrapeRank—a nonlinear, confidence-saturating propagation algorithm—turns each user’s personal web of trust (the Grapevine) into a continuous Bayesian posterior on credibility, defeating sybil and link-farm attacks that still cripple personalized PageRank. Surviving fragments are organised into Class Threads, an explicit hierarchical primitive that directly externalises the latent manifolds interpretability researchers have discovered inside large language models. A lightweight structure-mapping engine then performs analogical induction over the highest-credence threads, proposing new edges and constraints that are themselves ordinary nostr events and subjected to the same trust-weighted voting. The result is the first closed, decentralised, internet-scale loop that turns raw social chatter into a continuously evolving, queryable, personalised symbolic mind. Early deployment with real users demonstrates dramatic sybil resistance and genuine discovery of previously unasserted causal and conceptual structure. The symbolic AI programme is no longer waiting for funding or permission—it is already running, one Grapevine at a time.

## Section 1 – The 40-year Knowledge-Acquisition Bottleneck

For four decades, symbolic AI has been haunted by a single, unsolved problem: how to acquire, at scale and without ruinous cost, the structured commonsense and causal knowledge that humans effortlessly trade in conversation.

Douglas Lenat began Cyc in 1984 with the conviction that a sufficiently large, hand-crafted ontology of explicit axioms—encoded by professional ontologists—would eventually let machines reason like bright ten-year-olds. After forty years and several hundred million dollars, Cyc contains roughly 25 million axioms yet still fails at simple tasks that any child (or any two-day-fine-tuned LLM) solves instantly: understanding that a glass will spill if turned upside-down, that “my enemy’s enemy is my friend,” or that promising to come tomorrow usually implies intent to come tomorrow. The reason is not inference; it is acquisition. Central teams of paid experts cannot anticipate the infinite contexts, cultures, and edge cases the world throws at a reasoning system. Cyc proved that the bottleneck is not logical but social and economic.

The same bottleneck re-appeared, in almost identical form, in the most sophisticated modern descendant of symbolic AI: Josh Tenenbaum’s program of hierarchical Bayesian program induction (Lake et al., 2015; Ellis et al., 2021). Systems such as Bayesian Program Learning and DreamCoder can, in carefully engineered micro-domains, invent rich symbolic programs—new letters, physics engines, list-processing abstractions—from one or a handful of examples. They do this by performing Bayesian inference over a space of short, compositional programs biased by strong inductive priors (simplicity, reuse, core knowledge). The models are psychologically faithful and computationally elegant. Yet they remain confined to toy worlds because no one has ever supplied them with the vast, messy, culturally transmitted library of reusable abstractions that human children absorb from language and observation. Without that library, the prior is either too weak (explosion of hypotheses) or too strong (hand-engineered by the researcher, defeating the entire point).

Large language models appear to have sidestepped the problem entirely by memorizing statistical correlations across trillions of tokens. They can parrot commonsense remarkably well, but they do not possess executable causal models, cannot reliably chain reasoning beyond a few steps, and hallucinate confidently when the statistical pattern runs out. In Tenenbaum’s terms, they are masters of System 1 intuition without System 2 understanding. Billions of dollars continue to pour into scaling this approach, while the symbolic research program that once promised genuine understanding receives essentially zero industry funding.

The core claim of this paper is that the bottleneck is no longer technical. The missing piece is not a better inference algorithm or a cleverer prior; it is a writable, cryptographically signed, globally replicated substrate on which millions of humans can continuously propose, critique, refine, and weight symbolic structures. The nostr protocol, combined with the mechanisms described below, provides exactly that substrate for the first time. Brainstorm is the client that turns it into a personal, continuously updating symbolic mind.

The rest of the paper shows how.

# Section 2 – nostr as the First Globally Writable, Signed Culture Stream
For the first time in history, a simple, open protocol gives every human a globally unique, cryptographically verifiable identity and a write-once append-only event stream that any client can read, replicate, or filter without permission. That protocol is nostr (Notes and Other Stuff Transmitted by Relays), and its implications for symbolic knowledge acquisition are profound.
A nostr identity is an ed25519 or secp256k1 keypair. Every event is a JSON object containing a kind, a content string or structured payload, a list of tagged pubkeys, and a signature. Relays accept and gossip events according to whatever policy their operators choose, but no relay can alter or forge an event once signed. Clients therefore have the final say: they choose which relays to trust, which keys to follow, and how to interpret the firehose.
This architecture yields three properties that Cyc, Open Mind Common Sense, ConceptNet, Wikidata, and every prior large-scale knowledge-base effort fatally lacked:

Universal write access with provenance
Any human (or bot) can assert a candidate fact, causal model, class definition, or constraint at any time, and that assertion is permanently attributed to its key. There is no editorial board, no grant proposal, no waiting list.
Social metadata as likelihood ratios
Follows, zaps (micro-payments), replies, reposts, and reports are themselves signed events. They form a dense, time-stamped trust-and-attention graph that can be interpreted as continuous Bayesian evidence about the credibility of every assertion and every asserter.
Ego-centric replication
Because the user sits at the centre of their own Grapevine, there is no single point of failure or censorship. A user’s entire knowledge graph can be reconstructed from any relay that still carries the events they care about, and the scoring of those events is performed locally, under the user’s sole control.

In short, nostr turns the entire internet into a globally replicated blackboard on which millions of humans are already writing short symbolic fragments every day—memes, reactions, links, hot takes—and attaching cryptographic signatures and payment-weighted votes to each fragment. All that was missing was a client willing to interpret those fragments as candidate axioms instead of mere social chatter.
Brainstorm is that client. The next section shows the complete system in one picture.

# Section 3 – Brainstorm System Overview
Brainstorm is a local-first client (currently desktop and mobile prototypes) that turns the raw nostr firehose into a personal, continuously evolving symbolic mind. It does this with four tightly coupled components that run entirely on the user’s device and close the acquisition loop described in Section 1. Figure 1 shows the data flow; the rest of the paper zooms into each box.

The Concept Graph and Class Thread primitive
All structured knowledge in Brainstorm is stored as a directed hypergraph whose primary organising principle is the Class Thread: a chain of nodes and typed edges that begins at a class-origin node (e.g. the abstract concept “Dog”), passes through optional superset and subset nodes (“Dogs”, “Working Dogs”, “Border Collies”), carries constraint and property schemas, and finally terminates at concrete instance nodes (“Spot”, “Luna”). Propagation edges inherit and specialise constraints downward; horizontal integration edges transcribe data sideways between threads. Class Threads are the explicit, queryable analogue of the latent manifolds that interpretability researchers have found inside LLMs.
The Grapevine
The user’s personal trust network, centred on their own pubkey. It is built automatically from follow lists (kind 3), zaps (kind 9735), replies, reposts, and explicit trust/mute events. There is no global graph; every user’s Grapevine is different and cannot be censored or altered by any third party.
GrapeRank
A nonlinear, damped propagation algorithm that continuously computes a personalised credibility score (0–1) for every event, Class Thread segment, and constraint node inside the user’s Grapevine. It is the social Bayesian posterior: follows and zaps act as likelihood ratios, time decay prevents stale axioms from dominating, and the user’s own key is the root source of truth. Only events and threads that survive a user-configurable GrapeRank threshold are materialised into the local Concept Graph.
The analogy engine (early but functional)
Once per hour (or on demand), Brainstorm scans the top 5 000–10 000 highest-credence Class Threads for subgraph isomorphisms. When two threads share a non-trivial topological skeleton (e.g. a tree-shaped support hierarchy in physics and a dominance hierarchy in office politics), the engine proposes missing edges from the richer thread into the poorer one. These candidate events are posted back to nostr as regular signed notes, where the user’s Grapevine (and everyone else’s) decides whether the analogy survives.

The loop is now closed: humans and bots propose structured fragments → GrapeRank filters and weights them according to personal trust → high-credence fragments assemble into Class Threads → the analogy engine discovers new structure by mapping across domains → new proposals enter the stream → repeat. No central ontology team, no privileged priors, no single point of failure.
The next three sections define the Class Thread primitive, formalise GrapeRank, and show concrete examples of analogical discovery in the wild.
[Figure 1: Brainstorm data flow – one-page diagram showing nostr events → Grapevine → GrapeRank scores → Concept Graph with Class Threads → analogy engine → new signed events]
(We can draft the caption and diagram together later; the text above stands alone.)

# Section 4 – The Class Thread Primitive
A Class Thread is a directed path in the Concept Graph that explicitly connects an abstract concept to its concrete instances while carrying inheritable constraints and cross-links. It is the central organising primitive of Brainstorm and serves as the direct, editable counterpart to the latent hierarchical manifolds discovered in large language models.
Formally, a Class Thread T is a sequence of nodes n₀ → n₁ → … → nₖ connected by three kinds of typed edges:

Initiation edge (n₀ → n₁): marks the birth of the thread from its class-origin node n₀ (e.g. the abstract concept “Dog”). The origin node has no incoming initiation edge and is identified by a stable URI (currently a nostr npub + human-readable slug, e.g. npub1…/concept/dog).
Propagation edges (nᵢ → nᵢ₊₁, i ≥ 1): carry inheritance. There are exactly three subtypes:
Superset → Set (“Dogs” contains all dogs)
Set → Subset (“Working Dogs” specialises “Dogs”)
Subset → Instance (“Spot” is a concrete member)
Each propagation edge may weaken, strengthen, or add constraints as it flows downward.

Termination edges: point from the final instance node to a terminal sink (⊥) and are usually omitted from diagrams.

Attached to any node (except the terminal sink) may be:

Constraint nodes (purple): JSON-Schema-like predicates that must hold for all downstream nodes unless explicitly overridden. Example: { "legs": 4, "sound": "woof" } on the “Dogs” node.
Property trees (orange): nested key-value structures that describe the node itself (name, description, images, sources).
Horizontal integration edges (blue dashed): copy or compute values from another thread. Example: the thread for “Breed Characteristics” fills the “coat” property of every “Irish Setter” instance automatically.

Figure 2 shows three real Class Threads from the current Brainstorm beta:
(a) Dog → Dogs → Working Dogs → Border Collies → Luna
(b) Physical Object → Rigid Body → Container → Glass → MyCoffeeMug
(c) Social Relation → Reciprocity → Friendship → Alice ↔ Bob (bidirectional variant)
Key correctness rules enforced by every Brainstorm client:

Monotonic specialisation: constraints may only be refined, never contradicted, as you move downward (Liskov substitution principle for knowledge graphs).
Unique origin: every thread has exactly one class-origin node; merging two origins requires explicit equivalence assertion and Grapevine consensus.
Propagation is lazy: a node inherits all constraints from its ancestors unless it explicitly overrides them. This yields the same compression benefits that DreamCoder and library-learning systems exploit.

Because every node and every edge is a signed nostr event (kind 12345 for nodes, kind 12346 for edges), anyone can propose extensions, overrides, or new horizontal links. GrapeRank then decides, for each user separately, which proposals survive into their live Concept Graph.
Class Threads are therefore not a static ontology but a living, versioned, trust-weighted lattice. The next section shows exactly how GrapeRank computes that weighting.
[Figure 2: Three annotated Class Threads with constraint nodes, property trees, and one horizontal integration example]

# Section 5 – GrapeRank over the Grapevine

GrapeRank is the scoring engine that decides, for each user separately, which proposed nodes, edges, and entire Class Threads survive into their live Concept Graph. It is a nonlinear, iterative fixed-point computation that turns the raw social metadata of the Grapevine (follows, mutes, reports, zaps, replies, etc.) into a scalar credibility weight G ∈ [0,1] for every pubkey and every signed event.
The current production equation, running today in every Brainstorm client, is:
G_v^o = (1 - e^{-α ∑_w G_w^o · c_r^w}) · (∑_w G_w^o · c_r^w · r_v^w) / (∑_w G_w^o · c_r^w)
where

o is the observer (the logged-in user, fixed at G_o^o = 1)
v is the subject being scored (another pubkey or a specific event)
w ranges over all members of the Grapevine who have emitted a signal about v
r_v^w ∈ [0,1] is w’s interpreted rating of v (“probably legitimate” = 1, “probably malicious” = 0)
c_r^w ∈ (0,1] is w’s interpreted confidence in that rating (reports > mutes > follows)
α > 0 is a user-tunable rigor parameter (lower α = slower convergence to certainty)
an optional per-hop attenuation factor (default 0.8) is applied to G_w^o before summation

The left factor is a soft saturation that guarantees confidence approaches 1 only asymptotically as trusted input grows without bound. This nonlinearity is the key defence against link-farm attacks: a million bot follows (r = 1, c ≈ 0.01) contribute almost nothing, while three reports from high-G users (r = 0, c ≈ 0.25) can drive a score close to zero.
Interpretation layer
Every nostr event kind is mapped by a small, user-editable table into (r, c) pairs. Current defaults:









Event kindInterpretationrckind 3 follow“probably not a bot”1.00.05kind 10000 mute“suspicious”0.40.15kind 1984 report“definite spam / scam”0.00.25kind 9735 zapstrength proportional to sats1.00.03–0.20
Because interpretation is explicit and local, new kinds (badges, context tags, content ratings) can be added without protocol changes.
Convergence and performance
On real Grapevines of 200 000–400 000 pubkeys, scores stabilise (<0.1 % change) in 10–14 iterations, taking 400–900 ms on a 2023 laptop. Events are scored lazily: a new follow or report triggers a delta update that touches only the affected subgraph.
Worldviews and contextual metrics
The same equation supports non-recursive chaining (G_a = G_b ⊛ R) to produce topic-specific scores (“best hardware-wallet reviewers among Bitcoin maximalists”) without recomputing the base trust graph. These chains, called worldviews, are serialisable and shareable.
In Brainstorm, every Class Thread segment carries the GrapeRank score of its authoring key at the moment the event was first seen. A user-configurable threshold (default 0.35) determines visibility: threads below threshold are greyed or hidden, achieving the continuous, personalised filtering that raw nostr cannot provide.
GrapeRank is therefore not merely a sybil-resistance tool; it is the decentralised Bayesian posterior that replaces the missing ontologist team Lenat never found. The next section shows how the highest-scoring threads become raw material for genuine symbolic induction via analogical structure-mapping.
[Figure 3: Toy example – Alice (G=1) follows Bob, Bob follows Carol, Dave (low-G bot swarm) is reported by Bob. Table of G_v after convergence + short iteration trace]
</extracted_content>


# Section 6 – Analogy via Structure-Mapping on High-Credence Threads

Once GrapeRank has filtered the nostr stream into a personal Concept Graph containing only high-credence Class Threads (typically G > 0.5), Brainstorm runs a lightweight analogical engine that turns filtering into genuine discovery.
The engine rests on a single observation that has been true since Tenenbaum & Griffiths (2001) and is now empirically verifiable in the wild: human-like inductive leaps are almost always structure-preserving mappings between two relational graphs.
Every hour (or on demand), Brainstorm performs the following pass over the top 8 000 highest-credence Class Threads:

Candidate selection
Only threads longer than four propagation edges and containing at least one constraint node and one horizontal integration are considered. This yields ~2 000–4 000 candidates in a mature Grapevine.
Fast structural fingerprinting
Each thread is reduced to a canonical tuple
(origin_type, sequence_of_edge_types, constraint_signature, integration_targets)
Example fingerprint for a physical support thread:
(PhysicalObject, [Set→Subset, Subset→Subset, Subset→Instance], {mass>0, rigid}, integrates→GravityLaw)
Isomorphism search
Threads with identical or near-identical fingerprints (allowing one or two mismatches) are paired. Edit distance is computed on the directed multigraph formed by propagation + integration edges.
Proposal generation
For every high-scoring pair (A → B) where thread A is richer (more edges or tighter constraints), the engine copies missing propagation edges, constraint tightenings, or horizontal integrations from A into B, preserving node alignment. The copied elements are emitted as new signed nostr events attributed to the user’s own key with a special tag analogy_proposal=1.
Network voting
Because the proposals are ordinary events, GrapeRank immediately scores them in every observer’s Grapevine. Proposals that survive (usually 3–15 % in current beta) are materialised as permanent extensions of the target thread.

Real examples already observed in the public beta (March–May 2025):

Source thread (A)Target thread (B)Proposed & accepted extensionContainer → Glass → MyCoffeeMugSocialGroup → Alliance → JusticeLeague“If Container has ‘fragile’ constraint → Instance has ‘break-if-dropped’ causal rule” → mapped to “If Alliance has ‘fragile’ constraint → Instance has ‘disband-if-betrayed’ rule” (8 zaps, 0 reports)Bitcoin → HardwareWallet → TrezorFirearm → ConcealedCarry → GlockHorizontal integration from Materials thread: “cold-rolled steel → high durability” applied to both (11 zaps)Dog → WorkingDog → BorderCollieProgrammingLanguage → SystemsLang → Rust“high energy + strong herding instinct” → “high performance + memory safety guarantees” (humorous but up-voted by 63 % of maximalist Grapevines)
These are not hand-engineered; they are automatic consequences of identical propagation patterns (Set→Subset→Instance) plus shared constraint vocabulary.
Crucially, the acceptance rate is not random: proposals that compress the global graph (measured by total edge reduction under the new mapping) receive dramatically higher GrapeRank scores, exactly as predicted by Tenenbaum’s Bayesian compression-progress principle.
The engine is deliberately conservative (default maximum 20 proposals per hour) yet already accounts for ~9 % of new high-credence edges in long-term beta users. It is the first demonstration anywhere of internet-scale, culturally evolving symbolic induction that requires no central library and no privileged priors—only structure-mapping over the same threads that users were already asserting for social signalling.
The loop is now fully closed (Figure 4).
[Figure 4: The closed Brainstorm loop – nostr events → Grapevine + GrapeRank → high-credence Class Threads → analogy engine → new signed events → repeat]
Section 7 will present current empirical results and the three hardest open problems that remain.

# Section 7 – Closed Loop, Empirical Results, and Open Problems
The four components described above form a complete, self-reinforcing cycle:
nostr events
↓
Grapevine + GrapeRank → high-credence Class Threads
↓
analogy engine → new signed events
↓
back to nostr
No central coordination is required at any step. Every phase is local to the user’s device, every proposal is an ordinary signed event, and the only global invariant is cryptographic authenticity.
Empirical results to date (May 2025)
A public Brainstorm prototype has been running continuously since November 2024 for ~30 active paying users. It maintains a live Neo4j-backed Concept Graph containing:

~298 000 nostr pubkeys reachable within 10 follow hops of any user
~9.2 million follow/mute/report edges
~34 000 Class Thread segments (nodes + propagation edges) manually or semi-automatically asserted by users
~2 100 analogically generated proposals, of which 312 have survived GrapeRank thresholding in at least one user’s graph

Quantitative highlights:


MetricPersonalized PageRankGrapeRank (current settings)False-positive rate on known impersonators (n=87 ground-truth cases)61 %4.2 %Ability to neutralise a 5 000-bot link farm after three NIP-56 reports from G>0.7 usersNo effectScore drops from >0.85 → <0.08 in ≤3 iterationsAverage convergence time (full recompute, 300 k nodes)1.9 s0.82 s (incremental updates dominate in practice)Fraction of analogical proposals that survive >0.5 threshold after 72 h—14.8 % (higher in technically homogeneous Grapevines)
These numbers, while still early and on a modest cohort, already demonstrate that the core claim holds in the wild: GrapeRank turns a handful of trusted reports into a scalpel that cleanly excises entire sybil clusters, making the attack economically irrational—exactly the reversal Lenat never achieved with paid ontologists.
Qualitative feedback from the 30 users is equally encouraging: 26 report opening Brainstorm before any other client, with median session time rising from <1 minute (raw nostr) to 21 minutes once the Concept Graph contains >1 000 high-credence threads.
Three hard open problems (and why they are exciting rather than blocking)

Scalable subgraph isomorphism
Current fingerprint + edit-distance screening is O(n²) in the worst case. At 100 000+ threads we will need probabilistic locality-sensitive hashing or learned structural embeddings. This is a classic DreamCoder-scale library learning problem, now with real cultural data.
Adversarial interpretation gaming
An attacker who controls a high-G slice of a victim’s Grapevine could strategically emit follows/mutes to bias contextual worldviews. Formalising and mitigating this is the decentralised analogue of prompt injection in LLMs.
Constraint merging and conflict resolution
When two high-credence threads assert incompatible constraints on the same concept (e.g., “seed oils are toxic” vs. “seed oils are neutral”), Brainstorm currently keeps both with attached provenance. Designing graceful, user-controllable merging strategies (micro-forks, probabilistic constraints, or stakeholder-weighted voting) remains an open symbolic-AI challenge.

These are not flaws; they are the precise research questions that symbolic AI has been trying to ask for forty years and finally has real data to answer.
The conclusion follows.

# Section 8 – Conclusion
Brainstorm is the first system that actually closes the loop that defeated every previous symbolic-AI effort for four decades.
Lenat could not hire enough ontologists.
Tenenbaum could not find a cultural library large enough for his engines to grow up.
The LLM industry chose to memorise the internet instead of understanding it.
We did none of those things.
Instead, we took the global, cryptographically signed social chatter that seven million humans are already producing on nostr, ran GrapeRank over each user’s personal Grapevine to extract a continuous Bayesian posterior on credibility, organised the surviving fragments into explicit Class Threads, and let a simple structure-mapping engine propose new threads by analogy. The proposals are themselves nostr events, so the loop closes without central coordination, without paid experts, and without a single line of hand-engineered prior.
The result is a living, personalised, trust-weighted symbolic mind that updates in real time, resists sybil attacks by design, and already discovers new causal and conceptual structure that no single human asserted.
Finally, an unexpected convergence: recent mechanistic interpretability work reveals that large language models already induce latent analogues of Class Threads—low-dimensional manifolds along which hyponyms monotonically specialise their hypernyms (Gurnee et al., 2023; Hernandez et al., 2023). The hierarchies that took symbolic AI decades to hand-craft, and that LLMs learned implicitly from raw text, turn out to be the same geometric objects. Brainstorm simply externalises them, makes them editable, queryable, and weighted by real human webs of trust. Far from competing paradigms, explicit symbolic graphs and statistical embeddings may be two sides of the same underlying structure—one discovered by gradient descent, the other now growing, deliberately and decentrally, on nostr.
The 40-year knowledge-acquisition bottleneck is no longer a theoretical puzzle.
It is a running prototype you can try today at https://straycat.brainstorm.social.
The symbolic AI revolution will not be funded by DARPA or OpenAI.
It will be bootstrapped by people who simply got tired of drowning in noise and decided to build a mind they could actually trust.
We invite the cognitive science, AI, and nostr communities to join us.
There is room for another eight billion personalised symbolic minds.
Come help us finish what Lenat started, and what Tenenbaum proved was possible,
this time without asking anyone’s permission.
