# 06 — k-ary AND/OR is without loss of generality pairwise

## the theorems

Any k-ary conjunction reduces exactly to a chain of
⌈log₂ k⌉ binary conjunctions via associativity of AND.

Any k-ary disjunction reduces exactly to a chain of
⌈log₂ k⌉ pairwise log-odds updates via additivity
of the Turing-Good weight-of-evidence combination.

Both decompositions are exact — no approximation,
no information loss.

Formally verified in Lean 4 as:
    and_decomposes_to_binary in godel/ANDDecomposition.lean
    or_decomposes_to_binary  in godel/ORDecomposition.lean

## what it says

The BP construction in result 02 assumes pairwise factors
— each factor node connects exactly two variable nodes,
handled by exactly two attention heads per layer.

Real knowledge bases may have factors of higher arity.
"A date requires mutual liking AND a shared interest AND
compatible schedules" is a three-way conjunction.

This result says that restriction to pairwise factors is
without loss of generality. Any k-ary factor graph can be
binarized exactly, producing a graph of greater depth but
no greater width. Two attention heads per layer therefore
suffices for belief propagation on any factor graph.

## the AND decomposition

Boolean AND is associative:
    A AND B AND C = (A AND B) AND C

This associativity is exact. No approximation is involved.
A three-way conjunction becomes two binary conjunctions
with one intermediate node. A k-way conjunction becomes
⌈log₂ k⌉ binary conjunctions in a balanced tree.

The Lean proof verifies that this decomposition preserves
the exact boolean semantics — the intermediate nodes
compute exactly the partial conjunctions, and the final
node computes the full k-way conjunction.

## the OR decomposition

Log-odds addition is associative by the associativity
of addition in ℝ:
    logit(m₀) + logit(m₁) + logit(m₂)
        = (logit(m₀) + logit(m₁)) + logit(m₂)

Because logit and sigmoid are exact inverses, this means:
    updateBelief(m₀, m₁, m₂)
        = updateBelief(updateBelief(m₀, m₁), m₂)

A three-way disjunction chains as two binary disjunctions.
A k-way disjunction chains as ⌈log₂ k⌉ binary disjunctions.
The decomposition is exact because log-odds addition is
exactly associative.

## the architectural consequence

The binarized graph has maximum factor arity 2 and depth
d · ⌈log₂ k⌉ where d is the original depth and k is the
maximum arity. By result 02, one layer implements one round
of BP on a pairwise graph. Therefore d · ⌈log₂ k⌉ layers
implement full BP on the binarized graph, which is exactly
BP on the original graph.

Two attention heads per layer. Always. More complex
reasoning requires more layers, not more heads. The number
of heads is determined by the pairwise structure of binary
factors. The number of layers is determined by the depth
of the reasoning chain.

This is the counterintuitive architectural fact: width
is fixed by the binary decomposition, depth grows with
reasoning complexity.

## Lean source

    godel/
        ANDDecomposition.lean
            and_decomposes_to_binary
        ORDecomposition.lean
            or_decomposes_to_binary

## see also

02_explicit_bp.md — the construction that uses this result
to handle arbitrary-arity factor graphs.

05_finite_concept_space.md — the routing classes finite
result, which counts concepts in the binarized graph.