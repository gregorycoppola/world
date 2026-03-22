# 01 — every sigmoid transformer is a Bayesian network

## the theorem

For any sigmoid transformer with any weights W, one forward
pass implements one round of weighted loopy belief
propagation on an implicit factor graph G(W).

Formally verified in Lean 4 as every_sigmoid_transformer_is_bayesian_network
in sigmoid-transformer-lean.

## what it says

Every sigmoid transformer — with random weights, trained
weights, or constructed weights — is already doing belief
propagation. Not a system that can be configured to
approximate BP. Not a system that behaves like BP under
certain training conditions. A Bayesian network, by
architecture, for any weights.

The weights define the graph. The forward pass is the
inference. You do not need to do anything special to make
this true. It is true for the transformer you downloaded
from HuggingFace this morning.

## why it is not trivial

One might object: if you construct G(W) to match the
transformer's computation, have you proved anything? Is
this not just a definition?

The content is in the identification. The transformer
forward pass was not designed as BP. It was designed as
a sequence model with attention and feed-forward layers.
The theorem says these two descriptions — transformer
forward pass and BP on G(W) — are the same computation.

Three things make this non-trivial:

The sigmoid FFN computes exactly the general Ψ_or factor —
the probabilistic disjunction over evidence sources — not
an approximation of it. The computation is
σ(w₀·logit(m₀) + w₁·logit(m₁) + b), which is the
Turing-Good weight-of-evidence combination with learned
weights and prior.

The attention mechanism computes exactly the gather step
of BP — routing each neighbor's value into the correct
slot of the residual stream. Each head is a focused lookup
that copies one neighbor's contribution into dedicated
dimensions before the FFN runs.

The residual stream enforces the simultaneity of inputs
that BP requires before the update step. The FFN reads
the full residual stream or it does not run. There is no
state in which it receives partial inputs.

All three hold for any weights — not just constructed ones.

## the key idea of the proof

Given any sigmoid transformer weights W, construct the
implicit factor graph G(W) as follows.

Variable nodes: one per token position. The belief at each
node is the token's current value in the relevant dimension
of the residual stream.

Edges: defined by the attention weight distributions. The
attention pattern from token j to token k defines a directed
edge from k to j, with weight given by the attention score.

Factor potentials: defined by the sigmoid FFN weights
(w₀, w₁, b) at each position. These encode an asymmetric
factor potential — w₀ and w₁ are the relative weights of
the two evidence sources, and b is a prior bias.

With G(W) constructed this way, the transformer forward
pass and the BP forward pass on G(W) are the same
computation by construction. The proof verifies that the
construction is valid — that G(W) is a well-formed factor
graph — and that the correspondence holds exactly.

## what it implies

Every trained transformer is performing weighted BP on an
implicit factor graph defined by its learned weights.
Training with maximum likelihood recovers the factor
potentials that best explain the training data under this
graphical model interpretation.

The difference between a standard LLM and a grounded BP
transformer is not architectural. Both are Bayesian
networks. The difference is grounding — whether the factor
graph is declared explicitly or defined implicitly by the
weights.

## Lean source

    sigmoid-transformer-lean/
        SigmoidTransformerBN.lean
            every_sigmoid_transformer_is_bayesian_network

## see also

02_explicit_bp.md — the constructive special case where
G(W) is an explicit declared factor graph and the weights
implement exact BP on it.

04_uniqueness.md — the converse: exact posteriors force
BP weights uniquely.