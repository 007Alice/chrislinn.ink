# Learn You a Distributed Computing for Great Good!

<p style="font-size:100%" align="right";>－－ <a href="https://github.com/SebastianElvis/">Runchao Han</a> & <a href="https://chrislinn.ink/">Haoyu Lin</a></p>

> This work is ditributed under [WTFPL](http://www.wtfpl.net/).

<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

## ACID

数据库管理系统（DBMS）在写入或更新资料的过程中，为保证事务（transaction）是正确可靠的

+ atomicity 原子性
+ consistency 一致性
    * 数据库的完整性没有被破坏
    * from one valid state to another 从一个有效状态到另一个有效状态
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

传统的SQL数据库的事务通常都是支持ACID的强事务机制(强一致性的体现). NoSQL 通常注重性能和扩展性.

+ NoSQL 仅提供对行级别的原子性保证
    * 同时对同一个Key下的数据进行的两个操作，在实际执行的时候是会串行的执行，保证了每一个Key-Value对不会被破坏

BASE: 对CAP中一致性和可用性权衡的结果

+ Basically Availability 基本可用
    * 响应时间稍微增加
    * 电商功能降级
+ Soft state 软状态
    * 弱状态：也称为软状态，和硬状态相对，是指允许系统中的数据存在中间状态，并认为该中间状态的存在不会影响系统的整体可用性，即允许系统在不同节点的数据副本之间进行数据同步的过程存在延时。
+ Eventually consistency 最终一致性
    * 经过一段时间的同步后，最终能够达到一个一致的状态
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

## Permissioned vs Permissionless

+ Consortium Blockchain
    * Permissioned
    + 半信任的威胁模型
        * 被动性的错误（比如停止工作）
+ Public Blockchain
    * Permissionless
    + 拜占庭威胁模型
        * 主动攻击性的错误
        * 说谎，伪造消息，合谋攻击，或者展开具有选择性的 DoS 攻击

## BFT

Byzantine Fault Torelance

+ Lamport (图灵奖)
    + Paxos
        + Google
            * Bigtable: Chubby lock service
            * [Chubby vs Zookeeper](https://draveness.me/zookeeper-chubby)
        * [Tencent/phxpaxos](https://github.com/Tencent/phxpaxos)
        + 经 Stanford 简化 --> Raft
            * k8s 中 etcd 状态同步有使用

__Paxos/Raft__

+ 在比较温和的威胁模型里工作的 (__Crash Fault Torelance__, CFT, fail-stop)
+ 只对异步网络里的非拜占庭错误具有鲁棒性
+ 在非拜占庭威胁模型里，出错的节点只能犯被动性的错误（比如停止工作）而不能展开具有主动进攻性的攻击
+ 具有 n 个节点的系统最多能容忍的非拜占庭错误节点数是（n-1）/2

__TODO: 1/3__


拜占庭协议所使用的通讯网络是一个同步网络????


https://www.theblockbeats.com/news/6208

Network Assumption:

+ Synchrony
    * Algorand 怼了 Ouroboros (ADA, IOHK)
+ Partial Synchrony
+ Weak Synchrony
+ Asynchrony
    + pBFT
        * Barbara Liskov, 图灵奖
        * Paxos 协议的拜占庭版本
            - 在 Paxos 协议中加入了一个验证步骤来防止拜占庭错误
        * Steps
            - pre-prepare 序号分配
            - prepare 相互交互
            - commit 序号确认
    + Tendermint BFT
    + 广播
        * [Asynchronous Byzantine agreement protocols](https://dl.acm.org/citation.cfm?id=806743) by Bracha
            - 存在的问题是 scalability
                + 对于 transactions of size B
                    * bracha 的通信复杂度是 \\(O(n^2*B)\\)
                    * HoneyBadgerBFT 的是 \\(O(n*B)\\)
                    * [BEAT](https://dl.acm.org/citation.cfm?id=3243812) 的是 \\(O(B)\\)
                        - CCS'18 (ACM Conference on Computer and Communications Security 安全顶会)

__TODO:__

+ https://mp.weixin.qq.com/s/87ZAz_jVL0ja7OCMIEd4Uw



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
                - PODC'19  (ACM Symposium on Principles of Distributed Computing 分布式计算理论顶会)
                - Facebook's LibraBFT
            - Any correct leader sends only \\(O(n)\\) messages to drive a protocol to consensus.
        * Responsiveness
            - First defined in [Hybrid Consensus](https://eprint.iacr.org/2016/917.pdf).
                - DISC'17 (International Symposium on Distributed Computing 分布式计算理论顶会)
            - (Informal) The transaction confirmation time depends only on the network’s actual delay, but not on any a-prior known upper-bound.
            - A consensus protocol is responsive if nodes can reach the consensus in time depending only on the network’s actual \\(\delta\\) (message delays), not on the loose upper bound \\(\Delta\\) (known upper bound on message delays).

## Blockchain

__TODO: Sybil__

__TODO: PKQ__

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