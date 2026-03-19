# History

The arc from the QBBN (2024) to Transformers are Bayesian
Networks (2026).

## December 2024: The QBBN Implementation

The world repo was the original implementation of the
Quantified Boolean Bayesian Network — a unified model
for logical and probabilistic reasoning introduced in
arXiv:2402.06557.

The implementation ran a full NLP pipeline: tokenization,
clause extraction, argument extraction, coreference
resolution, entity recognition, knowledge base linking,
logical form generation, grounding, and belief propagation.
All in Python with Redis storage.

The key ideas already present: the AND/OR bipartite factor
graph, the boolean variable nodes, the updateBelief function,
the grounding mechanism, the iterative BP inference.

The limitation: it was an implementation, not a proof. The
connection between the QBBN structure and the transformer
architecture was noticed but not formalized.

## Early 2025: The Formal Program Begins

The observation that the transformer's attention-FFN
alternation matched the QBBN's AND/OR structure exactly
led to the formal research program. If the transformer is
already implementing the QBBN computation — not as an
approximation but exactly — then the transformer is a
Bayesian network by architecture.

The Lean 4 proof development began. transformer-bp-lean
proved the constructive result: there exist weights such
that the transformer implements exact BP. hard-bp-lean
formalized Pearl's 1988 result that BP is exact on trees.

## Mid 2025: The General Result

sigmoid-transformer-lean proved the general result: every
sigmoid transformer with any weights implements weighted
loopy BP on its implicit factor graph. Not just constructed
weights — any weights. The implicit factor graph exists for
any transformer and is well-defined.

The uniqueness theorem followed: exact posteriors force BP
weights uniquely. There is no other path through the sigmoid
architecture to exact inference.

## Late 2025: Experimental Confirmation

bayes-learner confirmed empirically that gradient descent
finds BP weights from scratch: val MAE 0.000752 on a
two-variable factor graph, 99.3% improvement over baseline.

learner confirmed that gradient descent finds TM simulation
weights from scratch: 100% accuracy on five structurally
distinct Turing machines, identical hyperparameters.

loopy confirmed that loopy BP converges on QBBN-structured
graphs empirically: 500 trials, all converge, near-exact
posteriors across five graph structures of increasing
complexity.

## Early 2026: The Paper and the Lower Bound

The shannon paper assembled all results into a unified
account. Five main theorems, all formally verified, all
empirically confirmed.

The pearl repo added a new result not in the original plan:
the Peirce lower bound. For any local message passing
algorithm on a chain of length N, N rounds are necessary.
Pearl gave the upper bound in 1988. Rumelhart et al.
described backprop in 1986. Neither proved the lower bound.
The pearl repo closed both gaps simultaneously with the
same proof — BP and backprop are instances of the same
abstract structure.

## The Current State

The formal theory is complete for the boolean case.
The mechanistic interpretability meta-analysis in the
interp repo established that nothing in the empirical
literature contradicts the BP account, and that the
apparent gap for function vector heads resolves into
a positive result: hybrid BP with boolean and continuous
nodes.

The next phase is the formal extension to hybrid factor
graphs and the empirical program of recovering the implicit
factor graph of production transformers via SAEs and
attribution graphs.

## What This Repo Was

The world repo was the starting point — an implementation
that embodied the key ideas before they were proved. It
has been superseded by the formal proofs and empirical
repos. It now serves as an index into the current program
for anyone following citations to the original QBBN work.

The ideas that were implemented here — AND/OR factor graphs,
grounding, belief propagation, the updateBelief function —
are now formally proved and empirically confirmed at scale.
The implementation was right. The proof came later.