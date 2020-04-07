# 区块链代码实现中的常见漏洞



由 [Whitepaper – Coinbugs: Enumerating Common Blockchain Implementation-Level Vulnerabilities](https://research.nccgroup.com/2020/03/26/whitepaper-coinbugs-enumerating-common-blockchain-implementation-level-vulnerabilities/) 翻译总结而来

## 网络分割

不同客户端实现可能导致验证区块的方式不一致，进而导致网络分割。

值得强调的是，网络分割有可能导致 **双花**。因为当网络重新统一时，其中一个网络的交易会被 回滚，如果黑客能设法不让交易转发到原链就有可能实现双花。
所以网络分割有可能是 **最重要** 的攻击向量。


### 不同客户端实现造成的网络分割

不同客户端实现有可能对区块验证的判断逻辑不同。本质上是客户端的 equivalence 问题。


但事实上，对一个协议的实现，实际上是 对一个协议的 一门方言 dialect 的实现：

+ [Towards a formal theory of computer insecurity: a languagetheoretic approach](https://www.youtube.com/watch?v=AqZNebWoqnc)
+ **Postel's law**: Be liberal in what you accept and conservative in what you send

况且......[Pieter Wuille 解释过: "consensus rules 实际上是怎样的" 事实上是不可知的](https://bitcoin.stackexchange.com/questions/54878/why-is-it-so-hard-for-alt-clients-to-implement-bitcoin-core-consensus-rules)，尤其是对于区块验证这种 context/state-dependent 的事情。比如:

+ OpenSSL's inconsistent DER parser.
+ Compressed vs hybrid public keys.
+ ...


**例子：**

+ geth` vs `Parity` state update 不同, 是否允许 reverting empty account deletions (以防 out-of-gas exception):
    * https://blog.ethereum.org/2016/11/25/security-alert-11242016-consensus-bug-geth-v1-4-19-v1-5-2/




**Note**: *文档作为 spec 其实是不够的，实际如何 implement 才最有可信力。bitcoin 的代码才是最准确的文档。*


### 运行环境差异造成的网络分割

### 区块 hash 投毒造成的网络分割

### 意外或未成熟的分叉导致的网络分割

### 分支混乱导致的网络分割

## 不正确的时间戳验证

## 整型数溢出漏洞

## Merkle 树实现漏洞

## 区块或交易处理时导致的存储资源耗尽

## 区块或交易处理时导致的 CPU 资源耗尽

