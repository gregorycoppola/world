# 03 — BP is exact on trees

## the theorem

For any tree-structured factor graph T and any initial
state, belief propagation converges in exactly diameter(T)
rounds to the exact marginal posteriors at every node.

Formally verified in Lean 4 as bp_exact_on_tree
in hard-bp-lean.

## what it says

Belief propagation is an approximate algorithm in general.
On loopy graphs it may not converge, and when it does the
result approximates but does not equal the true posteriors.

On trees it is exact. Not approximately exact. Not exact
in the limit. Exactly equal to the true marginal posteriors,
in exactly diameter(T) rounds, with no conditions on the
factor potentials or the initial beliefs.

This is Pearl's classical result from 1988, now machine-
verified in Lean 4 against standard mathematical axioms.

## why trees are special

On a tree, when node j gathers messages from its neighbors,
those messages come from disjoint subtrees. The subtrees
share no variables. The messages are therefore independent
by construction.

This is exactly the independence assumption that makes
the Turing-Good log-odds addition exact. On a tree the
independence is not an approximation — it is a structural
fact. The messages arriving at any node came from parts
of the graph that have never communicated with each other.

On a loopy graph, the same variable can influence a node
through multiple paths. The messages are no longer
independent. The log-odds addition is no longer exact.
BP on loopy graphs is an approximation because it applies
the independence assumption where independence does not hold.

## the proof idea

The proof proceeds by induction on the structure of the tree.

For a leaf node, the belief after one round equals the
exact marginal — there are no loops to introduce
correlation.

For an internal node, if all its children have computed
exact beliefs after k rounds, then after one more round
it computes an exact belief — because the messages from
its children are independent (they come from disjoint
subtrees) and the updateBelief function is exact for
independent evidence.

The base case and inductive step together give exactness
after diameter(T) rounds — the minimum number of rounds
for information to travel from every leaf to every node.

## the convergence rate

On a tree with n nodes, the diameter is at most n-1.
In practice knowledge bases tend to be shallow — a
reasoning chain of depth k requires a tree of depth k,
and k is typically small (single digits for most
practical inference tasks).

This gives the transformer depth requirement directly:
a transformer reasoning k steps deep needs at least k
layers. Depth is not an engineering hyperparameter.
It is determined by the diameter of the knowledge base.

## empirical confirmation

The bayes-learner repo confirms that gradient descent
finds BP weights from scratch on the two-variable factor
graph v₀ — f₁ — v₂ (a tree). Validation MAE 0.000752.
Posteriors matched to three decimal places on held-out
graphs. First run, no construction hints.

This confirms that the formal weight construction is not
merely theoretically possible — it is what gradient
descent finds given a clean training signal.

## Lean source

    hard-bp-lean/
        BPExactOnTree.lean
            bp_exact_on_tree
        UpdateBelief.lean
            updateBelief_pos
            updateBelief_neutral

## see also

02_explicit_bp.md — the construction that, combined with
this result, gives the no-hallucination corollary.

07_lower_bound.md — the matching lower bound: N rounds
are also necessary for N hops, not just sufficient.