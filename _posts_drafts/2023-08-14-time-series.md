---
layout: post
title: Learning Time Series Modelling in Stockholm.
date: 2023-08-15 15:09:01
description: I found some interesting book in the library and I want to tell you about it.
categories: statistics
featured: false
---

This is a post on me learning time series from the book Time Series Modelling, which I have here in front of me at the Stockholm public library. The book is authored by Andreas Jakobssonn and it is apparently used for a course at Lund University, Sweden. 

I am staying in Stockholm for the holiday, and the sightseeing in the city has so far been very nice. Now I wanted to do some studying, especially because of all the amazing mathematics books they have here at the library. This post will contain some interesting things I learned from the book.

--- 

##### Preliminaries: Stochastic vectors, Multivariate Normal Distribution

I assume you are familiar with a stochastic variable (s.v.) as a function $$ X : \Omega \rightarrow V$$ that takes an outcome $$\omega \in \Omega$$ and maps it to a $$v \in V$$. I will, without too much justification, assume that $$V$$ is some inner product vector space over $$\mathbb{R}$$ or $$\mathbb{C}$$, there is a *probability distribution function* $$F_X$$ that characterizes the s.v. and the PDF is nicely differentiable leading to a natural *probability density function* $$f_X$$. I will do this because the basic time series theory is developed from these assumptions. Also, I am on a vacation so I am too lazy to first dive into measure theory. Also most people reason about these things without having taken any course on measure theory, so why should I? Let me write my blog, please.

A s.v. $$\Omega \rightarrow V$$ where $$V$$ has dimension larger than 1 is called a *stochastic vector*. If you are familiar with linear algebra, this generalization will have almost no surprises for you. We do have to redefine things such as the *expectation* and *covariance* to make them work in arbitrary dimensions. For these definitions to work, I will assume that $$\dim V = p < \infinity$$ because I don't want to go into defining norms or metrics on infinite-dimensional vector spaces either. Again, the book doesn't even mention the possibility of infinite-dimensional s.v.'s, it is meant to teach time series modelling.

> **Probability Distribution/Density Function** if $$X$$ is a s.v. $$\Omega \rightarrow V$$ with $$V$$ as above, then the *Probability Distribution Function* $$F_X : V \rightarrow [0,1]$$ is defined from the probability measure $$\mathbb{P}$$ as:
> $$ F_X(\alpha) = \mathbb{P}(X_1(\omega) \leq \alpha_1; ... ; X_p(\omega)\leq\alpha_p), \\ \alpha \in V$$
We call $$X$$ *absolutely continuously* distributed if $$F_X$$ is definable from a so-called *probability density function* $$f_X : V \rightarrow \mathbb{R}_{\geq0}$$ as follows:
> $$F_X(\alpha) = \int ... \int_{\ksi_1 \leq \alpha_1; ... ; \ksi_p \leq \alpha_p} f_x(\ksi_1, ... , \ksi_p) d\ksi_1 d\ksi_p$$

By some multivariate fundamental theorem of analysis we see that from this it follows:

> $$f_

> **Expectation** For $$g: V \rightarrow W$$ a function between $$V$$ and some other inner product space $$W$$ over the same field as $$V$$, the *expectation* $$\mathbb{E}(g(X))$$ is defined to be:
> $$\int ... \int_V g(\ksi_1, ... , \ksi_p) f_X(\ksi_1, ... , \ksi_p)d\ksi_1 ... d\ksi_p$$
> **Conditional Expectation** We can condition, for any measurable event $$E$$ of the event space, the probability density to this event, say $$f_{X|E}. The conditional expectation is then simply the expectation, integrated with $$f_{X|E}$$ rather than $$f_{X}$$:
> $$\int ... \int_V g(\ksi_1, ... , \ksi_p) f_{X|E}(\ksi_1, ... , \ksi_p)d\ksi_1 ... d\ksi_p$$

We must also generalize covariance:

> **Covariance** For $$X, Y$$ two s.v.'s on $$\Omega \rightarrow V$$, $$\Omega \rightarrow U$$ where $$U$$ is some other i.p.s. of dimension $$q$$, we define their covariance as:
> $$\mathbb{C}(X,Y) = \mathbb{E}(XY^*) - E(X)E(Y)* = \mathbb{E}(XY^*) - E(X)E(Y*)$$
> Where $$v^*$$ denotes the *conjugate transpose* of the vector (or matrix) $$v$$.

> **Variance** Is simply defined in terms of covariance:
> $$\mathbb{V}(X) = \mathbb{C}(X,X)$$

> **Theorem** $$R = \mathbb{V}(X) is:
> - *positive semi-definite*, i.e. $$v^*Rv \geq 0$$ for all $$v\in V$$.
> - *Hermitian*, i.e. $$R^* = R$$.

> **Proof**
> - Well, we already know for a single-dimensional s.v. that $$\mathbb{V}X = \mathbb{E}((X - \mathbb{E}X)^2) \geq 0$$. We can use this, as $$v^*X$$ is a single-dimensional s.v. if $$v\in V$$:
> $$v^*Rv = v^*\mathbb{E}(XX^*)v  - v^*\mathbb{E}(X)\mathbb{E}(X^*)v = \mathbb{E}(v^*X(v^*X)^*) - \mathbb{E}(v^*X)\mathbb{E}((v^*X)^*) = \matbb{V}(v^*X) \geq 0$$.
> - Let $$\{e_1, ... , e_p\}$$ be the standard basis of $$V$$, so that $$R_{ij} = e_i^*Re_j$$. Then we use the symmetry $$\mathbb{C}(X_i, X_j) = \mathbb{C}(X_j,X_i)^*$$ that we know for *one*-dimensional s.v.'s $$X_i, X_j$$, to derive:
> $$R_{ij} = e_i^*Re_j = e_i^*\mathbb{E}(XX^*)e_j  - e_i^*\mathbb{E}(X)\mathbb{E}(X^*)e_j = \mathbb{E}(e_i^*X(e_j^*X)^*) - \mathbb{E}(e_i^*X)\mathbb{E}((e_j^*X)^*) = \mathbb{E}(X_iX_j^*) - \mathbb{E}X_i \mathbb{E}X_j^* = \mathbb{C}(X_i, X_j) = \mathbb{C}(X_j, X_i)^* = ... = (e_j^*Re_i)* = e_i^*R^*e_j = (R^*)_{ij}$$

---

##### Update

Although this blog is partially intended to show my knowledge of the basic theory, I have come to realize that it is too cumbersome to write a blog that develops this from scratch. There are many good textbooks on the foundations of probability theory and s.v.'s and I think I would rather tell you about new things that surprised me when studying this book. This is why I have decided to speed up and get to the actual time series models.

---
##### Definition of a stochastic process (I promise, the final standard definition I will mention)

According to the book:
> **Definition** A *stochastic process* (s.p.) is a family of s.v's $$Y_t$$ defined on a probability space for some index set $$I$$

> **Definition** A *Gaussian process* is a stochastic process $$(Y_t)_{t\in I}$$ for which any finite linear combination will be normally distributed.

--- 

##### Wide-sense stationarity (WSS)

> **Definition** A s.p. $$(Y_t)$$ is *wide-sense stationary* iff.
> - $$t \mapsto \mathbb{E}X_t$$ is a constant function, where the constant value is finite.
> - $$\mathbb{C}(Y_s, Y_t)$$ is a function of $$(s-t)$$.
> - $$\mathbb{E}\|Y_t\|^2 $$ is finite