<!-- 

https://jimmysong.io/kubernetes-handbook/

http://kubernetes.kansea.com/docs/

https://www.kubernetes.org.cn/docs

http://docs.kubernetes.org.cn/

http://docs.kubernetes.org.cn/92.html

http://docs.kubernetes.org.cn/227.html

http://docs.kubernetes.org.cn/230.html

https://www.kubernetes.org.cn/kubernetes-pod

https://www.kubernetes.org.cn/kubernetes-services

https://www.kubernetes.org.cn/kubernetes%E8%AE%BE%E8%AE%A1%E6%9E%B6%E6%9E%84

https://www.kubernetes.org.cn/kubernetes%E8%AE%BE%E8%AE%A1%E7%90%86%E5%BF%B5

https://www.abhishek-tiwari.com/a-sidecar-for-your-service-mesh/

https://jimmysong.io/posts/why-do-we-need-istio/

https://jimmysong.io/posts/istio-overview/

 -->


# Cloud Native

---
# Cloud Native
## 什么样的工作能为你带来价值感?

<!--
如何成为 Uber、Netflix、Airbnb 这样快速创新、快速增长的公司?
传统应用升级缓慢、架构臃肿、不能快速迭代、故障不能快速定位、问题无法快速解决

+ https://jimmysong.io/posts/what-is-cloud-native-application-architecture/
+ https://zhuanlan.zhihu.com/p/28663432
 -->

---
# Cloud Native
## 如何成为下一个 Uber、Netflix、Airbnb?

---
# Cloud Native
## 如何成为下一个 Uber、Netflix、Airbnb?
> how valuable your technology was. ... the number of users you have.
> 
> Users are the only real proof that you've created wealth.
> 
> ... solve problems that users care about.

-- Hackers & Painters

---
# 回顾历史

从最早的物理服务器开始，我们都在不断地抽象或者虚拟化服务器。
![虚拟化](https://chrislinn.ink/img/cloud-native/server-growth.jpg)

---
# physical
![physical](https://chrislinn.ink/img/cloud-native/physical.jpg)

---
# 虚拟化
+ 为什么要虚拟化
    * 资源隔离
    * 解决环境问题
        - 运行时
        - 系统工具
        - 系统库

---
# 物理机 vs 虚拟机
![vm](https://chrislinn.ink/img/cloud-native/vm.jpg)

---
# 云计算

* IaaS
* PaaS
* SaaS 

<!-- + 为什么要上云
+ 虚拟化使云计算成为可能
+ 云计算的发展历程
-->

---
# 容器 
![container](https://chrislinn.ink/img/cloud-native/why_containers.svg)

---
# 虚拟机 vs 容器 
![vm](https://chrislinn.ink/img/cloud-native/vm-vs-container.png)

<!-- 
https://stackoverflow.com/questions/16047306/how-is-docker-different-from-a-virtual-machine

虚拟机中需要模拟一台物理机的所有资源，比如你要模拟出有多少CPU、网卡、显卡等等，这些都是在软件层面通过计算资源实现的，这就给物理机凭空增加了不必要的计算量。容器仅仅在操作系统层面向上，对应用的所需各类资源进行了隔离。

+ 对比图
+ 优缺点
* docker 与 容器的关系
+ 为什么可以解决环境问题
 -->

---
# 容器 vs docker
容器的一种封装，提供简单易用的容器使用接口。目前最流行的容器解决方案。

+ 基于 LXC -> runc
+ 利用 namespaces 来做权限的控制和隔离
+ 利用 cgroups 来进行资源的配置
+ 通过 aufs 提高文件系统的资源利用率

---
# k8s 与 docker 的关系

---
# k8s 的好处是什么
+ 解决了 单纯用 docker 的什么问题
+ 为什么方便强大
    * Deployment
    * service discovery
    * DevOps
    * CI/CD

---
# k8s 的架构
![k8s-arch](https://chrislinn.ink/img/cloud-native/k8s-arch.jpg)

+ 解释 各个接口，和 组件的作用

---
# k8s 中的资源隔离层次
+ 为什么要资源隔离
    * 保证对集群资源的最大化和最优利用率
+ 隔离层次
    * containner
    * Pod
    * Sandbox
    * Node
    * Cluster

---
# k8s 中的资源隔离层次的区别
![k8s-node-port](https://chrislinn.ink/img/cloud-native/k8s-node-port.png)

---
# k8s 存在的问题
+ 流量控制
+ 负载均衡
+ 如何解决
    * 使用 service mesh

---
# service mesh
+ service mesh 是什麽
+ service mesh 解决了什么问题
+ 现有的 service mesh 方案
    * istio

---
# istio 的架构
![istio-arch](https://chrislinn.ink/img/cloud-native/istio-arch.jpg)

Istio架构分为控制层和数据层:

+ 数据层：由一组智能代理（Envoy）作为sidecar部署，协调和控制所有microservices之间的网络通信。
+ 控制层：负责管理和配置代理路由流量，以及在运行时执行的政策。

<!-- 
https://jimmysong.io/posts/istio-overview/
https://jimmysong.io/posts/why-do-we-need-istio/
 -->

---
# k8s + service mesh = cloud native
+ cloud native 架构特点
    * ![cloud-native-architecutre-mindnode](https://chrislinn.ink/img/cloud-native/cloud-native-architecutre-mindnode.jpg)
    * 引出 micro-service & serverless
        - micro-service 原生贴合 cloud-native
        - cloud native 也在推 serverless 这种架构

---
# micro-service
+ micro-service 是什麽
+ micro-service vs 单体架构
+ micro-service 的优缺点

---
# serverless
+ serverless 是什麽
+ serverless 的优缺点

---
# 8btc
+ 8btc 目前的方案
+ 使用 cloud native 可以带来哪些优点