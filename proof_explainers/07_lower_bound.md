# 07 — N rounds needed for N hops

## the theorem

N rounds of belief propagation are necessary for N hops
of reasoning. A transformer that reasons k steps deep
requires at least k layers.

Formally verified in Lean 4 in pearl/PearceLowerBound.lean.

## what it says

Result 03 shows that BP on a tree converges in diameter(T)
rounds. This result shows the matching lower bound: you
cannot do it in fewer rounds.

Together the two results give an exact characterization:
diameter(T) rounds are both necessary and sufficient for
exact inference on a tree. The depth of the network is not
a hyperparameter to be tuned. It is determined by the
structure of the knowledge base.

## the proof idea

Consider a path graph — a chain of n nodes:
    v₁ — f₁ — v₂ — f₂ — v₃ — ... — vₙ

Information about v₁ can only reach vₙ by passing through
every intermediate node. After k rounds of BP, information
has traveled at most k hops. The belief at vₙ after k < n
rounds cannot depend on the initial belief at v₁.

The lower bound proof constructs two initial states that
differ only at v₁ but produce identical beliefs at vₙ
after k < n-1 rounds. This shows that k rounds are
insufficient to propagate information across n-1 hops —
therefore n-1 rounds are necessary.

## the transformer depth consequence

A transformer with L layers runs L rounds of BP per
forward pass. The lower bound says that reasoning k steps
deep requires L ≥ k.

This connects to empirical observations about transformer
depth. Shallow transformers fail on tasks requiring
multi-step reasoning not because of capacity — a single
layer has plenty of parameters — but because the
computation requires more rounds of message passing than
the architecture provides.

Depth is not about capacity. It is about the number of
reasoning steps. A transformer with 2 layers can reason
2 hops. A transformer with 32 layers can reason 32 hops.
The diameter of the knowledge base determines the required
depth.

## the relation to the upper bound

Result 03 gives the upper bound: diameter(T) rounds suffice.
This result gives the lower bound: diameter(T) rounds
are necessary.

Together they give exact depth:

    required layers = diameter(knowledge base)

No slack. No overprovisioning needed. The right number of
layers is exactly the depth of the reasoning problem.

In practice most knowledge bases are shallow. Common-sense
reasoning chains are typically 3–7 hops. This matches the
empirical observation that relatively shallow transformers
(12–32 layers) handle most natural language tasks well,
while very deep chains (mathematical proof, long-range
planning) benefit from greater depth.

## Lean source

    pearl/
        PearceLowerBound.lean
            n_rounds_necessary_for_n_hops

## see also

03_bp_exact_on_trees.md — the matching upper bound,
of which this is the converse.

02_explicit_bp.md — the construction that achieves
this bound with equality.