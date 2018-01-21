---
layout: post
title: Mathematics for Computer Science By MIT
date: 2017-12-13 16:49:00 +1100
tags: notes mathematics
---

This is my study notes of [MIT6.042J Mathematics for Computer Science](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-042j-mathematics-for-computer-science-spring-2015/) 
offered by MIT OCW. The course covers elementary discrete mathematics for computer science and engineering. 
It emphasizes mathematical definitions and proofs as well as applicable methods.
The notes below contain my summaries, lecture materials, and some assessments answers.

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
`Modus Ponens`

What is the statement above the line called?
`Antecedent`

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
