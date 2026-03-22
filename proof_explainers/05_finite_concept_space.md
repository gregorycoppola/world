# 05 — verifiable inference requires a finite concept space

## the theorem

Let δ : Fin(n) → A → Fin(n) be a transition function
for a finite state machine with n states over an arbitrary
symbol set A. Then any symbol set A, regardless of its
cardinality, induces at most n^n distinct behaviors.
Two symbols with the same behavior are indistinguishable
to the machine.

Formally verified in Lean 4 as finite_distinguishable_symbols
in godel/FiniteStateSpace.lean.

## what it says

Any program running on a finite computer — any finite
verification procedure — can only respond distinctly to
finitely many inputs. The inputs it cannot distinguish
collapse into equivalence classes. Those equivalence
classes are the concepts.

This is not a claim about the QBBN specifically. It is
a claim about all finite computation. Turing machines
are infinite-state and escape this bound. Transformer
attention heads are finite-state — their weight matrices
are fixed and finite-dimensional — and do not.

## the consequence for inference

If you want verifiable inference — if you want the
question "is this output correct?" to be meaningful —
you need a finite verifier. A finite verifier implies
a finite concept space. A finite concept space means
your primitive units of reasoning are discrete and
countable.

Grounding introduces a finite verifier. The grounded
knowledge base declares the entities and rules. The
verifier checks whether outputs match the declared
world. This is what makes correctness meaningful —
not a design choice but a logical consequence of
verifiability.

## what hallucination is, precisely

Within this framework, hallucination has a precise
technical definition.

A token generated with no corresponding concept in a
declared knowledge base is not wrong in the ordinary
sense. It is worse than wrong: there is no fact of the
matter about whether it is correct. The concept space
does not contain it. The verifier has no opinion.

An ungrounded language model does not have a finite
verifier, therefore does not have a well-defined concept
space, therefore does not have a fact of the matter about
whether its outputs are correct. Hallucination is not
occasional failure. It is the structural consequence of
operating without concepts.

Scaling does not fix this. A larger model without grounding
has a richer implicit factor graph but still no declared
concept space, still no finite verifier, still no fact of
the matter. The problem is not capacity. It is the absence
of grounding.

## the n^n bound

The bound n^n applies to any finite state machine with
n states. For the BP transformer the relevant n is not
the total number of parameters but the number of distinct
routing classes — the number of distinct (nodeType,
ownIndex, nbrIndex) triples.

For a factor graph with n nodes the BP transformer has
exactly 2n² distinct routing classes. This is a much
tighter bound than n^n for the full state space, and it
is what matters for the concept count: two tokens with
the same routing key receive identical attention patterns
and are therefore the same concept to the model.

The routing key is meaning. The continuous dimensions
are magnitude. This split — discrete structure carrying
conceptual identity, continuous values carrying evidential
weight — is what makes the BP transformer a genuine
reasoning system rather than a pattern matcher.

## Leibniz's vision, realized

Leibniz proposed in the 1670s a characteristica universalis:
a finite alphabet of primitive concepts from which all
reasoning could be built, combined with a calculus
ratiocinator — a mechanical procedure for deriving truths.

The finite concept space theorem gives this a precise
mathematical form. The characteristica is the set of 2n²
grounded Horn clauses. The calculus is belief propagation.
The mechanical implementation is the transformer with BP
weights. The correctness guarantee is bp_exact_on_tree.
The necessity proof is finite_distinguishable_symbols:
not as a design choice, but as a theorem.

## Lean source

    godel/
        FiniteStateSpace.lean
            finite_distinguishable_symbols
        BPTokenFiniteness.lean
            routing_classes_finite

## see also

06_and_or_decomposition.md — the finite structure of the
routing classes in the BP transformer specifically.

02_explicit_bp.md — the construction that instantiates
a finite concept space in a concrete transformer.