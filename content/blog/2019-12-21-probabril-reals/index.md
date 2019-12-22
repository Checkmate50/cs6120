+++
title = "Real Number Analysis with Probabril"
extra.author = "Oliver Richardson and Dietrich Geisler"
extra.bio = """
  [Dietrich Geisler](https://www.cs.cornell.edu/~dgeisler/) is a 3rd year PhD student researching Language Design and Compilers.  Enjoys gaming and climbing.
  Oliver is a CS PhD student in theory at Cornell, who does decision theory, category theory, and spends far too much time on assignments after the due date.
"""
+++

# Introduction

Probabilistic programming relies on our ability to evaluate the probability distribution of a program involving random number generation and selection.
Most current solutions for calculating these distrubutions rely on sampling the program repeatedly, recording results, and perhaps inferring some approximate convergence.
For programs involving loops or cycles, however, it should be possible to compute the infinite series convergence directly.

In this project, we explore implementing a static algorithm for computing the _exact_ state space distribution of probabilistic programs involving real polynomials.
This algorithm was intended for implementation in `bril`, which restricts supported operations to arithmetic operations, conditions, and simple loops.
We add the `random` command to bril, which returns a random double between `0` and `1`, and implement interval-based and convergence-based analysis for doubles in `xbrili`.
We found that such a convergence algorithm was difficult to impelement, but still manage to recover some interval analysis based results for computing outer bounds on `bril` probabilistic state space.

TODO: Fix above paragraph with correct implementation details

# Background on Probabril

Oli TODO (Can be mostly copied from p1 I expect)

# The Problem of Reals

Oli TODO (I'm thinking background stuff here.  The headers are dumb, you can merge with the previous section however you want.  Could add some ranty stuff here about how the theory was hard.)

# Implementation



## Interval Analysis Approach

Dietrich TODO

## Polynomial Approach

Oli TODO (I might be using the wrong title for this section)

# Results

We evaluated our implementation on a series of minimal programs.  
These programs were selected both for using only the operations supported by our probabilistic analysis and for being simple enough to compute exact state probabilities by hand.
The programs chosent also reflect, somewhat accurately, the extent of the algorithm we managed to get working.
All programs were run on a Dell XPS 13 with an Intel I5-8250U CPU, 16 GB RAM, and WSL for Windows 10.

The intervals and runtimes for our algorithm are listed in the table below:

|Program|Exact Intervals|Calculated Intervals|Runtime|
|---|---|---|---|
|Simple|\[1-2\] 100%|\[1-2\] 100%|100s|
||\[2-3\] 50%|\[2-4\] 50%||
|Complex|\[0-1\] 100%|\[0-1\] 0|1000s|

Our basic interval algorithm clearly does not capture some of the intricacies of arithmetic operations on polynomials.
However, this approach terminates very quickly, and can capture simple control flow without issue.
It is worth noting that if our series convergence algorithm were finished, we expect it to calculate these intervals and associated probabilities exactly.
We expect that such an algorithm would be substantially slower than an interval implementation, however.

TODO: Collect the results I can

# Conclusion

While the approach of computing series convergence has more potential, polynomial structure appears difficult enough to reason about that such that such a solution will require substantially more work.
Our initial approach has shown that intervals can be constructed as accurate but loose bounds around probabilistic state space.
Theoretically, we have found that calculating convergence may require potentially catastrophic complexity; however, this is a realistic approach for small problems requiring exact solutions.
We have also found `bril`, or any representation with simple control structure, to be a good target for such analysis, and implemented the basics of what such an algorithm may eventually work from.

TODO: Fix up based on theory results.