# consistency

e.g thread 1: A = 1; print(B);
    thread 2: B = 1; print(A);

linearizable: 1, 1; 0, 1;

sequentially consistent: 1, 1; 0, 1;

## total store order (memory model in Intel)

memory -> L3 cache -> each processors' (L2 cache -> L1 cache -> processor)

write consults write buffer first, then all writes flushed to memory; TSO (total store order)

total store order: 1, 1; 0, 1; 0, 0 (if all writes are not flushed before print)

=> total store order is weaker than linearizability


# broadcast replica

broadcast a group of replica: all or none nodes get messages

view: status of current node in group

## view change

if node leaves the group (failure ... ) / new nodes join the group / ...

if view change overlap with broadcast: inconsistent => need to change the ordering of message delivery

## status

two nodes: buffer of messages that are received but not delivered (if receive messages with wrong order: store that message until the previous messages received)

received: receive messages

delivered: deliver messages (with correct order)


## types

### unicast (send messages to one node)

unreliable unicast: e.g UDP

reliable unicast: e.g TCP; at-least-once guarantee

### broadcast (built on reliable unicast)

#### goal

if a node broadcast a message, then all correct nodes will receive it (and otherwise as well)

#### reliable broadcast

if sender fails, and only part of the nodes get the message: those nodes that get the message send that message to all other nodes

#### partial ordering broadcast

FIFO broadcast: vector to store the number of messages sent to other nodes, and buffers the rest of the messages that are not delivered (could order messages from the same node => partial order); sequential ordering of messages on single node basis

causal (happened-before) broadcast: vector clock store the timestamp (partial order for concurrent messages)
    if sort the concurrent messages by the IP address of the sender: could achieve total order

#### total ordering broadcast

if multiple nodes send messages at the same time; need messages to be delivered in the same order

make sure all receivers agree on the order of delivery for those messages

isis broadcast:

- each node has an internal sequence number
- each time a node receives a message / send a message, the node propose a sequence number to sender, and the sender choose the highest sequence number for that message, then send back the official sequence number to all nodes
- a node should propose a sequence number that is higher than previous known numbers
- tie: two nodes broadcast at the same time: use lowest numbered id that proposes this number to compare which one is smaller (sender 0, propose 1: 1.0; sender 1, propose 1: 1.1)
