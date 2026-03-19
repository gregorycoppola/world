# Repository Index

All public repos supporting the Transformers are Bayesian
Networks research program.

## The Paper

    shannon
    The main paper in LaTeX. All sections, proofs, experiments.
    github.com/gregorycoppola/shannon

## Lean 4 Proofs

    sigmoid-transformer-lean
    Every sigmoid transformer is a Bayesian network.
    Uniqueness: exact posteriors force BP weights.
    github.com/gregorycoppola/sigmoid-transformer-lean

    transformer-bp-lean
    Transformer implements exact BP with constructed weights.
    github.com/gregorycoppola/transformer-bp-lean

    hard-bp-lean
    BP is exact on trees. Pearl (1988) formalized in Lean 4.
    github.com/gregorycoppola/hard-bp-lean

    pearl
    Peirce lower bound: N rounds necessary for N-hop reasoning.
    Backprop lower bound: N backward steps for N-layer credit.
    Same proof, same abstract structure.
    github.com/gregorycoppola/pearl

    godel
    Finite concept space theorem.
    AND/OR decomposition: k-ary factors reduce to binary.
    Routing classes finite: |RoutingKey(n)| = 2n².
    github.com/gregorycoppola/godel

    universal-lean
    Transformer is Turing complete via Boolean circuit simulation.
    github.com/gregorycoppola/universal-lean

## Empirical Confirmation

    bayes-learner
    Gradient descent finds BP weights from scratch.
    Val MAE 0.000752 on held-out factor graphs.
    github.com/gregorycoppola/bayes-learner

    learner
    Gradient descent finds TM simulation weights from scratch.
    100% accuracy on five structurally distinct Turing machines.
    github.com/gregorycoppola/learner

    loopy
    Loopy BP on QBBN-structured graphs.
    500 trials, all converge, near-exact posteriors.
    github.com/gregorycoppola/loopy

## Analysis

    interp
    Meta-analysis of the mechanistic interpretability literature
    through the BP lens. Typology of attention head types.
    Coverage analysis. Hybrid BP conclusion.
    github.com/gregorycoppola/interp

    pearl/python
    Empirical verification of the Peirce lower bound on chains.
    Two-pass exact BP vs iterative. DAG asymmetry experiments.
    github.com/gregorycoppola/pearl (python/ subdirectory)

## The Website

    bayespage
    Interactive explainer at transformersarebayesnets.com.
    Built with SolidJS. Covers boolean structure, scaling,
    attention, FFN, proofs, experiments.
    github.com/gregorycoppola/bayespage

## Earlier Work

    world (this repo)
    Original QBBN implementation (December 2024).
    Now serves as index to the current research program.
    The implementation has been superseded by the formal
    proofs and empirical repos above.

    bayes-star
    First implementation of the QBBN in Rust with Redis.
    Companion to arXiv:2402.06557.
    github.com/gregorycoppola/bayes-star