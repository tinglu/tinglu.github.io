---
layout: post
title: Programming Language Implementation Revision
date: 2018-07-30 15:57 +1000
tags: notes compilers
---

Some dot points of my study of COMP90045 Programming Language Implementation (SM1 2018).
The notes below also contain lecture materials (&copy; the University of Melbourne).

## I. Overview

Each `phase` performs a specific task.

Each `pass` processes the input (a program or a module) from beginning to end.

1. `Lexical analysis (scanning)` groups the characters of the input into tokens, the smallest meaningful units of the program.
    It also discards irrelevant parts of the input (white space, comments)

2. `Syntax analysis (parsing)` groups tokens into hierarchies of bigger and bigger units, with the biggest unit being the whole
    program.

    The `parse tree`, also called the `concrete syntax tree`, contains superficial details that have no effect on the rest of the compiler. Deleting them yields the `abstract syntax tree (AST)`.

3. `Semantic analysis` checks whether the program violates any of the rules of the programming language, e.g., by using undeclared variables.

    The information gathered by semantic analysis can be stored in the abstract syntax tree in the form of `attributes` attached to nodes.

    An attribute grammar extends a context-free grammar with:
    - rules for assigning values to attributes of nodes in a parse tree
    - conditions on attribute values—violations represent errors.

4. Instead of generating assembly language or machine code directly, compilers usually generate code for a simple abstract machine. This `intermediate representation (IR)` is designed to be easier to optimize than the target language.

5. The `optimizer` seeks to improve the program by making it execute **faster** or in **less space** (though it cannot guarantee actual optimality).

6. `Code generation` concerns when generating code for real machines:
    - Real machines have a limited number of registers.
    - Real machines often have more than one way to perform an operation.
    - Real machines often require some operands to be in particular places for particular operations.

- Some compilers translate IR to assembly code (easier to implement and to debug),
- while others convert it directly to machine code (speeds up compilation).

### Symbol Table

The data structures where compilers keep information about identifiers are collectively known as the `symbol table`.

In typical compilers the tables for local scopes (e.g., functions) are separate from the table(s) for the global scope.

## II. Lexical analysis (scanning)

## 1. Lexical Analysis

Many compilers use a separate `lexical analyzer`, (also known as the `lexer` or `scanner`) whose main job is to transform the input stream from a stream of characters into a stream of tokens. The syntax analyzer then operates on this stream of tokens.

Other Jobs of Lexical Analysis:

- Discarding unneeded parts of the input (white space and comments between tokens).
- Keeping track of line numbers and sometimes column numbers (for error reporting, and in e.g., Haskell, for the offside rule).
- Smashing case (for case-insensitive languages).
- Processing file inclusion directives, and keeping track of which parts of which files remain to be read.
- Processing macro definitions and expanding macros.

A `lexeme` is a sequence of input characters that forms a token: e.g., "while", "<=", "num_widgets", "1066".

Lexical analyzers associate with each kind of token one or more `patterns`; a lexeme forms a token of a given kind if it matches one of the token kind's patterns.

## 2. Regular Languages

- $\Sigma$ = alphabet (set of characters)
- language = set of strings
- $L_1 \cup L_2 = {s|s \in L_1 \, or s \in L_2}$
- $L_1 L_2 = {s_1s_2|s_1 \in L_1,s_2 \in L_2}$
- $L^* = {s_1s_2...s_n|n \in N,s_i \in L}$
- $\epsilon$ stands for the empty string

A `regular definition` gives a name to a regular expression: $d_i \to r_i$

- $alpha \to A|B|. . . |Z|a|b|. . . |z$
- $digit \to 0|1|...|9$
- $id \to alpha (alpha|digit)^*$
- $digits \to digit \, digit^*$
- $opt \, frac \to .digits| \epsilon$
- $opt \, exp \to E (+|-| \epsilon ) digits | \epsilon$
- $float \to digits \, opt \, frac \, opt \, exp$

## Scanner Generator

A `scanner generator` is a `program generator`. It takes a file containing a specification of a regular language, and outputs a source file that defines a lexical analyzer for that language.

`lex` (and clone `flex`) are the classic _**lexer generators**_ for $C$ toolchains. Scanner generators exist for many other programming languages. For
example, `alex` generates Haskell code.

An `alex` specification gives the definition of tokens, together with rules that specify how string snippets should be translated into tokens. It distinguishes names used for `$character sets$` (`$xyz`) for names used as `$regular expressions$` (`@xyz`):

    $digit = 0-9
    $alpha = [a-zA-Z]
    @ident = $alpha [$alpha $digit \_ \']*

`Rules` = a list of pattern/action pairs.

The lexing function divides the input stream into a sequence of strings, each of which matches a pattern in the scanner specification.

The lexer function repeatedly:

- reads input characters until it has built up the `$$longest string$$` of those characters that matches one of the patterns, and
- executes the action of the `$$first pattern$$` matching that string.

## 3. Finite-State Automata

A `recognizer` for a language L is an algorithm that takes an input string x and returns "yes" if x is in L and "no" otherwise.

The tools that generate scanners automatically are based on two kinds of `mathematical constructs`:

- `DFAs`, or deterministic finite automata, 
- `NFAs`, or nondeterministic finite automata.

### Deterministic finite automaton (DFA)

A `DFA` is a 5-tuple **$(Q,Σ,δ,q_0,F)$**, where:

- Q is a finite set of states,
- Σ is a finite alphabet,
- δ : Q × Σ \to Q is the transition function,
- $q_0$  \in  Q is the start state, and
- F ⊆ Q are the accept states.

Let $M = (Q,Σ,δ,q_0$,F)$ and let $w = v_1 v_2 ···v_n$ be a string from $Σ^*$. M accepts w iff there is a sequence of states $r_0, r_1, . . ., r_n$, with each $r_i  \in Q$, such that:

1. $r_0 = q_0$
2. $\delta(r_i,v_{i+1})=r_{i+1}$ for i=0,...,n−1
3. $r_n \in F$

M recognises language A iff A = {w | M accepts w}

### Nondeterministic finite automaton (NFA)

For any alphabet Σ, let Σǫ denote Σ ∪ {$\epsilon$}.

An `NFA` is a 5-tuple **$(Q,Σ,δ,q_0$,F)**, where:

- Q is a finite set of states,
- $\Sigma$ is a finite alphabet,
- $δ : Q × \Sigma_Q \to P(Q)$ is the transition function,
- $q_0 \in Q$ is the start state, and
- $F \subseteq Q$ are the accept states.

The only part of the 5-tuple that differs from DFAs is $\delta$: the transition function is nondeterministic, and it allows $\epsilon$ transitions.

## 4. How scanner generators work

A `scanner generator` like flex or alex takes a language definition as *input*, in the form of regular definitions. As *output* it produces a program — a scanner for the regular language specified.

Conceptually, the scanner generator makes two important transformations:

1. From an NFA to a DFA (**determinization**).
2. From a DFA to a minimal DFA (**minimization**).

If N has k states, then D may have up to 2k states. (???)

The algorithm for constructing a DFA D from an NFA N is called the `subset construction algorithm`. It uses these two operations on N:

1. $\epsilon-closure(T)$ gives the set of states that can be reached from any of the states $t \in T$ using $\epsilon$ transitions only.
2. $move(T,a)$ gives the set of states that can be reached from any of the states $t \in T$ on input symbol a.

_If a state in D contains more than one accept state of N, the only effective one is the one for the rule that occurs first._

## III. Syntax analysis (parsing)

## 6. Context-Free Grammars

The `parser` transforms a stream of tokens into a syntax tree.

The `terminal` symbols of a grammar are the tokens returned by the scanner. They are called “terminal” because the grammar considers them to be atomic.

The `nonterminal` symbols of a grammar denote syntactic categories, such as expressions, statements and declarations. They are called “nonterminal” because the grammar specifies how they can be decomposed into sequences of smaller units. One nonterminalis distinguished as the `start symbol`.

A context-free grammar consists of a set of `productions` of the form $nonterminal \to (terminal | nonterminal)^*$.

`α, β and γ` to denote sequences of zero or more symbols;
`ϵ` to denote the empty symbol sequence.

A `derivation` is a sequence of strings of symbols.

No algorithm can exists to determine whether the language generated by some grammar is inherently ambiguous. This is a well-known example of an `undecidable` problem.

## 7. LL(1) Grammars and Languages

`Top-down parsing algorithms` build parse trees from the top down (starting at the root of the parse tree, the start symbol),
while `bottom-up parsing algorithms` build them from the bottom up (starting at the leaves of the parse tree, the tokens).

## Common Parsing Methods

### Some top-down parsing algorithms

- LL parsing, for LL grammars.
- Recursive descent, for LL grammars.
- Recursive descent with backtracking, for arbitrary grammars.

### Some bottom-up parsing algorithms

- Shift-reduce parsing, for LR grammars.
- Operator-precedence parsing, for simple operator grammars.
- CYK parsing (named for its designers, Cocke, Younger and Kasami), for so-called Chomsky Normal Form grammars, a form that an arbitrary grammar can be brought into.

## 8. Recusive Decent (Parsing with Haskell)

`Parsec` is a library that supports construction of recursive descent
parsing, with backtracking.

In the Parsec view, a parser is a state transformer and is best thought
of as an instance of the state monad.

`type Parser a = Parsec Token UserState a`

## 9. Table-Driven LL(1) Parsing

The parsing table M tells the parser which production to use to
expand a given nonterminal when a given terminal symbol is the next
symbol in the input. Formally, $M[A, a] = A \to α$ for all a in
$LOOK(A \to α)$. For this to be well defined, the grammar must be
LL(1).

`LL(1) parsers` are also called `predictive parsers`.

## 10. Shift-Reducing Parsing

`Bottom Up Parsers` construct rightmost derivations.

`SLR`: Simple LR parser -> LR(0).
Input from **L**eft; derivation from **R**ightmost.

*`Handles` formalize the intuition:*

- A handle is a reduction that allows further reductions back to the start symbol.
- We only want to reduce at handles.

*To recognise a viable prefixes, we must:*

- recognize a sequence of partial rhs's of productions, where
- each partial rhs can eventually reduce to part of the missing suffix of its predecessor

*Important Fact about bottom-up parsing:*

(1). A bottom-up parser traces a rightmost derivation in reverse

(2). In shift-reduce parsing, handles appear only at the top of the stack, never inside

(3). For any grammar, the set of viable prefixes is a regular language.

### Bottom up parsers have two actions

- A `shift` action moves the first token in the remaining input to the top of the stack.
- A `reduce` action is applicable when the top n symbols on the stack match the right hand side of a production; it replaces those symbols with the nonterminal on the left hand side of that production.

### LR(k) Items

An **LR(0)** item has the form $A \to \alpha \bullet \beta$, where $A \to \alpha \beta$ is a production of the grammar.
This item indicates that so far we have seen a string matching α, and
if we next see β, we can reduce αβ to A.

An **LR(k)** item is an LR(0) item plus k terminal symbols of
**lookahead**. It indicates that so far we have seen a string matching α,
and if we next see β followed by the lookahead terminal symbols, we
can reduce αβ to A.

*An `item` is a production with a $\bullet$ somewhere on the rhs.*

*The states of the DFA are "canonical collections of items" or "canonical collections of LR(0) items".*

*$X -> b \bullet r$ is valid for a viable prefix $ab$ if $S' -> aXw -> abrw$ by a right-most derivation;
after parsing ab, the valid items are the possible tops of the stack of items.*

### Closure of LR(0) Items

> `Closure` operating is doing something as long as it makes some difference.

### Actions for Items

Items can be classified into two groups based on whether or not they
have some grammar symbols after the dot：

- An item such as $E \to E \bullet + T$ indicates that the string we have seen so far matches E, and suggests that if we now see a +, we should shift that symbol.
- An item such as $E \to E + T \bullet$ suggests that we should reduce that production.

## 11. SLR and LR(1) Parsers

### SLR Parsers

`Simple LR parsers` (`SLR` parsers for short, also called `SLR(1)` parsers)
reduce the production $A \to \alpha$ in a state containing the item $A \to \alpha \bullet$
only if the next token is in $FOLLOW(A)$.

### Constructing SLR - LR(0) Tables

We use the `sets-of-items construction` to compute ${I_1, . . . I_n}$, the collection of sets of LR(0) items. Then, for each set $I_i$:

1. If $A \to \alpha \bullet t \beta$ is in $I_i$, t is a terminal, and $goto(I_i, t) = Ij$, set $action[i, t]$ to '**shift** j'.
2. If $A \to \alpha \bullet$ is in $I_i$ where A is not S', set $action[i, t]$ to '**reduce** $A → α$' for all t in FOLLOW(A).
3. If $S' \to S \bullet$ is in $I_i$, set action[i, $] to '**accept**'.

For all _nonterminals_ A, if $goto(I_i, A) = I_j$, set $goto[i, A]$ to j.

All remaining entries are set to 'error'.

### Conflicts in Tables

If this algorithm doesn't try to assign two different actions to the same slot in the action table, then the grammar is SLR.

There are two ways in which the algorithm may assign more than one action to a table entry:

- A shift/reduce conflict: one item in a set suggests a shift action, another suggests a reduce action.
- A reduce/reduce conflict: different items suggest reductions using different productions.

### LR(1) Items

We can incorporate the extra right-context information into items.

An LR(1) item has the form $[A \to \alpha \bullet \beta, t]$ where t is a terminal or '$'.

An LR(1) item $[A \to \alpha \bullet, t]$ calls for the reduction $A \to \alpha$ only if the next input symbol is t.

Formally, an LR(1) item $[A \to \alpha \bullet \beta, a]$ is valid for a viable prefix $\gamma$ if there is a rightmost derivation $A \implies^* \delta A \omega \implies \delta \alpha \beta \omega$ where $\gamma = \delta \alpha$ and a is the first symbol of $\omega$$.

### Constructing LR(1) Tables

We use the `sets-of-items construction` to compute ${I_1, . . . I_n}$, the collection of sets of LR(1) items. Then, for each set $I_i$:

1. If $[A \to \alpha \bullet t \beta, u]$ is in $I_i$, t is a terminal, and $goto(I_i, t) = Ij$, set $action[i, t]$ to '**shift** j'.
2. If $[A \to \alpha \bullet, u]$ is in $I_i$ where A is not S', set $action[i, t]$ to '**reduce** $A → α$'.
3. If [$S' \to S \bullet$, $] is in $I_i$, set action[i, $] to '**accept**'.

For all _nonterminals_ A, if $goto(I_i, A) = I_j$, set $goto[i, A]$ to j.

All remaining entries are set to 'error'.

### Compacting LR Tables

- action table rows are often identical, so replace rows by pointers to rows.
- They are also sparse, so represent them as association lists: (terminal symbol, action), . . ., (default, action).
- The default action can replace an error by a reduction without compromising correctness; the error will be discovered later.
- goto table error entries are never consulted, so represent goto tables as an association list for each nonterminal, maybe with a default: (current state, next state), . . ., (default, next state).

### Left Recursion

While LL parsers cannot handle left recursion, LR parsers actually
perform better with left recursion than with right recursion.

## 12. LALR Parser Generation with happy

### Parser Generators

A `parser generator` takes a description of a context-free language and
builds a parser from that.

`LALR` - Look-Ahead LR parser

## IV. Semantic analysis

## 13. Semantic Analysis

### Attribute Grammars

Semantic analyzers can be formally described by attribute grammars.

An `attribute grammar` consists of：

- a context-free grammar,
- a set of attributes for each grammar symbol (both terminal and nonterminal),
- rules for assigning values to the attributes of the nodes in a parse tree, and
- a set of conditions on attribute values.

### Attributes

An attribute grammar effectively attaches one such structure to each node of the parse tree, except the nodes for grammar symbols that have no attributes.

Assigning values to the fields of these structures is called `annotating` or `decorating` the parse tree.

### Attributes: Environments

The environment is the symbol table, or a part thereof. If there is only one and it is updated destructively, people tend to call it the `symbol table`; if several different versions can exist simultaneously, people tend to call it the `environment`.

### Semantic Rules

In attribute grammars, each production has associated with it a set of `semantic rules`.

The attributes of each grammar symbol are divided into two sets:

- the `synthesized attributes`
- the `inherited attributes`.

If A.s is a `synthesized attribute` of A, then every production of the form $A \to X_1, . . ., X_n$ must have a semantic rule $A.s := f (c_1, . . ., c_k)$.

If A.i is an `inherited attribute` of A, then every production of the form $X \to X_1, . . ., A, . . ., X_n$ must have a semantic rule $A.i := f (c_1, . . ., c_k)$.

**Synthesized** attributes represent information flowing **upward** in the parse tree, while **inherited** attributes represent information flowing **down** and **sideways**.

_Terminals_ usually only have _synthesized_ attributes set by _lexical analysis_.

### Attribute Dependencies

Given a semantic rule $b := f(c_1, . . ., c_k)$, every one of the $c_i$ must be computed before we can compute b. We say that b `depends` on each of the $c_i$.

We can plot the dependencies for a given parse tree as a graph. This graph should be `acyclic`, since no attribute should depend on itself, directly or indirectly: if it does, then its value can never be computed.

Attributes can be evaluated in any order compatible with the dependencies. Such an order can be computed by a `topological sort` of the graph.

### S-Attributed Definitions

An `S-attributed definition` is a `syntax-directed definition` that **uses only synthesized attributes**.

### Syntax DAGs

The abstract syntax tree need not be a tree; it can be a `directed acyclic graph` (`DAG`). DAGs generalize trees by allowing a node to have more than one parent.

#### Constructing DAGs

To achieve this, get mkleaf and mknode to look up a hash table to see if the desired subtree already exists (this is `hash-consing`).

## 14. Syntax-Directed Definitions

### S-Attributed definitions with RD (recursive-decent) parsers

For each non-terminal A, build a function that returns A's single synthesized attribute. If A has more than one synthesized attribute, group them into a single structure.

The code of this function is an _extended version of recursive descent_. The code for each production handles each part of the RHS:

- **terminal X**: match token, advance input and return X's synthesized attribute.
- **nonterminal B**: generate $B.s := f (A_1.s, . . ., A_k .s)$ where B.s is B's synthesized attribute.
- **action code**: copy to function. The code has access to the synthesized attributes of the symbols on the RHS, and computes the value to be returned: the synthesized attribute of the LHS.

### L-Attributed definitions

In an `L-attributed definition`, a node may have synthesised attributes, and it may inherit attributes from its parent and from its siblings to its left, but not from its siblings to the right.

This allows all attribute values to be computed using a single _depth-first, left-to-right_ traversal of the parse tree.

### L-Attributed Definitions with RD Parsers

Building a recursive descent parser for an L-attributed definition is only slightly harder than building one for an S-attributed definition.

The main difference is that the functions we build now have arguments, one argument for each inherited attribute of the nonterminal they recognize.

The actions that compute the values of the inherited attributes of a nonterminal on the right hand side can be anywhere before the call to the function that recognizes that nonterminal.

## V. Intermediate code generation

## 15. Intermediate Code Generation

### Three-address code

The usual form of IR is some variant of three-address code.

`Three-address code` represents the program as a sequence of (possibly labelled) instructions for a relatively simple and more-or-less idealized abstract machine.

It gets its name from the fact that the most complex instructions of this abstract machine usually have the form **x := y op z**.

### Lvalues vs Rvalues

An `lvalue` designates a storage location—a memory cell or a register.

An `rvalue` designates a value that may or may not be stored in an lvalue.

Assignments have lvalues on the left and rvalues on the right. (This is how lvalues and rvalues got their names.)

Any lvalues on the right are implicitly dereferenced. `Dereferencing` looks up the value stored in a location.

Most three-address codes use lvalues whenever possible. Some opcodes put a constant into an lvalue; others then refer to it using that lvalue.

For example, x, y and z are usually all lvalues in x := y op z.

## 16. Syntax-Directed Translation

### Stack Slots for Temporaries

The code generator should keep two counters. One counts the number of temporary slots in use right now; the other records the maximum value the first counter has ever reached.

The size of the stack frame should be the sum of the number of slots needed for variables, the maximum number of slots needed for temporaries, and the number of slots needed for other purposes (for
example, the saved return address).

The compiler generates the `prologue` (which allocates the stack frame) after it generates the code of the body of the function, because only then does it know the second number.

### Boolean Expressions without Boolean Comparisons

Few instruction sets have comparison instructions that put a boolean result into a general purpose register. Most have comparison instructions that put the comparison result into a special register
called the `condition code register` or `CCR`, where only conditional branch instructions can look at it.

Compilers for such machines give each boolean expression E two inherited attributes, E.true and E.false, specifying the labels at which execution should resume if E is true and false respectively.

The statements where boolean expressions appear, for example, loops, if-then-elses and also assignments of booleans, supply the labels.

## VI. Runtime systems

## 17. Runtime environments, calls and returns

### Activation trees

We can depict **a run of a program** as a **tree** in which:

- each **node** represents a **function call** or **function activation**,
- each **edge** represents a call from the parent to the child, and
- a **child** on the left finishes before a child on its right begins.

#### Control stack

It is natural to use a stack to handle function activations, which is why `activation records` are also called `stack frames`.

A **call** **pushes** a new frame on the stack, a **return** **pops** a frame from the stack. The **state** of the stack at any one time corresponds to a **path** from the root of the activation tree to a node.

### Runtime address space layout

- `Text`: Executable code.
- `Read-only` data section: Constant global variables.
- `Data` section: Initialized writeable global variables.
- `Bss` section: Uninitialized writeable global variables.
- `Heap`: Dynamically allocated memory.
- `Stack`: Formal parameters and local variables.

_The exception is that local variables that keep their values from one
call to the next (static locals in C) are stored as if they were
globals, and have corresponding lifetimes._

### Scope

The `scope` of a declaration is the part of the program in which the declaration applies. The scope rules of the language specify which declaration applies to each occurrence of a name.

### Binding of names

An `environment` is a function that binds names to storage locations
(lvalues) that are either absolute or relative to a pointer (e.g., the
stack pointer). The environment changes when execution moves
from one scope to another.

A `state` is a function that maps lvalues to rvalues. The state changes
when the program executes an assignment statement.
The environment is controlled by the compiler, with help from the
linker and runtime system. The state is controlled by the program.

### Activation records

Most platforms have an `Application Binary Interface` (`ABI`) standard that dictates the form of activation records.

### Creating and destroying activation records

The ABI dictates the tasks of the caller and the callee, even if they
are written in different languages. For example, it may say:

Call sequence:

1. Caller pushes actual parameters and space for return value;
2. Caller pushes saved fp, saved sp and return address;
3. Caller sets both sp and fp to point to return address slot;
4. Callee increments sp to reserve space for its locals and temps.

Return sequence:

1. Callee places return value;
2. Callee restores the saved sp and fp;
3. Callee branches to return address;
4. Caller picks up return value and pops the actual parameters.

### Formal and actual prameters

**Formal parameters** are always **lvalues**.

**Actual parameters** are rvalues that may or may not also be **lvalues**.

### Parameter passing conventions

- call by value
- call by reference
- call by value-result
- call by name
- call by need

### Language evaluation order

When passing arguments on the stack, it is usually most convenient
for callers to evaluate arguments right to left. That way, each
evaluated actual parameter can be pushed on the stack, with the last
pushed parameter being the first parameter. This parameter ends up
on top, where any callees with variable-length argument lists can use
it to figure out how many other arguments there are.

When passing arguments in registers, it is usually most convenient for
callers to evaluate arguments left to right. That way, when evaluating
argument N into (say) register N, all the registers with numbers
higher than N are available for subexpressions.

To leave themselves freedom to pick either of these approaches,
language designers usually explicitly leave the argument evaluation
order undefined.

### Register save/restor

Registers that are (a) live in the caller and (b) used in the callee must
have their values saved in stack slots at call, and restored at return.

To support separate compilation and higher order calls (calls through
function pointers in C), neither the caller nor the callee should be
required to know the identity of the other.

The caller and callee thus each know only half of that condition.
Compilers therefore use a **conservative approximation** of the set of
registers that need saving and restoring: they guarantee saving and
restoring those registers, but may also save and restore some others.

### Caller save registers Vs. Callee save registers

The algorithm relies on the ABI classifying registers into two kinds:
caller save registers and callee save registers.

If a **caller save register** is live in the caller, then the caller must save
its value in its own stack frame before the call, and restore its value
after the return.

If a **callee save register** is used in the callee, then the callee must save
its value in its own stack frame just after the call, and restore its
value just before it returns.

The code of each procedure typically starts with a **prologue** that
performs the callee’s side of the call sequence and saves the callee
save registers at risk, and ends with an **epilogue** that restores those
callee save registers and performs the callee’s side of the return
sequence.

## 18. Symbol tables & Memory management

### Implementing nested procedures

If you want to support nested procedures, each stack frame should have an extra slot, the `access link` - points to the stack frame of the nearest scope enclosing the called procedure that has a stack frame, if there is such a scope.

### Stack slot allocation

We can _minimize the padding needed_ by allocating the largest variables, the ones with the strictest alignment requirements, first.

### Memory allocation

The algorithm we use to allocate stack slots can also be used to allocate slots to parameters passed on the stack and to the temporaries stored after the local variables. The only difference, besides the fact that _parameters must be passed in their original order_, is that the _starting offsets are different_.

For **temporaries**, it is the first stack slot free after the locals.

For **parameters**, it is the first parameter slot (dictated by the ABI), and the direction of allocation is in **_reverse_**.

The same algorithm also works for allocating memory to **globals**, and to **locals** that preserve their values across calls. The compiler must figure out which section should contain the variable (rodata, data or bss), and use the counter for that section.

### Structures

The layout of structures is also governed by alignment considerations, since most languages do not allow fields to be reordered.

### Multidimentional arrays

N-dimensional arrays can be handled as arrays whose elements are
themselves (N-1)-dimensional arrays.

$var: array[lo_1..hi_2, lo_2..hi_2, lo_3..hi_3]$ of type

With the standard **row-major layout**, the address of $var[i, j, k]$ is

    start + (i − lo1) * ((hi2 − lo2 + 1) * (hi3 − lo3 + 1) * eltsize)
    + (j − lo2) * ((hi3 − lo3 + 1) * eltsize)
    + (k − lo3) * (eltsize)

This can be more quickly computed as

    start + (((((i − lo1) * s2) + (j − lo2)) * s3) + (k − lo3)) * eltsize

using the constants

    s2 = hi2 − lo2 + 1 and s3 = hi3 − lo3 + 1.

### Heap allocation

When a function wants to create data that **outlives its stack frame**, it
must allocate that data on the `heap`.

Since the sizes of both local and global variables are fixed, any
variable-sized data structures must also be allocated on the heap
(with a few exceptions, e.g., declare x as array[n] of float).

The heap is _managed by the runtime system_, which provides
functions such as malloc and free to allocate and deallocate
memory on the heap.

They maintain a list of free memory blocks and their sizes as well as
a list of in-use memory blocks and their sizes. When there is no free
block that can accommodate a request, malloc asks the operating
system for more memory. Usually (not always) the request is granted.

### Memory management

Not freeing a memory block when it becomes
unreachable causes that memory block to be unavailable for any use
while the process is alive; this is a `memory leak`.

The solution adopted in some
languages (Haskell, OCaml, Prolog, Java, Scala,...) is to require the
runtime system to support `garbage collection`.

### Reference counting garbage collection

The simplest garbage collector extends each heap block with an extra
field, the `reference count`, counting the pointers pointing to the block.

Reference counting has high overhead, and cannot recover blocks of
garbage with cyclic references pointing to each other.

### Mark and sweep garbage collection

Other garbage collection algorithms kick in _only when memory has
run out or is about to_. Then they find and mark all live heap blocks
using a simple recursive algorithm:

- find all pointers in global variables and on the stack, and mark all blocks they point to as live;
- find all pointers in live blocks, and mark all blocks they point to.

Garbage collection is **inherently approximate**: `Reachable` means "could be used later".

### Non-moving Vs. Moving garbage collectors

A `non-moving garbage collector` links all garbage items together in a
freelist; future allocations take memory from there.

A `moving garbage collector` consolidates non-garbage into a
contiguous region of memory, adjusting all pointers to the moved
blocks as they go. This:

- allows memory to be allocated by simply advancing a pointer,
- gives greater locality of reference, and
- defragments free memory as items are consolidated.

### Identifying pointers

In the implementation of languages such as Lisp and Prolog, every
word of memory usually has a few bits that serve as a `tag`, _identifying
what the rest of the word holds_: an integer, a float, a pointer, . . .

For garbage collection, it is particularly important to be able to tell
what is a pointer and what is not. A **conservative collector** treats
anything that looks like a pointer as if it is a pointer. A **type-accurate
collector** maintains type information to decide which values are
pointers. It relies on the compiler to generate **read-only data
structures** (`RunTime Type Information` or `RTTI`) that tell the runtime
system, for each stack slot of each function and for each global, the
type of the value stored in that memory location.

### Generational garbage collection

A `generational garbage collector` tries to capitalize on the fact that
allocated items tend to have very different lifetimes, and a recently
allocated item is more likely to next become garbage than an item
that has been alive for a long time.

## VI. Final code generation

## 19. Code Generation and Local Optimization

### Instruction selection

The compiler associates a cost with each alternative code sequence,
and chooses the one with the best `cost measure`. The cost may be:

- the _size of the code sequence_ (measured in _bytes_),
- its _execution time_ (measured in _clock cycles_),
- or a _composite measure_.

Unfortunately, on modern CPUs, the time taken to execute an
instruction sequence depends on many **external factors** (e.g., the
_contents of the cache_, and what the _previous instructions_ in the
_pipeline_ are), so there may not be a choice that is always best.

### Looking up variables

When generating code that accesses the value of a variable, we need
to know all the places where the value is available. This can be any
subset of the following:

- in the variable’s memory location (its stack slot, or its slot in a rodata, data or bss section),
- in one or more registers,
- a known constant.

The code generator will prefer to pick up the value from a register,
since the instruction sequence using a register will be faster and
smaller than the alternatives.

If the variable is not available in any register, loading a constant into
a register is usually preferable to a memory access.

### Pattern matching

Since patterns can be described by grammars, the pattern matching
is often done using parsing technology.

Pattern matchers in code generators are therefore often **greedy**: they
try to implement as many IR instructions as possible with a single
target instruction, as long as doing so improves the cost.

### Optimisations

Compilers can perform optimizations both on IR and on the
generated target code. Usually, each optimization is done on only one
representation, but some compilers perform similar optimizations on
both the IR and the target code.

An optimization that makes the IR smaller reduces the size of the
input to later compiler passes.

#### Strength reduction

Look for opportunities, in loops, to replace a more expensive
operation by a cheaper one: e.g. replacing a multiplication to an addition.

#### Exploiting algebraic identities

E.g. The identity x ∗ 2 = x + x allows us to replace mul r1, r2, 2 with
add r1, r2, r2, which is probably faster, but won’t count that as
strength reduction.

Better yet, utilize x ∗ 2 = x ≪ 1 (a left shift).
More subtly, utilize, say, x ∗ 15 = (x ≪ 4) − x.

Other algebraic laws about _commutativity, associativity, distributivity,_
and so on, may help us reorganise expressions so that operations are
performed in a better order

#### Reordering of operations

#### Peephold optimisation (local optimisation)

A cheap way to optimize code sequences is to peep at them through
a small sliding window, and try to fit the instructions in that window
into known patterns. Code that fits a source pattern is then replaced
with equivalent code that is shorter and/or faster, created from the
target pattern associated with the source pattern.

#### Jump optimisations

Branch and jump instructions can be quite expensive on current
CPUs, because they disrupt the flow of instructions through pipelines.

Short-circuiting jumps needs a separate optimization pass, because
the knowledge it needs does not fit in a peephole window.

## VII. Optimization

## 20. Basic blocks and optimisation

### Basic blocks

A `basic block` is a sequence of instructions that can be entered only
at the first instruction (the leader) and can be left only from the last
instruction. Every instruction in a basic block is thus executed the
same number of times.

To convert an instruction sequence to basic blocks, first determine
the `leaders`:

- The first instruction is a leader.
- Any instruction that is the target of a branch of any kind (conditional, unconditional, call, return is a leader.
- Any instruction immediately following a branch instruction of any kind is a leader.

The basic block of each leader extends to the instruction just before
the next leader (if there is one).

### Control glow graph (CFG)

We can identify each _basic block_ by its _label_.

You can consider the implementation of a procedure to be a graph,
whose `nodes` are the _basic blocks of the procedure_ and in which an
`edge` from basic block B1 to basic block B2 represents a _potential
transfer of control_ from the last instruction of B1 to the first
instruction of B2.

The `transfer` may be _via a branch instruction_ or _by simple fallthrough_.

If there is an edge from B1 to B2, then B1 is a `predecessor` of B2,
and B2 is a `successor` of B1.

CFGs are very useful for optimizers. For example, basic blocks that
have no predecessors can eliminated as dead code.

### Reaching definitions

A `program point` lies just before and just after each instruction.

A `path` is a sequence of program points, each separated from the previous one by a single instruction or the traversal of an edge in the CFG.

A `definition` of an lvalue is an instruction that updates the rvalue stored in that lvalue.

An `ambiguous definition` of an lvalue is an instruction (such as a call or an assignment through a pointer) that may update the rvalue stored in that lvalue.

A definition d `reaches` a point p iff there is a path from d to p with
no unambiguous definition of the lvalue.

### Liveness analysis

A `use` of an lvalue is an instruction whose outcome depends on the
rvalue stored in that lvalue.

An lvalue lv is `live` at program point p if there is a path starting at p
on which lv is used before being (unambiguously) defined.

### Renaming Lvalues apart

Two basic blocks are equivalent and thus interchangeable if they
compute the same rvalues for the all lvalues that are (or are assumed
to be) live at the end of the block.

We can use this to ensure that the lvalues defined by the instructions
of a basic block are `syntactically distinct`.

### Reordering inside basic blocks

Avoiding repeated assignments to an lvalue in a basic block gives the
optimizer more freedom to reorder the instructions of the block.

### Reuse Lvalues

Many code sequences compute the same subexpression several times,
even when this is not apparent in the source.

### Common Subexpression Elimination

### Copy elimination

### Class of optimisations

An optimization that considers each basic block in isolation is a `local optimization`.

If it takes several basic block into consideration then it is a `global optimization`. Amongst global optimizations we distinguish those that consider interactions of procedures/functions:

An optimization that considers each procedure body in isolation is an `intraprocedural optimization`.

An optimization that considers procedure interactions is an `interprocedural optimization`.

## 21. Register allocation

### Register allocation

If there are more variables than free registers, figuring out which variables to keep in registers can be quite complex. No efficient optimal algorithm exists, since the problem of optimal register
allocation is **NP-complete**. We have to settle for heuristic approximations.

Register allocation can be done at the IR or the machine language level, and the techniques are the same.

It is more commonly done in the machine language, because, as we have seen, a single IR instruction can result in several machine-code instructions, and several IR instructions can sometimes be folded into a single machine-code instruction, and the need for registers may not be preserved. Also, some target machines impose constraints on which registers can be used by which instructions.

### Register allocation by usage count

> The critical register allocation decisions are for loops L.

For each variable x, we can try to get a rough estimate of the `benefit` we could expect from keeping x in a register. Savings, roughly:

1. for each use of x in a block where x does not get defined, plus
2. for each block where x gets defined and is live at exit.

Hence the “benefit” of choosing x to be kept in a register is
$benefit = \Sigma_{block B \in L} use(x, B) + 2 \times live(x, B)$

#### Register sharing and variable interference

If two or more variables live independent lives, they should be able to share the same register.

We say that u `interferes` with v if v is ever alive immediately after u is defined.

The condition implies that u and v can be alive at the same time, and so they cannot be stored in the same register.

(Actually we can weaken the condition: For a copy instruction u := v, the two variables are alive at the same time, but since they hold the same value anyway, they can still share a register.)

We will do **global register allocation** in the sense that we ensure a variable can stay in the same register throughout a function or procedure body.

#### Interference graph

Given the code of a procedure, consider each statement as if it were an IR instruction. Break the code into basic blocks and construct a CFG. Find out which variables are live at which program points.
The interference graph has a `node` for each **variable**.

There is an `edge` between u and v if they **interfere**. So the edge means that the two are alive at the same time, and thus cannot be stored in the same register.

### Register allocation by graph colouring

We can apply standard algorithms for `graph colouring` to the interference graph, with each colour representing a register.

The algorithms tell us whether the graph can be coloured with a given number of colours (the number of available registers), and if so, which sets of nodes should share a colour (which sets of variables
should be stored in the same variable).

#### Graph colouring

Given k registers. How to colour the graph with k colours, so no edge connects two nodes of the same colour (or decide it is impossible)?

This is an **NP-complete problem (for k > 2)**, so we cannot hope for an optimal algorithm.

But a good, sub-optimal algorithm is fine — we will just generate a target program that is slightly slower that it might have been, if we could colour optimally.

#### Reasonably good graph colouring

A good fast **heuristic** is this:

Until the graph is empty,

- pick _a node with fewer than k adjacent nodes_; remove it and its edges.
- We then allocate colours to nodes in the _reverse order of removal_.

Of course we may encounter a graph where no node has less than k
adjacent nodes.
At that stage we produce code for `register spilling` to memory.

#### The algorithm in general

Suppose we have N colours available. The **greedy colouring algorithm** says to choose a node with fewer than N edges, if possible, and remove that node.

When that is not possible we can choose randomly (or follow any one of many suggested sub-heuristics).

However, in the next phase, when colours are allocated, we have a real problem if we get to a node whose list of neighbours have already used up N colours.

In that case we pop the node and leave it uncoloured. The corresponding variable will need to spend (most of) its time in memory — no register can be globally allocated. We say that the variable is `spilled`.

#### Spills and other complications

If spilling occurs, the code needed to effect the spilling usually introduces `fresh variables`, so liveness analysis and register allocation may need to be repeated!

A compiler should allocate all variables accessed in inner loops to registers if this is at all possible. Beyond that, there are no universally superior heuristics about the choice of which variable to spill.

Choices about spills are also complicated by calls.

Register allocation may also be complicated by the fact that on some target machines (for example, x86), some instructions have their operands in fixed registers and cannot be told that their operands are in registers chosen by the compiler.

## 22. Global optimisation

### Reasoning about liveness

Here are the basic rules:

1. (liveness **generation**) If an instruction uses v then v is live just before the instruction.
2. (liveness **kill**) If an instruction assigns a value to v , and v is not also used by the instruction, then v is dead just before the instruction.
3. (livenss **propogation**) If v is live just after an instruction then v is live just after each immediately preceding instruction.
4. (livenss **propogation**) If v is live just after an instruction, and the instruction does not assign a value to v , then v is also live just before the instruction.

Rule 1 says how liveness is **generated**, rule 2 how liveness is **killed**, and rules 3 and 4 how it is **propagated**.

### Finding successors for instructions

Assume instructions are numbered consecutively.

The `set of successors`, $succ[i]$, of the instruction numbered $i$, is found
using these rules:
1 If the instruction is goto $l$ then $l$ is in $succ[i]$.
2 If the instruction is if p goto $l$, then $\{i + 1, l\} \subseteq succ[i]$.
3 Otherwise $i + 1$ is in $succ[i]$.
Note that we assume that both outcomes of p are possible.
(Whether they are is an undecidable problem.)
So we conservatively over-approximate the set of successors.

### The equations that define propogation

We want:

- the set $in[i]$ of variables that are live just before $i$, and
- the set $out[i]$ of variables that are live just after $i$.

To find these, solve this set of **recursive equations**:

$in[i]= gen[i] \cup (out[i] \setminus kill[i])$

$out[i] = \cup_{k \in succ[i]} in[k]$

To solve these, initialise all $in[i]$ and $out[i]$ to empty sets, and repeatedly use the equations to update until no more changes occur.

This works under the assumption that all variables are dead at the end of the program. If not, start from $out[i_{last}] = vs$, where $i_{last}$ is the last instruction and vs is the set of variables that are live after that.

#### solving liveness equations by iterations

If we do another iteration, we see that the result has not changed:
We have found a `fixed point` for the process.

The final iteration shows what is live at each program point (or we
should say: **could** be live).

### Dataflow analysis

Liveness analysis, as presented, is an _instance_ of a general framework for `data flow analysis`.

We discussed _subexpression elimination_ and _copy elimination_ as optimizations within basic blocks, but like many other optimizations, they can be extended to cross basic block boundaries using the same
idea as we used for liveness analysis.

Dataflow analysis associates four sets of facts with each instruction $i$:

- $gen[i]$ = facts added by i
- $kill[i]$ = facts removed by i
- $in[i$] = facts before i
- $out[i]$ = facts after i

The $in$ facts are usually defined exactly as we did for liveness.

`Liveness` is a **backward analysis**, propagating information _against the flow of control_.

If we instead aggregate over $k \in pred[i]$, we get **forward analyses**.

And if we change the $\cup$ to $\cap$, we get analyses that reason about
what `must` happen, rather than what `may` happen.

All four combinations have useful instances, for example:

|      | Forwards              | Backwards             |
| ---- | --------------------- | --------------------- |
| Must | Available expressions | Very busy expressions |
| May  | Reaching definitions  | Liveness              |

### Detailed Optimisations

#### common subexpressions, local

#### exploiting algebraic identities

#### common subexpressions elimination

#### copy propagation

#### dead code elimination

#### loop optimisations

> A very important target for optimization is the loop.

Even small improvements to inner-most loops can have a huge
impact on runtime performance.

##### loop optimisations: Code motion

The aim of code motion is to move invariant code outside loops.

##### loop optimisations: strength reduction

Strength reduction replaces an operation with a cheaper one.

For example, a compiler may look for opportunities to replace multiplications with additions.

##### loop optimisations: induction variables

We wish to identify a set of variables that remain in a fixed relationship throughout the execution of a loop, and remove all but one.

#### goto reduction

#### code specialisation

We can replace the if-then-else after the switch with an if-then-else at the end of every switch arm.

#### syntax-directed optimisation

Some optimizations can be done more conveniently on the abstract syntax tree (a representation of the source program) than on the IR (a representation of the target program), because the AST is higher
level and ignores low-level details.

One can perform dataflow analysis on the AST as well as the IR. One application is `bounds analysis`, which tries to optimize away array bound checks by keeping track of whether the index values are
guaranteed to be within bounds or not.

#### interprocedural optimisation

You can move f(x) (called inside a loop) out of the loop only if you know that it doesn’t have side effects.

In an imperative language, this requires interprocedural analysis: transmitting information from one procedure to another.

Many compilers can do that if the two procedures are in the same module. Few can do it well if the two procedures are in different modules.

#### inlining

Most compilers can perform `inlining`, which involves _replacing a call with a copy of the body of the called procedure_.

The copy has fresh names for all the locals and has all formal parameters replaced with the corresponding actual parameters (except with value-result, which requires copy-in/copy-out code).
Inlining avoids procedure call overheads, but is usually worthwhile only if the callee is small, or if the caller has information that can be used to optimize the callee.

#### high level optimisations

The optimizations we have discussed so far are low level: close to the machine. They can help speed up most programs, but typically only by small constant factors: a few percent to a few tens of percents.

If you want bigger speedups than that, the optimizer must change the algorithm.
e.g. in fib function, you can change the complexity of this code from exponential to
linear by caching the value of fib(k), once computed.

### Declarative languages

High level optimizations are typically not feasible when the source language is imperative, because with such programs the compiler usually cannot be sure whether reordering or eliminating statements
will change the output of the program.

When the source language is a pure functional or logic programming language, a call can be deleted if and only if its output is not needed, and two calls can be reordered if and only if neither call computes an input of the other call. No other analysis is needed.

Compilers for declarative languages can thus perform optimizations that can yield large speedups.

> Many of these optimizations are rarely applicable, but when they are, it is better for the compiler to do them than the programmer.