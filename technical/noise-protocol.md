# Noise Protocol

## [Noise Protocol Framework](https://zhuanlan.zhihu.com/p/96944134)
+ 是一个框架
    * 想开发一些直接基于 TCP 或者 UDP 的私有协议时
    * TLS 颇为笨重
    * 自己开发的安全协议不一定靠谱
    * 没有规定使用什么样的通讯协议
        - TCP/UDP 甚至是任何满足 read/write 接口的子系统，比如文件，管道（pipe）都可以
+ 对比 TLS 1.3
    * same
        - 协议栈清爽 (放弃老旧算法)
        - 握手只需要 1-RTT（甚至 0-RTT）
    * diff
        - TLS 需要 PKI, 协议栈复杂，很难应用到非中心化的 p2p 网络中
+ 安全信道建立的基石是 DH 算法
    * ECDH
* Noise 把协议变量设计为静态而非协商出来的
    - 而从用户的角度，用户写出来的使用 Noise 的应用往往是自己的节点跟自己的节点通讯，因而无需协商
    - `Noise_<握手的模式>_<公钥算法>_<对称加密算法>_<哈希算法>`
        * https://noiseexplorer.com/patterns/
* 协议的状态机
    - chaining key
    - HandshakeState
    - CipherState
- 用户接口
    + build
        * 根据协议变量和固定私钥，初始化 HandshakeState
    + write(msg, buf)
    + read(buf, msg)
    + into_transport_mode
        * 将 HandshakeState 转为 CipherState
    + rekey