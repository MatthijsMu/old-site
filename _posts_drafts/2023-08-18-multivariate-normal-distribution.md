---
layout: post
title: Multivariate Normal Distribution 
date: 2023-08-15 15:09:01
description: Discusses the multivariate normal distribution and some of its abundant applications in statistics or "data science".
featured: false
categories: statistics
---
## Introduction

The multivariate normal distribution (MND) is an extension of the univariate normal distribution, commonly known as the Gaussian distribution. However, instead of modeling a single variable, it accommodates multiple variables simultaneously. Each observation in a multivariate normal distribution is described by a vector of values, and the distribution is fully characterized by two main components: the mean vector $$\mu \in \mathbb R^p$$ and the covariance matrix $$\Sigma \in mathbbR^{p\times p}$$.

This blog aims to establish 2 specific facts, and look at 1 (probably very naive) application of the MND:
 - First, let's derive the formula of the multivariate normal density with the help of some linear algebra and our knowledge of the univariate normal density and general probability.
 - Second, let's use the same algebra to generalize the central limit theorem (CLT) to multivariate random variables.
 - For an application, we look at a naive way to diversify ones investment portfolio in a finance scenario.

## Derivation of the distribution

Suppose we have $$p$$ stochastic variables (SV's) $$\epsilon_1, .. , epsilon_p$$ which are all i.i.d. standard normal, so $$\epsilon_i \sim N(0,1)$$. Let $$\epsilon = (\epsilon_1, ... , \epsilon_p)^t$$. Let's also have a matrix $$A \in \mathbb R^{p\times p}$$ which is nonsingular/invertible/full rank whatever. Let's finally have a vector $$b \mathbb R^p$$. We now want to understand the density $$f_X$$ of $$X = A\epsilon + b$$, with as little work as possible.

We know what the multivariate density of $$\epsilon = A^{-1}X - b$$ is. It is:

$$f_\epsilon(u) = \prod_{i=1}^p \frac1{\sqrt{2\pi}} \exp(-\frac12 u_i^2) = (2\pi)^{-n/2} \exp(-\frac12 u^tu)$$. 

This is because of independence of the $$\epsilon_i$$. 

$$ f_X(x) = \nabla_x \int_{\forall i: \xi_i\leq x_i} f_\epsilon(A^{-1}\xi - b) d\xi_1...d\xi_p $$

Let's change the variables to $$\chi = A^{-1}\xi - b$$, which gives $$d\xi_1...d\xi_p = \det(A)d\chi_1...d\chi_p$$. Also, $$\nabla_x = (\frac \partial {\partial x_1, }, ... ,  \frac \partial {\partial x_p, })$$, and by the chain rule:

$$\frac \partial {\partial x_i} = \sum_{j = 1}^p \frac \partial {\partial \chi_j} \frac { \partial \chi_j}{\partial \x_i} $$

Which we can rewrite as:

$$ ... = \sum_{j = 1}^p \frac \partial {\partial \chi_j} \frac { \partial (\A^{-1}x - b)_j}{\partial \x_i} = \sum_{j = 1}^p \frac \partial {\partial \chi_j} \frac { \partial (\A^{-1}x - b)_j}{\partial x_i}


$$ f_X(x) = \nabla_x \int_{\forall i: \xi_i\leq x_i} f_\epsilon(A^{-1}\xi - b) d\xi_1...d\xi_p $$

Mathematically, a p-dimensional random vector X follows a multivariate normal distribution as follows:

X ~ N(μ, Σ)

Where:

    X is the p-dimensional random vector.
    μ is the mean vector of length p.
    Σ is the p x p covariance matrix.

Properties of the Multivariate Normal Distribution:

    Symmetry: Like the univariate normal distribution, the multivariate normal distribution is symmetric.
    Linear Combinations: Linear combinations of multivariate normal variables are also normally distributed.
    Marginal Distributions: Marginal distributions of individual variables are normal.
    Conditional Distributions: Conditional distributions of subsets of variables are also normal.