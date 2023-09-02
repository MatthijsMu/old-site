---
layout: post
title: Logic, Models, Proofs
date: 2023-08-04
description: Something about logic and models for logics (Unfinished)
categories: logic
featured: false
toc: 
  sidebar: right
bibliography: 2023-08-14-logic-model-theory.md
---
## Introduction and Motivation
As a sample blog post, I will dive into formal first-order logic.

The idea of mathematical logic is to make a formal language $$L$$ which contains all "logical sentences", and set up rules to manipulate sentences in this language. An example of such rules are proof trees modelling natural deduction, a technique we all find very "logical" and we use in all our everyday reasoning. Basically, we formalize reasoning by formulating it as a formal system and then we will argue about the strengths and weaknesses of that system. An example would be: *what properties can we not express in a sentence of this-or-that form?* It turns out, as we will see below, that first-order logic, for example, does not have a theory that can express that its model is a well-order. We understand what a well-order is, and yet we cannot define it by using first-order sentences alone.

Why do we want to abstract logic and then study it from a distance using the same logical reasoning that this systems tries to model? Two example reasons: 
 - First, to understand where our own mathematical reasoning can bring us. What *assumptions* (axioms) need to be part of a theory in order to derive certain *conclusions*? From a meta-level, what ideas can we *express* in first-order-logic? The theory of well-orders, for example, requires an axiom that states a property of subsets of a set (every subset of a well-ordered set has a least element). We will see that no equivalent statement exists in f.o.l., which is interesting: it means that we need a more expressible formal system to specify certain ideas.
 - Second, we may want to automate reasoning and/or theorem-proving. To let computers do this for us, we need a formal framework to model the reasoning process. The programming language Prolog allows you to define *facts* as instances of *atoms* and *rules* as clauses of *atoms*. It bases computations on the set of its facts, the so-called *knowledge base*. As we will see, there is a very tight correspondence between  Prolog's atoms and f.o.l.'s *relation symbols*, rules and *wff*'s, the knowledge base and a *structure*, and finally between the *querying* of a Prolog program and the *interpretation* of a sentence in a model.

What does this look like in practice? That is what I want to tell you about in this post. It will basically comprise a very brief summary of the 2nd-year course in mathematical logic one gets to study at Radboud University. 

---

## Defining a logical language


I will base the definitions on <d-cite key="moerdijk2018sets"></d-cite>, which was the book studied at my university. The book is super-rigorous and will start defining logical sentences in polish notation, because that uniquely fixes the structure of a sentence without the need of bracketing subclauses. Only then will they justify the bracketing notation, by tediously provjng the bijective correspondence between bracketed sentences and polish sentences. 

The book is in my opinion maybe a bit too rigorous, and I will skip this approach. You can probably justify the bracketed notation for yourself and this blog should not become more bureaucratic than a logic blog already is. Instead, I will define terms and wff's in the way most logic textbooks approach it, that is, using some unspoken of brackets in the right place to enforce syntax.

By the way, I assume you are familiar with some set theory. If not, the first chapter of <d-cite key="garling2013course"></d-cite> excellent and almost as rigorous as it gets for a formal introduction.

> **Definition** A *language* $$ L $$ is a triple $$(\text{con}(L),\text{fun}(L),\text{rel}(L)) $$, where:
> - $$\text{con}(L) $$ is a set of *constants* or *constant symbols*.
> - $$\text{fun}(L) $$ is a set of *function symbols*.
> - $$\text{rel}(L) $$ is a set of *relation symbols*.
> Every relation or function symbol comes with an *arity*, which is a number $$n\in \mathbb N _{\geq 0}$$ 

Practically, we could do without constants and use function symbols with arity 0 instead. However, the distinction (or, different naming) is often useful in proofs that have differing cases for terms that are constants and terms that are functions applied to terms.

As an example, we could look at the language $$ L_{group} $$ that has one constant, $$ e $$ for the identity, one function symbol $$ \circ $$ for group multiplication (we will write it infix, don't worry), and no relation symbols.

Additionally, we will use other symbols. We assume that all symbols are distinct, so that they are unambiguous to recognize in terms and formulas. These additional symbols are:

 - *Variables*, which is some countably infinite set of symbols
 - The *equality symbol* $$ = $$
 - The *falsum* or *absurdity* symbol $$\bot$$
 - Logical *connectives*, which are $$\land,\lor,\lnot,\rightarrow$$
 - *Quantifiers*, which are in f.o.l. the *universal* quantifier $$\forall$$ and the *existential* quantifier $$ \exists $$

> **Definition** We denote the union of the set of $$L$$'s symbols, the variables, the equality symbol, falsum, the connectives and the quantifiers, $$ \mathcal{C}_L$$. 

> **Definition** (Kleene star notation) We denote the set of all finite strings over some alphabet $$\mathcal{A}$$, $$\mathcal{A}^* = \cup_{n=0}^\infty \mathcal{A}^n$$ .


### The Terms

> **Definition** The set of $$ L $$-terms of a language $$ L $$ is the smallest subset $$ T $$ of $$\mathcal{C}^*_L$$ such that:
> 1. If $$c \in \text{con}(L)$$ then $$ c \in T $$
> 2. If $$x $$ is a variable then $$x \in T$$
> 3. If $$t_1, ... , t_n \in T $$ and $$f \in \text{fun}(L)$$ with arity $$n$$, then $$f(t_1,...,t_n) \in T $$

I want to note two things:

- The above objects are all (purely syntactical) strings of symbols! This is why the bracketed notation is a tad bit non-rigorous, because for example what is the exact length of such a term, do or don't we include bracket tokens? And where do the bracket symbols $$ ($$ come from $$) $$ in the first place? They are not in the alphabet!

 - Next, why is there a "smallest subset"? The definition means "smallest" in the sense that any set that satisfies 1., 2. and 3., must contain $$T$$ as a subset. Now why would such a set exist? Think about it in this way: if $$ \{T_\alpha\}_{\alpha\in I}$$ is some collection of sets that all satisfy 1. and 2. and 3., then $$T = \cap_{\alpha\in I}T_\alpha$$ will also contain all variables and constants, and since terms that are in $$T$$ are in $$T_\alpha$$ for all $$\alpha\in T$$, concatenation with a function symbol will keep them in all $$T_\alpha$$ and hence in $$T$$. That is why there must be a minimum set: if not, we take the intersection of two different minimal sets and reach a contradiction!

 Finally, for readers who are familiar with formal language theory in the field of computer science, yes the language defined above is indeed a *context-free* language generated by a context-free grammar. Computer scientists would maybe also want to do something about the ambiguity in the grammar that arises when no brackets are involved. On the other hand, they might want to think of sentences as an abstract syntax tree. In that case the polish notation would again make sense, because if we define the logical language as sentences in polish notations, we get a CFG with no need for brackets, namely with the grammar rules:

$$
 T \rightsquigarrow_1 f T T ... T 
$$

$$
 T \rightsquigarrow_2 c
$$

$$
 T \rightsquigarrow_3 x
$$

Where on the right-hand side of the first rule "$$\rightsquigarrow_1$$" there are exactly $$n$$ non-terminal symbols $$T$$, and this rule is defined separately for every $$n$$-ary symbol $$f\in \text{fun}(L)$$ (making it, in fact, a *rule schema*) and the rule "$$\rightsquigarrow_2$$" for every constant $$c\in \text{con}(L)$$, and the rule "$$\rightsquigarrow_3$$" for every variable $$x$$. We can show that this CFG is non-ambiguous, and a term in this notation corresponds to the depth-first traversal of the AST of the formula. You could parse this language $$T$$ in linear time from left to right, I suppose.

Next, we define what it means to substitute a term $$s$$ into a constant or variable $$x$$ in a given term $$t$$.


> **Definition** (Substitution on $$L$$-terms) For $$t, s \in T$$ and $$x$$ a variable or a constant of $$L$$: We define $$t[s/x]$$, the *substitution of* $$s$$ *for* $$x$$ *in* $$t$$, as:
> - if $$t = f(t_1, ..., t_n)$$ for some $$n$$-ary function symbol $$f$$ and terms $$t_1, ... , t_n \in T$$, then $$t[s/x] = f(t_1[s/x], ... , t_n[s/x])$$. 
> - if $$t $$ is a constant or a variable, then if $$t = x$$ we have $$t[s/x] = s$$ and otherwise $$t[s/x] = t$$

We can prove by *structural* induction on terms that this immediately implies that $$t[s/x]$$ is again in $$T$$ for $$t,s\in T$$ and $$x$$ a constant or variable. The principle of structural induction means that we have to prove the statement for all constants and variables $$t$$ and also that the statement holds for $$f(t_1, ... t_n) \in T$$ for all $$f\in \text{fun}(L)$$ $$n$$-ary, if it already holds for $$t_1, ... t_n$$.

---
## The formulas

For $$L$$-formulas, we give a very similar definition that makes use of $$L$$-terms.

> **Definition** The set of formulas $$F$$ of a language $$L$$ is the smallest subset of $$\mathcal{C}_L^*$$ satisfying:
> - It contains all *atomic formulas*, which are strings of the form:
>   1. $$t = s$$ for $$s, t \in T$$.
>   2. $$R(t_1, ..., t_n)$$ for $$R \in \text{rel}(L)$$ an $$n$$-ary relation symbol and $$t_1,...,t_n\in T$$.
>   3. $$\bot$$.
> - It contains all strings of the form:
>   4. $$\varphi \land \psi$$, $$\varphi \lor \psi$$, $$\varphi \rightarrow \psi$$, $$\lnot \varphi$$, for $$\varphi, \psi \in F$$.
>   5. $$ \forall x \varphi$$, $$\exists x \varphi$$ for $$x$$ a variable, $$\psi \in F$$.

The reason that this is well-defined is because again, when a collection of sets satisfying 1. to 5. is intersected, this intersection satisfies 1. to 5. as well.

The set of formulas, when we regard them as written in their polish notation, is again a non-ambiguous context-free language. We can think of them as being generated by:

$$
F \rightsquigarrow \land F F
$$

$$
F \rightsquigarrow \lor F F
$$

$$
F \rightsquigarrow \rightarrow F F
$$

$$
F \rightsquigarrow \lnot F
$$

$$
F \rightsquigarrow \exists x F
$$

$$
F \rightsquigarrow \forall x F
$$

$$
F \rightsquigarrow = T T
$$

$$
F \rightsquigarrow r T T ... T 
$$

$$
F \rightsquigarrow \bot
$$

The last rule is again a *rule schema* that is defined for every $$n$$-ary relation symbol $$r\in \text{rel}(L)$$. On the right-hand side of the $$\rightsquigarrow$$, we have precisely $$n$$ nonterminals $$T$$. The grammar for $$T$$ has been shown above.

By minimality of the set of $$L$$-formulas, we can have an induction principle on $$F$$, just like we had on $$T$$! That is:

> **Induction Principle on Formulas** If some statement holds for all atomic formulas, and if we have that (if the statement holds for $$\varphi \in F$$ and $$\psi \in F$$, then it holds for $$\varphi \land \psi$$, $$\varphi \lor \psi$$, $$\varphi \rightarrow \psi$$, $$\lnot \varphi$$, $$ \forall x \varphi$$ and for $$\exists x \varphi$$ for $$x$$), then the statement holds for all $$L$$-formulas.
> **Proof** Let $$E$$ be the set of $$L$$-formulas for which the statement holds. Since it is given that $$E$$ satisfies 1. to 5., it follows by minimality of $$F$$ that $$F\subset E$$, so we are done.

With this induction principle, it is now simple to prove a recursion principle:

> **Recursion Principle on Formulas** Let $$V$$ be the set of variables, $$A$$ the set of atomic formulas and $$X$$ some arbitrary set, with functions:
> $$f_a:A \rightarrow X$$
> $$f_\land, f_\lor, f_\rightarrow: X\times X\rightarrow X$$
> $$f_\lnot: X \rightarrow X$$
> $$f_\forall, f_\exists: V\times X \rightarrow X$$
> Then there is a unique function $$f: F\rightarrow X$$ that satisfies:
> $$f(\varphi) = f_a(\varphi)$$ if $$\varphi $$ is atomic
> $$f(\varphi) = f_\land(f(\chi), f(\psi))$$ if $$\varphi$$ is $$ \chi \land \psi$$
> $$f(\varphi) = f_\lor(f(\chi), f(\psi))$$ if $$\varphi$$ is $$\chi \lor \psi$$
> $$f(\varphi) = f_\rightarrow(f(\chi), f(\psi))$$ if $$\varphi$$ is $$\chi \rightarrow \psi$$
> $$f(\varphi) = f_\lnot(f(\psi))$$ if $$\varphi$$ is $$\lnot \psi $$
> $$f(\varphi) = f_\forall(x, f(\psi))$$ if $$\varphi $$ is $$ \forall x \psi$$
> $$f(\varphi) = f_\exists(x, f(\psi))$$ if $$\varphi $$ is $$\exists x \psi$$

Note: try not to confuse (meta) equalities the *equality symbol* that is used inside the sentences!

> **Proof** (Sketch) 
> - Unicity: let $$f,g$$ both satisfy the recursion and let $$\psi$$ be the *shortest* formula for which $$f(\psi) \not = g(\psi)$$. If $$\psi$$ is atomic, we see that this is not possible because $$f,g$$ are both fixed as $$f_a$$ on atomic formulas. So $$\psi$$ is composite. We can exhaust all cases and in every case we have to conclude that not all shorter subformula of $$\psi$$ can have the exact same image under $$f$$ as under $$g$$, hence $$\psi$$ is not shortest and we get a contradiction.

This means we can now safely define functions by recursion on formulas. The principle example of this is of course substitution:

> **Substitution on $$L$$-formulas** For $$\varphi \in F$$, $$x\in V$$ and $$s\in T$$, we define $$\varphi[s/x]$$ by recursion:
> - $$\varphi[s/x]  = (t_1[s/x]=t_2[s/x])$$ if $$\varphi = (t_1=t_2)$$ and likewise we just substitute on terms for other atomic formulas.
> - $$(\psi _ \chi)[s/x] = (\psi[s/x] _ \chi[s/x])$$ for $$_ \in \{\land,\lor,\rightarrow\}$$
> - $$(\lnot \psi)[s/x] = \lnot(\psi[s/x])
> - $$(\forall y \psi)[s/x] = \forall y (\psi[s/x])$$ if $$y \not = x$$, otherwise $$(\forall x \psi)[s/x] = \forall x \psi$$
> - $$(\exists y \psi)[s/x] = \exists y (\psi[s/x])$$ if $$y \not = x$$, otherwise $$(\exists x \psi)[s/x] = \exists x \psi$$

The above definition is interesting: rather than defining what a *bound* variable is and explaining why bound variables should not be substituted, we first explicitly state substitution rules, and from this we can define what it means for a variable to *occur* in a formula and to be *bounded*. A variable is bounded exactly when it is *not replaced by substitution*!

> **Definition** (occurrence, bounded, free, closed) Let $$\varphi \in F$$. 
> - An *occurrence* of $$x\in V$$ in $$\varphi$$ is a natural number $$i$$ such that the $$i-th$$ elemnt of the string $$\varphi$$ is $$x$$.
> - If $$i$$ is an occurrence of $$x$$ in $$\varphi$$, and let $$u\in V$$ not occur in $$\varphi$$. Then the occurrence $$i$$ of $$x$$ is called *bound* if the $$i$$-th element of $$\varphi[u/x]$$ is $$x$$.
> - *free* occurrences are those occurrences that are not bound.
> - A formula that has no free occurrences of any variable $$x\in V$$ is called *closed*, or also a *sentence*.

## Legitimacy of substitutions

Next, I will point out something that will immediately make you think that there is a hiat in our definitions so far.
Suppose I give you the formula $$ \varphi = \forall x R(x,y)$$. It expresses a property for $$y$$, you could say. It should mean the same as $$\forall x R(x,y)$$. Yet if we substitute the term $$f(x,z)$$ for $$y$$, we get two formulas that seem to have wildly different semantics:

> $$\forall x R(x, f(x,z))$$ versus $$\forall u R(u, f(x,z))$$

This cannot be right. The problem is precisely that occurences of some variables (namely $$x$$) in the substituted term $$t$$ get bound in $$\varphi[t/y]$$. Otherwise, this bifurcation of semantics would not happen. Hence, we make this type of substitution *non-legitimate*. Closed terms (no free variables in the term, that is) are of course always legitimate to substitute.

## Structures and interpretation: defining truth

So far, we have not yet spoken of the semantics or meaning of f.o.l.. Only in the previous section, I alluded to it in order to motivate legitimacy of substitutions, but I could have also thrown it at you without any motivation, and if you were naive and didn't have an idea of what the logical connectives etcetera *meant*, we might as well have used completely different symbols with no apparent semantics such as tables, chairs and beer mugs, and once we have developed a formal system to manipulate these syntactically it would make no difference as to which relationships between the names we would be able to derive.

But we want to do logic because we want to understand our reasoning, and we want to be sure that the abstract systems defined to study it, actually matches our interpretation (semantics) or "understanding" of logic, whatever that means. As mathematicians, we value a rigorous definition of this "interpretation" and these "semantics" as well, so we develop them using *model theory*.

The basic idea is that we take, on our meta-level from which we study the formal wff's, a meta-level set with on it $$n$$-ary functions and relations (not the symbols, but actual functions and relations from set theory). These we call the *interpretations* of the function and relation *symbols* in the logical sentences. We then recursively define what it means to interpret *terms*, then *closed formulas* and from this we derive a definition of truth. The development of this theory is largely attributed to Tarski. Let's begin.

---

> **$$L$$-structure** an $$L$$-*structure* $$M$$ is a *nonempty* set $$M$$ together with:
> - for every constant $$c \in \text{con}(L)$$ an element $$c^M\in M$$.
> - for every $$n$$ for every $$n$$-ary function symbol $$f\in\text{fun}(L)$$ a function $$f^M:M^n \rightarrow M$$.
> - for every $$n$$ for every $$n$$-ary relation symbol $$R\in \text{rel}(L)$$ a subset $$R\subset M^n$$

We call $$c^M$$ the interpretation of $$c$$ in $$M$$, and the same goes for $$f^M$$ and $$R^M$$.
first, we define the language $$L_M$$ for $$L$$ a language and $$M$$ an $$L$$-structure to be the language that has the same function symbols and relation symbols of $$L$$, but now as its constants it has $$\text{con}(L_M) = \text{con}(L)\cup M$$. As an example, we can take $$L_{ring}$$, the language of rings, and consider the $$L$$-structure $$M = \mathbb{Z}_3 = \{\bar0, \bar1\, \bar3\}$$. Then $$\text{con}(L_M) = \{0,1,\bar0,\bar1,\bar3\}$$. 

Note that if we define $$m^M = m\in M$$ for $$m\in \text{con}(L_M)$$, then $$M$$ is also an $$L_M$$-structure and this is how we will interpret $$L_M$$-constants in $$M$$ always. This allows us to recursively define interpretations of *closed* $$L_M$$-terms:

> **Interpretation of closed terms** For $$t$$ an $$L_M$$-term we define the interpretation $$t^M\in M$$ as:
> - for $$t = f(t_1,...,t_n)$$, we set $$t^M = f^M(t_1^M, ..., t_n^M)$$
> - for $$t $$ a constant $$c\in \text{con}(L)$$, we set $$t^M = c^M$$

This is well-defined and unique by structural recursion on terms. Also, note that $$t^M\in M$$ can be proved by recursion on terms as well. Finally, I want to remark that there is really not much we can do to interpret *open* terms. How should we interpret an arbitrary variable anyway? Such an interpretation can for example not depend on the free variable $$x$$ itself, because we want $$t[u/x]$$ and $$t$$ to have the same semantics, for $$u$$ another variable that is not yet in $$t$$.

Next comes "Tarski's definition of truth". We write $$M\models\varphi$$ and say that $$M$$ *satisfies* $$\varphi$$, $$\varphi$$ *holds in* $$M$$ or also that $$\varphi$$ *is true* in $$M$$. 
If not $$M\models \varphi$$, that is $$M$$ does not satisfy $$\varphi$$, then we write $$M\not\models\varphi$$.

> **Interpretation of closed formulas** For a closed $$L_M$$-formula $$\varphi$$ we define the relation $$M\models \varphi$$ as:
> - If $$\varphi$$ is a closed atomic formula, it is either $$\bot$$, $$(t_1=t_2)$$ or $$R(t_1,...,t_n)$$ for $$t_1, ... , t_n$$ closed $$L_M$$-terms. So define:
>   - $$M\models\bot$$ never holds.
>   - $$M\models (t_1= t_2) iff. $$t_1^M = t_2^M$$ in $$M$$.
>   - $$M\models R(t_1, ... , t_n)$$ iff $$(t_1^M, ... , t_n^M )\in R^M$$
> - If $$\varphi$$ is not atomic, it is either a connective followed by one or two closed $$L_M$$-formulas or it is one of the two quantors followed by a variable $$x$$ and a $$L_M$$-formula that may have only $$x$$ as a free variable but does otherwise not have any other variables with free occurences. Hence define:
>   - $$M\models(\chi\land\psi)$$ iff $$M\models\chi$$ and $$M\models\psi$$
>   - $$M\models(\chi\lor\psi)$$ iff $$M\models\chi$$ or $$M\models\psi$$
>   - $$M\models(\chi\rightarrow\psi)$$ iff $$M\models\psi$$ whenever $$M\models\chi$$
>   - $$M\models(\lnot\psi)$$ iff not $$M\models\psi$$, hence iff $$M\not\models\psi$$.
>   - $$M\models\forall x \psi$$ iff $$M\models\psi[m/x]$$ for all $$m\in M$$
>   - $$M\models\exists x \psi$$ iff $$M\models[m/x]$$ for any $$m\in M$$

> **Validity and logical equivalence** 
> - An $$L$$-formula $$\varphi$$ we call *valid* if for every $$L$$-structure $$M$$ and every substitution of constants of $$M$$ for free variables in $$\varphi$$ we have $$M\models \varphi$$.
> - Two $$L$$-formulas $$\chi$$ and $$\psi$$ are called equivalent if the formula $$\chi\leftrightarrow\psi$$ is valid.

Here we have the shorthand notation $$\chi\leftrightarrow\psi \equiv (\chi\leftarrow\psi) \land (\chi\rightarrow\psi)$$

One can then very tediously and bureaucratically prove all the logical equivalences of formulas that we already know very well intuitively, and I will not do that here.

An $$L$$-theory $$T$$ is simply a set of closed $$L$$-formulas. $$M$$ is called a *model* for $$T$$ if it is an $$L$$-structure that satisfies all formulas in $$T$$. We call $$T$$ *consistent* if it has ("admits") a model.

An interesting theorem says that $$T$$ is consistent iff. every one of its finite subtheories is consistent. This is the so-called *compactness theorem*, which can indeed be proven via the compactness theorem from topology. Another way of proving it is with *ultrafilters*. A third way is to just use proof theory and 



---

## Well-orders and the limitations of first-order logic

A well-order is a set $$W$$ with an order relation $$\leq$$ on it such that for every nonempty subset $$S\subset W$$, $$S$$ has a *least* element $$l\in S$$, i.e. for all $$s\in S$$ we have $$l\leq s$$. Writing this theory in f.o.l. requires us to make a statement about all *subsets* of $$W$$ rather than all *elements* of $$W$$. This is nontrivial, because in the interpretation semantics we described above, we *quantify over the elements of a model*. So can it be done? The answer is no, and the proof of it makes use of the compactness theorem.

## Proofs

We say that $$T\models \varphi$$ for $$T$$ a $$L$$-theory and $$\varphi$$ a $$L$$-sentence, if for every model $$M$$ that satisfies $$T$$'s axioms, we have that $$M\models \varphi$$.

Many frameworks are used to *formally* prove logical sentences. For such a framework to be acceptable, we want it to be able to be able to generate a formal proof from $$T$$ for all $$\varphi$$ such that $$T\models\varphi$$ (completeness), and vice versa we want it to not be stronger than that, that is, if $$T\vdash \varphi$$ (this is the notation we will use for "there exists a proof for $$\varphi$$ from assumptions in $$T$$") then $$T\models\varphi$$ (soundness).

The set of proofs is defined as the smallest subset of the set of all *marked trees* over $$F$$, that satisfies certain closure properties. I will now explain what marked trees are in this context (if you study cs/discrete maths, you are probably aware that there are 1001 definitions for trees and all sorts of variations) and what the closure properties are.

> **Tree** A (rooted) tree over a set $$X$$ is a finite subset $$Y\subset X$$ that is:
> - partially ordered, 
> - and has a least element,
> - and for any two $$x,y\in Y$$ then $$\{x,y\}$$ has an upper bound iff. $$x\leq y$$ or $$y\leq x$$.

We can display a tree as its Hasse diagram. You can then see that the third rule assures that there is no way two "upward branches" can meet again when we go up the diagram, because 

The definition given in *Sets, Models and Proofs* is somewhat different, though I believe that because $$Y$$ is finite, you might be able to prove that the two definitions are equivalent:

**Tree (Sets, Models, Proofs)** A tree over a set $$X$$ is a finite subset $$Y\subset X$$ that is: 
> - partially ordered,
> - for any $$x\in Y$$ the set $$\downarrow (x) = \{y\in Y:y\leq x\}$$ is well-ordered.

The *maximal elements* $$L\subset Y$$ of the tree are called *leaves*, they will act as our assumptions. Important is that assumptions can be *marked*, so we need to extend our trees to include a *marking function*.

> **Marked Tree** is a tree with a function $$f:L\rightarrow \{0,1\}$$ or equivalently a set $$L_{marked}\subset L$$.

It is very difficult to get a picture of these abstract definitions in your head. So let me include a picture of a marked tree over $$F$$, some set of abstract formulas. My example also happens to be a proof tree, but I will get back to that later.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/prooftree-example.png" title="proof tree image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

In the example, the bottom layer is the root, and the partial order relation is precisely displayed by having two formulas $$\chi$$ and $$\psi$$ such that $$\chi < \psi$$ and that there is no such $$\xi$$ that $$\chi < \xi < \psi$$ put in the diagram in such a way that $$\psi$$ is on a level immediately above $$\chi$$ and reachable from $$\chi$$ in one step up the tree. Marked leaves are shown as elements [between rectangular brackets]. You should ignore the numbering for now, it is not part of the tree definition in any way.

> **Proof Tree** Given a logical language $$L$$, the set $$\mathcal T$$ of proof trees is the smallest set of marked trees over the set of $$L$$-formulas that satisfies the following closure properties:

 - Actually, you can look up the definition in the freely downloadable book [*Sets, Models and Proofs*](https://www.a-eskwadraat.nl/Onderwijs/Boekweb/Artikel/48/Dictaat/Downloaden). I don't want to literally copy their definition here, that feels too much like plagiarism.
 - The general idea is that every connective and every quantor has an introduction rule to introduce it into a formula, and an elimination rule to eliminate it from a formula. Then there are assumption trees and finally a valid proof is a proof tree where all assumptions (leaves) except maybe those that occur in the *assumed* theory $$T$$ and/or the formula $$\exists x (x=x)$$ have been marked. 

 Once you have read the above definition in the provided book, you can check for yourself that the example tree I provided earlier is indeed a proof tree. It is a proof, or better, a proof schema (since it is more of a template where $$\varphi$$ is a completely abstract formula) of the *law of excluded third*. Any proof for this law makes use of $$\lnot E$$, and that is also why in *Intuitionistic Logic*, where $$\lnot E$$ is not one of the closure laws for proof trees, the law of excluded third does not hold.