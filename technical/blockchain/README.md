# Blockchain

## Reading List
+ papers
    * [theoretical-blockchain-research-papers](https://github.com/mathetake/theoretical-blockchain-research-papers)
    * [blockchain-papers](https://github.com/decrypto-org/blockchain-papers)
    * [scalingbitcoin](https://scalingbitcoin.org/papers/)
        - Bitcoin: A Peer-to-Peer Electronic Cash System
        - Information propagation in the Bitcoin network
    * https://github.com/bitcoinops/scaling-book
    * https://github.com/UCL-InfoSec/blockchain-reading
    * other
        - Boost Blockchain Broadcast Propagation with Tree Routing
        - Han Runchao list
        - https://github.com/tyrchen/unchained
        - why it takes 20 min to mine this block
    * https://github.com/UCL-InfoSec/blockchain-reading/wiki
    * https://github.com/trailofbits/publications
+ https://github.com/satoshilabs/slips
+ https://bitcointechtalk.com/
+ http://readingbitcoin.org
+ https://scalingbitcoin.org/
+ [Ethereum Research](https://ethresear.ch/)
+ https://github.com/ScottWorks/ConsenSys-Academy-Notes
+ [区块链技术指南](https://yeasy.gitbooks.io/blockchain_guide/content/)
+ 《精通比特币》第二版
    * https://legacy.gitbook.com/book/wizardforcel/masterbitcoin2cn/details
    * http://book.8btc.com/books/6/masterbitcoin2cn/_book/
+ [ethereumbook](https://github.com/ethereumbook/ethereumbook)
+ [以太坊白皮书](https://github.com/ethereum/wiki/wiki/%5B%E4%B8%AD%E6%96%87%5D-%E4%BB%A5%E5%A4%AA%E5%9D%8A%E7%99%BD%E7%9A%AE%E4%B9%A6)
+ [以太坊爱好者知识库](https://ethfans.org/wikis/Home)
+ btc
    * bitcoin-core
        - https://github.com/bitcoin/bitcoin
        - https://github.com/bitcoin/bips
    * btcsuite
        - https://github.com/btcsuite/btcd
            + https://github.com/btcsuite/btcchain
            + https://github.com/btcsuite/btcwire
        - https://github.com/btcsuite/btcwallet
+ eth
    * https://github.com/ethereum/go-ethereum
    * https://github.com/ethereum/pyethereum
    * docs
        - https://github.com/ethereum/EIPs
        - https://github.com/ethereum/research
        - https://github.com/ethereum/homestead-guide
        - https://github.com/ethereum/whisper
        - https://github.com/ethereum/devp2p
            + https://github.com/ethereum/pydevp2p
        - Ethereum 2.0 Specifications
            + https://github.com/ethereum/eth2.0-specs
    * evm
        - https://github.com/ethereum/py-evm
    * https://github.com/ethereum/serpent
        - Serpent is an assembly language that compiles to EVM code that is extended with various high-level features. 
    * solidity
        - https://github.com/ethereum/solidity
        - https://github.com/ethereum/py-solc
        - https://github.com/ethereum/solc-bin
    * https://github.com/ethereum/vyper
        - Pythonic Smart Contract Language for the EVM
    * test
        - https://github.com/ethereum/tests
        - https://github.com/ethereum/eth-tester
        - https://github.com/ethereum/pytest-ethereum
    * https://github.com/ethereum/web3.py
    * https://github.com/ethereum/mist
        - Mist. Browse and use Ðapps on the Ethereum network. 
    * https://github.com/ethereum/remix
        - Ethereum IDE and tools for the web
        - https://github.com/ethereum/remixd
            + Remixd is a tool that intend to be used with Remix IDE (aka. Browser-Solidity). It allows a websocket connection between Remix IDE (web application) and the local computer.
    * https://github.com/ethereum/twig
        - A tool for Ethereum smart contract development.
    * https://github.com/ethereum/btcrelay
        - Ethereum contract for Bitcoin SPV
    * https://github.com/ethereum/py-trie
        - Python library which implements the Ethereum Trie structure.
    * https://github.com/ethereum/casper
        - a Proof-of-Stake finality protocol that can be layered on any block proposal mechanism.
    * https://github.com/ethereum/beacon_chain
        - Implements a proof of concept beacon chain for a sharded pos ethereum 2.0. Spec in progress can be found here.
+ btccom
    * https://github.com/btccom/btcpool
    * https://github.com/fengchunjian/btcpool_code_notes
    * https://github.com/btccom/parity-ethereum
+ Master Bitcoin


## TODOS
+ 闪电网络
+ time warp attack
    * https://bitcoinops.org/en/newsletters/2019/03/05/
+ vbytes
+ 预言机
+ 致盲攻击
+ 盲签名
+ https://en.wikipedia.org/wiki/Authenticated_encryption
* https://learnblockchain.cn/2019/04/18/learn-zkSNARK/
* [IPFS/filecoin 技术架构分析](https://www.jianshu.com/c/06ddad58490f)
* https://github.com/ipfs/ipfs
    - 不是 go-ipfs
* https://www.jianshu.com/u/30081a05cf95
* https://github.com/wupeaking/tendermint_code_analysis
+ mimblewimble
+ bullet proof
+ rsa accumulators
    * https://bitcoinops.org/en/newsletters/2019/02/05/
    * 节省存储在磁盘上的 UTXO commitment 的大小
    * UTXO 累加器相关
        - UTXO Hash Set（UHO）理念结合起来，以便整个节点存储UTXO的哈希值，而不是完整的UTXO，以便显着减少整个节点使用的存储量，而不需要任何改变共识规则。
        - a scalable drop-in replacement for merkle trees
+ FlyClient
+ A Fast and Scalable Payment Network with Bitcoin Duplex Micropayment Channels
+ Bolt: Anonymous Payment Channels for Decentralized Currencies
+ eltoo: A Simple Layer2 Protocol for Bitcoin
+ SoK: Off The Chain Transactions
+ lightning & lapp
    * The bitcoin lightning network: Scalable off-chain instant payments
    * loop
    * https://bitcoin.stackexchange.com/a/85259
    * https://bitcoin.stackexchange.com/a/84953
+ ninji
+ GHOST
+ Bitcoin-NG
+ privacy
    * https://en.bitcoin.it/wiki/Privacy
    * https://en.bitcoin.it/wiki/Category:Privacy
    * Dandelion protocol
        - https://arxiv.org/abs/1701.04439
    * Pay-to-EndPoint（P2EP）
+ Schnorr-based signatures
    * https://github.com/sipa/bips/blob/bip-schnorr/bip-schnorr.mediawiki
    * https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2018-January/015614.html
    * https://bitcoin.stackexchange.com/questions/77234/schnorr-vs-ecdsa/77235#77235
    * https://bitcoin.stackexchange.com/questions/85213/when-schnorr-signatures-are-part-of-bitcoin-will-it-be-possible-validate-each-bl
    * https://bitcoin.stackexchange.com/questions/85101/does-musig-have-the-same-security-as-2-2-multisig
    * https://bitcoin.stackexchange.com/questions/84086/how-are-scriptless-scripts-and-taproot-different
    * https://www.8btc.com/article/390831
    * [Schnorr即将开启的多签时代意味着什么](https://mp.weixin.qq.com/s?__biz=MzUzNTg4MTE2MQ==&mid=2247483727&idx=1&sn=b4183119af89a78cfd146d8826e739f8&chksm=fafff5e9cd887cffe4269aaf6be7cb8f0b94696f3039edfce85e72404219c3f4f8cde05837d1&mpshare=1&scene=1&srcid=0403b9cxWSLRjoc0oPSimDPI&pass_ticket=TYxGOa7GCVcTL1vSpTis8Nbdu0oUNyiEQ%2Fha2tpydiRmUm3KK5JZ4uKMQYy2dXDe#rd)
    * [What makes batch verification of Schnorr signatures effective?](https://bitcoin.stackexchange.com/a/80702)
+ fibre
    * http://bitcoinfibre.org/
    * https://github.com/bitcoinfibre/bitcoinfibre
    * http://bitcoinfibre.org/public-network.html
    * https://bitcoin.stackexchange.com/questions/56485/can-someone-please-explain-fibre-to-me-like-im-5-and-why-is-it-useful/56494
+ chain.com, ada(iohk, cardano, upolos), cosmos(tendermint), PolkaDot(parity)
+ taint
+ P2P
    * https://github.com/AmarRSingh/NotThatNetworking_Research
+ layer2
    * https://github.com/Awesome-Layer-2/awesome-layer-2
+ MIM
+ Secp256k1
    * https://bitcointalk.org/index.php?topic=151120.0
    * https://en.bitcoin.it/wiki/Secp256k1
    * https://bitcoin.stackexchange.com/questions/85387/what-is-the-reasoning-behind-the-choice-of-2256-232-977-for-the-prime-on-the-s
+ lfs
    * c
    * rust
+ 编译器
    * type check
        - rust
        - ml/func
        - typescript
        - python
        - c
    * opt
        - c
        - rust
        - ml/func
+ stl

## Books

## BigchainDB 源码剖析

## DNA 源码剖析

## Chain 源码剖析

> https://gguoss.github.io/2017/05/26/chain/
> 
> chain设计了一种多种资产可以互相交易发布的分布式账本协议。用该协议的多条链可以独立的存在， 并且可以跨链交易， 这样不同的运营商可以以相同的形式互相交易。坚持最小权限原则， 资产的管理和分布式账本同步控制相分离。
> ...
> 当今社会的货币， 证券以及衍生品是以电子支付的形式进行交易的。电子支付需要太多的第三方中介参与，需要太多的人力物力去对帐。比特币虽然去中介化了，但堆栈式脚本实现的功能太少， 只支持本币的简单交易合约。以太坊的evm 又太过于复杂， 有太多的安全不确定性。再者， 他们是公有链，所有数据在公众的眼皮底下。 这时， Chain 恰好能够解决这些痛点。 Chain 是联盟链，支持多种资产的发布和交易， 也支持多种组织的跨链交易。


## Misc
> https://gguoss.github.io/2018/02/19/%E9%83%AD%E5%A4%A7%E4%BE%A0%E7%9A%842018/
> 
> 当今世界，我能看得上眼的链就5个半。可能真正有用的链也多不了几个。
> 
> + Bitcoin, 区块链的鼻祖， 伟大的发明， 打开了我们对区块链的认识。（算1个）
> + Ethereum， VB，Gavin Wood 杰出才能的结合，账户模型智能合约时代的到来。（算1个）
> + Cardano， IOHK最强区块链人才库的支持，以及对BTC和ETH认知的结合。（算1个）
> + EOS， 在我眼里，这不属于公有链， 但有BM这样人的技术尝试。 （算半个）
> + Polkadot, Gavin Wood 力做的多链技术， 因为明年才能落地。 （算半个）
> + Cosmos， 强大的Tendermint 共识， 但在我这里， 也不属于公有链。 （算半个）
> + Zcash， 零知识证明， 密码学的大尝试。 （算半个）
> + Filecoin , 有成熟的IPFS协议做支撑， 但filecoin 的落地，还是未知数。 （算半个）

