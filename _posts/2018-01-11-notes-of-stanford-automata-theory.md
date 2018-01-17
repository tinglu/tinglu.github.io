---
layout: post
title: Notes of Stanford Automata Theory
---

This is my study summary of [Automata Theory](https://lagunita.stanford.edu/courses/course-v1:ComputerScience+Automata+Fall2016/info) 
offered by Stanford Online Course.

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

`Strings` - Over an alphabet `Σ` is a list, each element of a string is a member of Σ, e.g. abc or 01101; 
`Σ*` represents set of all strings over alphabet Σ; `ε` represents empty strings.

e.g.

* 011 is a string of Σ = {0, 1} or Σ = {0,1,2} without 2
* {0,1}* = {ε, 0, 1, 00, 01, 10, 11, 000, 001, . . . }

> 0 as a string or a symbol look the same; but context determines the type; e.g. in C, we use single quote '0' to 
represent symbol/character whilest doule quotes "0" to represent the string.

`Languages` - a `subset` of `Σ*` for some alphabet Σ.

e.g. The set of strings of 0’s and 1’s with no two consecutive 1’s.
L = {ε, 0, 1, 00, 01, 10, 000, 001, 010, 100, 101, 0000, 0001, 0010, 0100, 0101, 1000, 1001, 1010, . . . }

A formalism for defining languages, consisting of:

* A finite set of states  (`Q`, typically).
* An input alphabet  (`Σ`, typically).
* A transition function  (`δ`, typically).
* A start state  (`q<sub>0</sub>`, in Q, typically).
* A set of final states  (`F ⊆ Q`, typically).

    “Final” and “accepting” are synonyms.

`Transition Function` - takes two arguments: a state & an input symbol

`δ(q, a)` = the state that the DFA goes to when it is in state q and input a is received. 
*Note: always a next state – add a `dead state` if no transition.*

* `Transition Graphs`
    
    * Nodes = states.
    * Arcs represent transition function.
    * Arc from state p to state q labeled by all those input symbols that have transitions from p to q.
    * Arrow labeled “Start” to the start state.
    * Final states indicated by double circles.
    
* `Transition Tables`
    
    * The rows each correspond to one of the states.
    * The columns correspond to the input symbols.
    * The final states is indicated by putting a star next to their name.
    * The start state is indicated by an arrow.
    * The entries of the table are the values of the transition function applied to the state that is the row and the 
    symbol that is the column.

**Convention:**

    * … w, x, y, z are strings;
    * a, b, c, … are single input symbols.

> `Language of a DFA`:
>
> - Automata of all kinds define languages.
> - If A is an automaton, L(A) is its language.
> - For a DFA A, L(A) is the set of strings labeling paths from the start state to a final state.
> - Formally: L(A) = the set of strings w such that δ(q<sub>0</sub>, w) is in F.

Example:

![Point size]({{ site.url }}/assets/automata/string101.png)

The language of this example DFA is: `{w | w is in {0,1}* and w does not have two consecutive 1’s}`

`w` read a set former as 'The set of strings w…'; `|` read as such that; the rest are conditions about w and are true.

`Proofs of Set Equivalence`:

> The theorem: the reverse of a regular language is also regular.

### Nondeterministic Finite Automata (NFA)

`Nondeterministic finite automaton` has the ability to be in several states at once. 
Transitions from a state on an input symbol can be to any set of states.

> `Language of an NFA`:
> - A string w is accepted by an NFA if δ(q0, w) contains at least one final state.
> - The language of the NFA is the set of strings it accepts.

### Subset Construction

Given an NFA with states Q, inputs Σ, transition function δ<sub>N</sub>, state state q<sub>0</sub>, and final states F, 
construct equivalent DFA with:

* States 2<sup>Q</sup> (Set of subsets of Q).
* Inputs Σ.
* Start state {q<sub>0</sub>}.
* Final states = all those with a member of F.

The transition function δ<sub>D</sub> is defined by:
`δ<sub>D</sub>({q<sub>1</sub>,…,q<sub>k</sub>}, a) is the union over all i = 1,…,k  of δ<sub>N</sub>(q<sub>i</sub>, a)`.

`Proofs of Set Equivalence`:

* The proof is almost a pun.
* Show by induction on |w| that δ<sub>N</sub>(q<sub>0</sub>, w) = δ<sub>D</sub>({q<sub>0</sub>}, w)
* Basis: w = ε: δ<sub>N</sub>(q<sub>0</sub>, ε) = δ<sub>D</sub>({q<sub>0</sub>}, ε) = {q<sub>0</sub>}.

### NFA’s With ε-Transitions

* Allow state-to-state transitions on ε input.
* These transitions are done spontaneously, without looking at the input string.
* A convenience at times, but still only regular languages are accepted.

### Closure of States

`CL(q)` = set of states you can reach from state q following only arcs labeled ε.

Closure of a set of states = union of the closure of each state. 

### Extended Delta

* Intuition:   $\hatδ$(q, w) is the set of states you can reach from q following a path labeled w.
* Basis:   $\hatδ$(q, ε) = CL(q).
* Induction:   $\hatδ$(q, xa) is computed by:
    * Start with   $\hatδ$(q, x) = S.
    * Take the union of CL(δ(p, a)) for all p in S.
    
### Summary

* DFA’s, NFA’s, and ε–NFA’s all accept exactly the same set of languages: the `regular languages`.
* The `NFA` types are *easier* to design and may have exponentially fewer states than a DFA.
* But *only* a `DFA` can be *implemented*!
