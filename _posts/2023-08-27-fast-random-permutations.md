---
layout: post
title: Fast Random Permutations?
date: 2023-08-21 
description: Some ways of generating random permutations.
categories: statistics, algorithms, cs-theory
featured: false
---

Many randomized algorithms make use of random permutations for their randomization. If many such random permutations are needed in one run of the algorithm, it is useful if a random permutation can be generated at a low computational complexity. This post explores three methods for generating random permutations.

## Problem formulation

A *permutation* is a bijection $$\{1,...,N\} \rightarrow \{1,...,N\}$$. Alternatively, we can represent it as a vector $$\pi \in \{1,...,N\}$$ which has all its components different. We denote the set of such permutations (be it the bijection or vector construction) as $$S_N$$
 On a computer, the simplest and usually the fastest way of representing a permutation on $$N$$ elements is as an array of size $$N$$ containing $$N$$ different integers in the range `1` to `N`. 
A *uniformly random permutation* is then a vector $$X = (X_1, ... , X_N) : \Omega \rightarrow S_N$$ where $$\mathbb{P}(X = \pi) = \frac{1}{n!}$$ for any permutation $$\pi \in S_N$$.

## Method 1: