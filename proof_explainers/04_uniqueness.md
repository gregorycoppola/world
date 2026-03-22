# 04 — uniqueness

## the theorem

A sigmoid transformer that computes exact Bayesian
posteriors for all inputs on a grounded factor graph
necessarily has BP weights: w₀ = w₁ = 1, b = 0 in the
FFN, and projectDim / crossProject structure in the
attention. The internal computations are provably the
BP quantities — not merely the outputs.

Formally verified in Lean 4 as uniqueness in
sigmoid-transformer-lean.

## what it says

Results 01 and 02 go forward: here are weights, here is
what they compute. This result goes backward: if a sigmoid
transformer produces exact Bayesian posteriors, what must
its weights be?

The answer is uniquely the BP weights. There is no other
path through the sigmoid architecture to exact posteriors.
The architecture does not merely permit the Bayesian
interpretation — for exact posteriors, it requires it.

## the proof: FFN uniqueness

The FFN computes σ(w₀·logit(m₀) + w₁·logit(m₁) + b).

The sigmoid function is injective — it maps each real
number to a unique value in (0,1). No two inputs to
sigmoid give the same output.

The exact posterior for independent evidence is
σ(logit(m₀) + logit(m₁)). For the FFN to match this
for all inputs m₀, m₁ ∈ (0,1), the argument to sigmoid
must equal logit(m₀) + logit(m₁) for all m₀, m₁.

The only linear function of logit(m₀) and logit(m₁)
that equals their sum for all inputs is the one with
w₀ = 1, w₁ = 1, b = 0. Any other values would produce
a function that agrees with the exact posterior on at
most a measure-zero set of inputs.

## the proof: attention uniqueness

For exact routing, the Q·K dot product must peak uniquely
at the correct neighbor for every input. This forces the
Q and K weight matrices to have projectDim structure —
rank-1, single-dimension peak — because any other
structure would produce ties or incorrect routing on
some inputs.

Similarly the V weight matrix must have crossProject
structure to route the correct value into the correct
scratch slot without contaminating other dimensions.

## what this closes

The three results together form a complete characterization:

General (01): every sigmoid transformer is doing weighted
BP on some factor graph.

Constructive (02): exact BP weights exist for any declared
factor graph.

Uniqueness (04): exact posteriors force those weights.

Together: a sigmoid transformer implements exact Bayesian
inference if and only if it has BP weights. The equivalence
goes in both directions. The Bayesian interpretation is not
imposed from outside — it is the unique characterization
of exact inference in the sigmoid architecture.

## what it means for trained transformers

Every sigmoid transformer trained on any task is performing
weighted BP on an implicit factor graph. The uniqueness
result says: if that training happened to produce exact
posteriors on some factor graph, the internal computations
are provably the BP quantities.

This is a stronger claim than output equivalence. The
internals match, not just the outputs. The transformer
is not finding some other way to produce the right answer
— the sigmoid architecture leaves no alternative route.

## Lean source

    sigmoid-transformer-lean/
        FFNUniqueness.lean
            ffn_uniqueness
        AttentionUniqueness.lean
            attention_uniqueness

## see also

01_general_bp.md — the forward direction of which this
is the converse.

02_explicit_bp.md — the explicit weights that this
result shows are the only solution.