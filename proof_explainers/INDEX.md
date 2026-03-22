# proof explainers

Plain-English explanations of each formal result in the
Transformers are Bayesian Networks research program.
Each proof is machine-verified in Lean 4 against standard
mathematical axioms with zero sorry placeholders.

## the results, in order

### 01 — every sigmoid transformer is a Bayesian network

The general result. For any sigmoid transformer with any
weights, one forward pass implements one round of weighted
loopy belief propagation on an implicit factor graph defined
by those weights. No conditions on the weights. No training
required. The architecture is BP, by construction.

Lean repo: sigmoid-transformer-lean
Explainer: 01_general_bp.md

### 02 — explicit weights for exact BP

The constructive result. There exist explicit weight matrices
such that the transformer implements exact belief propagation
on any declared factor graph. For tree-structured knowledge
bases this yields provably correct posteriors at every node.

Lean repo: transformer-bp-lean
Explainer: 02_explicit_bp.md

### 03 — BP is exact on trees

The classical Pearl result, formalized in Lean. On a
tree-structured factor graph, belief propagation converges
in exactly diameter(T) rounds to the exact marginal
posteriors. This is the no-hallucination guarantee —
combined with result 02, it gives exact inference on
grounded tree-structured knowledge bases.

Lean repo: hard-bp-lean
Explainer: 03_bp_exact_on_trees.md

### 04 — uniqueness

The converse result. A sigmoid transformer that produces
exact Bayesian posteriors for all inputs necessarily has
BP weights: w₀=w₁=1, b=0 in the FFN, and projectDim /
crossProject structure in the attention. There is no other
path through the sigmoid architecture to exact posteriors.
The weights are forced.

Lean repo: sigmoid-transformer-lean
Explainer: 04_uniqueness.md

### 05 — verifiable inference requires a finite concept space

A finite state machine with n states can distinguish at most
n^n distinct input behaviors. Any finite verification
procedure implies a finite concept space. An ungrounded
language model has no finite verifier, therefore no
well-defined concept space, therefore no fact of the matter
about whether its outputs are correct. Hallucination is not
a bug scaling can fix. It is the structural consequence of
operating without concepts.

Lean repo: godel
Explainer: 05_finite_concept_space.md

### 06 — k-ary AND/OR is without loss of generality pairwise

Any k-ary conjunction reduces exactly to a chain of binary
conjunctions via associativity of AND. Any k-ary disjunction
reduces exactly to a chain of binary updates via additivity
of log-odds. Two attention heads per layer therefore suffices
for belief propagation on any factor graph regardless of
arity. Only depth grows with reasoning complexity.

Lean repo: godel
Explainer: 06_and_or_decomposition.md

### 07 — N rounds needed for N hops

The lower bound result. N rounds of belief propagation are
necessary for N hops of reasoning. This gives the transformer
depth requirement: a transformer that reasons k steps deep
needs at least k layers. Depth is not an engineering
hyperparameter — it is determined by the structure of the
knowledge base.

Lean repo: pearl
Explainer: 07_lower_bound.md

## reading order

For someone coming from ML:
    01 → 02 → 03 → 04

For someone coming from logic:
    05 → 06 → 01 → 02

For someone who wants the full picture:
    01 → 02 → 03 → 04 → 05 → 06 → 07

## relationship to the paper

These explainers follow the structure of the paper
(shannon repo) but are written for human readers rather
than for Lean. The goal is to make each result understandable
without reading the formal proof — while being precise enough
that a reader could find the corresponding theorem in the
Lean source.

## contact

Greg Coppola — greg@coppola.ai — coppola.ai
transformersarebayesnets.com