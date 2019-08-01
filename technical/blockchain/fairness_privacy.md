# Fairness & Privacy

First, priewienv's [_secret sharing_](https://blog.priewienv.me/post/randomness-blockchain-1/) draws my attentions.

Some nice papers:

+ [Timed Commitments](https://www.iacr.org/archive/crypto2000/18800237/18800237.pdf)
+ [Homomorphic Time-Lock](https://eprint.iacr.org/2019/635)
+ [Anonymous Multi-Hop Locks for Blockchain Scalability and Interoperability](https://www.ndss-symposium.org/wp-content/uploads/2019/02/ndss2019_09-4_Malavolta_paper.pdf)
+ [Concurrency and Privacy with Payment-Channel Networks](https://eprint.iacr.org/2017/820)
    * using Multihop HTLC

## MPC
+ (输出结果)正确性
+ 隐私性
+ 输入独立性
+ 公平性
    * 一个参与者获得了输出，则其他参与者也必须获得输出
+ 保证输出送达
    * 每个诚实参与者都能获得输出

### Homomorphic Encryption, 同态加密
+ The computations are represented as either Boolean or arithmetic(加减乘除) circuits.
+ [What is the link, if any, between Zero Knowledge Proof (ZKP) and Homomorphic encryption?](https://crypto.stackexchange.com/questions/57747/what-is-the-link-if-any-between-zero-knowledge-proof-zkp-and-homomorphic-enc)
+ 分类
    * partially homomorphic
        - 只能实现一个运算
    * somewhat homomorphic
        - 实现两种门电路(运算), but only for a subset of circuits
    * leveled fully homomorphic
        - 先决有限深度的任意运算
    * fully homomorphic
        - 无限深度任意运算
        - 对于实际应用来说，主要是乘法深度比较重要

### Garbled Circuit & Oblivious Transfer
和电路也紧密相关

### Verifiable Secret Sharing
重建 secret, VSS 允许恶意参与者(submitting fake shares).

####  Shamir's Secret Sharing, SSS
其实就是门限 Secret Sharing

May not be VSS:

+ https://crypto.stackexchange.com/questions/47230/does-shamir-secret-sharing-provide-integrity
+ https://en.wikipedia.org/wiki/Lagrange_polynomial
+ https://crypto.stackexchange.com/questions/54578/how-to-forge-a-shamir-secret-share


## Threshold Signature Scheme
密钥打碎分开存储，然后在需要时通过MPC多方安全计算生成签名

和传统的 多签方案不同的是 多签是有多把私钥，如果私钥复用则泄漏了就有危险。传统多签是链上的，和链采用哪条曲线有关。tss 是链下的纯密码学的计算，目的是为了生成签名，兼容性更强。

和 secret sharing也不一样，secret sharing 虽然也打碎了密钥，但是最终要有一个 dealer 重构出密钥并进行签名，那么就存在 单点故障和重构出的密钥可能被泄露的问题。而 tss不需要 重构出密钥，就不用怕 密钥泄漏。（tss还有一些别的nice feature，key rotation 也就是私钥可变，更增加了攻击的难度）

## Timed Commitments
+ the receiver is kinda guaranteed (I mean, with high probability) to recover the signature from the commitment after given time
    + makes use of time needed for computing squarings
        + gradual revealing, eliminating possibility by trying again and again 
    + high computing power won't speed it up
    + the committer also need to convince the receiver the commiment is indeed the commitment of the desired signature
        + use zkp (a simulator can produce...)
+ there also is a proof as the shortcut to verify, so that others don't need to go through the recovery process again to verify
+ not sure whether the use of CA will introduce problem?
+ applications
    * Contract Signing
    * Collective coin-flipping
    * Honest-Preserving Autions
        - vs [uncheatable auctions](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.24.6692&rep=rep1&type=pdf)
    * in Zero-Knowledge
        - constant time/rounds by using force-opening


## Atomic Swap

Atomic Swap 的一些解释说明。论文rephrase可用。

+ https://en.wikipedia.org/wiki/Atomicity_(database_systems)
    * An indivisible and irreducible series of operations such that either all occur, or nothing occurs.
    * At one moment in time, it has not yet happened, and at the next it has already occurred in whole (or nothing happened if the transaction was cancelled in progress).
+ Atomic Cross-Chain Swaps, https://arxiv.org/pdf/1801.09515.pdf
    * An atomic swap protocol guarantees
        - (1) if all parties conform to the protocol, then all swaps take place,
        - (2) if some coalition deviates from the protocol, then no conforming party ends up worse off, and
        - (3) no coalition has an incentive to deviate from the protocol.

## Differential Privacy, 差分隐私
注入噪音或扰动

在或者不在这个数据集中，对查询结果没有影响。

攻击者通过对该数据集的任何查询或者背景知识都无法准确推断出是否在数据集中。

在不在数据集中都不会影响最终的查询结果，那么可以认为就不在这个数据集中，而如果不在数据集中，数据自然不会泄露。