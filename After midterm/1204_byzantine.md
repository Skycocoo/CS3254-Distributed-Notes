# byzantine failure

## rule

- all loyal Lieutenants follow the same order (could not do like one lieutenant attack the other retreat) even if commander is traitor
- if the commander is loyal, every loyal lieutenant obeys his command
- an disloyal lieutenant could lie

## implementation?

voting: if tie then retreat: works when only 1 traitor (but doesnt work if commander and one lieutenant are traitors)

what if there are two rounds of voting:

## Oral Messaging (OM) algorithm

all nodes: each node send next node what it got from commander (lieutenant becomes a mini-commander to other nodes)

each node stores a tree of messages (with providence of the message (where does the message come from))

- OM(n): n: number of potential traitors
- OM(0): accept the command
- OM(1): vote, deal with it (in terms of OM(0))

```
4 nodes, 1 traitor

Lieutenant 1:
    Commander(0, 0) => conclusion: value is 0
        --- (node 1) 0 (value), 01 (from 0 to 1), 0 (conclusion 0)
        --- (node 2) 0 (value), 02 (from 0 to 2), 0 (conclusion 0)
        --- (node 3) 0 (value), 03 (from 0 to 3), 0 (conclusion 0)

7 nodes, 2 traitors (need to go to layer 2)
    if the same messages are changed by some of the nodes, then those nodes might be traitors
    message sent from other node should check the other tree

```

works for n > 3m+1
