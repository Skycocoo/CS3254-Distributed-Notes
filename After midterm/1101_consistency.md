# consistency

## linearizability

### semantics

- single-client, single-replica semantics (as if one frontend, one backend)
- read gives last write (chronically)

### pro-con

con: incoming requests should be totally ordered (need to sync all replicas; mutex for entire data store)

### concurrent situation

backend: discretion: choose effective point for duration of operation

linearizable: some combinations of effectivity point demonstrates read gives last write results => possibility

### problem

- if high latency: not that efficient
- if two frontends writing at the same time: already a race condition

---

## sequential consistency

compared to linearizability: if drop read gives the last write semantics (data store could delay the execution of operations (while still keeping program-order execution))

could recorder the operations, as long as the program order of operations are maintained

### criteria

- interleaves sequence of operations meets the specification of a single correct copy of the objects
- order of interleaving operations is consistent with program order

### pro-con

pro: faster (only need to update one replica); preserve failure tolerance (not necessarily load balancing)
con: different processes have potentially different value stored

### scenario

chain: writes -> (N1) -> (N2) -> (N3) <- reads

reads wont be guaranteed to get the last write (could buffer writes; but clients can get sequentially same read)

---

## causal consistency

if drop single-client, single-replica semantics: doesnt need to sync when two operations do not have causal order to each other

order things in causal manner: compare timestamp; if two writes doesnt have causality, then those two writes are concurrent


---

# terminology

## replica

in contrast to one node: load balancing & failure recovery

### synchronization

1. passive replication / star pattern: frontend picks one replica, which updates other replicas (that replica reply to frontend after all updates are finished)

2. active replication: frontend talks to all replicas

---

## memory barrier

memory is not linearizable => put a memory barrier; "operations issued prior to the barrier are guaranteed to be performed before operations issued after the barrier"
