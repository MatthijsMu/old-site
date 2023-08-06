---
layout: post
title: The Metropolis-Hastings Algorithm
date: 2023-08-04 15:09:00
description: An explanation of the Metropolis-Hastings algorithm with an example application.
tags: statistics, algorithms, mcmc, markov processes
categories: statistics, algorithms
featured: true
---

Some algorithms are named after people with very fancy surnames. Other algorithms have enigmatic acronyms. At the intersect of this set is the *Metropolis-Hastings*  algorithm, which is often classified as an *MCMC method*. The acronym itself expands to *Markov Chain Monte Carlo*. Again, so extra.

But what does it all mean? And what does it do? This post aims to give an informal (that is, I mention the measure theory but ignore the proofs :wink:) introduction to Markov chains together with an explanation of the Metropolis-Hastings algorithm. The Metropolis-Hastings algorithm is not actually one specific algorithm but more of a framework with many specializations and extensions. The algorithm that I will work out in an example, could be viewed as an application of the simplest instance of the framework, which is often referred to as "the" Metropolis-Hastings (MH) algorithm.

---

### Markov Chains

#### Definition

The intuitive idea of a Markov Chain should probably be familiar to you (I will skip the intuitive explanation for now).
We do not discuss the most general form, because MH requires only the theory of discrete time, time-homogenous Markov Chains. We will restrict ourselves to countable state-spaces (that is, our $$ V $$ such that $$ X_t : \Omega \rightarrow V $$ is countable), which allows us to 


 - The existence of a unique stationary distribution is guaranteed by a-periodicity an irreducibility.
 - The event space of probability-measurable events can just be taken to be $$ \mathcal{P} (S) $$ where $$ S $$ is the state space, without leading to measure-theoretical issues.

I will be lazy and have you look up the definition of random variables (we take them as a mapping from $$ \Omega $$, the *sample space*, to a *value space* $$ \mathcal{V} $$ which is just going to be our state space $$ S $$) and stochastic processes (simply being an infinite sequence $$ \mathbb{N} \rightarrow (\Omega \rightarrow S) $$ of stochastic variables on $$ \Omega $$ ) and give you the definition of a Markov chain that we shall be working with:

**Definition 1** A *time-homogenous* discrete-time Markov chain on a finite state-space $$ S $$ is a stochastic process $$ \Omega \times \mathbb{N} \rightarrow S $$ with a probability measure $$ \mathbb{P} $$ on $$ \Omega $$, satisfying for all $$ t \in \mathbb{N} $$ and all $$ B \subset S $$ that:

\begin{equation}
\label{eq:markovproperty}
\mathbb{P}(X_{t+1} \in B \| X_t = y_t, X_{t-1} = y_{t-1}, ... X_0 = x_0) = \mathbb{P}(X_{1} \in B \| X_0 = y_t)
\end{equation}

I will not repeat the term "time-homogenous" every time, because we won't see time-inhomogenous Markov chains in this post.

Bon. Markov property established, time homogeneity established. The existence of such processes is not my concern now. Also note how $$ \mathbb{P} $$ is a measure on $$ (S, \mathcal{P}(S)) $$, which is not so well visible at all in the above equation. In essence, the event $$ \{X_t = y_t, X_{t-1} = y_{t-1}, ... X_0 = x_0 \} $$ is the pre-image of $$ (y_0, y_1, ... , y_t) \in S^t $$ under $$ (X_0, ... X_t) : \Omega \rightarrow S^t $$ and we define conditional probabilities from measure $$ \mathbb{P} $$ as usual.

Time-homogenous Markov chains in general can be characterized by a *Markov transition kernel* $$ Q : S \times \mathcal{F} \rightarrow [0,1] $$. Here we digress from $$ \mathcal{P}(S) $$ to a general event space $$ \mathcal{F} $$ over $$ S $$, although that requires us to take $$ S = \Omega $$. Such a kernel satisfies:

 - $$ Q(y, \cdot) : \mathcal{F} \rightarrow [0,1] $$ is a measure on $$ (S, \mathcal{F}) $$.
 - We have $$ \mathbb{P}(X_{1} \in B \| X_0 = y_t) = Q(y, B) $$ for all $$ B \in \mathcal{F} $$

The equality in the second point defines $$ Q $$ if we already have the Markov chain, and if we start with the measure there exists a markov chain that satisfies the equality, although such a theorem is really beyond this post.



If such a Markov chain is particularly nice, we can even describe $$ Q $$ using a density $$ q : S \times S \rightarrow [0,1] $$, such that $$ Q(y,B) = \int_B q(y, z) dz $$. Finite state-space Markov chains are always particularly nice, and in fact the density defines exactly the $$ \| S \| \times \| S \S $$ *transition matrix* $$ (q_{ij}) $$ that is usually used to define finite state-space time homogenous Markov Chains. The integral is just a sum (this is why measure theory is so nice: it is all the same):

\begin{equation}
\label{eq:matrixmarkovdensity}
Q(i,B) = \int_B q(i, j) dj = \sum_{j \in B} q_{ij}
\end{equation}

### Stationary Distribution

Next, we discuss stationary distributions. 

**Definition 2** A probability distribution $$ \Upsilon : \mathcal{F} \rightarrow [0,1] $$ is called stationary for $$ Q $$ if for all $$ B \in \mathcal{F} $$:

\begin{equation}
\label{eq:stationary}
\int_S Q(y,B)\Upsilon(dy) = \Upsilon(B)
\end{equation}

Again, in regularity land, the stationary distribution can be described by a density $$ \upsilon : S \rightarrow \mathbb{R}_{\geq 0} $$. This means that $$ \Upsilon(dy) = \upsilon(y)dy $$ (You can see I'm now basically dropping my mathematics background and speaking completely as a physicist). This gives that the density statisfies:

\begin{equation}
\label{eq:stationarydensU}
\int_S Q(y,B)\upsilon(y)dy = \int_B \upsilon(B) 
\end{equation}

And in the event that $$ S $$ is countable, the density is just a probability mass function and the integral could be read as a discrete sum. Taking that $$ Q(y,\cdot) $$ also has an associated density $$ q(y, \cdot) $$ for every $y \in S $$, we get another stationarity equation by 

\begin{equation}
\label{eq:stationarydensQdensU}
\int_B \int_S q(y,z)\upsilon(y)dy = \int_B \upsilon(y)dy
\end{equation}

And if $$ S $$ is countable we can take singleton events for $$ B $$ and relate the densities by "removing one shell of integration": 

\begin{equation}
\label{eq:stationarydensQdensU}
\int_B \int_S q(y,z)\upsilon(y)dy = \int_B \upsilon(y)dy
\end{equation}

We might also arrive at this when $$ S $$ is a metric space and $$ Q $$ is differentiable in the second component, in which case we allude to the fundamental theorem of calculus (again, this may al be generalized in measure theory, but I'm unaware of that yet).

\begin{equation}
\label{eq:stationarydensQdensU_withoutIntegral}
\int_S q(y,z)\upsilon(y)dy = \upsilon(y)
\end{equation}

In a countable state space $$ I $$ with transition probabilities $$q(i,j) = p_{ij} $$ this is simply the familiar eigenvalue problem:

\begin{equation}
\label{eq:stationaryMatrixform2}
\sum_{i\in I} p_{ij}\upsilon_i = \upsilon_j
\end{equation}



#### Unicity of stationary distributions

I will now initiate the connection with MCMC methods. The idea behind MCMC is as follows: when it is infeasible to simulate a certain probability distribution $$ \mathbb{P}_X $$ directly, we simulate a Markov chain  $$ (Y_0, Y_1, ...) $$ that has $$ \mathbb{P}_X $$ as its stationary distribution. The exact type of infeasibility arises from the context, and it is usually also this infeasibility that motivates which MCMC method is natural to use.

Crucial to such the simulated Markov chain is that it can only converge to one unique stationary distribution, otherwise we still have no guarantee that $$ (Y_0, Y_1, ...) $$ was sampled from $$ \mathbb{P}_X $$. What is the theoretical minimum a Markov chain needs in order to 


#### Reversibility and how to devise the right Markov chain

The next question you'd by now like to see answered: "*How* do we choose and simulate a Markov chain, given the requirement of a stationary distribution $$\upsilon $$ ?"