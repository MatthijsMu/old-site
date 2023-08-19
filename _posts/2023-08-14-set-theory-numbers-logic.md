---
layout: post
title: Set Theory, Numbers and Logic
date: 2023-08-04 
description: An overview of my first blog series, which explains set theory, numbers and some mathematical logic.
categories: set-theory
featured: true
---

**Update: I have decided to discontinue the series on Set Theory, Numbers and Logic. In particular, I left points 5-11 of the below outline unfinished. The reason for this is that I am currently following further advanced courses and would like to continue blogging about those. Right now, I feel that the rigorous approach that I have been trying to enforce while writing my blogs has deprived them from interest. I would like to write things more summarizingly sometimes, but that makes me wonder what the actual goal of these posts is in the first place. I did not want to develop a 6 EC course within a blog series, but the material that I originally planned to discuss is taught during two 6 EC courses at my University, and only now I realize that this also means a blog on the subjects would become very lengthy.**

**I think that developing all theory step-by-step such as taught in a course may be a better fit for textbooks than for blog posts anyway. I want to keep you, the reader, interested and excited about mathematics and I feel that the way I have been writing blog posts so far is a very different approach: the blogs are long, wordy and bureaucratic. Which is perfect if you dive into a course with zero knowledge on the subject and want to learn everything step-by-step. But this has never been the aim of my blog, honestly.**


### An Overview of the blogs on Set Theory, Numbers and Logic

As a first subject to write about in my blog, I chose to do a series on set theory, logic and models for number systems. These are based on courses I followed at Radboud University during my pre-university studies (Introduction to Mathematics and Numbers) and first year (though Mathematical Logic is actually a 2nd year course). My idea is to go from sets to numbers to logic and then to sets again. In particular:

 1. We start with the basic axioms of set theory, which are formulated in natural language. We need in particular *Extensionality*, *Empty set*, *Pairing*, *Union*, *Power set* and *Separation*, to be able to construct the *Cartesian product* of two sets.

 2. This is followed by the intruction of *relations* as subsets of the Cartesian product. We will consider pre-orders, partial and total orders, equivalences, partitions.

 3. Next are *functions*, which are in fact also a relation. We discuss composition, injections, surjections and bijections. We also introduce the axiom called *Replacement*, which is a natural thing to do in the light of functions. I will also explain in detail why we need it.

 4. We then move on to some classical results in set theory, such as the *Schröder-Cantor-Bernstein theorem*, and Cantor's diagonal argument. I *may* introduce *Cardinal numbers*, which are a way of expressing theorems about the sizes of sets. It depends on whether I would like to first develop some examples on various infinite sets, which will likely be done in a much later post.

 5. Fifth, we introduce yet two more axioms: the *Foundation axiom* and the *axiom of Infinity*. We use these to construct a *model* for the natural numbers. What exactly a *model* is, I shall discuss much later. We prove some theorems on the natural numbers and derive a *principle of induction*, which is actually also one of the *axioms* in the theory of the natural numbers (PA).

 6. I would like to delay the introduction of the *Axiom of Choice* (AC) for as long as possible, but I think the reader needs to be aware of its existence to understand that care needs to be taken when reasoning about infinite sets and functions. It is easy to think every surjection has a section, and when working with infinities one might just apply AC without even noticing. The axiom however, has dramatic consequences, which I shall discuss in the same blog.

 7. We next discuss the theory of well-ordered sets and some variations on the principle of induction for w.o.'s and principles of recursion on w.o.'s.

 8. Finally, we prove *Hartogs's lemma*, which is a result on w.o.'s that does not rely on AC. We can use Hartogs's lemma to show equivalence of AC with Zorn's lemma, Principle of Cardinal Comparability (also known as *Trichotomy*) and the *Well-Ordering Theorem* (also known as *Zermelo's Theorem*). This is all inspired on the proof given in [*Sets, Models and Proofs*](https://link.springer.com/book/10.1007/978-3-319-92414-4), chapter 1.5.

 9. I want to continue with a quick sketch of how to construct the integers, then the rational, then the real numbers from the natural numbers. Garling's [A Course in Mathematical Analysis](https://www.cambridge.org/core/books/course-in-mathematical-analysis/C0D89CA72FF3ED2B7F3280A922CF9D5B) devotes an entire chapter to this, so this may take much longer than expected. At that moment, I may just want to move on and end the blog, so I won't promise anything yet.

 10. We will go over the definition of *wff.*'s in a formal logical language $$L$$, and define theories, $$L$$-structures, interpretations and models for theories. I think examples are particularly important, to show that a theory in general has a multitude of models.

 11. I will skip much of the bureaucracy that a 2nd year student taking Mathematical Logic has to go to, because I want to keep my readers. Probably, I will jump straight to Gentzen-style natural-deduction-proof-trees and formally prove a few theorems. I will definitely not discuss soundness or completeness of natural deduction, but maybe I will give a sketch.

These blogs are intended to be read in an approximately linear fashion, starting from Set Theory, through Number Theory to Logic. I based these blogs on the following material:

 - Set Theory: based on lecture notes of Klaas Landsman, [*Introduction to Mathematics*](https://www.math.ru.nl/~landsman/InleidingWiskunde2017.pdf), as well as Garling's [*A Course in Mathematical Analysis, Volume I*](https://www.cambridge.org/core/books/course-in-mathematical-analysis/C0D89CA72FF3ED2B7F3280A922CF9D5B), Part 1, Chapter 1.

 - Numbers:  based on *Introduction to Mathematics* and Garling's A Course in Mathematical Analysis, Volume I, Part1, Chapter 2.

 - Logic: based on [*Sets, Models and Proofs*](https://link.springer.com/book/10.1007/978-3-319-92414-4).

These books were used in the first-semester course Introduction to Mathematics and the 4th-semester course Logic taught at Radboud University. They inspired me to write a blog, because the rigorous development of sets, functions, number systems, etcetera, is in many ways highly nontrivial and subtle, especially when dealing with infinite sets. The Logic blog is in some sense a natural continuation of rigorous set theory, but can also be read as a separate blog on formal logic (i.e. proofs and model theory). It does however also require an informal development of sets to start with, and the number systems discussed in Numbers will certainly provide us with some interesting models for various theories.

---

#### Sets 

This blog starts with an axiomatic definition of sets. It will then continue to explore set theory, diving into relations, including orders, equivalences and functions. Once functions have entered the play, we introduce the axiom of choice and various equivalent axioms, and study their equivalence using the theory of well-orders.

When defining sets, the aim is not to explain "what" a set is, but rather to specify the axioms according to which sets behave. The notion of a set is simply too abstract to describe intuitively, moreover if we want to do rigorous mathematics with them then the last thing we want is only an intuitive notion: we need the laws according to which the object behaves to make rigorous derivations of other properties.

---

#### Logic

This separation between intuition and formalism will be taken a step further when we will discuss logic. In first order logic, we study theories, which are sets of logical sentences, another mathematical object, in two ways: one way is to interpret the sentences using a model, that is, we define what it means for a sentence to hold in a structure and if a structure satisfies all sentences of the theory, it "models" the sentence. This is a good way to create an intuition of the objects that are described by the theory. The other way of studying theories looks not at interpretations of sentences but at rules for recombining logical sentences into new logical sentences. The recombination leads us to definitions such as proof trees and ultimately to the notion of a formal "proof" of a "theorem".

If you have not seen mathematical logic yet, be aware that these proofs are different from a proof using natural language that is usual in mathematics texts. Formal proofs in logic are mathematical objects that we can again prove properties/theorems about in natural language. In a sense, we study the way we prove and reason about mathematical things from a meta-level.

---

#### Numbers

Even though logic studies mathematical reasoning at the lowest possible level of abstraction, a rigorous course in model theory and proof trees is usually only given in the second year of a mathematics B.S. The reason is that students should be familiar with theories such as those of the natural, integer, rational and real numbers before being exposed to formal theories and models for them. These number systems are the focus of Numbers. I highly recommend reading this blog secondly, before continuing to Logic. The real number system will also provide the foundations for Mathematical Analysis (which is, not coincidentally, the actual subject of Garling's book), which I might discuss in a later blog, although I think I will by then prefer to tell you something about more advanced material.

---

#### Further Studies

The theory of rings, groups, fields and ordered fields in these posts is only developed for the integer, rational and real number systems, Much more can be said about these theories in, say, the courses at Radboud such as Group Theory and Rings and Fields. It is unfortunate. If you do like to know, you should consider studying mathematics (advertising intended)! 