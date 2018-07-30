---
layout: post
title: Distributed Systems Revision
date: 2018-01-20 15:52 +0800
tags: notes distributed-systems
---

Some dot points of my study of COMP90015 Distributed Systems (SM2 2017).
The notes below also contain lecture materials (&copy; the University of Melbourne).

## 1. Distributed Systems Introduction

### DS Challenges

#### (1). Heterogeneity

* operating systems
* hardware architectures
* communication architectures
* programming languages
* software interfaces
* security measures
* information representation

#### (2). Distribution transparency

The system is perceived as a whole rather than a collection of independent components.

* Access transparency
* Location transparency
* Failure transparency
* Replication transparency
* Migration (mobility/relocation) transparency
* Concurrency transparency
* Performance
* Scaling transparency
* Application level transparencies*
  * Persistence transparency
  * Transaction transparency

#### (3). Fault tolerance

* `Failure` - no longer available or very slow to be usable
* `Fault` - cause of a failure
* `Fault tolerance` - no failure despite faults

Mechanisms:

* `Fault detection` - Checksums, heartbeat
* `Fault masking` - Retransmission of corrupted messages, redundancy
* `Fault toleration` - Exception handling, timeouts
* `Fault recovery` - Rollback mechanisms

#### (4). Scalability

Challenges:

* Cost of physical resources
* Performance loss
* Preventing software resources from running out, e.g. numbers (IP) or Y2K
* Avoiding performance bottlenecks

#### (5). Concurrency

* Fair scheduling
* Preserve dependencies
* Avoid deadlocks

#### (6). Openness and Interoperability

Open standards/specifications

#### (7). Security

Components:

* `Confidentiality` - Protection against disclosure to unauthorised individual information, e.g. ACLs
* `Integrity` - Protection against alteration or corruption
* `Availability` - Protection against interference targeting access to the resources, e.g. Dos, DDos
* `Non-repudiation` - Proof of sending/receiving an information, e.g. digital signature

Mechanisms:

* `Encryption`, e.g. Blowfish, RSA
* `Authentication`, e.g. passoword, public key authentication
* `Authorisation`, e.g. access control lists

## 2. Sockets

### Networking Basics

|   | TCP | UDP |
| --- | --- | --- |
| Concept | Transmission Control Protocol | User Datagram Protocol |
| Connection | Connection-oriented communication SYN, SYN-ACK, ACK | Connectionless communication No handshake |
|   | Two-way stream abstraction | Message passing abstraction |
|   | Stream no message boundaries | Transmit a single message (datagram) to the receiving process |
|   | Stream provide the basis for producer/consumer communication; Data sent by producer are queued until the consumer is ready to receive them; Consumer must wait when no data is available. | Simplest form of Interprocess Communication (IPC) |
| Header | 20 bytes | 8 bytes(msg, length, host, port) |
| Weight | Heavy-weightNeed to setup connection | Light-weightNo tracking connection |
| Reliability | Provides reliable data flow between two computers | Sends independent packets of data (Datagram); no guarantee of arrival and arrival order |
| Order | Rearrange data packets in the order specified | No inherent order |
| Speed | Slower | Faster – &quot;Best effort&quot; |
| Usage | For applications required high reliability; transmission time is less critical. | For applications need fast, efficient transmission; answer small queries from huge numbers of clients. |
| Example applications | HTTP, FTP, Telnet. Skype uses TCP for call signalling, and both TCP &amp; UDP for transporting media traffic. | DNS   Clock server, Ping, Live streaming/broadcasting |

## 4. Architectural Models

### Failure model

|   | Class of Failure | Affects | Description |
| --- | --- | --- | --- |
|  Omission and Arbitrary Failures | Fail-stop | Process | Process halts and remains halted. Other processes may detect this state. |
|    | Crash | Process | Process halts and remains halted. Other processes may not be able to detect this state. |
|    | Omission | Channel | A message inserted in an outgoing buffer never arrives at the other end&#39;s incoming message buffer. |
|    | Send-omission | Process | A process completes a send, but the message is not put in its outgoing message buffer. |
|    | Receive-omission | Process | A message is put in a process&#39;s incoming message buffer, nut that process does not receive it. |
|    | Arbitrary (Byzantine) | Process/Channel | Process/channel exhibits arbitrary behaviours: it may send/transmit arbitrary message at arbitrary times, commit omissions; a process may stop or take an incorrect step. |
|  Timing Failures | Clock | Process | Process&#39;s local clock exceeds the bounds on its rate of drift from real time. |
|    | Performance | Process | Process exceeds the bounds on the interval between two steps. |
|    | Performance | Channel | A message&#39;s transmission takes longer than the stated bound. |

## 6. RMI Programming

### Fault tolerance measures

| **Fault tolerance measures**   |    |  | **Call semantics** |
| --- | --- | --- | --- |
| **Retransmit request message** | **Duplication filtering** | **Re-execute procedure or retransmit reply** |
| No | N/A | N/A | Maybe |
| Yes | No | Re-execute procedure | At-least-once |
| Yes | Yes | Retransmit reply | At-most-once |

## 7. Security

### TLS handshake configuration options

| Component | Description | Example |
| --- | --- | --- |
| Key exchange method | Method to exchange session key | RSA with public key certificates |
| Data transfer cipher | Block/stream cipher used for data | IDEA (International Data Encryption Algorithm) |
| Message digest function | Create message authentication codes (MACs) | SHA (Secure Hash Algorithm) |

## 8. DFS

### Distributed File system/service requirements

#### (1). Transparencies

* `Access` - Same operations (client programs are unaware of distribution of files)
* `Location` - Same name space after relocation of files or processes (client programs should see a uniform file name space)
* `Mobility` - Automatic relocation of files is possible (neither client programs nor system admin tables in client nodes need to be changed when files are moved).
* `Performance` - Satisfactory performance across a specified range of system loads
* `Scaling` - Service can be expanded to meet additional loads or growth.

#### (2). Concurrency

Changes to a file by one client should not interfere with the operation of other clients simultaneously accessing or changing the same file.

Concurrency properties:

* Isolation
* File-level or record-level locking
* Other forms of concurrency control to minimise contention

#### (3). Replication

File service maintains multiple identical copies of files

* Load-sharing between servers makes service more scalable
* Local access has better response (lower latency)
* Fault tolerance

Full replication is difficult to implement.

Caching (of all or part of a file) gives most of the benefits (except fault tolerance)

#### (4). Heterogeneity

* Service can be accessed by clients running on (almost) any OS or hardware platform.
* Design must be compatible with the file systems of different OSes
* Service interfaces must be _open_ - precise specifications of APIs are published.

#### (5). Fault tolerance

* Service must continue to operate even when clients make errors or crash.
* Service must resume after a server machine crashes.
* If the service is replicated, it can continue to operate even during a server crash.

#### (6). Consistency

* Unix offers one-copy update semantics for operations on local files - caching is completely transparent.
* Difficult to achieve the same for distributed file systems while maintaining good performance and scalability.

#### (7). Security

Must maintain access control and privacy as for local files.

* based on identity of user making request
* identities of remote users must be authenticated
* privacy requires secure communication

Service interfaces are open to all processes not excluded by a firewall.

* vulnerable to impersonation and other attacks

#### (8). Efficiency

Goal for distributed file systems is usually performance comparable to local file system.