---
layout: post
title: Automata Theory By Stanford
date: 2018-01-11 12:16:00 +1100
tags: notes automata tbc
---

This is my study notes of [Automata Theory](https://lagunita.stanford.edu/courses/course-v1:ComputerScience+Automata+Fall2016/info) 
offered by Jeff Ullman at Stanford Online in the theory of automata and languages.
The notes below contain my summaries and lecture materials.

## W1: Finite Automata

`Automaton` - 自动机

    The set of strings accepted by an automaton A is the language of A.
    Denoted L(A).

### Deterministic Finite Automata (DFA)

`Alphabets` - any finite set of symbols.

e.g.

* ASCII, Unicode,
* {0,1} (binary alphabet),
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

A `formalism` for defining languages, consisting of:

* A finite set of states  (Q, typically).
* An input alphabet  (Σ, typically).
* A transition function  (δ, typically).
* A start state ($q_{0}$, in Q, typically).
* A set of final states  (F ⊆ Q, typically).
* "Final" and "accepting" are synonyms.

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
  * The entries of the table are the values of the transition function applied to the state that is the row and the symbol that is the column.

**Convention:**

    * … w, x, y, z are strings;
    * a, b, c, … are single input symbols.

> Language of a DFA:
>
> * Automata of all kinds define languages.
> * If A is an automaton, L(A) is its language.
> * For a DFA A, L(A) is the set of strings labeling paths from the start state to a final state.
> * Formally: L(A) = the set of strings w such that δ($q_0$, w) is in F.

Example:

![String 101]({{ site.url }}/assets/automata/string101.png){:width="300"}

The language of this example DFA is:
{w |  w is in {0,1}* and w does not have two consecutive 1’s}

`w` read a set former as 'The set of strings w…'; `|` read as such that; the rest are conditions about w and are true.

> Proofs of Set Equivalence
>
> The theorem: the reverse of a regular language is also regular.

### Nondeterministic Finite Automata (NFA)

Nondeterministic finite automaton has the ability to be in several states at once. 
Transitions from a state on an input symbol can be to any set of states.

> Language of an NFA:
>
> * A string w is accepted by an NFA if δ(q0, w) contains at least one final state.
> * The language of the NFA is the set of strings it accepts.

### Subset Construction

Given an NFA with states Q, inputs Σ, transition function $δ_N$, state state $q_0$, and final states F, construct equivalent DFA with:

* States $2^Q$ (Set of subsets of Q).
* Inputs Σ.
* Start state {$q_0$}.
* Final states = all those with a member of F.

The transition function $δ_D$ is defined by:
*$δ_D({q_1,…,q_k}, a)$ is the union over all $i = 1,…,k$  of $δ_N(q_i, a)$*.

> Proofs of Set Equivalence
>
> * The proof is almost a pun.
> * Show by induction on |w| that $δ_N(q_0, w) = δ_D({q_0}, w)$
> * Basis: $w = ε: δ_N(q_0, ε) = δ_D({q_0}, ε) = {q_0}$.

### NFA’s With ε-Transitions

* Allow state-to-state transitions on ε input.
* These transitions are done spontaneously, without looking at the input string.
* A convenience at times, but still only regular languages are accepted.

### Closure of States

`CL(q)` = set of states you can reach from state q following only arcs labeled ε.

Closure of a set of states = union of the closure of each state. 

### Extended Delta

* Intuition:   $\hatδ(q, w)$ is the set of states you can reach from q following a path labeled w.
* Basis:   $\hatδ(q, ε) = CL(q)$.
* Induction:   $\hatδ(q, xa)$ is computed by:

  * Start with   $\hatδ(q, x) = S$.
  * Take the union of CL(δ(p, a)) for all p in S.

### Summary

* DFA’s, NFA’s, and ε–NFA’s all accept exactly the same set of languages: the `regular languages`.
* The `NFA` types are *easier* to design and may have exponentially fewer states than a DFA.
* But *only* a `DFA` can be *implemented*!

## W2: Regular Expressions & Regular Languages

RE's use three operations:

* union (like addition, is commutative and associative)
* concatenation (like multiplication, is associative)
* Kleene star

### k-Paths

* A `k-path` is a path through the graph of the DFA that goes through no state numbered higher than k.
* Endpoints are not restricted; they can be any state.
* n-paths are unrestricted.
* RE is the union of RE’s for  the n-paths from the start state to each final state.

#### k-Path Induction

> Let $R_{ij}^{k}$ be the regular expression for the set of labels of k-paths from state i to state j.
>
> Basis: k=0. $R_{ij}^{0}$ = sum of labels of arc from i to j.
>
> * ∅ if no such arc.
> * But add ε if i=j.

Example:

![k-path]({{ site.url }}/assets/automata/k-path.png){:width="150"}

* 0-paths from 2 to 3: RE for labels = 0.
* 1-paths from 2 to 3: RE for labels = 0+11.
* 2-paths from 2 to 3: RE for labels = (10)*0+1(01)*1
* 3-paths from 2 to 3: RE for labels = ??

#### k-Path Inductive Case

A k-path from i to j either:

* Never goes through state k, or
* Goes through k one or more times.

> $R_{ij}^{k} = R_{ij}^{k-1} + R_{ik}^{k-1} (R_{kk}^{k-1})* R_{kj}^{k-1}$.

Apply to the above example: $R_{23}^{3} = R_{23}^{2} + R_{23}^{2}(R_{33}^{2})^{*}R_{33}^{2} = R_{23}^{2}(R_{33}^{2})^{*}$

Substitute $R_{23}^{2} = (10)^{*}0+1(01)^{*}1$ and $R_{33}^{2} = ε + 0(01)^{*}(1+00) + 1(10)^{*}(0+11)$

so that $R_{23}^{3} = [(10)^{*}0+1(01)^{*}1] [ε + (0(01)^{*}(1+00) + 1(10)^{*}(0+11))]^{*}$

#### Regular Languages Representation Conversions

![representation conversion]({{ site.url }}/assets/automata/representation-conversion.png){:width="200"}

Each of the three types of automata (DFA, NFA, ε-NFA) and regular expressions define exactly the same set of languages: 
the regular languages.
Any NFA could be converted to DFA that's the subset construction. 
Any x one NFA could be converted to an ordinary NFA that was the closure construction. 
Any regular expression can be converted to an ε-NFA.
Every DFA can be converted to a regular expression.
Any language defined by any one of these four notations is defined by all the others.

#### Identities and Annihilators

* ∅ is the identity for union. R + ∅ = R.
* ε is the identity for concatenation. εR = Rε = R.
* ∅ is the annihilator for concatenation. ∅R = R∅ = ∅.

### Applications of Regular Expressions

**Unix**, from the beginning, used regular expressions in many places, including the “grep” command
(“Global (search for a) Regular Expression and Print”).

One example is **Text Processing**. The DFA example for recognizing strings that end in “ing” involves a lot of explanation, as it should consider where to go from each of the states that represented some progress toward finding “ing”.
However, there is an easy regular expression for this same language.
Using the UNIX dot, it is just dot-star-i-n-g. Or even if not have the dot, we could simply replace it by all the legal input symbols connected by pluses.
In fact, it is even much easier to design an NFA for this language than it is to design a DFA.

Another example is **Lexical Analysis**. The first thing a compiler does is break a program into tokens (substrings) that together represent a unit.

### Properties of Regular Languages

A `language class` is a set of languages, e.g. the regular languages.
Language classes have two important kinds of properties:

#### 1. Closure Property

A `closure property` of a language class says that given languages in the class, an operation (e.g., union) produces another language in the same class, e.g. the regular languages are obviously closed under union, concatenation, and (Kleene) closure.

#### 2. Decision Property

A `decision property` for a class of languages is an algorithm that takes a formal description of a language (e.g., a DFA) and tells whether or not some property holds, e.g. Is language L empty?

#### Pumping Lemma

`Pumping Lemma` (泵引理) statement:

> For every regular language L, there is an integer n, which happens to be the number of states of some DFA for L, such that for every string w in L whose length is at least n.

We can break w into **w=xyz**, where y is the label of the first substring of w that goes from a state to the same state, such that three things are true:
First, **|xy| < n**, the prefix xy of w is short – it is of length at most n.
We assure that by making y be the label of the first cycle we encounter.
Second, **|y| > 0**, y is not the empty string. We assured this, because y connects two different occurrences of the same state along the path of w.
And lastly, **for all i > 0, xyiz is in L**, xyiz is in L for all integers i.
