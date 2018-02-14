---
layout: post
title: Propositional Logic...
date: 2018-01-26 15:08 +0800
tags: notes propositional-logic automata
---

This is supposed to be a complementary note to the post of [Models of Computation](/models-of-computation).
The notes below are from the book [Foundations of Computer Science](http://i.stanford.edu/~ullman/focs.html),
that is a good place to learn the fundamental theories of computer science.

> Propositional logic is a mathematical model that allows us to reason about the
> truth or falsehood of logical expressions [[...]](http://i.stanford.edu/~ullman/focs/ch12.pdf)

## Logical Expressions

Propositional variables and the logical constants, TRUE and FALSE, are `logical expressions`. 
These are the atomic operands.

A logical expression’s meaning is a function that takes truth assignments as arguments 
and returns either TRUE or FALSE. Such functions are called `Boolean functions`. 
For example, the logical expression E: p AND (p OR q).

### Associativity and Precedence of Logical Operators

1. NOT (highest) 
2. NAND - *commutative*
3. NOR - *commutative*
4. AND - *associative & commutative*
5. OR - *associative & commutative*
6. !
7. $$\equiv$$ (lowest) - *associative & commutative*

e.g. p ! q NOT p OR q is grouped (p ! q) $$\equiv$$ (NOT p) OR q.

The set of operators AND, OR, NOT is a `complete set`, meaning that
every Boolean function has an expression using just these operators.
NAND operator and NOR operator by itself is complete.

1. (p AND q) $$\equiv$$ (p NAND q) NAND TRUE
2. (p OR q) $$\equiv$$ (p NAND TRUE) NAND (q NAND TRUE)
3. (NOT p) $$\equiv$$ (p NAND TRUE)

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

## From Boolean Functions to Logical Expressions


## Designing Logical Expressions by Karnaugh Maps


## Tautologies


## Some Algebraic Laws for Logical Expressions


## Tautologies and Methods of Proof


## Deduction


## Proofs by Resolution