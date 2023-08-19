---
layout: post
title: Set Theory 3 (Unfinished)
date: 2023-08-15 15:09:01
description: Discusses functions.
featured: true
categories: set-theory
---

## Functions

A function can now be defined as follows:

> **Function** A function $$f : A\rightarrow B$$ is a relation $$f\subset A\times B$$ such that 
> *For all* $$ x \in A$$ *!Exists* $$y \in Y$$ $$xfy$$

We just introduced a new shorthand notation, which I shall define:

> - ($$!Exists y \in Y$$ $$\varphi$$) *iff.* ($$!Exists y$$ ($$y \in Y$$ *and* $$\varphi$$))
> - ($$!Exists y$$ $$\psi$$) *iff.* (($$Exists y$$ $$\psi$$) *and* (*For all* z ($$\varphi[z/y]$$ *iff.* $$z=y$$))

That means, the $$y$$ satisfying $$xfy$$ is unique for every $$x$$.

## Axiom of replacement

Suppose we have a well-formed formula $$\varphi(x,y)$$ that only has free variables $$x$$ and $$y$$, such that for all sets $$a$$, there is at most one set $$b$$ for which $$\varphi(a,b)$$ holds. We would like to have a set $$B$$ of all such $$b$$. We immediately think of using the axiom of separation for this, but what is the set to separate the $$b$$'s from? Since there is no universal set:

> **Theorem** There is no set $$U$$ such that if $$a$$ is a set then $$a\in U$$.
> **Proof** Suppose we had such a $$U$$. Then let us use Separation to define the set $$R = \{x\in U : x\not \in x\}$$. We get Russel's paradox again by asking ourselves whether $$U\in U$$ or not.

We cannot use any of the preceding axioms to construct the desired $$B$$. But we would like to have a function $$f:D_A\rightarrow B$$, where the domain $$D_A = \{x\in A:$$ *Exists* $$y$$ $$\varphi(x,y)\}$$ can be separated from any set $$A$$.

The replacement axiom assumes that we can do it. Like the separation axiom, it is a schema axiom over all wff.'s $$\varphi$$.

> *For all* $$A$$ *Exists* $$B$$ *For all* $$b\inB$$ *Exists* $$x\in A$$ $$\varphi(x,b)$$

(Unfinished)

*In order to construct the real numbers from this point onwards, we would need to do the following:*
  - *Define for any set $$a$$, the successor to be $$a^+  := a\cup\{a\}$$. A successor set is a set $$A$$ with $$\emptyset\in A$$ and for every $$a\in A$$ also $$a^+\in A$$*. We don't know yet whether there are successor sets.
  - *Add the axiom of Infinity to assure there is an infinite set containing $$\emptyset$$, $$\emptyset\cup\{\emptyset\}$$, etcetera, so a set $$I$$ with $$\emptyset \in I$$ and for $$a\in I$$ we have $$a^+\in I$$. This is not guaranteed by the other axioms, because in formal proofs we can only use a finite number of deductions: thus, we can only construct finite sets containing "up to $$n$$" successors of $$\emptyset$$ from pairing, union and emptyset alone and need an axiom to postulate such an infinite set.*
  - *Let $$\omega$$ be the intersection of all successor sets: this set also is a successor set, since it has $$\emptyset$$ and all successors of its elements. It is also minimal.*
  - *Add the axiom of regularity, which states that if $$A\not = \emptyset$$ then there is a $$a\in A$$ s.t. $$a\cup A = \emptyset$$. From this we conclude $$a\not \in a$$ for any set $$a$$, because if it were then $$a\cup\{a\} \not = \emptyset$$, which is clearly in contradiction with the axiom.*
  - *Show that $$\omega$$ models the axioms of Peano Arithmetic (PA): in particular, we need the axiom of regularity to show $$m^+ \not = n^+$$ if $$m\not=n$$.*
  - *Construct the integers $$\mathbbZ$$ from equivalence classes of pairs of natural numbers.*
  - *Conclude that the $$\mathbbZ$$ models the commutative ring axioms, and form an infinite set. Also conclude that that $$\mathbbZ$$ forms a domain, that is, it has no zero divisors*
  - *Use the concept of the localization of rings $$R$$ at a multiplicatively subset $$S\subset R$$ to construct from $$S = \mathbbZ \backslash \{0\}$$ the rational numbers $$\mathbbQ = S^{-1}\mathbbZ$$*
  - *Define Dedekind-cuts on the rationals.*
  - *Model the real numbers as Dedekind cuts.*

In retrospect, this is far too tedious for a blog post. Read Garling or *Sets, Models and Proofs* instead. They are excellent sources on this.