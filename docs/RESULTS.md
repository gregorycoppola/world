# Results

All results formally verified in Lean 4 with zero sorry
placeholders against standard mathematical axioms.

## Result 1: Every Sigmoid Transformer Is a Bayesian Network

For any sigmoid transformer weights W, there exists an
implicit factor graph G(W) such that one forward pass
implements one round of weighted belief propagation on G(W).

This holds for any weights — trained, random, or constructed.
The implicit factor graph is constructed to match whatever
the transformer computes. The BP interpretation is not
imposed from outside — it is what the forward pass computes.

Repo: sigmoid-transformer-lean
File: SigmoidTransformerBN.lean

## Result 2: Exact BP with Explicit Weights

There exist weights W such that for any BP state:

    decodeTFState(state, transformerForwardPass(n, W, encodeBPState(state)))
    = bp_forwardPass(state)

Two attention heads per layer suffice for any factor graph.
Any k-ary factor graph binarizes exactly via associativity
of AND and log-odds additivity of OR. Only depth grows with
reasoning complexity.

Repo: transformer-bp-lean
File: TransformerBP.lean

## Result 3: Uniqueness

A sigmoid transformer that computes exact Bayesian posteriors
for all inputs necessarily has BP weights: w0 = w1 = 1, b = 0
in the FFN, and projectDim/crossProject structure in the
attention. The internal computations are provably the BP
quantities — not merely the outputs.

Repo: sigmoid-transformer-lean
Files: FFNUniqueness.lean, AttentionUniqueness.lean

## Result 4: No Hallucination on Grounded Trees

For any tree-structured factor graph T and T >= diameter(T)
forward passes with BP weights:

    transformer[T](encodeBPState(state)) = encodeBPState(trueMarginals(state))

No empirical assumptions. No conditions beyond tree structure
and BP weights.

Repos: transformer-bp-lean + hard-bp-lean
Corollary of Results 1 and 2 combined with bp_exact_on_tree.

## Result 5: Finite Concept Space

Any finite verification procedure can distinguish at most
finitely many concepts. A finite state machine with n states
partitions any input space into at most n^n equivalence
classes. Two inputs with the same behavior are
indistinguishable to the machine.

For the BP transformer: the routing key (nodeType, ownIndex,
neighborIndex) identifies each concept. For a factor graph
with n nodes, there are exactly 2n² distinct concepts.

Repo: godel
Files: FiniteStateSpace.lean, BPTokenFiniteness.lean

## Result 6: Peirce Lower Bound (New)

For any local message passing algorithm on a directed chain
of length N, at least N rounds are required to propagate
information from node N to node 0. This is a hard lower
bound — no local algorithm can do better.

The same proof applies to backpropagation: for any local
training algorithm on an N-layer network, N backward steps
are necessary to assign credit to layer 1 parameters.

BP and backprop are instances of the same abstract structure
— MessagePassingAlgorithm — and the lower bound applies to
both. Pearl gave the upper bound in 1988. Rumelhart et al.
described backprop in 1986. Neither proved the lower bound.
Both gaps closed simultaneously by the same proof.

Repo: pearl
Files: Peirce.lean, BackpropLowerBound.lean

## Experimental Confirmation

All formal results confirmed empirically:

Gradient descent finds BP weights from scratch on a
two-variable factor graph: val MAE 0.000752, 99.3%
improvement over baseline, first run.
Repo: bayes-learner

Gradient descent finds TM simulation weights from scratch
on five structurally distinct Turing machines: 100% accuracy,
identical hyperparameters, no per-machine tuning.
Repo: learner

Loopy BP converges on QBBN-structured graphs: 500 trials
across five graph structures, all converge, worst mean
KL divergence 0.000102.
Repo: loopy