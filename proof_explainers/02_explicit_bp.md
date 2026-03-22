# 02 — explicit weights for exact BP

## the theorem

There exist explicit weight matrices W such that for any
BP state:

    decodeTFState(transformerForwardPass(W, encodeBPState(s)))
        = bp_forwardPass(s)

One forward pass with these weights implements one round
of exact belief propagation on any pairwise factor graph.

Formally verified in Lean 4 as transformer_implements_bp
in transformer-bp-lean.

## what it says

The general result (01) shows that every transformer is
doing some kind of BP. This result goes further: it exhibits
the specific weights that make the transformer implement
exact BP on a declared knowledge base.

The weights are written down explicitly. The proof is
constructive — not merely an existence argument. You could
implement these weights in PyTorch today and the transformer
would compute exact Bayesian posteriors on any pairwise
factor graph you declare.

## the encoding

Each factor graph node is encoded as one token with
D_model = 8 dimensions, split as follows:

    dim 0       own belief (initialized to 0.5)
    dims 1–4    factor table entries [f00, f01, f10, f11]
    dim 5       node type (0 = variable, 1 = factor)
    dim 6       own index / (n-1)
    dim 7       neighbor index / (n-1)

Dims 5–7 are the routing key — the complete inferential
identity of the token. Two tokens with the same routing
key are indistinguishable to the attention mechanism
regardless of their belief values or factor table entries.
Dims 0–4 are the continuous parameters — the specific
values the inference computes over.

## the weight construction

Two weight families appear in both this proof and the
Turing completeness proof. This is not a coincidence —
both use attention for the same purpose: routing
information between tokens by index matching.

projectDim(d) — a diagonal projection extracting dimension d.
As a Q or K weight, the attention score peaks when token k's
stored index matches token j's query value. This is the
focused lookup that implements the gather step.

crossProject(s, d) — an off-diagonal projection reading
source dimension s and writing to destination dimension d.
As a V weight, this routes a token's dimension-s content
into dimension d of the output, leaving all other
dimensions untouched.

Head 0 uses projectDim(1) for Q and K, crossProject(0, 4)
for V: the Q/K dot product peaks when token k is neighbor 0,
placing neighbor 0's belief into dim 4 of the residual
stream. Head 1 is symmetric, writing neighbor 1's belief
into dim 5.

The FFN with sigmoid activation then computes
updateBelief(m₀, m₁) = σ(logit(m₀) + logit(m₁)) exactly
from dims 4 and 5, writing the result to dim 0.

## the two-parent assumption is without loss of generality

The construction assumes pairwise factors. This is not
a fundamental restriction — see 06_and_or_decomposition.md
for the proof that any k-ary factor graph binarizes exactly.

The consequence: two attention heads per layer suffices for
BP on any factor graph regardless of arity. Width is fixed.
Only depth grows with reasoning complexity.

## the tree corollary

Combined with the result that BP is exact on trees
(03_bp_exact_on_trees.md):

For any tree-structured factor graph T and T ≥ diameter(T)
forward passes, the transformer with these weights computes
exact marginal posteriors at every node. No empirical
assumptions. No conditions beyond tree structure.

This is the no-hallucination guarantee. A transformer with
BP weights running over a fully grounded tree-structured
knowledge base cannot hallucinate.

## Lean source

    transformer-bp-lean/
        TransformerBP.lean
            transformer_implements_bp
        Weights.lean
            projectDim
            crossProject

## see also

01_general_bp.md — the general result of which this is
the constructive special case.

03_bp_exact_on_trees.md — the tree exactness result that
combines with this to give the no-hallucination corollary.

06_and_or_decomposition.md — why the two-parent assumption
is without loss of generality.