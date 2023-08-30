---
layout: post
title: Moment-Generating Functions for some discrete s.v.'s
date: 2023-08-20
description: Let's do the derivations!
categories: statistics, probability
featured: false
toc:
  sidebar: right
---


We will consider some basic discrete probability models, and derive their expectance, variance and moment generating functions. My goal is to derive these in a calculus-y fashion. I hope you can fill in the analytic gaps.

--- 

A discrete random variable is a real random variable $$X : \Omega \rightarrow \mathbb R$$, but with a *support* (we will denote the support of $$X$$ by $$\mathcal X$$) $$\mathcal X \subset \mathbb Z$$, that is: $$\mathbb P [X \in A] = \mathbb P [X \in A \cap \mathbb Z]$$.

We will not use the probability measure directly but always assume that random variables can be characterized by their CDF $$F_\theta: x \mapsto \mathbb P [X \leq x]$$ and the associated PDF (continuous) or PMF (discrete). Moreover, we assume that the random variable is 
real and either discrete or with an almost everywhere continuously differentiable CDF, we will call that a continuous CDF.

The moment generating function (MGF) of a r.v. $$X$$ is defined as:

$$M_X(t) = \mathbb E [e^{tX}]$$

If the implicit integral (or sum) converges. 

If $$X_1, X_2, ... $$ is a sequence of independent r.v.'s then for any $$N \in \mathbb N$$ we have:

$$ M_{X_1 + ... + X_N} (t) = \mathbb E [e^{t(X_1 + ... + X_N)}] = \mathbb E [e^{tX_1}\cdot ... \cdot e^{tX_N}]$$

By independence of the  $$X_i$$, $$e^{tX_i}$$ are independent, so we can pull the product through the $$\mathbb E$$ and get:

$$ M_{X_1 + ... + X_N} (t) = \mathbb E [e^{tX_1}]\cdot ... \cdot \mathbb E [e^{tX_N}] = M_{X_1}(t) \cdot ... \cdot M_{X_N}(t)$$

Which will be useful when we consider convolutions later.

A model is simply a collection of CDF's $$\{F_\theta \mid \theta \in \Theta \}$$ where all $$F_\theta$$ are either continuous or discrete CDF's, for real-valued s.v.'s $$X$$. 

We will write $$f_\theta$$ for the PDF or PMF corresponding with $$F_\theta$$.

I will now skip the probability theoretical bureaucracy because that belongs in a textbook rather than in a blog.

---

## Bernoulli or Alternative distribution

A probability model with a one-dimensional parameter $$p \in [0,1]

$$X \sim \text{alt}(p)$$ iff.
1. $$\mathbb X = \{0,1\}$$
2. $$f_p(x) = p \cdot 1_{\{x = 1\}} + (1-p) \cdot 1_{\{x = 0\}}$$

It clearly has:

$$ \mathbb E [X] = 1 \cdot p + 0 \cdot (1-p) = p$$

$$ \text {var} [X] = 1^2 \cdot p + 0^2 \cdot (1-p) - p^2 = p(1-p) $$

$$ M_X(t) = pe^{t\cdot 0} + (1-p) e^{t\cdot 1} = p + (1-p)e^t $$

The formula still holds in the edge cases $$\in \{0,1\}$$.

---

## Binomial distribution

Given a two-dimensional parameter $$(n,p) \in \mathbb N \times [0,1]

$$ X \sim \text{bin}(n,p) $$ iff.
1. $$\mathcal X = \{0,1, ... , n\}$$
2. $$f_{n,p}(x) = \binom{n}{x}p^xq^{n-x}$$

Where we define $$q = 1-p$$ (it will later become clear that this simplifies calculations).

An equivalent definition is that $$X$$ follows the distribution of $$Y_1 + ... + Y_n$$, where $$Y_i \sim \text{alt}(p)$$ i.i.d.
You can prove this rigorously by induction on N:

> Induction Basis: Clearly true for $$n = 1$$
> Induction Step:
> - Suppose $$X_{n-1}$$ has the same PMF as $$Y_1 + ... + Y_{n-1}$$.
> - Consider $$X_n = X_{n-1} + Y_n$$
> - We can calculate $$\mathbb P [X_n = x] $$ by partitioning over two events: $$Y_n = 0$$ and $$Y_n = 1$$:
> - Then $$\mathbb P [X = x] = \mathbb P [X_n = x, Y_n = 0] + \mathbb P [X_n = x, Y_n = 1] = \mathbb P [X_{n-1} = x, Y_n = 0] + \mathbb P [X_{n-1} = x-1, Y_n = 1]$$
> - Since $$X_{n-1}$$ is only a function of $$Y_1, ... , Y_{n-1}$$, the events for $$X_{n-1}$$ and $$Y_n$$ are independent:
> - This gives $$\mathbb P [X = x] = \mathbb P [X_{n-1} = x]\mathbb P[Y_n = 0] + \mathbb P [X_{n-1} = x-1]\mathbb P [Y_n = 1]$$
> - Use the induction hypothesis to substitute $$f_{n-1,p}(x)$$ and $$f_{n-1,p}(x - 1)$$ into the above equality. Also fill in $$\mathbb P [Y_n = 1] = p$$, $$P[Y_n = 0] = q$$
> - Actually, now you can finish the proof by only some algebra! Hint: [Pascal's Identity](https://en.wikipedia.org/wiki/Pascal%27s_rule). 

Because of this, we now have some very easy derivations for the mean, variance and MGF:

$$ \mathbb E X = \mathbb E [ \sum_{i=1}^n Y_i ] = \sum_{i=1}^n \mathbb E Y_i = np $$

$$ \text{var}[X] = \sum_{i = 1} ^n \text{var}[Y_i] = npq $$

$$ M_X(t) = M_{Y_1}(t) \cdot ... \cdot M_{Y_n}(t) = (p + 1 e^t)^n$$

---

## Geometric Distribution

We now have to be cautious and restrict $$p$$ to the open interval: $$p \in (0,1)$$.

$$ X \sim \text{geom}(p)$$ iff.
1. $$\mathcal X = \mathbb N_{\geq 0}$$
2. $$f(x) = pq^{x-1}$$

We can see that if $$Y_1, Y_2, ... $$ is an infinite sequence of i.i.d. $$\text{alt}(p)$$-distributed r.v.'s, then $$T_1 = \min \{ k \in N_{\geq 0} \mid Y_k = 1\}$$ is geometrically distributed with parameter $$p$$, according to the above definition.
This is because $$\mathbb P [ T_1  = x ] = \mathbb P [ Y_1 = 0, ... , Y_{k-1} = 0, Y_k = 1] = \mathbb P [ Y_1 = 0]  ... \mathbb P [Y_{k-1} = 0] \mathbb P [Y_k = 1]$$ by independence of the $$Y_i$$

Where all the above derivations of expectations, variances and MGFs were done using finite sums, we now have a random variable with infinite support, and hence we are going to to do derivations using infinite series, in particular the geometric series:

$$ \mathbb E X = \sum_{x = 1} ^ \infty xpq^{x-1} $$

Since $$\sum_{x = 1} ^ \infty y^{x} $$ converges (absolutely) to $$(1-y)^{-1}$$ for all $$\vert y \vert < 1$$, $$\sum_{x = 1} ^ \infty py^{x} $$ converges to $$p/(1-y)$$, and 
we can even say that the series is term-wise differentiable w.r.t. $$y$$ and thus has derivative $$\sum_{x = 1} ^ \infty xpy^{x-1} $$, which for $$y = q$$ is exactly our $$\mathbb E [X]$$.

So, we conclude $$\mathbb E X = \frac{d}{dy} \frac p {1-y} \vert_{y = q} = \frac p {(1-y)^2} \vert_{y=q} = \frac p {p^2} = p^{-1}$$

We can do the variance in a similar way, but I will derive it via the MGF. Because if the MGF converges, it *generates the moments* of $$X$$ because its taylor expansion in its variable $$t$$ looks as follows::

$$M_X(t) = \sum_{x=0}^\infty \mathbb E [X^n] \frac {t^n} {n!}$$

In our case, the MGF is very simple:

$$M_X(t) = \sum_{x = 1} ^ \infty e^{tx} pq^{x-1} = \sum_{x = 1} ^ \infty p e^{[t+\ln(1-p)]x} $$

Which converges iff. $$e^{[t+\ln(1-p)]} < 1 $$, which is the case iff. $$t < -\ln(1-p)$$

The second moment can thus be found by differentiating the MGF twice and evaluating in $$0$$:

$$ M_X '' (0) = \frac {d} {dt} \frac {pqe^t}{1 - qe^t} \vert_{t=0} = \frac {(1 - qe^t)^2pqe^t - pqe^t \cdot 2(q- qe^t) \cdot -qe^t} {(1 - qe^t)^4} \vert_{t = 0}$$

$$ = \frac{p^3q + 2p^2q^2} {p^4} = \frac {pq} {p^2}  + \frac {2q^2} {p^2} $$

Then, we add in $$(\mathbb E X) = (\frac q p)^2$$. That will finally give us:

$$\text{var}[X] = \mathbb [X^2] - (\mathbb X)^2 = \frac {qp} {p^2} + \frac {q^2} {p^2} = \frac{p - p^2 + p^2 - 2p + 1} {p^2} = \frac q {p^2}$$

---

## Inverse Binomial, or Polya distribution

We now want to model a sequence of experiments $$Y_1, Y_2, ...$$ and the distribution of the number $$T_n$$ at which the $$n$$th success occurs, in other words $$T_n : = \min \{ k \in \mathbb N \mid \sum_{i = 1} ^k Y_i = n\}$$
Note that the set might not have a minimum in the event that all $$Y_i$$ are $$ 0$$ before $$n$$ successes are reached. This was also the case for the geometric distribution. However, $$T_n$$ is non-defective: this is
because the event $$\cap_{i = l}^\infty \{Y_i = 0\}$$ for any $$l$$ has measure 0, for we have continuity of our probability measure:

$$\mathbb P [\lim_{N\rightarrow \infty} A_N] = \lim_{N \rightarrow \infty} \mathbb P [A_N]$$ 

and this together with:

$$ \mathbb P[\cap_{i = l}^N \{Y_i = 0\}] = q^N$$,

implies that indeed $$\mathbb P [\cap_{i = l}^\infty \{Y_i = 0\}] = \lim_{N\rightarrow \infty} q^N = 0$$.

Lovely little derivation. And we can conclude that the support of $$T_n$$ is indeed $$\mathbb N \geq n$$, and we don't need to include the value "$$\infty$$" in $$\mathcal X$$ (which is done in the case of a defective r.v., then we define the event $$\{T_n = \infty\}$$ to be the event "$$\{ \ \{ k \in \mathbb N \mid \sum_{i = 1} ^k Y_i = n\} = \emptyset \ \}$$", which may not have measure 0).

To derive the PMF, we simply note that if we set $$Z_i = \sum_{j = 1} ^i Y_j$$, then $$Z_i \sim \text{bin}(i,p)$$, and $$Z_i$$ and $$Y_{i+1}$$ are independent, so that:

$$f(x) = \mathbb P [Y_x = 1, Z_{x-1} = n-1] = \mathbb P [Y_x = 1]\mathbb P[ Z_{x-1} = n-1] = p \cdot \binom {x-1} {n -1} p^{x-1} q^{x-1 - (n-1)} = \binom {x-1} {n -1} p^xq^{x-n}$$

To derive the MGF, I will try another good trick: We can argue that the number of experiments needed for a next success, $$\{T_k - T_{k-1}\}_{1\leq k \leq n}$$, is an independent set of i.i.d. $$\text{geom}(p)$$ r.v.'s. In that case, if we put $$S_k := T_k - T{k-1}$$ for $$1 \leq k \leq n$$ (Yes, I see you, but we can simply define $$T_0 \equiv 0$$), we get:

$$M_{T_n} = M_{S_1}(t)\cdot ... \cdot M_{S_n}(t) = (\frac p {1 - qe^t})^n$$

And from this MGF we can again derive the first moment:

$$\mathbb E T = M'_{T_n}(0) = \frac d {dt} p^n (1 - qe^t)^{-n} = (-n)(-q)p^n(1-q)^{-n-1} = nqp^{-1} = n \mathbb E S_i$$

Alternatively, we can also argue that the expectation of a sum of independent variables is the sum of the expectations: $$$$\mathbb E T = n \mathbb E S_i = nqp^{-1}$$. The same holds for the variance:

$$\text{var}[T_n] = \sum_{i=1}^n \text{var}[S_i] = n p^{-2}q$$

----

## Poisson distribution

What if we would consider a sequence of binomial distributions $$(\text{bin}(n, p_n))_{n=0}^\infty$$ such that $$n\cdot p_n \rightarrow \lambda$$ as $$n\rightarrow \infty$$? You already know the answer: Poisson! Let's skip Stirling's approximation and the entire derivation that comes after, and state the PMF:

We have one parameter which is usually $$\lambda$$ and we require $$\lambda >0$$.

 1. $$\mathcal X = \mathbb N_{\geq0}$$
 2. $$f(x) = e^{-\lambda}\frac {\lambda^x}{x!}$$

From this we can derive the MGF:

$$M_X(t) = \sum_{x=0}^\infty e^{tx}e^{-\lambda}\frac{\lambda^x}{x!} =  e^{-\lambda}\sum_{x=0}^\infty \frac {e^{[\log(\lambda)+t]x}}{x!} = e^{-\lambda}e^{e^{[\log(\lambda)+t]}} = e^{\lambda(e^t-1)}$$

The first moment is $$\mathbb E X = M_X ' (0) = e^{\lambda(e^t-1)}\cdot \lambda e^t \vert_{t=0} = e^0\cdot\lambda\cdot e^0 = \lambda$$

The second moment is $$\mathbb E X = M_X '' (0) = [\frac d {dt} e^{\lambda(e^t-1)}\cdot \lambda e^t ]_{t=0} = \lambda^2 + \lambda$$, giving $$\text{var}[X] = \lambda^2 + \lambda - \lambda^2 = \lambda$$ as well.

