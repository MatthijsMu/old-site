---
layout: post
title: Set Theory 3
date: 2023-08-15 15:09:01
description: Discusses relations, in particular orders, equivalences and functions.
tags: set-theory, functions, bijections, injections, surjections
categories: set-theory
featured: true
---

###### Functions

A function can now be defined as follows:

> **Function** A function $$f : A\rightarrow B$$ is a relation $$f\subset A\times B$$ such that 
> *For all* $$ x \in A$$ *!Exists* $$y \in Y$$ $$xfy$$

We just introduced a new shorthand notation, which I shall define:

> - ($$!Exists y \in Y$$ $$\varphi$$) *iff.* ($$!Exists y$$ ($$y \in Y$$ *and* $$\varphi$$))
> - ($$!Exists y$$ $$\psi$$) *iff.* (($$Exists y$$ $$\psi$$) *and* (*For all* z ($$\varphi[z/y]$$ *iff.* $$z=y$$))

That means, the $$y$$ satisfying $$xfy$$ is unique for every $$x$$