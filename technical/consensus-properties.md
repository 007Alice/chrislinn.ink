# Consensus Properties

<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

## ACID

数据库管理系统（DBMS）在写入或更新资料的过程中，为保证事务（transaction）是正确可靠的

+ atomicity 原子性
+ consistency 一致性
    * 数据库的完整性没有被破坏
    * from one valid state to another
    * 写入的资料必须完全符合所有的预设约束、触发器、级联回滚等
    * 防止数据库被污染
+ isolation 隔离性, 独立性
    * 允许多个并发事务同时对其数据进行读写和修改的能力
    * 事务隔离分为不同级别
        - 未提交读（Read uncommitted）
        - 提交读（read committed）
        - 可重复读（repeatable read）
        - 串行化（Serializable）
+ durability 持久性
    * 事务处理结束后，数据的修改是永久的，系统故障也不会丢失


__TODO: 2PC__

__TODO: Atomic Comit__

## CAP

+ Consistency 一致性
+ Availability 可用性
+ Partition tolerance 分区容错性

## BASE

对CAP中一致性和可用性权衡的结果

+ Basically Availability 基本可用
    * 响应时间稍微增加
    * 电商功能降级
+ Soft state 软状态
    * 弱状态：也称为软状态，和硬状态相对，是指允许系统中的数据存在中间状态，并认为该中间状态的存在不会影响系统的整体可用性，即允许系统在不同节点的数据副本之间进行数据同步的过程存在延时。
+ Eventually consistency 最终一致性
    * 放弃 Strong consistency 强一致性

## SMR

- [State machine replication](https://dl.acm.org/citation.cfm?id=98167) is a general paradigm for implementing fault-tolerant services by replicating servers and coordinating client interactions with server replicas.
    + Place copies of the State Machine on multiple, independent servers.
    + Receive client requests, interpreted as Inputs to the State Machine.
    + Choose an ordering for the Inputs.
    + Execute Inputs in the chosen order on each server.
    + Respond to clients with the Output from the State Machine.
    + Monitor replicas for differences in State or Output.
- Properties
    + safety
        * (Informal) When decisions are made by any two correct nodes, they decide on non-conflicting transactions.
        * 诚实的节点对合法交易将达成统一的 (consistent) 意见
    + liveness
        * (Informal) \\(T\\)-Liveness: each honest node terminates and outputs a value at the end of \\(T\\).
            - The value of \\(T\\) depends on the research problems and protocols.
        * 一笔合法交易在合理时间长度内会被确认
    + total ordering
        * taken from [BEAT: Asynchronous BFT made practical (DRZ18)](https://www.csee.umbc.edu/~hbzhang/files/beat.pdf)
        > If a correct replica has delivered \\(m_1, m_2, \dots ,m_s\\) and another correct replica has delivered \\(m'\_1, m'\_2, \dots , m'\_{s'}\\), then \\(m_i = m'\_i\\) for \\(1 \leq i \leq min(s, s')\\).

## CFT

Crash Fault Torelance

__TODO:__

## BFT

__TODO:__

Byzantine Fault Torelance

## Consensus
* https://disco.ethz.ch/courses/podc_allstars/lecture/chapter16.pdf
> There are \\(n\\) nodes, of which at most \\(f\\) might be faulty (or Byzantine). Each node \\(P_i\\) starts with an input value (say \\(u_i\\)). The nodes must decide one of those values (say \\(v_i\\)), satisfying three properties: Agreement, Validity and Termination.
    + Classic
        * Agreement
            - All correct processes must agree on the same value.
            - For any two honest players \\(P_i\\) and \\(P_j\\), \\(v_i = v_j\\).
        * Validity
            - The decision value must be the input value of a node.
            - \\(\forall i: u_i \in \langle v_i \rangle\\).
        * Termination
            - (Informal) All correct nodes terminate in finite time.
            - All the honest players terminate with probability 1.
    + Emerging
        * Linearity
            - First proposed in [HotStuff](https://arxiv.org/abs/1803.05069).
                - PODC'19  分布式计算理论顶会
                - Facebook's LibraBFT
            - Any correct leader sends only \\(O(n)\\) messages to drive a protocol to consensus.
        * Responsiveness
            - First defined in [Hybrid Consensus](https://eprint.iacr.org/2016/917.pdf).
                - DISC'17 分布式计算理论顶会
            - (Informal) The transaction confirmation time depends only on the network’s actual delay, but not on any a-prior known upper-bound.
            - A consensus protocol is responsive if nodes can reach the consensus in time depending only on the network’s actual \\(\delta\\) (message delays), not on the loose upper bound \\(\Delta\\) (known upper bound on message delays).

## Blockchain
- Common prefix (Consistency)
    + \\(k\\)-common-preifx
        * First proposed in [The bitcoin backbone protocol: Analysis and applications (GKL15)](https://eprint.iacr.org/2014/765.pdf).
            - For any pair of honest players \\(P_1\\), \\(P_2\\) adopting the chains \\(C_1\\), \\(C_2\\) at rounds \\(r_1 \leq r_2\\), it holds that \\(\mathcal{C}_{1}^{\lceil k} \preceq \mathcal{C}_2\\).
    + \\(T\\)-consistency
        * [Analysis of the blockchain protocol in asynchronous networks (PSS17)](https://eprint.iacr.org/2016/454.pdf) refines Common Prefix to \\(T\\)-Consistency in order to provide a black-box reduction.
* Chain growth
    + \\((\tau, s)\\)-Chain growth
        * For any honest party \\(P\\) with chain \\(C\\), it holds that for any  \\(s\\) rounds there are at least \\(\tau \cdot s\\) blocks added to the chain of \\(P\\).
- Chain quality (Fairness)
    + \\((\mu, k)\\)-Chain quality (Fairness)
        * The proportion of blocks in any \\(k\\)-long subsequence produced by the adversary is less than \\(\mu \cdot k\\), where \\(\mu\\) is the portion of mining power controlled by the adversary.