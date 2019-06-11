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


remaining parts
described and analyzed
the optimal parameters to use for the sketches and a series of fallback steps to take if a particular attempt at set reconciliation fails.

a simulated network of 60,000 nodes (similar in number and use to the current Bitcoin network)

a live set of 100 nodes spread internationally across several datacenters


reduces the amount of bandwidth used to announce the existence of new transactions by . 


seems like a reasonable price to pay for a major reduction in bandwidth.




take about 80% (2.6 seconds) longer for transactions to propagate to all of the nodes in the network.
    Bitcoin transactions still only be confirmed once every ten minutes on average
    a three second slowdown

---

读了 blockstream 的关于节省转发交易带宽的新论文 Bandwidth-Efficient Transaction Relay in Bitcoin

总结

bitcoin 目前的方式是，节点 a 收到一个交易后会广播 txid 给（之前没把这 txid 发给 a 的）peers。
但这种方式效率不高，因为往往peers已经在不久之前从别的节点处收到了这个 txid。
论文中显示，这种方式造成了 40% 带宽的浪费。

erlay 协议通过把 转发 分成 fanout & reconciliation 两步。来减少这种冗余通信造成的浪费。

fanout:
只会将新交易通知给最多 8 个peers。

reconciliation:
节点会定期向外拨 peers 请求新交易的 short txids 梗概(sketch)。Sketches 使用 libminisketch 库创建，使用错误纠正码有效节省带宽。从一个 peer 收到梗概后，节点自身也会生成 梗概，进行对比取差集。然后向该 peer 请求自身没有的 tx。接着再向下一个节点重复上述步骤。每个新节点每秒一次。

论文中 网络带宽的分析采用了2种网络环境进行测试:
1. 有 60,000 节点的模拟网络，这与目前 bitcoin 网络在个数和使用规模上相近   
2. 分布在不同国家数据中心的100 节点的真实网络

数据显示，Erlay 减少了 84% 带宽使用。虽然需要多花  80% (2.6 seconds) 的时长来把交易广播到全网。但比起平均10min 的区块确认时间，这个开销很值得。




论文中还对如何优化 sketches、reconciliation 失败时如何 fallback 进行了分析讨论。


----


Having established that the protocol is a worthwhile efficiency improvement, the paper considers the most important of its secondary aspects: its effect on privacy. Currently, each Bitcoin Core node slightly delays the relaying of transactions to its peers; this makes it more difficult for spy nodes to use timing correlations to guess that the first peer they receive a transaction from is the peer that created the transaction. Multiple simulations were run testing the effectiveness of the current relay protocol against the effectiveness of Erlay for various amounts of spy nodes among network peers, from 5% spy nodes to 60% spy nodes. In cases where the spy nodes were public nodes accepting incoming connections, Erlay always performed as well or better than the current protocol. In cases where the spy nodes were private nodes making connections to honest nodes, Erlay sometimes performed better and sometimes performed worse—but never more than 10% worse (and this worst case is an unlikely situation). 

Erlay is compatible with the proposed BIP156 Dandelion protocol for further improving the network’s resistance to spy nodes.

Considering future possible changes to Bitcoin relay policy, the paper notes that an increase in the number of outbound peers from 8 to 32 would only increase the bandwidth used by a node to announce the existence of new transactions by 32% with Erlay compared to 300% using the current protocol. As described in the paragraph above about Erlay’s two phases, new transactions would still only be fanned out via direct announcements to 8 peers, but nodes would perform set reconciliation with all 32 peers. A four-fold improvement in relay connectivity improves the chance that time-sensitive transactions for contract protocols like LN will make it to miners quickly.


the CPU intensive part of set reconciliation has been benchmarked to take less than a millisecond under conditions that are worse than expected in practice.
