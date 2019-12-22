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

In [our last adventure](probabril) we laid out an exact solver for the distribution represented by probabilistic Bril programs, but in this setting the only source of randomness is a coin flip, which gives all distributions discrete, finite support. In our exact abstract interpreter, we thought of a coin flip as a world split.
Of course, in real languages, the standard is to give people access to a `rand` instruction, a uniform sample from [0,1]. Analogous to our motivating example before, we would like to run programs such as

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

Our original technique would work here too, if it were reasonable to split into a separate world with its own environment for each of the $2^32$ different values that a 32-bit float could take, but alas it is not. Paying that cost once would already be hard to stomach, but doing it every time we wanted access to randomness would be a bit much.

Instead, we can lazily avoid splitting into worlds until we have to branch and split the environment or control flow.




Dietrich TODO

[probabril]: https://www.cs.cornell.edu/courses/cs6120/2019fa/blog/probabril/
[float-bril]: https://www.cs.cornell.edu/courses/cs6120/2019fa/blog/floats-static-arrays/

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

Dietrich TODO (I'll also collect the results I can)

# Conclusion

Dietrich TODO (Will be short, depends on results a bit I suppose)
