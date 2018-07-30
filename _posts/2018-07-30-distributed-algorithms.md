---
layout: post
title: Distributed Algorithms Revision
date: 2018-07-30 15:43 +1000
tags: notes distributed-systems algorithms
---

Some dot points of my study of COMP90020 Distributed Algorithms (SM1 2018).
The notes below may contain lecture materials (&copy; the University of Melbourne).

> Sidenotes:
>
> Communication channels are FIFO ordered

## Background

### Assumptions

Unless stated otherwise, we assume:

* a strongly connected network
* message passing communication
* asynchronous communication
* processes don’t crash
* channels don’t lose, duplicate or garble messages
* the delay of messages in channels is arbitrary but finit
* channels are non-FIFO
* each process knows only its neighbors
* processes have unique id’s

### Transition systems

The (global) state of a distributed system is called a `configuration`

The configuration evolves in discrete steps, called `transitions`.

A `transition system` consists of:

* a set $C$ of configurations;
* a binary transition relation $\to$ on $C$; and
* a set $I \subseteq C$ of initial configurations.

$\gamma \in C$ is _terminal_ if $\gamma \to \delta$ for no $\delta \in C$.

### Executions

An `execution` is a sequence $\gamma_1 \gamma_2 \gamma_3 ...$ of configurations that is either infinite or ends in a terminal configuration, such that:

* $\gamma_0 \in I$, and
* $\gamma_i \to \gamma_{r+1}$ for all $i \geq 0$.

A configuration $\delta$ is `reachable` if there is a $\gamma_0 \in I$ and a sequence $\gamma_1 \gamma_2 \gamma_3 ... \gamma_k = \delta$ with $\gamma_i \to \gamma_{r+1}$ for all $0 \leq i \lt k$.

### States and Events

* A `configuration` of a distributed system is composed from
* the states at its processes, and the messages in its channels.
* A `transition` is associated to an event (or, in case of synchronous
* communication, two events) at one (or two) of its processes.
* A process can perform `internal`, `send` and `receive` events.
* A process is an `initiator` if its first event is an internal or send event.
* An algorithm is `centralized` if there is exactly one initiator.
* A `decentralized` algorithm can have multiple initiators.

### Assertions

* An `assertion` is a _predicate_ on the configurations of an algorithm.
* An assertion is a `safety` property if it is true in each configuration of each execution of the algorithm. - **"something bad will never happen"**
* An assertion is a `liveness` property if it is true in some configuration of each execution of the algorithm. - **"something good will eventually happen"**

### Invariants

Assertion P is an `invariant` if:

* $P(\gamma)$ for all $\gamma \in I$, and
* if $\gamma\to\delta$ and $P(\gamma)$, then $P(\delta)$.

Each _invariant_ is a _safety_ property.

### Causal order

In each configuration of an _asynchronous_ system, applicable events
at different processes are _independent_.

The `causal order` $\prec$ on occurrences of events in an execution is the smallest transitive relation such that:

* if a and b are events at the _same process_ and a occurs before b, then $a \prec b$; and
* if a is a send and b the corresponding receive event, then $a \prec b$.

If neither $a \prec b$ nor $b \prec a$, then a and b are called `concurrent`.

### Comsputations

A permutation of concurrent events in an execution doesn’t affect the result of the execution.

These permutations together form a `computation`.

All executions of a computation start in the same configuration, and if they are finite, they all end in the same terminal configuration.

## Time and Global States

### Assumptions

* A Distributed System (DS) of N processes
* Each on a single processor with its own physical clock
* No shared memory
* Each process p has a state s at a given time - State depends on internal variable values, files it works on, etc
* Processes can only communicate via messages
* Events in a process can be ordered i.e. $e \to_i e'$
* Multi-threading - Cannot change this: we are on a single processor for each process
* History of a process $h_i = <e_i^0, e_i^1, ...>$

### Clocks

`Hardware clock` of a system is $H_i(t)$

`Software clock` is a scaled and offset added version $C_i(t) = \alpha H_i(t) + \beta$

`Skew` is relative difference in clock values of two processes.

`Drift` is relative difference in clock rates of two processes.

`Coordinated Universal Time` is atomic time that is adjusted (rarely) to astronomical time `(UTC)`.

### External & Internal synchronisation

`External synchronisation`:

* Use an external time server to synchronise process times
* For a synchronisation bound $D > 0$ and for a source S of UTC time: $|S(t) - C_i(t)| < D \textit{ for i = 1,2,...,N}$ and for all real times t in intercal I
* The clocks $C_i$ are accurate to within the bound D

`Internal synchronisation`:

* For a sunchronisation bound $D>0: |C_i(t) - C_j(t)| < D \textit{ for i = 1,2,...,N}$ and for all real times t in the interval I
* The clocks $C_i$ agree within the bound D
* Once synchronised, processes can communicate
* If all clocks drift then all can become difference than the initial external time service

> If clocks are externally synchronised, are they also internally synchronised?
>
> * External synchronisation with D => Internal synchronisationw ith 2D;
> * Internal synchronisation does not imply external synchronisation; the entire system might drift away form the external clock S!

### Faulty clocks

`Faulty clock` is a clock that do not obey the **monotonicity condition** and/or the bounds on its drift.

A clock is said to had a `crash failure` if it totally stops running, else it is said to have an `arbitrary failure`.

> A correct clock is not necessarily an accurate clock!

### Synchronization in a synchronous system

A `synchronous distributed system` is one in which the following bounds are defined:

* the time to **execute** each step of a process has known lower and upper bounds
* each message **transmitted** over a channel is received within a known bounded time
* each process has a local clock whose **drift** rate from real time has a known bound

#### Optimal point to set clocks on a network

* 2 clocks: **t + (max + min)/2**
* N clocks optimal bound on clock skew is **u(1-1/N)** where **u = max - min**
* This cannot work for the Internet

### Cristian's algorithm: Asynchronous system

> Primarily for `Intranets`

* A time server S receives signals from a UTC source
* Process p requests time in $m_r$ and receives t in $m_t$ from S
* p sets its clock to $t + Tround/2$
* Accuracy $\pm (Tround / 2 - min)$:
* the earliest time S puts t in message mt is min after p sent $m_r$
* the latest time was min before $m_t$ arrived at p
* the time by $S'$s clock when $m_t$ arrives is in the range $[t + min, t + Tround - min]$

Disadvantages:

* A single time server might fail, need to use of a group of synchronized servers
* It does not deal with faulty servers

### Birkeley algorithm

> Primarily for `Intranets`

* An algorithm for internal synchronization of a group of computers
* A master polls to collect clock values from the others (slaves)
* The master uses round trip times to estimate the slaves’ clock values
* It takes an average (eliminating any above some average round trip time or with faulty clocks)
* It sends the required adjustment to the slaves (why not the updated times?)
  * allowed to increase the clock value but should never decrease (because it may violate event ordering within the same process)
  * allowed to change speed of clock

### Network time protocol (NTP)

> A time service for the `Internet` - synchronizes clients to `UTC`

#### Modes of synchronization

* Multicast
  * A server within a high speed LAN multicasts time to others which set clocks assuming some delay (not very accurate)
* Procedure call
  * A server accepts requests from other computers (like Cristian’s algorithm). **Higher accuracy**. Useful if no hardware multicast.
* Symmetric
  * Pairs of servers exchange messages containing time information
  * Used where very high accuracies are needed (e.g., for higher levels)

#### Messages exchanged between a pair of NTP peers

* All modes use UDP
* Each message bears timestamps of recent events:
  * Local times of Send and Receive of previous message
  * Local times of Send of current message
* Recipient notes the time of receipt $T_i$ (we have $T_{i-3}, T_{i-2}, T_{i-1}, T_i$)
* In symmetric mode there can be a non-negligible delay between messages

#### Accuracy of NTP

For each pair of messages between two servers, NTP estimates an offset o, between the two clocks and a delay $d_i$ (total time for the two messages, which take t and t')

$T_{i-2} = T_{i-3} + t + o$ and $T_i = T_{i-1} + t' - o$

This gives us (by adding the equations):
Total delay $d_i = t + t' = T_{i-2} - T_{i-3} + T_i - T_{i-1}$

Also (by subtracting the equations)
$o = o_i + (t' - t)/2 \textit { where } o_i = (T_{i-2} - T_{i-3} + T_{i-1} - T_i)/2$

Using the fact that $t, t' \gt 0$ it can be shown that
$o_i - d_i /2 \le o \le o_i + d_i /2$

$o_i$ is an estimate of the offset and $d_i$ is a measure of the accuracy

NTP servers filter pairs $<o_i, d_i>$, estimating reliability from variation, allowing them to select peers

In general, higher level peers are preferred as they are closer to the UTC

### Logical Time & Clocks (Lamport 1978)

* Idea of a logical time
  * Absolute order in physical time is not necessary
  * But the causality relationships between events has to be preserved
  * Use logical times to express causal order
* Local events
  * Ordered in time for each process
  * Logical times of all events have to respect all dependencies between events
* Order of two events in a distributed system
  * Global relation, called happened-before, denoted by $\to$
  * happened-before $\to$ is based on a local (and thus easily observable) local happened-before relation $\to$p within a process p

#### Happened-Before Relation $\to$

> Definition:
>
> Let a, b, c be three events. Then, the following **global** happened-before orders hold:
>
> * **HB1** (over process): If $\exists$ process p: a $\to$p b, then a $\to$ b
> * **HB2** (over channel): For any message m: a = send(m) $\to$ b = receive(m)
> * **HB3** (transitive relation): If a $\to$ b and b $\to$ c, then a $\to$ c

$\to$ is a partial order: If two events a and b happen in different processes which do not exchange messages, then a and b are not ordered with respect $\to$
that is neither a $\to$ b nor b $\to$ a holds

_**Note** that the happened-before relation does not state anything about who caused what: An event occuring earlier does not mean that it’s the cause of a later event!_

#### Lamport’s Logical Clocks

> Apply logical timestamps $L_i$ to events for each process $p_i$
> * **LC1**:
>   * $L_i$ is incremented by 1 before each event at process pi
> * **LC2**:
>   * When process pi sends message m, it piggybacks $t= L_i$
>   * When $p_j$ receives (m,t) it sets $L_j := max(L_j, t)$ and applies LC1 before time stamping the event receive(m)

* Each process has its logical clock initialized to zero
* $e \to e'$ implies $L(e) \lt L(e')$
* However, $L(e) \lt L(e')$ does not imply $e \to e'$

LC update globally.

#### Vector Clocks

> Generating vector clock time stamps:
> * Vector clock $V_i$ at process $p_i$ is an array of N integers
> * **VC1**: set $V_i[j] = 0 \textit{ for } i, j = 1, 2, …N$
> * **VC2**: before $p_i$ timestamps an event it sets $V_i[i] := V_i[i] + 1$
> * **VC3**: pi piggybacks $t = V_i$ on every message it sends
> * **VC4**: when $p_i$ receives (m, t) it sets $V_i[j] := max(V_i[j], t[j]), j = 1, 2,…N$ (and adds 1 before the next event to its own element using VC2)

Properties:

* Vector timestamps are used to timestamp **local** events

Vector time stamp comparison:

* V = V’ iff V [j] = V’ [j] for j = 1, 2, ..., N
* V <= V’ iff V [j] <=V’ [j] for j = 1, 2, ..., N
* V < V’ iff V <= V’ and V != V’
* e $\to$ e’ iff V(e) < V(e’)
