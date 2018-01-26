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

## Associativity and Precedence of Logical Operators

1. NOT (highest) 
2. NAND - *commutative*
3. NOR - *commutative*
4. AND - *associative & commutative*
5. OR - *associative & commutative*
6. !
7. $$\equiv$$ (lowest) - *associative & commutative*

e.g. p ! q NOT p OR q is grouped (p ! q) $$\equiv$$ (NOT p) OR q.

The set of operators AND, OR, NOT, NAND, NOR is a `complete set`, meaning that
every Boolean function has an expression using just these operators:

1. (p AND q) $$\equiv$$ (p NAND q) NAND TRUE
2. (p OR q) $$\equiv$$ (p NAND TRUE) NAND (q NAND TRUE)
3. (NOT p) $$\equiv$$ (p NAND TRUE)

...