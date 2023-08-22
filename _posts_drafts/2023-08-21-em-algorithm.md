---
layout: post
title: The Expectation-Maximization Algorithm
date: 2023-08-21 
description: An explanation of the general concept of the EM-algorithm with an example application in Gaussian Mixture Models
categories: statistics, algorithms
featured: false
---

The Expectation-Maximization (EM) algorithm is a statistical technique used for estimating parameters in statistical models when dealing with incomplete or missing data. It's particularly useful in situations where we have a dataset with hidden or unobservable variables that influence the observed data.

Concretely, suppose that we have made a parametric statistical model $$\mathcal P = \{\mathbb P_\theta : \theta \in \Theta \}$$ for an i.i.d. sample $$(X_1,Z_1), ... , (X_N,Z_N)$$ which are all distributed like $$(X,Z) : \Omega \rightarrow \mathbb R ^d \times R^h$$. Suppose also that the variable $$Z$$ is not observed or unknown. It could be the hidden state of a system, missing data from subjects who dropped out of a study, a topic of a text, etcetera.

---
## Outline of the method

The EM algorithm consists of two main steps that are iteratively repeated until convergence: the E-step (Expectation step) and the M-step (Maximization step). Let's break down each step:

 - **E-step (Expectation step):**
In this step, we start with a guess $$\hat\theta^{(t-1)}$$ for the model parameters. Then, using these parameters, we compute the distribution of the latent variable $$Z$$ given the parameter and data, that is the likelihood of $$Z$$ given the event $$\{X_1=x_1; ... ; X_N = x_N\}$$ under the probability measure $$\mathbb P _\theta$$ given by parameter $$\theta = \theta^{(t-1)}$$. We shall denote this likelihood as $$p(z\mid X=x, \theta = \theta^{(t-1)})$$.

Next, we use this distribution for $$Z$$ to construct the so-called $$Q$$-function: $$\theta \mapsto Q(\theta\mid \theta^{(t-1)})$$, where $$Q(\theta \mid \theta^{(t-1)})$$ is defined as the *expected likelihood* of $$\theta$$ given that $$Z$$ is distributed according to $$p_{\theta^{(t-1)}}(z \mid \{X = x\})$$:

$$ Q(\theta\mid \theta^{(t-1)}) = \mathbb E _{Z \sim p(\cdot \mid X = x, \theta^{(t-1)})} [\log (p_\theta (Z,X)) ] $$

- **M-step (Maximization step):**
In this step, we make an update $$\hat\theta^{(t)}$$ of the the estimates of the model parameters to maximize $$Q(\theta\mid \hat\theta^{(t-1)})$$. The exact form of the optimization problem depends hugely on the shape of the parameter space $$\Theta$$ (which give the constraints for $$\theta$$) and the criterium function $$\theta \mapsto Q(\theta\mid\theta^{(t-1)})$$, of course! EM can thus only be applied in instances of models where the optimization problem of the instance is feasible to solve.

$$\theta^{(t)} := \text{arg}\max_{\theta\in \Theta} Q(\theta \mid \theta^{(t-1)})$$

The name "Expectation-Maximization" comes from the fact that the E-step computes the expected distribution of the hidden variables (expectation) given the previous estimate of the model parameter, and the M-step maximizes the likelihood by adjusting the model parameter (maximization). These steps are repeated iteratively until, hopefully, the parameter estimates converge to a stable value for $$\theta$$. That is, when $$\theta^{(t+1)}\approx \theta^{(t)}$$, we stop. Two questions remain:

 - Is $$(\theta^{(t)})_{t= 0} ^\infty$$ guaranteed to converge?
 - If it does converge, have we maximized $$\theta \mapsto p _{X\mid\theta} (x)$$ over the entire $$\Theta$$ or have we attained a local maximum somehow?

It depends entirely on the instance of $$\mathcal P$$, Because this determines the shape of $$\Theta$$, $$z\mapsto p(z\mid x,\theta)$$, $$x\rightarrow p(x \mid \theta)$$. 

---

## Example: