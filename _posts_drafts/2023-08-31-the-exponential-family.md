---
layout: post
title: The exponential family of distributions
date: 2023-08-31
description: We define the exponential family of distributions and show that a very large number of probability distributions belongs to this family.
categories: statistics, probability
featured: false
toc:
  sidebar: right
---

> Definition (The Exponential Family of Distributions)
> A real random variable $$X$$ with a probability distribution that is *regular*
> (thus with a completely discrete support $$\mathcal X$$, or with a CDF that admits 
> a continuous PDF (i.e. absolutely continuous)) is said to be a member of a 
> $$k$$-parameter exponential family, if its density (or frequency) can be written as:

> $$f_X(x; \phi) =  \exp (\sum_{i=1}^k \phi_i T_i(x) - \gamma(\phi) + S(x))$$

> Where $$\phi$$ is a $$k$$-dimensional parameter in $$\mathbb R ^k$$. Further, we require that:
> 1. The support/sample space of $$X$$ does not depend on $$\phi$$.
> 2. $$T_i, S$$ are functions $$\mathbb X \rightarrow \mathbb R$$
> 3. $$\gamma$$ is a function $$\mathbb R ^k \rightarrow \mathbb R$$

We call $$\phi$$ the *natural > parameter* and the above expression the *natural representation* of the PDF/PMF, > which is often different from the actual representation.

What is so special about the above form? 

First of all, we require that the sample space of $$X$$ to be independent of the parameter. Not all random variables have this property. For example, if $$\text{Un}[u,l]$$ with real parameters $$u < l$$, does not admit a natural representation because the sample space depends on $$u$$ and $$l$$.

Second, even though every nonnegative function can be written as $$\exp(\log(f(x)))$$ on its support, we require the representation to factor in a special way:
  1. $$\exp(-\gamma(\phi)) \cdot \exp(S(x))$$, so two parts where $$x$$ and $$\phi$$ *do not interact*: $$\phi$$ simply functions as a way to scale the distribution and $$x$$ influences the probability via $$\exp(S(x))$$ without interacting with $$\phi$$
  2. A part where there is interaction but only in a particular way: $$\exp(\sum_{i=1}^k \phi_i T_i(x))$$, so the part where $$x$$ and $$\phi$$ interact, must be an $$\exp$$ taken of the inner product between $$\phi$$ and $$T(x)$$ which is a function only dependent on $$x$$.

The motivation for this specific form will become clear later. As you will see, the natural representation is motivated by many practical theorems and not at all by mathematical aesthetics only.


But first, let's show that the following discrete distributions
are exponential families (perhaps when one of their parameters is held
fixed):
1. The binomial distribution.
2. The Bernoulli distribution.
3. The Poisson distribution.
4. The geometric distribution.
5. The negative binomial / Polya distribution.

And for the following continuous ones:
6. The normal distribution.
7. The exponential distribution.
8. The gamma distribution.
9. The chi-square distribution.

> Note 1: If the $$\text{binomial}(n,p)$$-distribution family is an exponential family, this would automatically imply that the $$\text{Bernoulli}(p)$$-distribution family is an exponential family, because this is a subset of the binomial family for $$n=1$$.

> Note 2: The same holds for the Polya distribution and the geometric distribution. The first generalizes the latter, which is really a $$\text{Polya}(1,p)$$-distribution. In turn, the $$\text{Polya}(n,p)$$-distribution is the distribution of a sum of $$n$$ geometric random variables.

> Note 3: Finally, the chi-square distribution is a special case of the gamma distribution, $$\chi_k^2 = \text{Gamma}(k/2, 1/2)$$, as is the exponential distribution $$\text{Exp}(\lambda) = \text{Gamma}(1,\lambda)$$. In fact, the exponential distribution is in a sense the continuous counterpart of the geometric distribution. The Gamma distribution, being the distribution of sums of independent $$\text{Exp}(\lambda)$$-random variables, is the continuous counterpart of the Polya distribution.

## 1. The binomial distribution.

We are given $$X \sim \text{binomial}(n,p)$$, with PMF $$f_{n,p}(x) = \binom n x p^x (1-p)^{n-x}$$. Observe that the sample space actually depends on $$n$$, hence we regard $$n$$ as *fixed* rather than a parameter, so we show that for a fixed $$n$$, the family of distributions $$\text{binomial}_n(p)$$, $$p\in (0,1)$$, is an exponential family. If we apply this for the case $$n=1$$, we get that $$\text{Bernoulli}(p)$$ is an exponential family.

Observe that:

$$\binom n x p^x (1-p)^{n-x} = \exp(\log(\binom n x p^x (1-p)^{n-x})) = 
\exp(\log(\binom n x) + x\log(p) + (n-x)\log(1-p))$$

And this form can be written in the appropiate form:

$$... = \exp(\log(\binom n x) + x\log(p/(1-p)) + n\log(1-p))$$

And this gives us the *natural parameter* $$\phi = \log(p/(1-p))$$, together with $$T(x) = x$$ and $$S(x) = \log(\binom n x)$$.

## 2. The Poisson distribution.


Here, the sample space is $$\mathbb N_{\geq -}$$ and does not depend on $$\lambda$$. That is the first requirement. For the PMF, we have $$f_\lambda (x) = \exp(-\lambda)\frac {\lambda ^x}{x!}$$. This equals:

$$\exp(-\lambda)\frac {\lambda ^x}{x!} = \exp(-\lambda + \log(\lambda)x - \log (x!))$$

With the natural parameter $$\phi = \log(\lambda)$$, $$T(x) = x$$, $$S(x) = \log(x!)$$, $$\gamma(\phi) = \exp(\phi)$$.

## 3. The negative binomial / Polya distribution.

The sample space is $$\mathbb N_{\geq n}$$, observe that it does depend on $$n$$. So again, leave $$n$$ fixed and look at a family generated by variation of the parameter $$p$$, say $$\text{Polya}_n(p)$$, $$p\in (0,1)$$.

Let's factorize the PMF:

$$f_{p}(x) = \binom {x-1} {n -1} p^x(1-p)^{x-n} = \exp(\log(\binom {x-1}{n-1}) + x \log(p(1-p)) - n \log(1-p))$$.

We can simply take:

$$S(x) = \log\binom{x-1}{n-1}$$

$$T(x) = x$$

Then it follows $$\phi = \log(p(1-p))$$. We now need to find out if we can close the gap by finding an appropriate $$\gamma$$, such that:

$$-\gamma(\log(p(1-p))) = -n\log(1-p)$$

