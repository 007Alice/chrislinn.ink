# Erlay

> Bandwidth-Efficient Transaction Relay in Bitcoin

a node learns about a new transaction
it announces the txid of that transaction to all of its peers (except any peers that announced it previously)

not efficient
already learned about from a different peer a few moments before
44% bandwidth waste

attempts to reduce that redundancy
separates relaying into two phases,
fanout
    only directly announce new transactions to a maximum of eight peers
reconciliation
    a node will periodically request from each of its outbound peers a sketch of short transaction identifiers (short txids) for all the new transactions that peer would normally announce to the node. 

    Sketches are created using the libminisketch library, which implements a highly bandwidth efficient set reconciliation technique based on error correction codes. 

    Upon receipt of a requested sketch, the node itself also generates a sketch of all the new transactions that it would normally announced to its peer. The node combines its sketch with the peer’s sketch to produce a list of any differences between the sketches. That diff contains the short txids of any transactions that are in one set of new transactions but not the other. The node can then request any transactions it doesn’t have from that peer and proceed to querying its next peer for that peer’s sketch. This is repeated for a new peer once each second, allowing the node to quickly receive any transactions it didn’t receive via a fanout announcement. 


remaining parts
described and analyzed
the optimal parameters to use for the sketches and a series of fallback steps to take if a particular attempt at set reconciliation fails.


a simulated network of 60,000 nodes (similar in number and use to the current Bitcoin network)
a live set of 100 nodes spread internationally across several datacenters

Erlay reduces the amount of bandwidth used to announce the existence of new transactions by 84%. 

take about 80% (2.6 seconds) longer for transactions to propagate to all of the nodes in the network.
    Bitcoin transactions still only be confirmed once every ten minutes on average
    a three second slowdown seems like a reasonable price to pay for a major reduction in bandwidth.

Having established that the protocol is a worthwhile efficiency improvement, the paper considers the most important of its secondary aspects: its effect on privacy. Currently, each Bitcoin Core node slightly delays the relaying of transactions to its peers; this makes it more difficult for spy nodes to use timing correlations to guess that the first peer they receive a transaction from is the peer that created the transaction. Multiple simulations were run testing the effectiveness of the current relay protocol against the effectiveness of Erlay for various amounts of spy nodes among network peers, from 5% spy nodes to 60% spy nodes. In cases where the spy nodes were public nodes accepting incoming connections, Erlay always performed as well or better than the current protocol. In cases where the spy nodes were private nodes making connections to honest nodes, Erlay sometimes performed better and sometimes performed worse—but never more than 10% worse (and this worst case is an unlikely situation). 

Erlay is compatible with the proposed BIP156 Dandelion protocol for further improving the network’s resistance to spy nodes.

Considering future possible changes to Bitcoin relay policy, the paper notes that an increase in the number of outbound peers from 8 to 32 would only increase the bandwidth used by a node to announce the existence of new transactions by 32% with Erlay compared to 300% using the current protocol. As described in the paragraph above about Erlay’s two phases, new transactions would still only be fanned out via direct announcements to 8 peers, but nodes would perform set reconciliation with all 32 peers. A four-fold improvement in relay connectivity improves the chance that time-sensitive transactions for contract protocols like LN will make it to miners quickly.


the CPU intensive part of set reconciliation has been benchmarked to take less than a millisecond under conditions that are worse than expected in practice.
