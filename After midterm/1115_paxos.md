# PAXOS algorithm

## rules

- Proposer send prepare message with propose numbers; Acceptor either no accepted value, or send accepted value, or nack (not accepted, send larger proposal number) (reject: could do nothing, sliently drop message)
- Proposer send accept to Acceptors with proposal number and value (quorum of highest numbered proposal number / its own value) (Acceptor not necessary to send back accepted message, since Proposer can check by sending another prepare message)
- Learner chose a quorum accepted proposal number from Acceptors (send prepare message)

## problem

lots of overhead => should limit the amount of consensus

if a quorum of Acceptors are down: no available chosen value will be ever determined

if P0 (n) => A1, A2; Acceptors reply with okay; P1 (n+1) => A1, A2; P0 (Accept n) => A1, A2; Acceptors reject & P0 retry with (n+1); P0 (n+2) => A1, A2; Acceptors reply with okay; P1 (Accept n+1) => A1, A2; Acceptors reject ... (dual proposers never reach conclusion)

solve (ameliorate): randomized backoff? if rejected by n times: enter mode that wait for randomized time before prepare another message

=> if one node has multiple roles?

## application

database of bank accounts:

if PAXOS on the entire database (cannot change the database afterwards, once the database value is chosen)

PAXOS could be a part of state machine (log)
- log of operations (if an operation executed, it cannot be reversed); could restore database if there is a log of operations
- PAXOS: need to agree on each operations (what operation could be performed for each slot)

=> multi-PAXOS (propose message: proposal number, value, slot number)

=> problem: slow algo

# Multi-PAXOS for log

if only one Proposer proposes (one Leader Proposer, if fail other nodes take over the position), then no need to send prepare message
- choose leader: Bully algorithm (all nodes send messages with its id, and choose the lowest number, if no consensus do that sending messages again); still need to send prepare message to Acceptors when changing leader
- leader when Accept: send with a number n so that [0, n) slots are all complete, where [n, ~) might have some node that doesnt have chosen value yet

role: Client: to start new operations (replica)


<!-- RAFT strategy-->
<!-- Project: if doing read: fine if no quorum -->
