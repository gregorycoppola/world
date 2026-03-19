# Transformers are Bayesian Networks

A research program by Greg Coppola (coppola.ai, March 2026).

The core claim: a sigmoid transformer is a Bayesian network.
Not approximately. Not as a useful analogy. By architecture,
for any weights, provably and formally.

## The Paper

> Coppola, G. (2026). *Transformers are Bayesian Networks.*
> [transformersarebayesnets.com](https://transformersarebayesnets.com)

## The Results

Five results, all formally verified in Lean 4:

1. Every sigmoid transformer with any weights implements
   weighted loopy belief propagation on its implicit factor
   graph. One layer is one round of BP.

2. There exist explicit weights such that the transformer
   implements exact BP on any declared factor graph.

3. Exact posteriors force BP weights uniquely. There is no
   other path through the sigmoid architecture to exact
   inference.

4. On tree-structured grounded factor graphs with BP weights,
   the transformer computes exact marginal posteriors at every
   node. No hallucination is possible.

5. Verifiable inference requires a finite concept space. A
   finite state machine with n states partitions any input
   into at most n^n equivalence classes. Hallucination is
   not a bug scaling can fix — it is the structural consequence
   of operating without a grounded concept space.

All proofs are machine-verified with zero sorry placeholders
against standard mathematical axioms.

## The Repos

See [docs/REPOS.md](docs/REPOS.md) for the full index.

Key repos:

    shannon                 the paper (LaTeX)
    sigmoid-transformer-lean    every sigmoid transformer is a BN
    transformer-bp-lean     transformer implements exact BP
    hard-bp-lean            BP is exact on trees
    pearl                   Peirce lower bound — N rounds for N hops
    godel                   finite concept space, AND/OR decomposition
    bayes-learner           gradient descent finds BP weights empirically
    interp                  mechanistic interpretability meta-analysis

## The Website

Interactive explainers, proofs, and experiments:

[transformersarebayesnets.com](https://transformersarebayesnets.com)

## Citation

For the main paper:

    @article{coppola2026transformers,
      title={Transformers are Bayesian Networks},
      author={Coppola, Greg},
      year={2026},
      note={transformersarebayesnets.com}
    }

For the original QBBN paper that introduced the boolean
AND/OR factor graph structure:

    @article{coppola2024qbbn,
      title={The Quantified Boolean Bayesian Network:
             Theory and Experiments with a Logical Graphical Model},
      author={Coppola, Greg},
      journal={arXiv preprint arXiv:2402.06557},
      year={2024}
    }

## Contact

Greg Coppola — greg@coppola.ai — coppola.ai