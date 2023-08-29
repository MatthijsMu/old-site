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
void random_shuffle (T* to_swap, ) {
    // PRE: to_swap has size N.
    // POST: to_swap contains a random permutation of its elements

    for (int k = 0; k < N; k++) {
        R = std::rand() % (N - k);
        std::swap(to_swap + R, to_swap + N - k);
    }
}
```

I have not given a proof of correctness, but by the previous argument drawing on the Lotto-algorithm, you might intuitively see why the above algorithm draws every permutation with equal probability.

## Inversions

The second method relies on the fact that we can identify any $$N$$-permutation with its *inversion vector*. An inversion for $$\pi \in S_N$$ is a pair $$(i,j) \in \{1,...,N\}^2$$ such that $$i<j$$ but $$\pi^{-1}(i)>\pi^{-1}(j)$$. More specifically, we can count the number of inversions $$(i,j)$$ for one element $$i\in N$$, say $$\text{inv}(i)$$, and put these in a vector: $$(\text{inv}(1), ... , \text{inv}(N))$$. $$\text{inv}(i)$$ is a count of all $$j>i$$ with $$\pi^{-1}(j)<\pi^{-1}(i)$$. 

The vector is called an inversion vector, and we obviously see that every different permutation gives rise to a different inversion vector. So the mapping from a permutation $$\pi$$ to its inversion vector $$\text{inv}(\pi)$$ is injective. Moreover, $$0\leq \text{inv}(i)\leq N - i$$, so there are at most $$1 \cdot 2 \cdot 3 \cdot ... \cdot (N-1) \cdot N$$ inversion vectors. Since there are $$N!$$ permutations, we now know that the mapping $$\pi \mapsto \text{inv}(\pi)$$ is in fact a bijection. The inversion vector can sometimes be a more natural way to represent a permutation. It is certainly easier to check whether a representation is indeed a valid one: since we can more easily check whether an inversion array satisfies the property $$1\leq \text{inv}(i)\leq i$$ (this can be done in $$O(n)$$ time), than whether a direct representation has no duplicates (you could do this by sorting in $$O(n \log n)$$ time and then passing over the array in $$O(n)$$ time).

We have showed the existence of a bijection, but no explicit inverse of $$\pi \rightarrow \text{inv}(\pi)$$ yet. However, it is not so hard to see how an inversion vector can lead to a permutation:

 - $$\text{inv}(N) = 0$$ always as there are no $$j > N$$. Put it in a singleton list.
 - $$\text{inv}(N-1)$$ is either $$0$$ or $$1$$. If it is 0, this means that in the direct array representation, it is put before $$N$$ since $$N$$ is the only number larger than $$N-1$$. If it is $$1$$, it must be behind $$N$$.
 - In general, we can interleave the element $$N- k$$ with the intermediate list of $$N, N-1, ... , N - k +1 $$ in $$k+1$$ different ways. The number $$0 \leq \text{inv}(N-k) \leq k $$ will tell us at which position to insert $$N - k$$: $$0$$ means completely in front of the intermediate list, $$1$$ means behind the first element of that list, etcetera. This is because all elements that $$N - k$$ could possibly have an inversion with, are already emplaced, so its position is uniquely determined by its inversion number.

This gives us two methods of translating between direct representations and inversion vector representations. Note however that the method $$\text{inv}(\pi) \mapsto \pi$$ requires $$N - 1$$ insertions, making it hideous to do in an ordinary array (a naive insert into an array requires us to shift all elements that come after the insert). There is a clearer way of going about:

 - $$\text{inv}(1)$$ determines the total number of elements that come before `1`.
 - $$\text{inv}(2)$$ determines the total number of elements before `2`, with or without counting `1` (since `1` does not count as an inversion for `2`). This means that if $$\text{inv}(2) < text{inv}(1)$$, then we can only have that `2` has to appear before `1`, whereas if $$\text{inv}(2) \geq text{inv}(1)$$, then having `2` appear before `1` will make `2` have at most $$\text{inv}(1) -1$$ inversions, which will lead to a contradiction. So `2` has to appear behind `1`, making the actual index of `2` equal to $$\text{inv}(2) + 1$$.
 - In general, at the `k`-th step we have the indices $$\text{idx}(j)$$ at which $$j=$$`1`, ... , `k-1` appear, so we compare $$\text{inv}(k)$$ with these indices. For every $$j$$ in `1`, ... , `k` such that $$\text{inv}(k) \geq \text{idx}(j)$$, we have to take $$\text{idx}(k)$$ one higher compared to $$\text{inv}(k)$$. 

Once we have all the `idx[j]`, we have computed a direct representation of the inverse $$\pi^{-1}
