# Paxos

Paxos: is used to build a finite state machine

=> achieve consensus on one state, and continue to the next state => multipaxos (log)

# Consistency

- eventual (multiple datastore; at some point in the future the inconsistent conflict would be resolved: depend on the usage of the datastore)
- causal (single datastore, causality -> concurrency)
- sequential (single datastore, keep program order) (shouldnt change the point of read?)
- linearizable (single datastore, read last write)

# Broadcast ordering

what order to broadcast from one side of nodes to other side of the nodes

- total ordering: all nodes agree on the ordering of messages
- partial ordering: some of the messages are ordered in the same way; causal, FIFO

# Bitcoin
