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

Dietrich TODO (I'll also collect the results I can)

# Conclusions and Future Work

Dietrich TODO (Will be short, depends on results a bit I suppose)
