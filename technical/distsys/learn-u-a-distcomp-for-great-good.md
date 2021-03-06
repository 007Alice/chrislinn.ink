# Learn You a Distributed Computing for Great Good!

Revision v0.06

这份笔记大部分是 [我]((https://chrislinn.ink/)) 从 [韩神](https://github.com/SebastianElvis) 和 [邱巨](https://priewienv.me/) 处抄袭、总结或与他们请教而来；如果有任何错误，那肯定是我搞错了，与大腿们无关。

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


## CAP

+ Consistency 一致性
    * 一致性分类
        * https://en.wikipedia.org/wiki/Consistency_model
            - Strict consistency
            - Sequential consistency
            - Causal consistency
            - ...
        - [Serialisability v.s. Linearisability](http://www.bailis.org/blog/linearizability-versus-serializability/)
            + [Serialisability](https://en.wikipedia.org/wiki/Serializability)
                * multi-operation, multi-object, arbitrary total order
                * a guarantee about transactions, or groups of one or more operations over one or more objects.
                * __ACID 中的 I__
            + Linearisability
                * single-operation, single-object, real-time order
                * a guarantee about single operations on single objects
                * Linearizability for read and write 是 “atomic consistency”, 是 __CAP 中的 C__
+ Availability 可用性
+ Partition tolerance 分区容错性

## Protocols

+ Concurrency Control
    * Optimistic Concurrency Control (OCC)
        - the database writes (but doesn't commits) the transaction when receiving it
        - check validity/conflict --> commit/roll-back
    * Pessimistic Concurrency Control (PCC)
        - lock the resource before being written to the log
        - if locking successfully --> commit & unlock
    * Timestamp Ordering (T/O)
    * Coordinator-based Concurrency Control
+ Atomic Commit
    * Two Phase Commit (2PC)
        - Steps
            + Propose
            + Commit
        + 不能处理fail-stop形式的节点failure 
        + 同步阻塞
    * 3PC
        - Steps
            + Propose
            - PreCommit
            - Commit
        * 加入超时
        * 可以有效的处理fail-stop的模式, 但不能处理  一致性问题 (比如 fail-recover)
    * ...


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
        * (Informal) ![](http://latex.codecogs.com/gif.latex?T)-Liveness: each honest node terminates and outputs a value at the end of ![](http://latex.codecogs.com/gif.latex?T).
            - The value of ![](http://latex.codecogs.com/gif.latex?T) depends on the research problems and protocols.
        * 一笔合法交易在合理时间长度内会被确认
    + total ordering
        * taken from [BEAT: Asynchronous BFT made practical (DRZ18)](https://www.csee.umbc.edu/~hbzhang/files/beat.pdf)
        > If a correct replica has delivered ![](http://latex.codecogs.com/gif.latex?m_1, m_2, \dots ,m_s) and another correct replica has delivered ![](http://latex.codecogs.com/gif.latex?m'_1, m'_2, \dots , m'_{s'}), then ![](http://latex.codecogs.com/gif.latex?m_i = m'_i) for ![](http://latex.codecogs.com/gif.latex?1 \leq i \leq min(s, s')).
+ [SMR vs NoSQL vs Blockchain](https://rink1969.github.io/Blockchain-consistency_model)
    * replicated data structure一致性这么弱，是因为 replicated data structure选择了高可靠性，一致性自然要弱一些。
    * 区块链跟一般意义上的replicated data structure还不太一样，区块链是通过atomic broadcast来同步(写操作)，其一致性在整个大类里面是最强的。
    * 分布式数据库能达到线性一致性，是因为分布式数据库的读写操作都是由主节点排序的，而区块链的写操作是无序的，并且读操作跟写操作是完全分离的。从CAP的角度来说就是分布式数据库舍弃了部分可用性。分布式数据库主节点不可用的时候，整个系统是不可用的。但是区块链在切换出块节点的过程中是一直保持可用性的。这主要是靠节点间会相互转发交易，当然这也就造成结论的第三点中的情况，上链的顺序跟用户最初发出交易的顺序就不一致了。

## Permissioned vs Permissionless

+ Consortium Blockchain
    * Permissioned
    + 半信任的威胁模型
        * 被动性的错误（比如 fail-stop 停止工作）
+ Public Blockchain
    * Permissionless
    + [拜占庭威胁模型](#BFT)
        * 主动攻击性的错误
        * 说谎，伪造消息，合谋攻击，或者展开具有选择性的 DoS 攻击


## Paxos/Raft
+ Paxos
    + Lamport (图灵奖)
    - Google
        + Bigtable: Chubby lock service
        + [Chubby vs Zookeeper](https://draveness.me/zookeeper-chubby)
    + 腾讯的 [phxpaxos](https://github.com/Tencent/phxpaxos)
    - 经 Stanford 简化 --> Raft
        + k8s 中 etcd 状态同步有使用
+ 在比较温和的威胁模型里工作的 (__Crash Fault Torelance__, CFT, fail-stop)
    + 只对非拜占庭错误具有鲁棒性

## BFT

Byzantine Fault Torelance

Requires >= 2f+1 honest nodes (<= f Byzantine nodes) in order to achieve safety ~~and liveness~~, where there are totally n = 3f + 1 nodes

__TODO:__

+ https://medium.com/thundercore/consensus-series-preliminaries-a3bab33ae09
    * 1/2 in sync, 1/3 in async?
+ PBFT paper section 3

## Network Assumption

+ Synchrony
    * a known finite time bound
    * Algorand 怼了 Ouroboros (ADA, IOHK) 说它是 sync
+ Partial Synchrony
    * an unknown finite time bound, or a known finite time bound plus a known clock drift (Global Stabilisation Time, GST)
        + [HotStuff](https://arxiv.org/abs/1803.05069).
            * PODC'19  (ACM Symposium on Principles of Distributed Computing 分布式计算理论顶会)
            * Facebook's LibraBFT
+ Weak Synchrony
    * 长时间但是有上限的 async 之后 strongly sync 一段足够的时间 (to ensure safety)
    * 也就是 每段 有界时间段里面有一部分 strong sync
        + Algorand
+ Asynchrony
    * delivered eventually but without a finite time bound
    + PBFT
        * async
            - 在这之前的之前的 BFT 协议中拜占庭协议所使用的通讯网络是一个同步网络
            - can achieve safety in asynchronous networks, but not liveness
            - PBFT requires synchrony to achieve liveness
        * OSDI'99 (Symposium on Operating Systems Design and Implementation 操作系统顶会)
        * Barbara Liskov
            - 图灵奖
        * Paxos 协议的拜占庭版本
            - 在 Paxos 协议中加入了一个验证步骤来防止拜占庭错误
        * Steps
            - pre-prepare 序号分配
            - prepare 相互交互
            - commit 序号确认
        * pre-prepare & prepare 保证同一个 view 中的请求有序
        * Prepare & Commit 保证不同 view 中的请求被有序地执行
        * 怀疑primary节点出错时, 进行 View change
            - 防止backup节点无限地等待请求的执行。
            - 确保即使当前的primary节点出错，整个系统也能继续运行。
        * [PBFT 中 assume 了 weak synchrony](https://www.usenix.org/legacy/events/osdi99/full_papers/castro/castro_html/node3.html#SECTION00030000000000000000):
        > it must rely on synchrony to provide liveness
        + HoneyBadgerBFT 提出了改进，说 PBFT 的 liveness assumption 不 practical
        > guarantees liveness without making any timing assumptions
            + HoneyBadgerBFT 还是太慢
    + Tendermint BFT
        * elastico: "tendermint is essentially a variant of PBFT"
            - http://tendermint.com/posts/tendermint-vs-pbft
    + 广播
        * [Asynchronous Byzantine agreement protocols](https://dl.acm.org/citation.cfm?id=806743) by Bracha
            - 存在的问题是 scalability
                + 对于 transactions of size B
                    * bracha 的通信复杂度是 ![](http://latex.codecogs.com/gif.latex?O(n^2*B))
                    * HoneyBadgerBFT 的是 ![](http://latex.codecogs.com/gif.latex?O(n*B))
                        + HoneyBadgerBFT 用的是 erasure-coded broadcast
                            * small client-load 时可以考虑使用 bracha broadcast 来代替，throughput 更大，lantency 更低
                    * [BEAT](https://dl.acm.org/citation.cfm?id=3243812) 的是 ![](http://latex.codecogs.com/gif.latex?O(B))
                        - CCS'18 (ACM Conference on Computer and Communications Security 安全顶会)

### Sybil Attack 女巫攻击

BFT 要求一大部分(通常是 2/3 以上)的参与者都不是恶意的, 在非许可环境下，一个攻击者可以使用被称作女巫攻击 (Sybil Attack) 的手段模拟出大量的参与者，从而轻易地控制住参与者中的绝大多数。

PoW 中出块其实就是 block producer 的 election, 通过 PoW 使 block producer 身份伪造有成本，PoW 可以抗 Sybil Attack.

PoW 其实只是 membership 的门槛，  nakamoto consensus 除了 PoW 其实还和 longest chain 紧密相关。（Assume honest 节点占大多数，于是他们出块更快，更容易占据更长链。）



## Consensus

* [苏黎世理工 课程讲义](https://disco.ethz.ch/courses/podc_allstars/lecture/chapter16.pdf)
> There are ![](http://latex.codecogs.com/gif.latex?n) nodes, of which at most ![](http://latex.codecogs.com/gif.latex?f) might be faulty (or Byzantine). Each node ![](http://latex.codecogs.com/gif.latex?P_i) starts with an input value (say ![](http://latex.codecogs.com/gif.latex?u_i)). The nodes must decide one of those values (say ![](http://latex.codecogs.com/gif.latex?v_i)), satisfying three properties: Agreement, Validity and Termination.
    + 教科书上的
        * Agreement
            - All correct processes must agree on the same value.
            - For any two honest players ![](http://latex.codecogs.com/gif.latex?P_i) and ![](http://latex.codecogs.com/gif.latex?P_j), ![](http://latex.codecogs.com/gif.latex?v_i = v_j).
        * Validity
            - The decision value must be the input value of a node.
            - ![](http://latex.codecogs.com/gif.latex?\forall i: u_i \in \langle v_i \rangle).
        * Termination
            - (Informal) All correct nodes terminate in finite time.
            - All the honest players terminate with probability 1.
    + 这几年新提出的
        * Linearity
            - First proposed in [HotStuff](https://arxiv.org/abs/1803.05069).
            - Any correct leader sends only ![](http://latex.codecogs.com/gif.latex?O(n)) messages to drive a protocol to consensus.
        * Responsiveness
            - First defined in [Hybrid Consensus](https://eprint.iacr.org/2016/917.pdf).
                - DISC'17 (International Symposium on Distributed Computing 分布式计算理论顶会)
            - (Informal) The transaction confirmation time depends only on the network’s actual delay, but not on any a-prior known upper-bound.
            - A consensus protocol is responsive if nodes can reach the consensus in time depending only on the network’s actual ![](http://latex.codecogs.com/gif.latex?\delta) (message delays), not on the loose upper bound ![](http://latex.codecogs.com/gif.latex?\Delta) (known upper bound on message delays).

## Blockchain Properties

- Common prefix (Consistency)
    + ![](http://latex.codecogs.com/gif.latex?k)-common-preifx
        * First proposed in [The bitcoin backbone protocol: Analysis and applications (GKL15)](https://eprint.iacr.org/2014/765.pdf).
            - For any pair of honest players ![](http://latex.codecogs.com/gif.latex?P_1), ![](http://latex.codecogs.com/gif.latex?P_2) adopting the chains ![](http://latex.codecogs.com/gif.latex?C_1), ![](http://latex.codecogs.com/gif.latex?C_2) at rounds ![](http://latex.codecogs.com/gif.latex?r_1 \leq r_2), it holds that ![](http://latex.codecogs.com/gif.latex?\mathcal{C}_{1}^{\lceil k} \preceq \mathcal{C}_2).
    + ![](http://latex.codecogs.com/gif.latex?T)-consistency
        * [Analysis of the blockchain protocol in asynchronous networks (PSS17)](https://eprint.iacr.org/2016/454.pdf) refines Common Prefix to ![](http://latex.codecogs.com/gif.latex?T)-Consistency in order to provide a black-box reduction.
            - Eurocrypt'17 密码学顶会
            - Ouroboros 和 DFINITY 等项目的论文 __均以此模型和部分结论为基础进行安全性证明__
                + 适合区块链的一致性应该是要求诚实的参与者在不考虑潜在的一小部分的，![](http://latex.codecogs.com/gif.latex?T) 个在链末端的“未确认的”块的情况下，对当前的链达成一致
                    * 只需证明 ![](http://latex.codecogs.com/gif.latex?T)-consistency 不能保持的概率相对于 ![](http://latex.codecogs.com/gif.latex?T) 可以 __被忽略__
* Chain growth
    + ![](http://latex.codecogs.com/gif.latex?(\tau, s))-Chain growth
        * For any honest party ![](http://latex.codecogs.com/gif.latex?P) with chain ![](http://latex.codecogs.com/gif.latex?C), it holds that for any  ![](http://latex.codecogs.com/gif.latex?s) rounds there are at least ![](http://latex.codecogs.com/gif.latex?\tau \cdot s) blocks added to the chain of ![](http://latex.codecogs.com/gif.latex?P).
        * 以相对于 ![](http://latex.codecogs.com/gif.latex?T) __压倒性__ (overwhelming) 的概率，在任意时刻，诚实参与者的链在过去的 ![](http://latex.codecogs.com/gif.latex?T/g) 轮中，至少增长了 ![](http://latex.codecogs.com/gif.latex?T) 个消息。称 ![](http://latex.codecogs.com/gif.latex?g) 为该协议的 chain growth.
- Chain quality (Fairness)
    + ![](http://latex.codecogs.com/gif.latex?(\mu, k))-Chain quality (Fairness)
        * The proportion of blocks in any ![](http://latex.codecogs.com/gif.latex?k)-long subsequence produced by the adversary is less than ![](http://latex.codecogs.com/gif.latex?\mu \cdot k), where ![](http://latex.codecogs.com/gif.latex?\mu) is the portion of mining power controlled by the adversary.
        * 以相对于 ![](http://latex.codecogs.com/gif.latex?T) 压倒性的概率，任意诚实参与者的链中的连续  ![](http://latex.codecogs.com/gif.latex?T) 个消息中，诚实参与者提供的消息所占比例至少为 ![](http://latex.codecogs.com/gif.latex?\mu)，称 ![](http://latex.codecogs.com/gif.latex?\mu) 为该协议的 chain quality 。

## Propagation

优化网络传播: 网络传播影响孤快率/分叉, 影响一致性 (reference: [[PSS17]](https://eprint.iacr.org/2016/454.pdf), [[DW13]](https://www.gsd.inesc-id.pt/~ler/docencia/rcs1314/papers/P2P2013_041.pdf))

> 在一个完全异步的环境中，对抗方可以随意延迟消息。对抗方只需控制一小部分算力就可以很容易地使来自诚实方的消息延迟足够长的时间，长到对抗方确保能够拿出比所有诚实方更长的链（这条链可以包含任何对抗方想要的记录），从而随时可以使得 __诚实方切换到对抗方的链__。


一些优化网络传播值得一看的论文/文章:

+ [Secure High-Rate Transaction Processing in Bitcoin](https://eprint.iacr.org/2013/881.pdf) (GHOST)
+ [Prism: Deconstructing the Blockchain to Approach Physical Limits](https://arxiv.org/pdf/1810.08092.pdf)
+ [Bandwidth-Efficient Transaction Relay for Bitcoin](https://arxiv.org/abs/1905.10518) (Erlay)
+ [On scailing decentralized blockchains](https://link.springer.com/chapter/10.1007/978-3-662-53357-4_8)
+ [Information propagation in the Bitcoin network](https://www.gsd.inesc-id.pt/~ler/docencia/rcs1314/papers/P2P2013_041.pdf)
+ [BIP 152](https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki)
+ [BloXroute](https://bloxroute.com/wp-content/uploads/2019/11/bloXrouteWhitepaper.pdf)
+ [FIBRE](https://bitcoinfibre.org/)


## 一些攻击

此处只列出一些比较重要的攻击，更多攻击 见 https://chrislinn.gitbooks.io/blockchain-cheatsheet/content/blockchain/attack.html

+ 51% 攻击
    * 需要激励矿工维持全网算力
        - p2pool 和 一些山寨币 就是因为没有算力维护所以容易被 51%
        - incentive compatible, game theory
            + SoK: Tools for Game Theoretic Models of Security for Cryptocurrencies
            + Incentive Compatibility of Bitcoin Mining Pool Reward Functions
            + sucker punch makes you richer
            + Lay Down the Common Metrics: Evaluating Proof-of-Work Consensus Protocols' Security
+ selfish mining
    * 获得超出与算力相匹配的收益
+ Sybil Attack 女巫攻击
+ Eclipse Attack 日蚀攻击
    * 可以将整个网络划分为两个部分，降低 51%所需要的算力。
    * [Eclipse attack vs. Sybil attack](https://bitcoin.stackexchange.com/questions/61151/eclipse-attack-vs-sybil-attack)
+ PoS 中的 long range attack 长程攻击
    * 解决办法
        - PoW
        - VDF
            + 增强随机数的安全性
                * 太坊 2.0 信标链中使用 RANDAO + VDF 随机选取出块人
            + 还可以解决 Nothing-at-stake Attack
            + Proof of Space and Time
                * [Simple Proofs of Space-Time and Rational Proofs of Storage](https://eprint.iacr.org/2016/035.pdf)
                    - CRYPTO'19 密码学顶会
+ sharding 中的 1% 攻击
    * PoW
        - 比如分 100 片，那么分片上的算力只占 全网 1%，也就是说只需要 1% 的算力，就能完全控制该分片(1% 攻击) 
        * 解决办法: 参考 [Monoxide: Scale out Blockchains with Asynchronous Consensus Zones](https://www.usenix.org/conference/nsdi19/presentation/wang-jiaping)
            - NSDI '19 (USENIX Symposium on Networked Systems Design and Implementation 计算机网络顶会)
    * PoS
        * 解决办法: 随机数
            + 将节点随机分配到分片，防止作恶者将算力汇集到某一分片
                * 无法选择被分配到哪个分片
                * 无法提前知道会被分配到哪一个分片
            + 使用 blockhash 可能被操纵
            + VRF, 可被验证但不可被预测
                + Zilliqa 中选取节点
                + Algorand 中抽签
                    * 生成种子
                    * 选择有效的提案
                + Avalanche 也用了 VRF，但是其 committee  更加去中心化

## Sharding 中要考虑的一些问题

+ 节点分配的 randomness
+ cross-shard 的 conflict
+ cross-shard 的 atomicity
+ Sharding 中的 网络假设
    * 同步还是异步，同步的话可能被 卡停 或 反复重来
        * 比如收集区块 CoSi 多重签名时
