# Distributed Computing


## Curriculum

+ https://github.com/SebastianElvis/sharding-bib
+ MIT 6.852
+ ethz
    * https://disco.ethz.ch/courses/ss04/distcomp/
    * https://disco.ethz.ch/courses/ss03/distcomp/
    * https://disco.ethz.ch/courses/podc/
    * https://disco.ethz.ch/courses/distsys/
+ princeton
    * https://www.cs.princeton.edu/courses/archive/fall18/cos418/schedule.html
+ MIT 6.S974
    * theoretical
+ MIT 6.824
    * distributed system
+ talent-plan
+ https://github.com/aphyr/distsys-class

## PBFT

最近看 consensus 的 心得分享一下

[PBFT 中 assume 了 weak synchrony](https://www.usenix.org/legacy/events/osdi99/full_papers/castro/castro_html/node3.html#SECTION00030000000000000000):
>it must rely on synchrony to provide liveness

HoneyBadgerBFT 提出了改进，说 PBFT 的 liveness assumption 不 practical
>guarantees liveness without making any timing assumptions

HoneyBadgerBFT 用的是 erasure-coded broadcast<br>
small client-load 时可以考虑使用 bracha broadcast 来代替，throughput 更大，lantency 更低

bracha broadcast 相关(虽然 citation 不如 PBFT 多，但是很多知名论文都引用了它)：

+ ethz 课件：https://disco.ethz.ch/courses/ss04/distcomp/lecture/chapter9.pdf
+ 论文: https://dl.acm.org/citation.cfm?id=806743


bracha 存在的问题是 scalability: 对于 transactions of size B

+ bracha 的通信复杂度是 `O(n^2*B)`
+ 而 HoneyBadgerBFT 的是 `O(n*B)`
+ BEAT 这个还没看，做到了 `O(B)`
    * BEAT 这篇论文发到了 ACM CCS 上: https://dl.acm.org/citation.cfm?id=3243812

还有一个很有意思的是 beat 中谈到, pbft 的 view-change 实现很难, 且 async 做起来其实比 sync 容易, 很多论文都选择了不实现 view-change

[Tendermint 也是对viewchange进行了 bracha 改造](http://drops.dagstuhl.de/opus/volltexte/2017/8016/pdf/LIPIcs-DISC-2017-1.pdf)<br>
但是好像 tendermint 现在已经不用 PBFT 了（待考证）

## SMR
+ https://en.wikipedia.org/wiki/State_machine_replication
+ https://www.cs.cornell.edu/fbs/publications/SMSurvey.pdf
+ http://www.di.fc.ul.pt/~bessani/publications/tutorial-smr.pdf
+ https://www.inf.usi.ch/faculty/pedone/Paper/2011/2011DSN.pdf
+ https://segmentfault.com/a/1190000004033730


## Raft

+ https://draveness.me/etcd-introduction
+ https://zhuanlan.zhihu.com/p/49792009
+ https://jiajunhuang.com/articles/2018_11_20-etcd_source_code_analysis_raftexample.md.html
+ https://jiajunhuang.com/articles/2018_11_22-etcd_source_code_analysis_raft.md.html
+ https://blog.csdn.net/xxb249/article/details/80787501
+ https://zhuanlan.zhihu.com/p/43282243
+ https://reading.developerlearning.cn/reading/32-2019-03-02-etcd-raft/
+ https://jin-yang.github.io/post/golang-raft-etcd-sourcode-details.html
+ https://www.jianshu.com/p/ae462a2d49a8
+ https://studygolang.com/articles/14731
+ http://blog.betacat.io/post/raft-implementation-in-etcd/
