# Quanta

Quanta is meant to be an alternative to a satoshi blockchain that enables:

* Instant-ish transactions
* Mining pools not required
* No wasted work


## Some Code

https://github.com/XertroV/quanta-test/blob/master/quanta.py


## Deregulating the target

By deregulating the target and calculating the reward as a function of the word done we can enable very quick spam-resistant updates.

This also decentralises the mining process.


## The DAG

### Nodes

Each node contains a `parent_hash` and an optional `uncle_hash`.

### Ordering

Recursive ordering:

```
def order_from(early_node, late_node, carry=None):
    carry = [] if not carry else carry
    if early_node == late_node:
        return [late_node]
    if late_node.parent_hash == 0:
        raise Exception('Root block encountered unexpectedly while ordering graph')
    main_path = exclude_from(Graph.order_from(early_node, late_node.parent), carry)
    aux_path = exclude_from(Graph.order_from(early_node, late_node.uncle), carry + main_path) if late_node.uncle is not None else []
    return main_path + aux_path + [late_node]
```
