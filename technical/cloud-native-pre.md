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
# 如何成为下一个 Uber、Netflix、Airbnb?

Hackers & Painters:

> how valuable your technology was. ... the number of users you have.
> 
> Users are the only real proof that you've created wealth.
> 
> ... solve problems that users care about.


---
# 回顾历史

从最早的物理服务器开始，我们都在不断地抽象或者虚拟化服务器。
![虚拟化](https://chrislinn.ink/img/cloud-native/server-growth.jpg)

---
# physical
![physical](https://chrislinn.ink/img/cloud-native/physical.png)

---
# 虚拟化
+ 为什么要虚拟化
    * 解决环境问题
        - 运行时
        - 系统工具
        - 系统库
    * 资源隔离

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
# 容器 vs 虚拟机
![vm-vs-container](https://chrislinn.ink/img/cloud-native/vm-vs-container.png)

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
容器编排，应用上云，让容器应用进入大规模工业生产时代

---
# k8s 的方便强大

* Deployment
* Service Discovery
* DevOps: 开发测试环境一致、CI/CD，监控

---
# k8s 的架构
![k8s-arch](https://chrislinn.ink/img/cloud-native/k8s-arch.jpg)

---
# k8s 中的资源隔离层次
* Containner
* Pod
* Node
* Cluster

---
# sidecar
![sidecar](https://chrislinn.ink/img/cloud-native/sidecar.jpg)

---
# sidecar
![ContainerSidecarPattern](https://chrislinn.ink/img/cloud-native/ContainerSidecarPattern.png)

---
# k8s service
![services](https://chrislinn.ink/img/cloud-native/services.svg)

---
# k8s service
![k8s-node-port](https://chrislinn.ink/img/cloud-native/k8s-node-port.png)

---
# k8s 存在的问题
+ 流量控制
+ 负载均衡
+ 如何解决? service mesh!

---
# service mesh
+ 是什麽
+ 解决了什么问题

---
# service mesh 方案
+ 第一代
    * Hystrix, Stubby
+ 第二代
    * Linkerd, Istio, Linkerd2

---
# istio 的架构
![istio-arch](https://chrislinn.ink/img/cloud-native/istio-arch.svg)

<!-- 
Istio架构分为控制层和数据层:

+ 数据层：由一组智能代理（Envoy）作为sidecar部署，协调和控制所有microservices之间的网络通信。
+ 控制层：负责管理和配置代理路由流量，以及在运行时执行的政策。

https://jimmysong.io/posts/istio-overview/
https://jimmysong.io/posts/why-do-we-need-istio/
 -->

---
# k8s + service mesh = cloud native!
![cloud-native-architecutre-mindnode](https://chrislinn.ink/img/cloud-native/cloud-native-architecutre-mindnode.jpg)

<!-- 
* 引出 micro-service & serverless
    - micro-service 原生贴合 cloud-native
    - cloud native 也在推 serverless 这种架构
 -->

---
# micro-service
![monolithic-vs-microservices](https://chrislinn.ink/img/cloud-native/monolithic-vs-microservices.png)
<!-- 
+ account, inventory, shipping
+ api gateway
+ service discovery
 -->

---
# serverless
![serverless-architecture](https://chrislinn.ink/img/cloud-native/serverless-architecture.png)

---
# cloud history
![history](https://chrislinn.ink/img/cloud-native/history.png)

<!-- 

1. 什么样的工作带来价值感，控制一下时间
2. 牛B -> 伟大
3. 压缩一下前3页的PPT?
4. 感觉前面的PPT占用的页数有点多？
5. 云计算 这个页面对我们这次的主题帮助是？
6. 加入一点数字案例来表示精准，比如用虚拟机可以启动多少个服务，用dockr可以启动多少个服务？
7. 没有了解到k8s到底是做什么的？是不是可以尝试加入一张图来说明？
8. 问题： 为什么k8s要做资源隔离？
9: 一些定义，解释可以写一下核心点？不然听了就忘了
10. K8s资源隔离层次，是否可以加入一些简单的图？
11. 建议，不要单图？尝试图+文字？
12. 通过讲解安利来展示k8s的是否希望有图，可以更直观化
13. 想要感受到k8s到底解决了单纯容器化的什么问题
13. service mesh解决了什么问题，文字化核心点一下？
14. service mesh 第一代，第二代方案的进化路程对这次主题的帮助是？
15. 想知道service mesh和k8s是怎么结合的，替换了k8s上的哪些模块？
16. 介绍server mesh的时候概念多听的有些模糊了，可否假设一个真实的案例来说明？
17. 微服务这一块应该是在比较前面吧？和docker在差不多的位置？
18. 听完之后缺少一种感觉，对k8s， server mesh模糊，不确定为什么要使用这一套
19. 想到点：cloude native的讲说案例可否直接使用Bycoin的服务器来解决？
20. 将cloude native的好处的时候，画个图？类似于k8s和iota是怎么样的？服务是怎么一个装备 -


->