---
layout: post
title: Simulating the Geometric distribution in constant time
date: 2023-08-04
description: Using bitwise integer arithmetic instructions and Diadic rationals.
categories: statistics, algorithms
featured: false
toc:
  sidebar: right
---

On my morning run today I was wondering whether there are faster ways to implement a random generator that samples from a geometric distribution. The naive way is to perform Bernoulli-experiments using uniform distributions until the first success occurs, but this seemed to me hideously expensive as most information of one Bernoulli simulation is thrown away and you could probably do this using some kind of bitwise integer arithmetic. I came up with something and I want to use this post to discuss it with you and establish a connection with simulating the continuous relative of the geometric distribution, that is, the exponential distribution.

---

## The Geometric distribution

If $$X_1, X_2, ...$$ is an infinite sequence of independently Bernoulli$$(p)$$-distributed random variables, then we call $$T_1 := min \{k\in \mathbb N_{\geq 1} \mid X_1 = 1 \}$$ geometrically distributed, $$T_1 \sim \text{geom}(p)$$. 

We can interpret $$T_n$$ as the number of experiments needed for the first "success" ($$X_i = 1$$) to occur.

Using independence of the $$X_i$$, we can derive $$\mathbb P[T_1 = k] = ppq^{k-1}$$ where $$q = 1-p$$.

## Simulating it the textbook way

The naive way of thinking is: If we can simulate a $$X_i \sim \text{Bernoulli}(p)$$, then we can use that simulator to simulate a $$X \sim \text{geom}(p)$$ by just repeatedly simulating $$X$$ until a first success occurs, and counting the total number of experiments that we did.

The basic (pseudo-) random number generating capabilities that are included in general-purpose programming language such as `C++` are:
 - a simulator for random integers, which generates a random `int` value in the range [`MIN_INT`, `MAX_INT`].
 - a simulator for the $$\text{uniform}[0,1]$$-distribution, which picks a floating-point type value uniformly random from the range $$[0,1]$$

The second capability is usually given as the basis for a $$\text{Bernoulli}(p)$$-generator. Observe that if $$U \sim \text{uniform}[0,1]$$, then $$\mathbb P [U \leq p] = p$$ exactly, so we get the following very simple procedure:



## My problem with this approach + my alternative

If you think about the above algorithm in terms of what is happening on the hardware level, you quickly realize that it does way too much work. 

 - Random number generators [natively work with integral types](https://en.wikipedia.org/wiki/Random_number_generation#Uniform_distributions). So generating a `float`, `double`, etcetera from the range $$[0,1]$$ consists actually of two steps: first, generate a random `unsigned_integral` type value and then divide by `MAX_unsigned_integral`. 
 - Division is expensive, for one. Also we need something much simpler: we just need a `0` with probability $$q$$ and a `1` with probability $$p$$.

Let me just jump to my own idea because the above arguments only make sense when I show you a better implementation to compare against.

## The bits of random integers are bernoulli experiments

The key point is that if we have an 64-bit `int_A` $$\sim$$ `random_int()`, then all its bits, which I will denote as `int_A[i]`, like the integer is some sort of array of booleans, then all bitstrings `int_A[0], ... , int_A[63]` are equally likely to occur. This since they all correspond to a unique integer `int_A`, and all integer values should by definition of `random_int()` occur equally likely. 

If we marginalize to the distribution of a single `int_A[i]`, we realize that it must have a $$\text{Bernoulli}(\frac12)$$-distribution. So when we simulate a random 64-bit integer, we actually get information worth 64 i.i.d. samples from a $$\text{Bernoulli}(\frac12)$$-distribution. This is what I meant when I said that it is a huge detour to first generate a random `int`, then divide to a floating-point type in the range $$[0,1]$$ and only then decide, on 64 bits of information and two expensive arithmetic instructions (floating-point division *and* floating-point comparison), whether a coin toss is a 0 or a 1.

## Using bitwise instructions to make it even faster

With 64 $$\text{Bernoulli}(\frac12)$$-samples in one random number generation, we are left with the task of counting the longest initial segment of failures before a success.

The next great feature of integers is that modern CPU architectures come with ALUs that have all kinds of specific circuits for bitwise operations, meaning that manipulating the bits in an integer can be done with dedicated constant-time instructions. For a nice overview, see for example the Wikipedia page on [Bitwise Operations](https://en.wikipedia.org/wiki/Bitwise_operation).

Chess engines, for example make heavy use of such cheap bitwise operations when generating moves. They store the positions of one piece type in 64-bit integers called `bitboard`s and when they need to calculate all possible fields that, say a bishop, can move to, they take a 64-bit integer which has `1`s on the bishop's rays, shift it so that the bishop is at the center of the "rose", perform a bitwise `and` with the `or` of all other pieces, to "mask" out the "blockers". The blockers-`bitboard` is then used as an index in a lookup table that shows the rays of squares onto which the bishop can actually move. And often, it is cheaper to use a hash of the blockers-`bitboard` as there are more possible blocker configurations than movement configurations. And this is, in very simple terms, what they mean with "magic bitboards".

In order to compute the square index (that is, the bit 0-63) at which we have the first occurence of a piece, there is the `ctz` or count-zero instruction, which computes the number of trailing zeroes starting from the least significant bit. The `ctz`-instruction is exactly what we need to compute the initial number of failures! 

If the instrcution is directly implemented in the hardware, we can regard it as practically constant-time (although on the hardware level, the time theoretically still depends on the actual number of initial zeroes!). In that case we have achieved the following cheap implementation of a random-$$\text{geometric}(p)$$-generator:

```c++
#include <cstlib>

const SEQUENCE_LENGTH = CHAR_BIT * max(sizeof(), sizeof());

int random_geometric() {
    int x = 0
    int seq = std::rand();
    while (seq == 0) {
        x += SEQUENCE_LENGTH;
    }
}
```
If `seq == 0`, we have only failures. This means that the first 64, 48 or 32 experiments (depending on the number of bits `int` can contain and the number of bits `std::rand()` generates), failed and we have to add 64/48/32 to `x` and generate the next sequence of experiments (i.e. the next random integer).

Note that calling `std::rand` might actually not populate the entire `int` if `std::rand()` generates shorter integers than the `int`-type defined on the machine. Or, the other way around, it might actually generate a longer bitstring and part of this is cut off. In either way, we need that the actual sequence of bernoulli-experiments resides in the shortest of `int` and `std::rand()`, so we know the length of the sequence is the minimum of those lengths. We can compute the length of `int` and `std::rand()` by taking the `sizeof()` operator. This returns the size in *bytes*. But "depending on the computer architecture, a byte may consist of 8 or more bits, the exact number being recorded in CHAR_BIT." ([cppreference](https://en.cppreference.com/w/cpp/language/sizeof), 2023)

The extra condition `seq == 0` is of course almost never met: the probability of more than $$N$$ failures is namely:

$$\mathbb P [X > N] = 1 - \sum_{n=0}^N pq^n = p\cdot \frac {1-q^{N+1}}{1-q} = 1 - q^{N+1}$$