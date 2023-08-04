---
layout: post
title: 
date: 2023-08-04 15:09:00
description: some explanation on the Metropolis-Hastings algorithm with an example application.
tags: statistics, algorithms
categories: statistics, algorithms
featured: true
---

Some algorithms are named after people with very fancy surnames. Other algorithms have enigmatic acronyms. At the intersect of this set is the *Metropolis-Hastings*  algorithm, which is often classified as an *MCMC method*. The acronym itself expands to *Markov Chain Monte Carlo*. Again, so extra.

But what does it all mean? And what does it do? This post aims to give an informal (that is, I mention the measure theory but ignore the proofs :winks) introduction to Markov chains together with an explanation of the Metropolis-Hastings algorithm. The Metropolis-Hastings algorithm is not actually one specific algorithm but more of a framework with many specializations and extensions. The algorithm that I will work out in an example, could be viewed as an application of the simplest instance of the framework, which is often referred to as "the" MH algorithm.

---

### Markov Chains

The intuitive idea of a Markov Chain should probably be familiar to you (I will skip the intuitive explanation for now).
We do not discuss the most general form, because the MH algorithm requires only the theory of discrete time, time-homogenous Markov Chains. We can even restrict ourselves to finite state-spaces since in essence anything represented on a computer is finite, even a state space (yes, it can grow *large*, but finite it remains). So this is what most texts on MCMC I've read <d-footnote>Exactly 1.</d-footnote> do, and it simplifies the situation a lot: 


 - The existence of a unique stationary distribution is guaranteed by a-periodicity an irreducibility.
 - The event space of probability-measurable events can just be taken to be $$ \mathcal{P} (S) $$ where $$ S $$ is the state space, without leading to measure-theoretical issues.

I will be lazy and have you look up the definition of random variables (we take them as a mapping from $$ \Omega $$, the *sample space*, to a *value space* $$ \mathcal{V} $$ which is just going to be our state space $$ S $$) and stochastic processes (simply being an infinite sequence $$ \mathbb{N} \rightarrow (\Omega \rightarrow S) $$ of stochastic variables on $$ \Omega $$ ) and give you the definition of a Markov chain that we shall be working with:

**Definition 1** A *time-homogenous* Markov chain on a finite state-space $$ S $$ is a stochastic process $$ \Omega \times \mathbb{N} \rightarrow S $$ with a probability measure $$ \mathbb{P} $$ on $$ \Omega $$, satisfying for all $$ t \in \mathbb{N} $$ and all $$ B \subset S $$ that:

\begin{equation}
\label{eq:markovproperty}
\mathbb{P}(X_{t+1} \in B \| X_t = y_t, X_{t-1} = y_{t-1}, ... X_0 = x_0) = \mathbb{P}(X_{1} \in B \| X_0 = y_t)
\end{equation}

I will not repeat the term "time-homogenous" every time, because we won't see time-inhomogenous Markov chains in this post.

Bon. Markov property established, time homogeneity established. The existence of such processes is not my concern now. Also note how $$ \mathbb{P} $$ is a measure on $$ (S, \mathcal{P}(S)) $$, which is not so well visible at all in the above equation. In essence, the event $$ \{X_t = y_t, X_{t-1} = y_{t-1}, ... X_0 = x_0 \} $$ is the pre-image of $$ (y_0, y_1, ... , y_t) \in S^t $$ under $$ (X_0, ... X_t) : \Omega \rightarrow S^t $$ and we define conditional probabilities from measure $$ \mathbb{P} $$ as usual.

Time-homogenous Markov chains in general can be characterized by a *Markov transition kernel* $$ Q : S \times \mathcal{F} \rightarrow [0,1] $$. Here we digress from $$ \mathcal{P}(S) $$ to a general event space $$ \mathcal{F} $$ over $$ S $$, although that requires us to take $$ S = \Omega $$. Such a kernel satisfies:

 - $$ Q(y, \dot) : \mathcal{F} \rightarrow [0,1] $$ is a measure on $$ (S, \mathcal{F}) $$.
 - We have $$ \mathbb{P}(X_{1} \in B \| X_0 = y_t) = Q(y, B) $$ for all $$ B \in \mathcal{F} $$

The equality in the second point defines $$ Q $$ if we already have the Markov chain, and if we start with the measure there exists a markov chain that satisfies the equality, although such a theorem is really beyond this post <d-footnote>*and* my working knowledge...</d-footnote>.

If such a Markov chain is particularly nice, we can even describe $$ Q $$ using a density $$ q : S \times S \rightarrow [0,1] $$, such that $$ Q(y,B) = \integral_B q(y, z) dz $$. Finite state-space Markov chains are always particularly nice, and in fact the density defines exactly the $$ \| S \| \times \| S \S $$ *transition matrix* $$ (q_{ij}) $$ that is usually used to define finite state-space time homogenous Markov Chains. The integral is just a sum (this is why measure theory is so nice: it is all the same):

\begin{equation}
\label{eq:matrixmarkovdensity}
Q(i,B) = \integral_B q(i, j) dj = \sum_{j \in B} q_{ij}
\end{equation}

Next, we discuss stationary distributions. 

**definition 2** A probability distribution $$ \Gamma : \mathcal{F} \rightarrow [0,1] $$ is called stationary for $$ Q $$ if for all $$ B \in \mathcal{F} $$:

\begin{equation}
\label{eq:stationary}
\int_S Q(y,B)\Gamma(dy) = \Gamma(B)
\end{equation}
