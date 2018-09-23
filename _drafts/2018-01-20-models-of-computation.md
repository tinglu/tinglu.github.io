---
layout: post
title: Models of Computation Revision
date: 2018-01-20 15:36 +0800
tags: notes automata propositional-logic tbc
---

I'd like to study COMP30026 Models of Computation independently in my spare time.
The notes below contain my summaries and lecture materials (&copy; the University of Melbourne).

## Why's

### Why study logic and discrete maths

Logic and discrete mathematics provide the **theoretical foundations** for computer science.
`Propositional logic` has applications in hardware and software verification, planning, testing and fault finding, etc.
`Predicate logic` is essential to artificial intelligence, computational linguistic, automated theorem proving, logic programming, etc.
`Algebra` underpins theories of databases, programming languages, program analysis, data mining, etc.

### Why study computability

Because it is important to have an understanding of the **limitations of algorithmic problem solving**.
It is important to know that there are simple functions that are **not computable**.
It is important to know that there are simple questions that are **not decidable**.
Computability is full of philosophically challenging ideas, and exciting (sometimes surprising) results.

## Topics

* Propositional and predicate logic;
* Sets, functions, and relations;
* Proof principles, induction and recursion, well-ordering;
* Regular languages, finite-state automata, context-free languages;
* Computability and decidability.

## Propositional logic
  
### Syntax

![propositional logic syntax]({{ site.url }}/assets/moc/propositional-logic-syntax.png){:width="300"}

	wff -> A|B|C|...|Z|f|t 
		| (¬wff)
		| (wff∧wff) 
		| (wff∨wff) 
		| (wff⇒wff) 
		| (wff⇔wff) 
		| (wff⊕wff) 

{% raw %}
((P $$\wedge$$ ($$\neg$$Q)) $$\Rightarrow$$ (P $$\lor$$ (P $$\Leftrightarrow$$ Q))) 
is equivalent to 
P $$\wedge$$ $$\neg$$Q $$\Rightarrow$$ P $$\lor$$ (P $$\Leftrightarrow$$ Q)
{% endraw %}

### Concepts

A propositional formula is `valid` if no truth assignment makes it false. Otherwise it is `non-valid`.

It is `unsatisfiable` if no truth assignment makes it true. Otherwise it is `satisfiable`.

A valid propositional formula is a `tautology`.

An unsatisfiable propositional formula is a `contradiction`.

### Models, Logical Consequence, and Equivalence

Let $$\theta$$ be a truth assignment and $$\phi$$ be a propositional formula. If $$\theta$$
makes $$\phi$$ true then $$\theta$$ is a `model` of $$\phi$$.
$$\psi$$ is a `logical consequence` of $$\phi$$ iff every model of $$\phi$$ is a model of $$\psi$$	
as well.
In that case we write {% raw %} $$\phi |= \psi$$ {% endraw %}.
If $$\phi |= \psi$$	and $$\psi |= \phi$$ both hold, that is, $$\phi$$ and $$\psi$$	have exactly the
same models, then $$\phi$$ and $$\psi$$	are (logically) `equivalent`.
In that case we write $$\phi \equiv \psi$$

### Equivalences

* `Absorption`: 
	
	$$P \wedge P \equiv P$$
	
	$$P \lor P \equiv P$$

* `Commutativity`: 

	$$P \wedge Q \equiv Q \wedge P$$
	
	$$P \lor Q \equiv Q \lor P$$

* `Associativity`: 
	
	$$P \wedge (Q \wedge R) \equiv (P \wedge Q) \wedge R$$
	
	$$P \lor (Q \lor R) \equiv (P \lor Q) \lor R$$

* `Distributivity`: 

	$$P \wedge (Q \lor R) \equiv (P \wedge Q) \lor (P \wedge R)$$
	
	$$P \lor (Q \wedge R) \equiv (P \lor Q) \wedge (P \lor R)$$
	
* `Double negation`: 

	$$P \equiv \neg\neg P$$

* `De Morgan`: 
	
	$$\neg(P \wedge Q) \equiv \neg P \lor \neg Q$$
	
	$$\neg(P \lor Q) \equiv \neg P \wedge \neg Q$$
	
* `Implication`: 
	
	$$P \Rightarrow Q \equiv \neg P \lor Q$$

* `Contraposition`: 
	
	$$\neg P \Rightarrow \neg Q \equiv Q \Rightarrow P$$

	$$P \Rightarrow \neg Q \equiv Q \Rightarrow \neg P$$
	
	$$\neg P \Rightarrow Q \equiv \neg Q \Rightarrow P$$
	
* `Biimplication`: 
	
	$$P \Leftrightarrow Q \equiv (P \wedge Q) \lor (\neg P \wedge \neg Q)$$
	
* `Duality`:

	$$\neg \top \equiv \bot$$
	
	$$\neg \bot \equiv \top$$
	
* `Negation from absurdity`: 

	$$P \Rightarrow \bot \equiv \neg P$$
	
* `Identity`:

	$$P \lor \bot \equiv P$$
	
	$$P \wedge \top \equiv P$$
	
* `Dominance`:

	$$P ∧ \bot \equiv \bot$$
	
	$$P \lor \top \equiv \top$$
	
* `Contradiction`: 

	$$P ∧ \neg P \equiv \bot$$
	
* `Excluded middle`: 

	$$P \lor \neg P \equiv \top$$

*Let $$\bot$$ be any unsatisfiable formula and let $$\top$$ be any valid formula.*


## Terminology

`axiomatization` - 公理化

`decidability` - 可判定性

`undecidability` - 不可判定性

`consequence` - 蕴含