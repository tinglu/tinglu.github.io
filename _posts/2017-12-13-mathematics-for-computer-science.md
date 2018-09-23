---
layout: post
title: Mathematics for Computer Science By MIT
date: 2017-12-13 16:49:00 +1100
tags: notes mathematics tbc
---

This is my study notes of [MIT6.042J Mathematics for Computer Science](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-042j-mathematics-for-computer-science-spring-2015/) offered by MIT OCW. The course covers elementary discrete mathematics for computer science and engineering. It emphasizes mathematical definitions and proofs as well as applicable methods.
The notes below contain my summaries, lecture materials, and some assessments answers.

# Unit 1: Proofs

## 1.1 Intro to proofs

* `有理数`：有限小数/无限循环小数
* `无理数`：无限不循环小数
* `proposition`: a statement that is either true or false
* `predicate`(谓词，断言): a proposition whose truth depends on the value of one or more variables
* `axiom`(公理): a proposition that is simply accepted as true
* `proof`: a sequence of logical deductions from axioms and previously proved statements that concludes with the proposition in question
* `theorem`(定理): an important true proposition
* `lemma`(引理，辅助定理): a preliminary proposition useful for proving later propositions
* `corollary`(推论): a proposition that follows in just a few logical steps from a theorem

Let P be a proposition, Q be another proposition.
What is a proposition of the form IF P, THEN Q called?
`Implication`

IF P, THEN Q is the general form of an implication and is often written as `P IMPLIES Q`. Thus, given specific P and Q, P IMPLIES Q is itself a `proposition` and can be either true or false.

A fundamental inference rule says:

$$\dfrac{P,\;\; P \text{ IMPLIES } Q}{Q}$$

What is this inference rule called?
`Modus Ponens` (演绎推理；肯定前件论式)

What is the statement above the line called?
`Antecedent` (前减，先例)

What is the statement below the line called?
`consequent/conclusion`

Proving a proposition's contrapositive is as good as (and sometimes easier than) proving the proposition itself.

Which of the following is logically equivalent to the `contrapositive` of P IMPLIES Q?
`NOT(Q) IMPLIES NOT(P)`

Draw a `Venn diagram` with P inside of Q.

At the end of a proof, it is customary to write down either the delimiter _____ or the symbol _____.

A proof should begin with "`Proof by ...`" and end with "`QED`" or `◻`.

`Logical deductions`, or `inference rules`, are used to prove new propositions using previously proved ones.

A fundamental inference rule is `modus ponens`.

$$\dfrac{P,\;\; P \text{ IMPLIES } Q}{Q}$$

A key requirement of an inference rule is that it must be `sound`: an assignment of truth values to the letters, P , Q, . . . , that makes all the antecedents true must also make the consequent true. So if we start off with true axioms and apply sound inference rules, everything we prove will also be true.

## 1.2 Proof methods

`Proof by contradiction` often involves clever application of proven knowledge to arrive at a contradiction. In the example proof of 2⁆'s irrationality, what is the key underlying assumption? (We will prove this later in the course!):

The product of two odd numbers is odd.

`Proof by cases` might be used when a complicated proof could be broken into cases, where each case is simpler to prove and the cases together cover all possibilities.

## 1.3 Well Ordering Principle

## 1.4 Logic & Propositions

### Digital Logic

$$\bar{x} ::= NOT(x)$$

    1 ::= T
    0 ::= F
    . ::= AND
    + ::= OR

### 1.4.1 Propositional Operators: Video

### 1.4.2 Propositional Operators

### 1.4.3 Digital Logic: Video

### 1.4.4 Truth Tables: Video

A formula is `satisfiable` iff it is true in some environment.

A formula is `valid` iff it is true in all environments.

```text
e.g.
satisfiable: P, NOT(P)
not satisfiable: (P AND NOT(P))
valid: (P OR NOT(P))
```

G and H are `equivalent` exactly when (G IFF H) is valid.

Verifying Valid, Satisfiable: Truth table size doubles with each additional variable -- **exponential growth**. Makes truth tables impossible when there are hundreds of variables. (In current digital circuits, there are millions of variables.)

### 1.4.5 Equivalence and Truth Table

### 1.4.6 Implies: Video

### 1.4.7 Propositional Logic: Video

Lukasiewicz’ proof system(?)

Algebraic & deduction proofs in general are no better than truth tables. No efficient method for verifying validity is known.

### 1.4.8 Soundness and Validity

### 1.4.9 Logical Connectives

## 1.5 Quantifiers & Predicate Logic

### 1.5.1 Predicate Logic 1: Quantifiers ∀,∃

> Predicate: Propositions with variables.

`∀x` For `ALL` x; like `AND`; (symbol is upside down A)

`∃y` There `EXISTS` some y; like `OR`

### 1.5.2 Predicate Logic 2: Validity & Satisfiability

DeMorgan's Law

### 1.5.3 Satisfiability

### 1.5.4 Predicate Logic 3: ∀ ∃ in English

#### Power & Limits of Logic

Two Profound Meta- Theorems about Mathematical Logic:

* (Thm 1) [Gödel's Completeness Theorem] good news: only need to know a few axioms & rules to prove all valid formulas.  (in theory; in practice need  lots of rules)

* (Thm 2) [Validity is undecidable], Bad News: there is no procedure to determine whether a quantified formula is valid (in contrast to propositional formulas).

### 1.5.5 Name That Predicate

### 1.5.6 Quantifiers

### 1.5.7 Propositions with Quantifiers

### 1.5.8 Predicate Logic

## 1.6 Sets

### 1.6.1 Sets Definitions: Video

`Powerset`:

    pow({T, F}) = { {T}, {F}, {T, F}, ∅ }
    Z ∈ pow(R)
    B ∈ pow(A) iff B ⊆ A

### 1.6.2 Sets Operations: Video

### 1.6.3 Difference

## 1.7 Binary Relations

### 1.7.1 Relations & Functions

> A binary relation associates elements of one set called the domain, with elements of another set called the codomain.

`Binary relation` R from a set A (`domain`) to a set B (`codomain`) associates elements of A with elements of B.

Graph(R) ::= the arrows

range(R) ::= elements with arrows coming in = R(A)

    定义域 domain f: X->Y   Set X is the domain of f, set Y is the codomain of f
    上域 codomain （陪域，到达域，靶target，对应域）
    值域 range f(X), f(X) is a subset of codomain Y

> Functions are relations.

A `function`, F, from A to B is a relation which associates each element, a, of A with at most one element of B (called F(a)).

    F: A -> B is a function IFF |F(a)| <= 1
    IFF a F b AND a F b' IMPLIES b = b'

### 1.7.2 Range of a Relation

### 1.7.3 Relational Mappings: Properties (Archery)

`surjection` 满射, onto, R is a surjection iff R (A) = B (**>=1 arrow in**)

`injection` 单射, invective function, one-to-one function (**<=1 arrow in**)

`bijection` 双射, bijective function, one-to-one correspondence (**exactly 1 arrow out and 1 arrow in**)

    A bijective function f: X -> Y is a one-to-one (injective) and onto (surjective) mapping of a set X to a set Y

    A bijection from A to B implies |A| = |B| for finite A and B

### 1.7.4 Total Injection

### 1.7.5 Finite Cardinality: Video

Mapping problem to counting problem.

“jection” relations:

    A bij B ::= ∃bijection:A->B
    A surj B ::= ∃surj func:A->B
    A inj B ::= ∃total inj relation:A->B

Mapping Lemma:

    A bij B IFF A = B
    A surj B IFF A ≥ B
    A inj B IFF A ≤ B
    for finite A, B

### 1.7.6 A inj B

### 1.7.7 Total Relations

### 1.7.8 Surjective Relations

### 1.7.9 Inverse Relations

### 1.7.10 In- ,Sur-, and Bijections

### 1.7.11 Mapping Lemma: Sizes of Domains and Codomains

## 1.8 Induction

### 1.8.1 Induction: Video

### 1.8.2 Bogus Induction: Video

### 1.8.3 Same Colored Horses

### 1.8.4 Strong Induction: Video

### 1.8.5 Unstacking Game Score

### 1.8.6 WOP vs Induction: Video [optional]

### 1.8.7 Strong vs Ordinary Induction vs WOP [optional]

### 1.8.8 Induction by n+3

### 1.8.9 Induction Rules

### 1.8.10 Postage by Induction

### 1.8.11 A Bogus Induction

## 1.9 State Machines - Invariants

### 1.9.1 State Machines Invariants: Video

### 1.9.2 State Machine Invariants

### 1.9.3 Derived Variables: Video

### 1.9.4 Derived Variables and Termination

### 1.9.5 Integer Multiplication

### 1.9.6 Chocolate Bars

## 1.10 Recursive Definition

### 1.10.1 Recursive Data: Video

### 1.10.2 Matching Parentheses

### 1.10.3 Functions of F18

### 1.10.4 Structural Induction: Video

### 1.10.5 Structural Induction: Definition

### 1.10.6 Counting Cases

### 1.10.7 Recursive Functions: Video

## 1.11 Infinite Sets

### 1.11.1 Cardinality: Video

### 1.11.2 Cantor, Schroeder-Bernstein

### 1.11.3 Countable Sets: Video

### 1.11.4 Cantor's Theorem: Video

### 1.11.5 Cantor's Diagonal Argument

### 1.11.6 Countable and Uncountable Sets

### 1.11.7 The Halting Problem: Video [Optional]

### 1.11.8 Halting Problem Basics [Optional]

### 1.11.9 Russell's Paradox: Video

### 1.11.10 Russell's Paradox [and ZFC optional]

### 1.11.11 Set Theory Axioms: Video [Optional]

### 1.11.12 Set Theory Axioms [Optional]

# Unit 2. Structures

## 2.1 GCDs

### 2.1.1 GCDs & Linear Combinations: Video

### 2.1.2 Euclidean Algorithm: Video

### 2.1.3 Run Euclid Run

### 2.1.4 Pulverizer: Video

### 2.1.5 GCDs I

### 2.1.6 Revisiting Die Hard: Video

### 2.1.7 Prime Factorization: Video

### 2.1.8 Unique Primes

### 2.1.9 Divisors

### 2.1.10 GCDs II

## 2.2 Congruences

### 2.2.1 Congruence mod n: Video

### 2.2.2 Divisibility and Congruence

### 2.2.3 Inverses mod n: Video

### 2.2.4 Inverses mod n

### 2.2.5 Multiplicative Inverses

### 2.2.6 Inverses With Linear Combinations

## 2.3 Euler's Theorem

### 2.3.1 Modular Exponentiation Euler's Function: Video

### 2.3.2 Euler's Totient Function

### 2.3.3 The Ring Z: Video

### 2.3.4 The Ring

### 2.3.5 Z mod n

### 2.3.6 Fermat's Little Theorem

### 2.3.7 Euler's Theorem

## 2.4 RSA Encryption

### 2.4.1 RSA Public Key Encryption: Video

### 2.4.2 RSA Encryption

### 2.4.3 Reducing Factoring To SAT: Video

### 2.4.4 Relative Primality

### 2.4.5 RSA computations

## 2.5 Digraphs: Walks & Paths

### 2.5.1 Digraphs: Walks & Paths: Video

### 2.5.2 Walks and Paths

### 2.5.3 Digraphs: Connected Vertices: Video

### 2.5.4 Longest Path

### 2.5.5 Adjacency Matrix

### 2.5.6 Counting Paths

## 2.6 Directed Acyclic Graphs (DAGs) & Scheduling

### 2.6.1 DAGs: Video

### 2.6.2 DAGs

### 2.6.3 Scheduling: Video

### 2.6.4 Scheduling Prerequisites

### 2.6.5 Time versus Processors: Video

### 2.6.6 Processor Time Bounds

### 2.6.7 The Divisibility DAG

## 2.7 Partial Orders and Equivalence

### 2.7.1 Partial Orders: Video

`Transitivity`:

Theorem: R is a transitive iff R = G+ for some digraph G.

Strict partial order (SPO):

Theorem: R is a SPO iff R = D+ for some DAG D.

Linear orders: Given any two elements, one will be “bigger than” the other one.

Reflexivity: relation R on set A is reflexive iff a R a for all a ∈A, G* is reflexive.  *implies everything is related to itself*.

Antisymmetry: binary relation R is antisymmetric iff it is asymmetric except for a R a case.

Asymmetry: aRa is never allowed; *implies nothing is related to itself*.

Weak partial orders (WPO):

* same as a strict partial order R, except that a R a always hold.
* transitive & antisymmetric & reflexive
* Theorem: R is a WPO iff R = D* for some DAG D

### 2.7.2 Population Partial Order

### 2.7.3 Representing Partial Orders As Subset Relations: Video

### 2.7.4 Equivalence Relations: Video

### 2.7.5 Relational Properties

### 2.7.6 Properties Of Relations

### 2.7.7 Equivalence Relations & Partial Orders

## 2.8 Degrees & Isomorphism

### 2.8.1 Degree: Video

### 2.8.2 Counting Degrees & Edges

### 2.8.3 Isomorphism: Video

### 2.8.4 Isomorphism

### 2.8.5 Extreme Graphs

### 2.8.6 Isomorphic Graphs

### 2.8.7 Non-Isomorphic Graphs

## 2.9 Coloring & Connectivity

### 2.9.1 Coloring: Video

### 2.9.2 Chromatic Number

### 2.9.3 Connectivity: Video

### 2.9.4 k-Connectivity: Video

### 2.9.5 k-Connected [optional]

### 2.9.6 Graph Coloring I

### 2.9.7 Graph Coloring II

### 2.9.8 Connected Components in Integers

## 2.10 Trees

### 2.10.1 Trees: Video

### 2.10.2 Trees: Many Definitions

### 2.10.3 Tree Coloring: Video

### 2.10.4 2-Colorable Trees

### 2.10.5 Spanning Trees: Video

### 2.10.6 Span all the Graphs

### 2.10.7 Tree or Not Tree

### 2.10.8 Leaves

### 2.10.9 Minimum Spanning Trees

### 2.10.10 Graph Algorithm

## 2.11 Stable Matching

### 2.11.1 Stable Matching: Video

### 2.11.2 Matching Ritual: Video

### 2.11.3 Derived Variables

### 2.11.4 Mating Ritual

### 2.11.5 Optimal Stable Matching: Video

### 2.11.6 Boy Optimal

### 2.11.7 Bipartite Matching: Video

### 2.11.8 Bipartite Equivalence Relation

### 2.11.9 Hall's Theorem: Video

### 2.11.10 Bottleneck

### 2.11.11 Bipartite Graphs

### 2.11.12 Matching

### 2.11.13 Stable Matching Invariants
