---
layout: post
title: The Metropolis-Hastings Algorithm
date: 2023-08-04 15:09:01
description: An explanation of the Metropolis-Hastings algorithm with an example application.
tags: statistics, algorithms, mcmc, markov-processes
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

Crucial to such the simulated Markov chain is that it can only converge to one unique stationary distribution, otherwise we still have no guarantee that $$ (Y_0, Y_1, ...) $$ was sampled from $$ \mathbb{P}_X $$. What is the theoretical minimum a Markov chain needs in order to possess a stationary distribution, and what is needed to make it unique?

When dealing with a state space that is countable


#### Reversibility and how to devise the right Markov chain

The next question you'd by now like to see answered: "*How* do we choose and simulate a Markov chain, given the requirement of a stationary distribution $$\upsilon $$ ?"

This is the creative part of the process. I will approach this rather informally, because the ingenuity of the idea is easily snowed under by the correctness and formalism. If we knew $$ \upsilon $$, we could simulate a Markov chain by just picking $$ Y_t \sim \upsilon $$ i.i.d. for all $$ t $$. That would be a valid Markov chain, and it would have transition probabilities $$ p_{ij} = \upsilon_j $$, so  $$\upsilon$$ obviously satisfies the stationary equation $$ \sum_{i \in I} p_{ij}\upsilon_i = \upsilon_j$$ because the sum reduces to $$ \sum_{i \in I} \upsilon_j\upsilon_i = \upsilon_j \sum_{i \in I} \upsilon_i = \upsilon_j \cdot 1 $$ because of the axiom that total probability is 1.

We don't have $$\upsilon$$, but surely there are many other conditional densities that we *do* know. Take such a conditional density, say $$q : S \times S \rightarrow \mathbb{R}_{\geq 0}$$, which will act as a *proposal* transition density. $$ q $$ does not satisfy the detailed balance equation everywhere, i.e. for some $$ i, j \in I $$ we have:

\begin{equation}
\label{eq:detailedBalanceNotSatisfied}
q(i,j)\upsilon(i) > q(j,i)\upsilon(j)
\end{equation}

(We can permute $i,j$ without loss of generality). So when we simulate the Markov chain, we should not always *accept* transitions from $$i$$ to $$j$$ when *proposed* by $$q$$. 

And that is it. We simulate a Markov chain proposed by transition probabilities $$q$$, but before every transition we take an extra acceptance step where we do a Bernoulli experiment with succes probability $$\alpha(i,j)$$ to decide whether we will indeed take the transition or just stay in $$i$$. 

What then, should the density $$\alpha: I \times I \rightarrow \mathbb{R}_{\geq 0}$$ be? In the above situation described by equation $$\ref{eq:detailedBalanceNotSatisfied}$$, $$\alpha(j,i)$$ should be 1, of course, and $$\alpha(i,j)$$ should be smaller to filter some of the abundant transitions from $$i$$ to $$j$$ out. Formally, the added acceptance step creates a new markov chain with transition density

\begin{align}
\label{eq:actualtranstions}
p_{ij} &= q(i,j)\alpha(i,j), \ i \neq j \n
&= 1 - \sum_{i\neq j} q(i,j)\alpha(i,j)
\end{align}

For these to satisfy the detailed balance relation, we need

\begin{equation}
\label{eq:detailedBalanceNotSatisfiedcontd}
q(i,j)\alpha(i,j)\upsilon(i) = q(j,i)\upsilon(j)
\end{equation}

Where we assumed the previous inequality to hold and hence take $$\alpha(j,i) = 1$$, without loss of generality for we can rename i and j. This gives us the required value for $$\alpha(i,j)$$:

\begin{equation}
\label{eq:detailedBalanceNotSatisfied}
\alpha(i,j) = \frac{q(j,i)\upsilon(j)}{\upsilon(i)q(i,j)}
\end{equation}

This fills in the missing piece of the puzzle. But in a way that is very convenient to us: in most applications, we only know $$\upsilon(i)$$ as its proportionality to $$i$$, and we don't know what the proportionality constant should be such that the density integrates to 1. Think of Bayesian statistics: we want to compute the posterior density of the stochastic parameter $$\bar\Theta$$, but this requires a "renormalization" of the posterior, i.e.:

\begin{equation}
\label{eq:renormalize}
p_{\bar\Theta \| X=x}(\theta) = \frac{p_{\theta}(x)\pi(\theta)}{\int_\Theta p_{\mathcal\theta}(x)\pi(\mathcal\theta) d\mathcal\theta}
\end{equation}

So yes, we do now the posterior $$p_{\bar\Theta \| X=x}(\theta) \prop p_{\theta}(x)\pi(\theta) $$, but the actual density needs to be renormalized and this either requires us to recognize the proportionality term $$p_{\theta}(x)\pi(\theta) $$ as a known standard distribution, or to calculate the integral. If the first does not hold and the latter is tedious, we are nowhere without a computer. <d-footnote> And this is why in the past, Bayesian theory evolved around Beta and Gamma distributions, because they had the convenient property that they combined with common likelihoods to give known posteriors. In particular, many common likelihoods have priors that give poseriors from the same distribution family as the prior. This is the concept of [conjugate priors](https://en.wikipedia.org/wiki/Conjugate_prior#). It is more than an algebraic convenience, because it can also be insightful to see how the data likelihood "updates" the parameters of the prior. </d-footnote>

In this case, we want to simulate from the posterior, knowing $$\upsilon(\theta) \prop  p_{\theta}(x)\pi(\theta) $$.

We have completed the theory part and discussed a first application. There are many applications of the method in statistics, but before we get to the code let's discuss performance. Because how fast are we to expect the MH-simulated chain to converge on a stationary distribution? Because stationarity of $$\upsilon$$ does not mean that the chain behaves as $$\upsilon$$ from the start, and asymptotic behaviour is nice but we don't like waiting. This gives rise to two main concerns:

 1. How easily does the Markov chain move through the state space? This determines the speed with which the limiting distribution is reached.
 2. How do we ensure that the first iterations of the chain do not imbalance the sample $$(Y_0, ... , Y_n)$$ that is simulated by the MH algorithm? Because in theory, the initial segment doesn't impact the limiting behaviour of the chain $$(Y_0, Y_1 ...)$$ if we let it go on "forever", but in practice the sample generated is finite and an initial segment that does not display the limiting behaviour will introduce bias.

The second issue is resolved by throwing away the results $$(Y_0, ... Y_b)$$ from a $$b$$-length initial segment of the chain, the so-called "burn-in". The first issue can be addressed by having a proposal $$q$$ that leads to acceptance probabilities $$\alpha(i,j)$$ that are not too small in most states of $$I$$. The problem is well-known and often referred to as "getting a proposal that *mixes* well", that is, we do not reject too often. In the light of this, it is important to mention that there are two main approaches to picking a proposal $$q$$ in the first place:

 1. The *random-walk* approach: we consider a neighbourhood of current state $$i$$ and pick $$j$$ according to density $$g(i-j) = q(i,j)$$ where $$g$$ is a density on $$S$$. Notice how this requires $$S$$ to have an additive operation defined: usually is is a (subset of) $$\mathbb{R}^k$$, and we are dealing with continuous densities. We can extend the theory of stationary distributions and densities to continuous state spaces using tools from measure theory. I will definitely skip the details for now.
2. 