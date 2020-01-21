# Noise Protocol

## [Noise Protocol Framework](https://zhuanlan.zhihu.com/p/96944134)
+ 是一个框架
    * 想开发一些直接基于 TCP 或者 UDP 的私有协议时
    * TLS 颇为笨重
    * 自己开发的安全协议不一定靠谱
+ 对比 TLS 1.3
    * same
        - 协议栈清爽 (放弃老旧算法)
        - 握手只需要 1-RTT（甚至 0-RTT）
    * diff
        - TLS 需要 PKI, 协议栈复杂，很难应用到非中心化的 p2p 网络中
+ 安全信道建立的基石是 DH 算法
    * ECDH
    * 