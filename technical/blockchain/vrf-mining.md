# V 字仇杀队: 通过 VRF 来消灭矿池

<!-- ex_nolevel -->

这篇文章简要地阐述了 我 和 [润超](https://github.com/SebastianElvis)  如何通过 VRF 来达到消灭矿池的效果， 回归中本聪去中心化的初衷

## 矿池中心化问题

在 https://www.blockchain.com/zh-cn/pools 上可以看到，前三大矿池算力加起来已经超过 51% (2020/01/23 日数据)。

超过 51% 就可以双花，矿池们可以合谋起来双花，自己变身印钞机。

而且也可以故意不打包你的交易，等等。比特大陆矿池甚至曾经故意暂停矿池出块，造成网络的临时瘫痪。

（甚至如果采用自私挖矿策略的话，只需要 25%～33% 的算力就可以双花。）

## VRF

VRF 可以用来作为一种哈希的方法，就像它名字中表明的一样，随机、可验证。

`alpha` 是要被哈希的内容， `PK` 是公钥， `SK` 是私钥。

生成一个哈希： `beta = VRF_hash(SK, alpha)`

生成一个 proof：`pi = VRF_prove(SK, alpha)`

验证 proof: `VRF_verify(PK, alpha, pi)`


## 为什么可以用 VRF 来解决 中心化矿池问题

从上面的 VRF 的函数中 可以看到，计算 PoW hash 需要知道 私钥 `SK`。

我们的构造是这样的：

__TODO:__

具体的哈希函数的设计选取看这里: https://hackmd.io/@ZcwjuAe3RUCFVPrXtvriPQ/S1YM1KZWI#Instantiating-VRF


也就说如果开矿池，矿池就要把自己的私钥告诉矿工，但是矿池又不可能把私钥告诉矿工，否则就会导致自己的钱全被矿工偷走。而且矿池和矿工双方都没法证明自己有没有偷钱。


## 你可能想问

__Q:__ 去中心化 挖矿我以前有听说过，比如 P2Pool 什么的。这个相比起来有什么好处呢？

__A:__ P2Pool 容易被引入算力攻击， 并且网络延迟严重，出块效率很差。事实上 P2Pool 已经好几年没出块了。我们也和现有的其他协议进行了对比，包括 SmartPool，2P-PoW，stratumV2 用的 BetterHash，还有 Andrew Miller 的 non-outsourceable scratch-off puzzle，得出的结论是我们比他们的更好。详见 https://hackmd.io/@ZcwjuAe3RUCFVPrXtvriPQ/S1YM1KZWI#Related-work

简单来说:

+ SmartPool
    * 需要链支持智能合约
+ BetterHash & 2P-PoW
    * 并不能消灭矿池, ，只是减少了矿池的中心化程度
* Andrew Miller's
    - 构造麻烦，而且他的协议只能使矿池丢失这个块挖到的币，我们的会导致矿池直接丢失私钥。

__Q:__ 我看了一下 2P-PoW，他们用私钥进行签名 来算哈希，这样不也能达到如果想算 PoW 必须知道私钥的效果吗，为什么要用 VRF，有什么意义？

__A:__ 2P-PoW 只是一个提案，事实上没有一个真正的 construction。他们的问题在于，他们需要 一个 deteministic 的签名函数，而在 椭圆曲线上找不到这样的 签名函数：椭圆曲线的签名函数引入了一个随机数，来保证安全性。那么同一个 nonce，每次签出来的名不一样，就可以碰撞出 满足难度的结果。 __TODO__

__Q:__ 这个好像不能 解决 ASCI 问题吧？

__A:__ 是的。这是我们下一步的目标，目前我们正在参考门罗的  RandomX。

__Q:__ 私池自己挖呢？

__A:__ 这个暂时没有办法解决，我们在看通过 DID 去中心化认证的方式来阻止。但是其实也不用太担心，因为私池算力是有限的，规模一大就要引入很多人管理，就引入了自己人作恶盗窃私钥的风险。


## 致谢

感谢以下大神们的 review 和反馈:

+ Omer Shlomovits [[1]](https://cyber.biu.ac.il/member/omer-shlomovits/) [[2]](https://twitter.com/omershlomovits),  Lindell 亲传弟子, ZenGo co-founder
+ [Cheng Wang](https://ethresear.ch/u/chengwang)
+ [Jiangshan Yu](https://www.jiangshanyu.com/)
+ [Tromp](https://forum.grin.mw/u/tromp),  Cuckoo Cycle 算法发明者
+ Mikerah, ETH 社区成员, Hashcloak 创始人 (v 神 2018 年底送了 1k eth 给她 ...)
+ Silur, 前 ETH 基金会成员, Algorand 匈牙利区大使
+ 吴逸飞