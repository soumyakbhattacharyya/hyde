---
title: Consistent Hashing
date: 2019-03-29 00:00:00 Z
---

#### Basic Concept

- consistent hashing is a way in which keys and nodes are mapped to same id space; through the use of similar hash function
- the algorithm ensures that moving clockwise, a key is being mapped to nearest node
- the key benefit of the mechanism is this way, if the hash function creates truly random outcome, keys are uniformly distributed, across the nodes
- another key benefit of consistent hashing over hash table is unlike hash table, **the impact of addition or removal of a node is quite small**. once, added or removed, the algorithm routes keys to newly added node if it is faster reachable than more distant node or keys are directed to next reachable node, should a node in between gets removed
- this specific behavior has potential to create cluster of keys on a specific node
- the solution to this problem is creating virtual id for _A_ node, so that the node is found in multiple position and keys get routed to the nodes 

##### References

1. https://www.youtube.com/watch?v=zaRkONvyGr8







