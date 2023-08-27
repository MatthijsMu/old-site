---
layout: post
title: Fast Random Permutations?
date: 2023-08-21 
description: Some ways of generating random permutations.
categories: statistics, algorithms, cs-theory
featured: false
---

Many randomized algorithms make use of random permutations for their randomization. If many such random permutations are needed in one run of the algorithm, it is useful if a random permutation can be generated at a low computational complexity. 

I want to make a short post discussing two methods for generating a random permutation. The first one is derived from a more general "Lotto simulation" algorithm. I learned it in a chapter on simulation in the course Advanced Probability at Radboud.

This algorithm requires the simulation of $$N$$ random integers if $$N$$ is the length of the permutations we want to simulate. Today, I wondered wether we could do with a constant number of random numbers, and I found the second method. 

## Problem formulation

A *permutation* is a bijection $$\{1,...,N\} \rightarrow \{1,...,N\}$$. Alternatively, we can represent it as a vector $$\pi \in \{1,...,N\}$$ which has all its components different. We denote the set of such permutations (be it the bijection or vector construction) as $$S_N$$
 On a computer, the simplest and usually the fastest way of representing a permutation on $$N$$ elements is as an array of size $$N$$ containing $$N$$ different integers in the range `1` to `N`. 
A *uniformly random permutation* is then a vector $$X = (X_1, ... , X_N) : \Omega \rightarrow S_N$$ where $$\mathbb{P}(X = \pi) = \frac{1}{n!}$$ for any permutation $$\pi \in S_N$$.

In a more general setting, we want to use such a random permutation to randomly shuffle an sequential container. In this case, we are speaking of a bijection on $$N$$ elements which are not necessarily the integers `1` to `N`. Usually, the generation of the permutation and the shuffle according to this permutation is done in one step, saving time and space. See e.g. `std::shuffle` in `<algorithm>`, `c++`.

## Method 1

This algorithm is the formalization of what we would intuitively do when we write down a random permutation of $$1, ... , N$$. We first pick a random element $$i \in \{1, ... , N\}$$, and let $$\pi_1 = i$$. Then we remove $$i$$ from the set $$\{1, ... , N\}$$ and keep picking and removing elements from this set until we are done. It is as if we pick numbered balls from an urn and put these in sequence in the order that picked them. In fact, the more general algorithm applies to simulating a sequence of $$n$$ random picks from an urn containing $$N$$ different balls, a Lotto simulation.

To do this on a computer, we need an efficient way to represent the urn and to do the picking process. The urn is an array of $$N$$ elements containing the elements `1` to `N` consecutively, that is, we put every ball in its own "slot". At every step `k`, we pick a ball by simulating a random number `R_k` between `1` and `k`, and add the ball from slot `R_k` to the sequence at position `k`. To ensure that the remaining `k-1` balls are exactly in the first `k-1`-slice of the urn array, we write the `k`th element of the urn array into the `R_k`th slot, which is not used anymore. 

Repeat this for `k = 1, ... , n` and we are done. Let's see an implementation in, say, `C++`:

```c++
#include <numeric>
#include <random>

template <size_t N>
void lotto (int out[], size_t n) {
    // PRE: out has size n.
    // POST: out is populated with a uniformly random permutation of n
    // elements from 1, ... , N.

    // Create the urn array:
    int urn[N];
    std::iota(&urn, &urn + N, 1);

    for (int k = 0; k < n; N++) {
        R = std::rand() % (N - k);
        out[k] = urn[R];
        urn[R] = urn[N - k - 1];
    }
}
```
> Note: to use `iota` from `<numeric>`, you need at least `C++11`.

If we use this method with `n = N`, we get a random permutation generator. 

In a setting where we want to shuffle a generic array `arr` of `N` elements, it would be costly to first generate an array representation of the permutation using `random_permutation` and only after this shuffle `arr` with a separate utility function. We could easily extend the above method to accept `urn` as a parameter for the to be shuffled array.

```c++
#include <numeric>
#include <random>

template <size_t N, typename T>
void random_shuffle (T out[], T urn[]) {
    // PRE: out has size N. urn has size N and contains elements to be permuted
    // POST: out[0, ... , n] is populated with a uniformly random permutation of n
    // elements from urn[1, ... , N]

    for (int k = 0; k < N; N++) {
        R = std::rand() % (N - k);
        out[k] = urn[R];
        urn[R] = urn[N - k - 1];
    }
}
```

Notice that the method will change `urn` array, because at the `k`th step it overwrites elements in the lower `N-k`-slice. So we should not care so much about our input array anyway. Second, notice that while we only use the lower `N-k`-slice of `urn` at every step, we only use the lower `k`-slice of `out` at every step. The conclusion is that the above version of `random_shuffle` is very naive and we can do all of this in-place, by using the upper `k`-slice of `urn` as `out`. Instead of writing our pick to `out[k]`, we write it to `urn[N - k - 1]`. Since `urn[R]` becomes the slot of what was previously at `urn[N - k - 1]`, we need a `swap`. Here is the revised implementation:

```c++
#include <numeric>
#include <random>

template <size_t N, typename T>
void random_shuffle (T to_swap[], ) {
    // PRE: to_swap has size N.
    // POST: to_swap contains a random permutation of its elements

    for (int k = 0; k < n; N++) {
        R = std::rand() % (N - k);
        std::swap(
    }
}

template<typename T> 
void swap (T* t1, T* t2) {
    T temp = *t1;
    *t1 = *t2;
    *t2 = t1
}
```

I have not given a proof of correctness, but by the previous argument drawing on the Lotto-algorithm, you might intuitively see why the above algorithm that

## Method 2

The second 


