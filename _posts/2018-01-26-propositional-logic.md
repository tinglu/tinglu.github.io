---
layout: post
title: Propositional Logic
date: 2018-01-26 15:08 +0800
tags: notes propositional-logic automata tbc
---

This is supposed to be a complementary note to the post of [Models of Computation](/models-of-computation).
The notes below are from the book [Foundations of Computer Science](http://i.stanford.edu/~ullman/focs.html),
that is a good place to learn the fundamental theories of computer science.

> Propositional logic is a mathematical model that allows us to reason about the
> truth or falsehood of logical expressions [[...]](http://i.stanford.edu/~ullman/focs/ch12.pdf)

## Logical Expressions

Propositional variables and the logical constants, TRUE and FALSE, are `logical expressions`.
These are the atomic operands.

A logical expression’s meaning is a function that takes truth assignments as arguments and returns either TRUE or FALSE. Such functions are called `Boolean functions`.
For example, the logical expression E: p AND (p OR q).

### Associativity and Precedence of Logical Operators

1. NOT (highest) 
2. NAND - *commutative*
3. NOR - *commutative*
4. AND - *associative & commutative*
5. OR - *associative & commutative*
6. !
7. $\equiv$ (lowest) - *associative & commutative*

e.g. p ! q NOT p OR q is grouped (p ! q) $\equiv$ (NOT p) OR q.

The set of operators AND, OR, NOT is a `complete set`, meaning that
every Boolean function has an expression using just these operators.
NAND operator and NOR operator by itself is complete.

1. (p AND q) $\equiv$ (p NAND q) NAND TRUE
2. (p OR q) $\equiv$ (p NAND TRUE) NAND (q NAND TRUE)
3. (NOT p) $\equiv$ (p NAND TRUE)

The set of operators AND and OR by themselves are not complete.
For example, they cannot express the function NOT.
AND and OR are `monotone`, meaning that when you change Monotone any one input from 0 to function 1, 
the output cannot change from 1 to 0.
NOT is not monotone. Hence there is no way to express NOT by AND’s and OR’s.

## Truth Tables

### Karnaugh map

> There is a graphical technique for designing sum-of-products expressions from truth tables; 
> the method works well for Boolean functions up to four variables. 
> The idea is to write a truth table as a two-dimensional array called a Karnaugh map 
> (pronounced “car-no”) whose entries, or “points,” each represent a row of the truth table.

## Tautologies

A `tautology` is a logical expression whose value is true regardless of the values of its propositional variables.
For a tautology, all the rows of the truth table, or all the points in the Karnaugh map, have the value 1. 

Simple examples of tautologies are:

* TRUE
* p + $\neg$p
* $(p + q) \equiv (p + p \neg q)$

### Substitution principle

Tautologies remain tautologies when we make any substitution for one or more of its variables. 
Of course, we must substitute the same expression for each occurrence of a given variable.

### Inherent intractability

The class of problems that — like satisfiability — can be “solved” by guessing followed by a polynomial time check 
is called `NP`.

However, there are many problems in NP that can be proved to be as hard as any in NP , and these are the `NP-complete` problems.
(Do not confuse “completeness” in this sense, meaning “hardest in the class,” with “complete set of operators” meaning 
“able to express every Boolean function.”)

The family of problems solvable in polynomial time with no guessing is often called `P`.

The tautology problem is not believed to be in NP, but it is as hard or harder than any problem in NP 
(called an `NP-hard` problem) and if the tautology problem is in P, then P = NP.

## Some Algebraic Laws for Logical Expressions

### Laws of Equivalence

* Reflexivity of equivalence: $p \equiv p$.
* Commutative law for equivalence: $(p \equiv q) \equiv (q \equiv p)$.
* Transitive law for equivalence:  $(p \equiv q) AND (q \equiv r) \to (p \equiv r)$.
* Equivalence of the negations: $(p \equiv q) \equiv (\neg p \equiv \neg q)$.

### Laws Analogous to Arithmetic

* The commutative law for AND: $pq \equiv qp$.
* The associative law for AND: $p(qr) \equiv (pq)r$.
* The commutative law for OR: $(p + q) \equiv (q + p)$.
* The associative law for OR: $p + (q + r)  \equiv  (p + q) + r$.
* The distributive law of AND over OR: $p(q + r) \equiv (pq + pr)$.
* 1 (TRUE) is the identity for AND: $(p AND 1) \equiv p$.
* 0 (FALSE) is the identity for OR: $p OR 0 \equiv p$.
* 0 is the annihilator for AND: $(p AND 0) \equiv 0$.
* Elimination of double negations: $(NOT NOT p) \equiv p$.

### Ways in Which AND and OR Differ from Plus and Times

* The distributive law for OR over AND: $(p + qr) \equiv  (p + q)(p + r)$. 
* 1 is the annihilator for OR: $(1 OR p) \equiv 1$.
* Idempotence of AND: $pp \equiv p$.
* Idempotence of OR: $p + p \equiv p$.
* Subsumption:

  * $(p + pq) \equiv p$.
  * $p(p + q) \equiv p$.

* Elimination of certain negations:

  * $p(\neg p + q) \equiv pq$. 
  * $p + \neg p q \equiv p + q$.

### DeMorgan’s Laws

* NOT$(pq) \equiv \neg p + \neg q$.
* $NOT(p + q) \equiv \neg p \neg q$.

### Laws Involving Implication

* $(p \to q)$ AND $(q \to p) \equiv (p \equiv q)$.
* $(p \equiv q)\to(p\to q)$.
* Transitivity of implication: $(p \to q)$ AND $(q \to r) \to (p \to r)$.
* It is possible to express implication with AND and OR. The simplest form is:

  * $(p\to q) \equiv (\neg p + q)$.
  * (p<sub>1</sub>p<sub>2</sub> ···p $\to$ q) $\equiv$ ($\neg$ p<sub>1</sub> + $\neg$ p<sub>2</sub> +···+ $\neg$ p<sub>n</sub> + q).

  Both the left-hand and the right-hand sides of the equivalence are true whenever q is true or one or more of the p’s are false; both sides are false otherwise

## Tautologies and Methods of Proof

## Deduction

## Proofs by Resolution