---
layout: post
title: Notes of Stanford Automata Theory
---

This is my study summary of [Automata Theory](https://lagunita.stanford.edu/courses/course-v1:ComputerScience+Automata+Fall2016/info) offered by Stanford Online Course.

## W1: Finite Automata

`Automaton`: 自动机

    The set of strings accepted by an automaton A is the language of A.
    Denoted L(A).

### Deterministic Finite Automata (DFA)

`Alphabets` - any finite set of symbols.

e.g.

* ASCII, Unicode,
* {0,1} (binary alphabet ),
* {a,b,c}, {s,o},
* set of signals used by a protocol.

`Strings` - Over an alphabet `Σ` is a list, each element of a string is a member of Σ, e.g. abc or 01101; `Σ*` represents set of all strings over alphabet Σ; `ε` represents empty strings.

e.g.

* 011 is a string of Σ = {0, 1} or Σ = {0,1,2} without 2
* {0,1}* = {ε, 0, 1, 00, 01, 10, 11, 000, 001, . . . }

> 0 as a string or a symbol look the same; but context determines the type; e.g. in C, we use single quote '0' to represent symbol/character whilest doule quotes "0" to represent the string.

`Languages` - a `subset` of `Σ*` for some alphabet Σ.

e.g. The set of strings of 0’s and 1’s with no two consecutive 1’s.
L = {ε, 0, 1, 00, 01, 10, 000, 001, 010, 100, 101, 0000, 0001, 0010, 0100, 0101, 1000, 1001, 1010, . . . }

A formalism for defining languages, consisting of:

* A finite set of states  (`Q`, typically).
* An input alphabet  (`Σ`, typically).
* A transition function  (`δ`, typically).
* A start state  (`q0`, in Q, typically).
* A set of final states  (`F ⊆ Q`, typically).

    “Final” and “accepting” are synonyms.

`Transition Function` - takes two arguments: a state & an input symbol

`δ(q, a)` = the state that the DFA goes to when it is in state q and input a is received. *Note: always a next state – add a `dead state` if no transition.*

* `Transition Graphs`
* `Transition Tables`

> Convention:
>
> … w, x, y, z are strings.
>
> a, b, c,… are single input symbols.

`Proof Techniques`

### Nondeterministic Finite Automata (NFA)
