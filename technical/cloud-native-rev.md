+ 单体架构
    * 硬件隔离
        - 存储
            + cgroups
            + 加机器
                * 成本
    * MySQL 分表
        - 加机器
    * 抢红包
        - 微服务
            + 加机器
        - 熔断降级速率限制
            + 遥测

---
+ 微服务
    * _服务发现_
        - traditional
            + client-side
                * code/framework in app
                    - Eureka
        - k8s
            + network layer
                * dns
                * service
                * ingress
    * load-balancing
        - traditional
            + ribbon

---
+ 微服务治理
    * 限流
    * 熔断
    * 灰度发布
    * ...

---
+ 加机器
    * 负载均衡
        - dns
            + 慢
            + 控制权限小
        - 硬件
            + 只能流量，无法内存
            + 成本
            + single point
            + k8s 动态扩容
                * 一个命令动态扩展10个 nginx 节点，线上运行
                * 流量洪峰, 响应调度
    * 折旧率
        - 想放上云
            + 弹性计算
                * _无状态_
                    - 声明式 vs 命令式
                    - 幂等
                    - 横向扩展

---
+ 虚拟机
    * 隔离
    * 依赖库
    * 吃资源启动慢, 宕机时间, 云产商成本
        - 容器, 进程角度
            + docker
                * namespaces
                    - ipc, posix
                    - pid
                    - network
                        + device
                        + protocol
                        + firewall
                        + routing
                        + host, domain
                        + user group
                * cgroups
                    - mem
                    - cpu
                    - devices
                    - hang process
                    - bandwidth, flow_priv
            + 应用才是价值所在
                * compose
                * swarm
                * k8s

---
+ k8s 成本
    * etcd 
        - 重要角色, 集群可用性上限
            + 服务配置发现信息&配置信息
        - 3, 5, 7
    * docker 成本
+ 传统成本
    + 非线性
    + 硬件负载
    + 动态扩容???
    + 维护成本???
+ 开发测试运维 vs DevOps
    * 快速迭代

---
+ 遥测
    * 多少请求
    * 多少测试
    * 多久回复


---
+ service mesh
    * traffic management, api gateway
    * observability
        - 服务调用
        - 性能分析
    * policy enforcement
        - 控制服务访问策略
            + 多级调用
        - service identity & security
            + 安全保护
