+++
title = "Real Number Analysis with Probabril"
[extra]
bio = """
  [Oliver][] is a CS PhD student in theory, who is under the impression he does many things but struggles to remember what they are.

  [Dietrich][] is a 3rd year PhD student researching Language Design and Compilers.  Enjoys gaming and climbing.

[dietrich]: https://www.cs.cornell.edu/~dgeisler/
[oliver]: https://www.cs.cornell.edu/~oli
"""
latex = true

[[extra.authors]]
name = "Oliver Richardson"
link = "https://www.cs.cornell.edu/~oli"
[[extra.authors]]
name = "Dietrich Geisler"
link = "https://www.cs.cornell.edu/~dgeisler"
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

In [a previous adventure](probabril) we laid out an exact solver for the distribution represented by probabilistic Bril programs, but in this setting the only source of randomness is a coin flip, which gives all distributions discrete, finite support. In our exact abstract interpreter, we thought of a coin flip as a world split.
Of course, in most languages, the standard is to give people access to a `rand` instruction, a uniform sample from [0,1], rather than a coin. Analogous to our motivating example before, we would like to run programs such as

```
main {
  main {
    b: bool = const false;
    f: double = const .25;
  start :
    b: bool = not b;
    x: double = rand;
    z: bool = flt x y;
    br z start end;
  end :
    print x;
    ret;
  }
```

Our original technique would work here too, if it were reasonable to split into a separate world with its own environment for each of the $2^{32}$ different values that a 32-bit float could take, but alas it is not. Paying that cost once would already be hard to stomach, but doing it every time we wanted access to randomness is too much, and besides, most of these environments would be copies of one another.

This is one of several optimizations that can be enabled by representing many related outcomes with ranges or densities. Densities, of course, subsume ranges, but are much trickier to get right; we decided to try both.

It is much cheaper to instead lazily avoid splitting into worlds until we have to branch, and just keep track of the conditional distributions of all variables given that we are where we are.
The purpose of this assignment was to do an optimization of some kind.


[probabril]: https://www.cs.cornell.edu/courses/cs6120/2019fa/blog/probabril/
[float-bril]: https://www.cs.cornell.edu/courses/cs6120/2019fa/blog/floats-static-arrays/

# A New Abstract Interpreter

Oli TODO (I'm thinking background stuff here.  The headers are dumb, you can merge with the previous section however you want.  Could add some ranty stuff here about how the theory was hard.)

## The Operations

### Random

Ironically, those things that were the most difficult before, are considerably easier.
To interpret a `rand` instruction, we need only

### Adding and Multiplying Numbers

Consider the following program:

```
x := rand
y := rand
z := x + y
```

The distribution on `x` and `y` are uniform; What is the distribution on `z`? Its density is a triangle distribution:

![a]()

In general, the density of two random variables must be convolved: if $f_X(x)$ is the density of $X$, and $f_Y(y)$ is the density of y, and the two are independent, then the variable $Z = X + Y$ can take the value $z$ only if there is some event $X = x$ occuring simultaneously with $Y = z-x$, for any value of $x$. Correspondingly, the density $f_Z$ is therefore given by:

$$ f_Z(z) = \int_{-\infty}^\infty f_X(x) f_Y(z-x) \mathrm{d}x $$

Similarly, for $Z = XY$, the density at $z$ is given by:

$$ f_Z(z) = \int_{-\infty}^\infty f_X(x) f_Y(z/x) \frac{1}{|x|} \mathrm{d}x $$

This means that abstractly interpreting distributions of floats made out of uniform distributions

### Branching

Branching, like randomness, is one of the major sources of problems, but it turns out that we will need to do the difficult work before we ever get to the branch. We can implement control structures straightforwardly, but

### Comparisons

The most difficult part of the entire endeavor is interpreting the comparison between two floats.


# Implementation



## Interval Analysis Approach

Dietrich TODO

## With Splines

As we have seen, intervals are only so precise. They work incredibly well for "axis-aligned"

let's reconsider the full situation, in which you would like to track the exact densities of all of


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

# Conclusions and Future Work

While the approach of computing series convergence has more potential, polynomial structure appears difficult enough to reason about that such that such a solution will require substantially more work.
Our initial approach has shown that intervals can be constructed as accurate but loose bounds around probabilistic state space.
Theoretically, we have found that calculating convergence may require potentially catastrophic complexity; however, this is a realistic approach for small problems requiring exact solutions.
We have also found `bril`, or any representation with simple control structure, to be a good target for such analysis, and implemented the basics of what such an algorithm may eventually work from.

TODO: Fix up based on theory results.
