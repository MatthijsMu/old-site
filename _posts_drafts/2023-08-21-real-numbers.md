---
layout: post
title: On the Uniqueness of the Real Numbers
date: 2023-08-15 15:09:01
description: Let's review some theorems at the intersection of ring/field theory and real analysis.
featured: false
categories: statistics
---

## Definitions

> **Definition 1** A *ring* is a quintiple $$(R, 0, 1, +, \cdot)$$ satisfying the following properties:
> - $$0, 1 \in R$$, $$+$$m $$\cdot$$ are functions $$R\times R \rightarrow R$$.
> - $$(R, 0, +)$$ is an *Abelian group*.
> - $$(R, 1, \cdot)$$ is a *monoid*.
> - $$+$$ and $$\cdot$$ satisfy two distributive laws:
>   - $$(a + b)\cdot c = (a\cdot c) + (b\cdot c)$$, $$\forall a,b,c\in R$$
>   - $$c \cdot (a + b) = (c\cdot a) + (c\cdot b)$$, $$\forall a,b,c\in R$$

> **Definition 2** An *ordered ring* is a ring with a relation $$\leq \subset R\times R$$ s.t.
> - $$\leq$$ is a *total ordering relation*.
> - if $$a\leq b$$ and $$ s \in R$$ then $$a + s \leq b + s$$.
> - if $$a\leq b$$ and $$s \in R$$ then $$as \leq rs$$.

We write $$a < b$$ iff. $$a \leq b$$ and $$a \neq b$$. Analogously, we define $$\geq$$ and $$>$$ to be the *opposite orders* $$a \geq b$$ iff. $$b \leq a$$ and $$a > b$$ iff. $$b<a$$.

Also note that we will only consider commutative rings, where the second statement of Definition 2 is equivalent to:
> - if $$a\leq b$$ and $$s \in R$$ then $$sa \leq sr$$.

But what exactly can be said about non-commutative ordered rings, is maybe not as trivial.

Let's start by proving some lemmas.

---

> **Lemma** Any commutative ordered ring $$R$$ has a unique ring homomorphism $$\kappa : \mathbb Z \rightarrow R$$. Moreover $$\kappa$$ is an embedding and order-preserving if $$0_R \rightarrow 1_R $$ holds.

> **Proof** 
> - Uniqueness: for $$\kappa(1) = 1_R$$ has to hold, hence for $$m>0$$ it is required that $$\kappa(m) = 1_R + ... + 1_R$$, a sum of $$m$$ terms $$1_R$$, and so it follows $$\kappa (-1) = -1_R$$ and this also fixes $$\kappa(m)$$ for $$m < 1$$. 
> - Existence: so let $$\kappa$$ be defined by $$\kappa(m) = 1_R + ... + 1_R$$, a sum of $$m$$ terms $$1_R$$, and $$\kappa(-m) = -\kappa(m)$$ if $$-m < 0$$. Then we easily see that this function satisfies homomorphism properties. 
> - For injectivity, we simply note that $$\kappa(m) < \kappa(n)$$ if $$m < n$$ in $$\mathbb Z$$. This requires us to have $$0_R < 1_R$$ in $$R$$, because we could also have an order in $$R$$ that has $$1_R < 0$$ (the "opposite order", where the negative and positive elements of $$\mathbb Z$$ give positive and negative elements in $$R$$). I will not make this precise (I think you understand the intuition). Anyway, since $$m<n \implies \kappa(m)<\kappa(n)$$ and $$\leq$$ is total on $$Z$$, we have $$m \neq n \implies m < n or m>n \implies \kappa(m)\neq \kappa(n)$$, thus injectivity.

**Aside** We also say that since the unique embedding $$\kappa : \mathbb Z \rightarrow R$$ has a trivial kernel, $$R$$ has *characteristic* 0. If $$\text{ker} \kappa = d\mathbb Z$$ for some $$d\in \mathbb Z$$ (remember, any kernel is of this form since it is an ideal in $$\mathbb Z$$, which is a principal ideal domain), we say $$R$$ is of characteristic $$d$$. $$d$$ is uniquely determined since $$\kappa$$ is.

> **Lemma** If $$F$$ is a field, then if it has characteristic $$0$$, it has to contain a copy of $$\mathbb Q$$, in other words there is an embedding $$\lambda : \mathbb Q \rightarrow F$$.

> **Proof**
> Given $$\kappa$$, we can make the well-defined definition $$\lambda: \frac m n \mapsto \kappa (m) \kappa (n) ^{-1} \in F$$.
> First, we need to check whether $$\kappa(n)^{-1}$$ always exists. This is so since $$n\neq 0$$ for $$\frac m n \in \mathbb Q$$ and $$\kappa$$ is injective so $$\text{ker} \kappa = \{0\}$$. 
> Second, we need to check well-definedness in the sense that if $$\frac m n = \frac k l$$, then $$\lambda(\frac m n) = \lambda(\frac k l)$$. This follows from the homomorphism-property of $$\kappa$$:
> $$\frac m n = \frac k l$$ means by definition $$ml = nk$$ in $$\mathbb Z$$, hence $$\kappa(m)\kappa(l) = \kappa(ml) = \kappa(nk) = \kappa(n)\kappa(k)$$. Taking inverses of $$\kappa(n)$$ and $$\kappa(l)$$ now gives the required equality.
> We now observe easily that $$\lambda(m) = \kappa(m)\kappa(1)^{-1} = \kappa(m)$$ for $$m\in \mathbb Z$$, so $$\lambda$$ extends $$\kappa$$. Moreover, using the addition and multiplication $$\frac m n + \frac k l := \frac {ml +nk} {nl}$$ $$\frac m n \cdot \frac k l := \frac {mk}{nl}$$ and the homomorphism rules of $$\kappa$$, we can (tediously) derive that $$\lambda$$ indeed satisfies the homomorphism rules and also preserves the order on $$\mathbb Q$$ (which is defined as $$\frac m n \leq \frac k l$$ iff. $$ml \leq nk$$ in $$\mathbb Z$$).

So far, we have only used theory that leaned on ring theory and elementary field theory, with a slight mixing in of ordered rings, although we didn't do this all that rigorously. The main theorem which proves the uniqueness of the reals up to (order-and-field-) isomorphism, requires us to blend in some properties of the real numbers as well.

---
> **Definition** An ordered field $$F$$ satisfies the *supremum property* if every subset $$S\subset F$$ with an upper bound in $$F$$ has a *least upper bound* $$u$$ in $$F$$, that is:
 - $$s \leq u$$ for all $$s \in S$$ (that is, $$s$$ is an upper bound).
 - if $$b$$ is another upper bound of $$S$$ then $$s \leq b$$.

The next theorem hinges upon some properties that fields with the supremum property actually have. I will not go into those details. For a self-contained exposition of the real numbers and their construction, see [A Course in Mathematical Analysis, Vol. 1](https://www.cambridge.org/core/books/course-in-mathematical-analysis/C0D89CA72FF3ED2B7F3280A922CF9D5B) by Garling.


> **Theorem** Let $$F$$ be an ordered field satisfying $$0_F < 1_F$$ and the supremum property. Then $$F$$ is order- and field-isomorphic with the real numbers. That is, there is a bijection $$\mu \mathbb R \rightarrow F$$ that preserves order and the field operations $$+, \cdot$$.

> **Proof** We already have the embedding $$\lambda : \mathbb Q \rightarrow F$$. We will show that this can be extended to all the reals $$\mathbb R$$, and 