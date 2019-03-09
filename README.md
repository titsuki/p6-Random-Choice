[![Build Status](https://travis-ci.org/titsuki/p6-Random-Choice.svg?branch=master)](https://travis-ci.org/titsuki/p6-Random-Choice)

NAME
====

Random::Choice - A Perl 6 alias method implementation

SYNOPSIS
========

```perl6
use Random::Choice;

say choice(:size(5), :p([0.1, 0.1, 0.1, 0.7])); # (3 3 3 0 1)
say choice(:p([0.1, 0.1, 0.1, 0.7])); # 3
```

DESCRIPTION
===========

Random::Choice is a Perl 6 alias method implementation. Alias method is an efficient algorithm for sampling from a discrete probability distribution.

METHODS
-------

### choice

Defined as:

    multi sub choice(:@p! --> Int) is export
    multi sub choice(Int :$size!, :@p! --> List)

Returns a sample which is an Int value or a List. Where `:@p` is the probabilities associated with each index and `:$size` is the sample size.

FAQ
===

Is `Random::Choice` faster than Mix.roll?
-----------------------------------------

The answer is YES when you roll a large biased dice but NO when a biased dice is small.

Why? There are some possible reasons:

  * `Random::Choice` employs O(N) + O(1) algorithm whereas `Mix.roll` employs O(N) + O(N) algorithm (rakudo 2018.12).

  * `Mix.roll` is directly written in nqp. In general, nqp-powered code is faster than naive-Perl6-powered code when they take small input.

A benchmark result is here (For more info, see `example/bench.p6`):

```bash
$ perl6 bench.p6 
Benchmark: 
Timing 1000 iterations of Mix, Random::Choice...
       Mix: 0.121 wallclock secs (0.140 usr 0.003 sys 0.142 cpu) @ 8245.995/s (n=1000)
Random::Choice: 0.264 wallclock secs (0.290 usr 0.005 sys 0.295 cpu) @ 3791.757/s (n=1000)
O----------------O--------O------O----------------O
|                | Rate   | Mix  | Random::Choice |
O================O========O======O================O
| Mix            | 8246/s | --   | -60%           |
| Random::Choice | 3792/s | 153% | --             |
O----------------O--------O------O----------------O
Benchmark: 
Timing 1000 iterations of Mix, Random::Choice...
       Mix: 2.071 wallclock secs (2.069 usr 0.000 sys 2.069 cpu) @ 482.973/s (n=1000)
Random::Choice: 1.438 wallclock secs (1.441 usr 0.000 sys 1.441 cpu) @ 695.211/s (n=1000)
O----------------O-------O------O----------------O
|                | Rate  | Mix  | Random::Choice |
O================O=======O======O================O
| Mix            | 483/s | --   | 45%            |
| Random::Choice | 695/s | -31% | --             |
O----------------O-------O------O----------------O
Benchmark: 
Timing 1000 iterations of Mix, Random::Choice...
       Mix: 173.402 wallclock secs (173.244 usr 0.064 sys 173.308 cpu) @ 5.767/s (n=1000)
Random::Choice: 14.767 wallclock secs (14.752 usr 0.008 sys 14.759 cpu) @ 67.721/s (n=1000)
O----------------O--------O------O----------------O
|                | Rate   | Mix  | Random::Choice |
O================O========O======O================O
| Mix            | 5.77/s | --   | 1076%          |
| Random::Choice | 67.7/s | -91% | --             |
O----------------O--------O------O----------------O
```

AUTHOR
======

titsuki <titsuki@cpan.org>

COPYRIGHT AND LICENSE
=====================

Copyright 2019 titsuki

This library is free software; you can redistribute it and/or modify it under the Artistic License 2.0.

The algorithm is from:

  * Vose, Michael D. "A linear algorithm for generating random numbers with a given distribution." IEEE Transactions on software engineering 17.9 (1991): 972-975.

