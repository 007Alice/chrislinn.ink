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

`alpha` 是要被哈希的内容， `pk` 是公钥， `sk` 是私钥。

生成一个哈希： `beta = VRF_hash(SK, alpha)`

生成一个 proof：`pi = VRF_prove(SK, alpha)`

验证 proof: `VRF_verify(PK, alpha, pi)`


## 为什么可以用 VRF 来解决 中心化矿池问题


## 你可能想问

__Q:__ 去中心化 挖矿我以前有听说过，比如 P2Pool 什么的。这个相比起来有什么好处呢？

__A:__

__Q:__ 这个好像不能 解决 ASCI 问题吧？

__A:__ 是的。这是我们下一步的目标，目前我们正在参考门罗的  RandomX。

__Q:__

__A:__

__Q:__

__A:__


## 致谢

感谢以下大神们的 review 和反馈:

+ Omer Shlomovits [[1]](https://cyber.biu.ac.il/member/omer-shlomovits/) [[2]](https://twitter.com/omershlomovits),  Lindell 亲传弟子, ZenGo co-founder
+ [Cheng Wang](https://ethresear.ch/u/chengwang)
+ [Jiangshan Yu](https://www.jiangshanyu.com/)
+ [Tromp](https://forum.grin.mw/u/tromp),  Cuckoo Cycle 算法发明者
+ Mikerah, ETH 社区成员, Hashcloak 创始人 (v 神 2018 年底送了 1k eth 给她 ...)
+ Silur, 前 ETH 基金会成员, Algorand 匈牙利区大使
+ 吴逸飞