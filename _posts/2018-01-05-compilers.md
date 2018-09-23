---
layout: post
title: Compilers By Stanford
date: 2018-01-05 11:04:00 +1100
tags: notes compilers tbc
---

This is my study notes of [CS143 Compilers](https://lagunita.stanford.edu/courses/Engineering/Compilers/Fall2014/info)
offered by Stanford Online and the resources can also be found on the [web homepage](http://web.stanford.edu/class/cs143/).
The notes below contain my summaries, lecture materials, and some assessments answers.

## Intro

### The Structure of a Compiler

1. Lexical Analysis - Syntactic
2. Parsing - Syntactic
3. Semantic Analysis - Types scope
4. Optimization
5. Code Generation - Translation

## Lexical Analysis

`Tokens` correspond to sets of strings.

* `Identifier`: strings of letters or digits, starting with a letter
* `Integer`: a non-empty string of digits
* `Keyword`: "else" or"if" or "begin" or …
* `Whitespace`: a non-empty sequence of blanks, newlines, and tabs

An implementation must do two things:

1. Recognize substrings corresponding to tokens
2. Return the value or lexeme of the token – The lexeme is the substring

The goal of lexical analysis is to:

* Partition the input string into lexemes
* Identify the token of each lexeme

Left-to-right scan => lookahead sometimes required

### Regular Languages

> Def. The regular expressions over $\Sigma$ are the smallest set of expressions including:

* ε = {""}
* 'c' = {"c"} where c $\in$ $\Sigma$
* A + B = A U B where A, B are rexp over $\Sigma$
* {% raw %}$$AB = \{ ab | a \in A \land b \in B \} $${% endraw %}
* A* = A is a rexp over $\Sigma$

L:Exp -> Sets of Strings

Meaning is many (syntax) to one (semantic).

### Lexical Specification

Regular expressions to Lexial specification - Ambiguities:

Rule: Pick longest possible string in L(R)
– The **"maximal munch"**

### Finite Automata

> Regular expressions = specification
>
> Finite automata = implementation

#### Deterministic Finite Automata (DFA)

– One transition per input per state
– No ε-moves

#### Nondeterministic Finite Automata (NFA)

– Can have multiple transitions for one input in a given state
– Can have ε-moves

Execution of Finite Automata:

* A DFA can take only one path through the state graph – Completely determined by input
* NFAs can choose

  * Whether to make ε-moves
  * Which of multiple transitions for a single input to take

#### NFA vs. DFA

* NFAs and DFAs recognize the same set of languages (regular languages)
* DFAs are faster to execute – There are no choices to consider (but less compact)
* For a given language NFA can be simpler than DFA (slower but more concise)
* DFA can be exponentially larger than NFA

High-level sketch: **Lexical Specification => RegExp => NFA => DFA => Table-driven Implementation of DFA**

#### NFA to DFA

##### The Trick

* Simulate the NFA
* Each state of DFA = a non-empty subset of states of the NFA
* Start state = the set of NFA states reachable through ε-moves from NFA start state
* Add a transition S →a S’ to DFA iff S’ is the set of NFA states reachable from any state in S after seeing the input a, considering ε-moves as well

##### Remark

* An NFA may be in many states at any time
* How many different states? - N states
* If there are N states, the NFA must be in some subset of those N states
* How many subsets are there? – **${2}^N$ - 1** (**finite set of possible configurations**) = finitely many

## Parsing

Phase | Input | Output
---------|----------|---------
Lexer | String of characters | String of tokens
Parser | String of tokens | Parse tree

### Context-free grammars (CFG’s)

A CFG consists of:

* A set of terminals T
* A set of non-terminals N
* A start symbol S (a non-terminal)
* A set of productions: $X \to dY_1Y_2 … Y_n$ where X $\in$ N and $Y_i$ $\in$ T $\cup$ N $\cup$ {$\epsilon$}

The language of a CFG:

Let G be a context-free grammar with start symbol S. Then the language of G is: {$a_1$ … $a_n$ | S ${\to}^*$ $a_1$ … $a_n$ and every $a_i$ is a terminal }

`Terminals` are so-called because there are no rules for replacing them;

* Once generated, terminals are permanent;
* Terminals ought to be tokens of the language.

### Derivations

Notes on derivations:

* A parse tree has

  * Terminals at the leaves
  * Non-terminals at the interior nodes

* An **in-order traversal** of the leaves is the original input
* The parse tree shows the association of operations, the input string does not

### Ambiguity

A grammar is `ambiguous` if it has more than one parse tree for some string; Equivalently, there is more than one right-most or left-most derivation for some string.

### Error Handling

Purpose of the compiler is

* To detect non-valid programs
* To translate the valid ones

Many kinds of possible errors (e.g. in C)

Error kind | Example | Detected by
---------|----------|---------
Lexical | … $ … | Lexer
Syntax | … x *% … | Parser
Semantic | … int x; y = x(3); … | Type checker
Correctness | your favorite program | Tester/User

#### Syntax Error Handling

Error handler should:

* Report errors accurately and clearly
* Recover from an error quickly
* Not slow down compilation of valid code

Approaches - From simple to complex

* Panic mode
* Error productions
* Automatic local or global correction

### Top-Down Parsing & Recursive Decent Parsing

Start with top-level non-terminal E – Try the rules for E in order

#### Left Recursion

In general {% raw %} S → S α1 $$\|$$ … $$\|$$ S αn $$\|$$ β1 $$\|$$ … $$\|$$ βm {% endraw %}; All strings derived from S start with one of β1,…,βm and continue with several instances of α1,…,αn; Rewrite as:

    S → β1 S’ | … | βm S’
    S’ → α1 S’ | … | αn S’ | ε

### Bottom-Up Parsing

Like recursive-descent but parser can "predict" which production to use

* By looking at the next few tokens
* No backtracking

`Predictive parsers` accept `LL(k)` grammars

* L means "left-to-right" scan of input
* L means "leftmost derivation"
* k means "predict based on k tokens of lookahead"
* In practice, LL(1) is used

### LL(1) vs. Recursive Descent

* In recursive-descent,

  * At each step, many choices of production to use
  * Backtracking used to undo bad choices

* In LL(1),

  * At each step, only one choice of production
  * That is

    * When a non-terminal A is leftmost in a derivation
    * The next input symbol is t
    * There is a unique production A → α to use – Or no production to use (an error state)

* **LL(1) is a recursive descent variant without backtracking**

`Left-Factoring`: Factor out common prefixes of productions

#### Notes on LL(1) Parsing Tables

If any entry is multiply defined then G is not LL(1)

* If G is ambiguous
* If G is left recursive
* If G is not left-factored
* And in other cases as well

Most programming language CFGs are not LL(1)

`Bottom-up parsing` is more general than top-down parsing

* And just as efficient
* Builds on ideas in top-down parsing

Bottom-up is the preferred method.

### Shift-Reducing Parsing

`Bottom Up Parsers` construct rightmost derivations.

`SLR`: Simple LR parser -> LR(0).
Input from **L**eft; derivation from **R**ightmost

`Handles` formalize the intuition:

* A handle is a reduction that allows further reductions back to the start symbol.
* We only want to reduce at handles.

To recognise a viable prefixes, we must:

* recognize a sequence of partial rhs's of productions, where
* each partial rhs can eventually reduce to part of the missing suffix of its predecessor

Important Fact about bottom-up parsing:

(1). A bottom-up parser traces a rightmost derivation in reverse

(2). In shift-reduce parsing, handles appear only at the top of the stack, never inside

(3). For any grammar, the set of viable prefixes is a regular language.

An `item` is a production with a `"."` somewhere on the rhs.

The states of the DFA are "canonical collections of items" or "canonical collections of LR(0) items".

X -> b.r is valid for a viable prefix ab if S' -> aXw -> abrw by a right-most derivation;
after parsing ab, the valid items are the possible tops of the stack of items.

### SLR Parsing
