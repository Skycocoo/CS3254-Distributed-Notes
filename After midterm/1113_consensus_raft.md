# consensus <-> broadcast

CA related (consistency and availability)

## characteristics (desirable properties)

### safety

- only a single ```value``` can be ```chosen``` (chosen: decided final result)
- only a value that has been proposed can be chosen
- a process can learn that a value is chosen only after the value is chosen

### liveness (availability)

- some proposed value is eventually chosen
- if a value is chosen, a process eventually learns it


## problem

if a process can fail: not all of those characteristics could be guaranteed => totally reliable multicast

## relationship between consensus and broadcast

if you have an algorithm C that does consensus, a totally-ordered broadcast can be defined in terms of it: consensus on ordering of the message (propose the value)

if you have an algorithm B that does totally-ordered broadcast, a consensus algorithm can be defined in terms of it: broadcast the proposed values from all nodes, and the consensus could be made that the first value being broadcasted is the final chosen value

## PAXOS algorithm

### steps (roles: Proposer, Acceptor, Learner; one process could have multiple rules)

Proposer sends a "propose" message to all Acceptors, containing its proposed value, and a unique proposal number (id = counter + "." + nodeID)

Acceptor replies ("promise" message) to the Proposer with any already accepted value and proposal number, if it has one; or with a special "no accepted value" if it doesnt have one.

Each Acceptor promises to not accept any proposal with a lower-numbered proposal number than any number thats proposed.

The Proposer should receive responses from a majority (quorum) of Acceptors:
- If it doesnt receive a response from a quorum, it cannot proceed (resend the Proposer message);
- If it does receive a response from a majority of Acceptors, the it must use the highest-numbered proposal number and value that it received;
- If it does not receive a proposal number and value (all of the replied acceptors doesnt have an accepted value), then it may use its own value

Then the Proposer sends an Accept message to Acceptors (after its replied by a quorum of Acceptors; those messages could be lost), using the highest-numbered proposal that it got in the previous stage, or its own value if not

Learners then send a message to the Acceptors, and if they get a quorum of responses, the majority value is the chosen value

#### invariants

- an Acceptor must accept the first proposal that it receives
- if a proposal with value V is chosen, then every higher-numbered proposal that is chosen has value V (shouldnt change chosen value; Proposer only accept when it receives a quorum of the Acceptors & it only proposes the highest-numbered values that are replied)
- a> if a proposal with value V is chosen, then every higher-numbered proposal accepted by any Acceptor has value V
- b> if a proposal with value V is chosen, then every higher-numbered Accept message by any Proposer has value V
- for any V and N, if a proposal with value V and number N is issued, then there is a set S consisting of a majority of acceptors such that either (A) no acceptor in S has accepted or will accept any proposal numbered less than N or, (B) V is the value of the highest-numbered proposal among all proposals numbered less than N accepted by the acceptors in S.


<!--
### first attempt: single Acceptor

one Acceptor, choose first one that arrives, and tell Proposers about the outcome

problem: single-point failure

### second attempt: multiple Acceptors

multiple Acceptors, each accepts the first one, choose the majority of proposed value, and tell Proposers about the outcome

problem: network partition / delay: different Acceptors could chose different values
 -->
