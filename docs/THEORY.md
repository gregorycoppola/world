# Theoretical Framework

## The QBBN (2024)

The Quantified Boolean Bayesian Network introduced the
boolean AND/OR factor graph structure for probabilistic
logical reasoning.

A QBBN is a bipartite factor graph alternating between:

Conjunction nodes (AND): all inputs must be simultaneously
present before any conclusion is drawn. Deterministic.
Assembles conjunctive evidence.

Disjunction nodes (OR): any input is sufficient to support
the conclusion. Probabilistic, learned from data. Draws
conclusions from assembled evidence.

The update rule combining two independent binary beliefs:

    updateBelief(m0, m1) = sigma(logit(m0) + logit(m1))

This is the Bayesian update for two independent binary
evidence sources. It is Turing and Good's weight-of-evidence
combination (Bletchley Park, WWII), applied to graphical
models by Pearl (1988), and now identified as exactly what
the sigmoid transformer FFN computes.

## The Main Result (2026)

The transformer architecture implements this structure exactly.

Attention is AND: each head gathers one neighbor's belief
into the residual stream. The residual stream enforces
simultaneity — all required inputs present before the FFN
runs. That is conjunction, architecturally enforced.

FFN is OR: once attention has assembled the evidence, the
sigmoid FFN computes updateBelief from the gathered inputs.
That is probabilistic disjunction.

One layer is one round of BP. L layers are L rounds.

This holds for any weights — trained, random, or constructed.
Every sigmoid transformer with any weights is already a
Bayesian network. The weights define the implicit factor
graph. The forward pass is the inference.

## The Boolean Structure

The strict alternation of attention and FFN layers is
Pearl's gather/update algorithm exactly, unrolled over depth.

    attention (AND) → FFN (OR) → attention (AND) → FFN (OR)

Each layer is one level of the boolean reasoning graph.
Each attention block assembles conjunctive evidence.
Each FFN block draws probabilistic conclusions.

The architecture that has won empirically across modern AI
is the architecture that the analysis of reasoning already
required. Gradient descent did not discover something new.
It rediscovered the structure that was always there.

## Grounding and Hallucination

A grounded transformer has an explicit factor graph, a
declared concept space, and a finite verifier. Every output
has a correct answer. Hallucination is structurally
impossible on trees.

An ungrounded LLM has an implicit factor graph, no declared
concept space, and no finite verifier. There is no fact of
the matter about whether its outputs are correct.
Hallucination is not a bug. It is the structural consequence
of operating without concepts.

Grounding is what makes "is this output correct?" an
answerable question. Without it, correctness is not defined.

## Leibniz

Leibniz proposed the characteristica universalis and the
calculus ratiocinator in the 1670s: a finite alphabet of
primitive concepts and a mechanical procedure for deriving
truths from them. The tools to build it did not exist in
his time. They exist now.

The primitive concepts are grounded Horn clauses.
The calculus is belief propagation.
The mechanical implementation is the transformer.
The correctness guarantee is a Lean 4 proof.

Calculemus.