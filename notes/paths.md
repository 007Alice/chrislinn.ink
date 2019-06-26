# Paths

## Slides
+ https://github.com/gnab/remark
+ https://github.com/yhatt/marp
+ https://github.com/hiroppy/fusuma
+ https://github.com/jacksingleton/hacker-slides

## Blockchain json statetest
### Modify parity_listStorageKeys
+ Make argument _Quantity_ oponal. When `Quantity == null` , return all storage keys.

### parity_storage
Arguments same as __Modified__ _parity_listStorageKeys . Return is a _JSON_ object
```
{
    StorageKey(HexString): Corresponding value (HexString)
}
```


impls/parity.rs
client/client.rs

a collection of type `std::vec::Vec<ethereum_types::H256>` cannot be built from `std::iter::Iterator<Item=(std::vec::Vec<u8>, elastic_array::ElasticArray128<u8>)>`


version: _modified_
curl --data '{"method":"parity_listStorageKeys","params":["0xab7c74abC0C4d48d1bdad5DCB26153FC8780f83E",null,null],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST localhost:8545 

curl --data '{"method":"parity_listStorageKeys","params":["0xab7c74abC0C4d48d1bdad5DCB26153FC8780f83E",null,null],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST localhost:8545 

加上--mode=offline黎停止同步


+ 0xab7c74abC0C4d48d1bdad5DCB26153FC8780f83E
+ 0x61EDCDf5bb737ADffE5043706e7C5bb1f1a56eEA
+ 0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe
+ 0x75bA02c5bAF9cc3E9fE01C51Df3cB1437E8690D4
+ 0x7da82C7AB4771ff031b66538D2fB9b0B047f6CF9

./target/debug/parity --fat-db=on --mode=offline

cargo build

> There are paths for everyone

<!-- 
利益才是前进的动力，金钱、成就感。名誉当然也是有助于利益的，对于人脉而言。（所以在甲方做安全还是有点亏的，因为难以直接看到收益。虽然企业多么希望招到牛逼的安全开发、运维、架构，高薪一口气解决问题。）

张狂、傲慢是挺爽，要有实力才能很好地张狂傲慢起来。但是恐怕不利于团队合作和交朋友。

越来越大了，其实企业不需要多么天才的程序员，所谓的地表最强coder，要学要练的东西那么多，无非是给自己平添不切实际的压力。

人脉、经验、投资的眼光和信息、架构、数学、建模、算法及其应用。要找到自己不可替代的核心竞争力，实现财务自由，能进行更自由的选择，也有自己自由的时间。
 -->

<!-- 
## 我感兴趣的
+ 时间毕竟有限
+ 工作来说肯定还是 go > rust
+ 抄其实很正常，是学习的必经过程，关键是抄完之后学到什么
+ 那么其实 解决问题的思维最重要，所以 算法、技巧、代码设计(衍生到架构、高并发)、高性能计算 > 密码学 > 系统 = 编译器
 -->

<!-- 

## PhD
+ RegExp
    ```
    (crypt|secur|dete|intru|penetra|cyber|malic|priva)
    ```
+ location
    * AU
        - UniMelb
            + [Peter Schachte](http://www.cis.unimelb.edu.au/people/staff.php?person_ID=456)
            + [Rao Kotagiri](http://www.cis.unimelb.edu.au/people/staff.php?person_ID=16028)
                * ml intrusion dection
            + [Harald Sondergaard](http://www.cis.unimelb.edu.au/people/staff.php?person_ID=13416)
            + [Peter Stuckey](http://www.cis.unimelb.edu.au/people/staff.php?person_ID=14142)
            + [DR Jeffrey Chan](https://www.findanexpert.unimelb.edu.au/display/person7602#tab-overview)
                * Machine Learning and Data Mining
                * Network Security (intrusion detection, cloud security)
            + Udaya
                * Blockchain?
                * Machine Learning Intrusion Detection System
                * Differential Privacy?
            + [Dr Sarah Monazam Erfani](http://www.cis.unimelb.edu.au/people/staff.php?person_ID=639922)
            + [Professor Christopher Leckie](http://www.cis.unimelb.edu.au/people/staff.php?person_ID=6335)
            + [Dr Toby Murray](http://www.cis.unimelb.edu.au/people/staff.php?person_ID=780796)
            + [Dr Ben Rubinstein](http://www.cis.unimelb.edu.au/people/staff.php?person_ID=20074)
            + [Professor Richard Sinnott](http://www.cis.unimelb.edu.au/people/staff.php?person_ID=342078)
            + [Dr Vanessa Teague](http://www.cis.unimelb.edu.au/people/staff.php?person_ID=34563)
        - [sydney](http://sydney.edu.au/engineering/it/about/people/list.php)
            + [DR RALPH HOLZ](http://sydney.edu.au/engineering/people/ralph.holz.php)
                * V
            + [PROFESSOR SEOKHEE HONG?](http://sydney.edu.au/engineering/people/seokhee.hong.php)
            + [PROFESSOR ALBERT ZOMAYA](http://sydney.edu.au/engineering/people/albert.zomaya.php)
            + [DR YING ZHOU](http://sydney.edu.au/engineering/people/ying.zhou.php)
            + [PROFESSOR DACHENG TAO](http://sydney.edu.au/engineering/people/dacheng.tao.php)
            + [ASSOCIATE PROFESSOR UWE ROEHM](http://sydney.edu.au/engineering/people/uwe.roehm.php)
            + [EMERITUS PROFESSOR PETER EADES](http://sydney.edu.au/engineering/people/peter.eades.php)
            + [Michael Fry](https://chai.it.usyd.edu.au/people/michaelfry/)
                * [Yu_YY_thesis.pdf](https://ses.library.usyd.edu.au/handle/2123/10277)
        - UNSW
        - Monash
        - ANU?
        - WA?
        - Queensland?
        - Adelaide?
    * HK
        - HKUST
            + https://www.seng.ust.hk/web/eng/faculty_research2.php?id=96
                * https://www.seng.ust.hk/web/eng/people_detail.php?id=351&cur2=research
                    - V - 20170924
                * https://www.seng.ust.hk/web/eng/people_detail.php?id=377&cur2=research
                * https://www.cse.ust.hk/~ricci/
        - HKU
            + http://www.cs.hku.hk/people/academic.jsp
                * http://www.cs.hku.hk/research/interest.jsp
                    - http://www.cs.hku.hk/research/profile.jsp?teacher=smyiu
                        + V - 20170924
                    - http://www.cs.hku.hk/research/profile.jsp?teacher=hui
            + https://www.eee.hku.hk/people/
        - CUHK
            + http://www.cse.cuhk.edu.hk/v7/en/people/lec.html
                * http://www.cse.cuhk.edu.hk/~wei/
                * http://www.cse.cuhk.edu.hk/~cslui/
            + http://www.ie.cuhk.edu.hk/people/people.shtml
                * http://personal.ie.cuhk.edu.hk/~cchan/
                * http://www.ie.cuhk.edu.hk/people/sherman.shtml
                    - V - 20170924
                * http://www.ie.cuhk.edu.hk/people/khzhang.shtml
                * http://www.ie.cuhk.edu.hk/people/wclau.shtml
                * http://www.ie.cuhk.edu.hk/people/mhchen.shtml
        - CityU
            + http://www.cs.cityu.edu.hk/people/academic_staff.html
                * http://www6.cityu.edu.hk/stfprofile/cslfkwok.htm
                    - V - 20170924
                * http://www.cs.cityu.edu.hk/profile/congwang.html
                * http://www6.cityu.edu.hk/stfprofile/gphancke.htm
            + http://www.ee.cityu.edu.hk/home/people_academic_staff.html
                * http://www.ee.cityu.edu.hk/~rcheung/Welcome.html
                * http://www.ee.cityu.edu.hk/~eellc/
                * http://www.ee.cityu.edu.hk/~lcheng/
        - PolyU
            + http://www.comp.polyu.edu.hk/en-us/staffs/index/1
                * http://www.comp.polyu.edu.hk/en-us/staffs/detail/1283
                    - V - 20170924
                        + ?
                * http://www.comp.polyu.edu.hk/en-us/staffs/detail/1252
                * http://www.comp.polyu.edu.hk/en-us/staffs/detail/1470
                * http://www.comp.polyu.edu.hk/en-us/staffs/detail/2244
                * http://www.comp.polyu.edu.hk/en-us/staffs/detail/3751
                * http://www.comp.polyu.edu.hk/en-us/staffs/detail/3646
                * http://www.comp.polyu.edu.hk/en-us/staffs/detail/1419
    * Sg
        - NUS?
        - NYTU
    * En
        - G10
    + topic
        * bitcoin
        * ML intrusion detection
        * differential privacy

## 移民分数研究
+ 2年工签 psw
    * 485

## 攒机
+ 刚爆出来 Intel 的 CPU 有个硬件 bug，即使是修复后性能也会下降 5%-30%. 所以，把 8700k 换成 1800x
    * [这硬件 bug 可以说很牛了： 英特尔处理器发现严重设计漏洞， AMD 不受影响](https://www.v2ex.com/t/419683)
        - 操作系统就是使用的 cpu 提供的功能来隔离不同程序及内核的，现在这个隔离出了问题，意味着不同程序可以访问内核数据，其中可能存在一些密码等数据。看起来安全上对单机用户可能影响不大，但是对于云服务商来讲是个大问题。不过如果执行修复会造成悲剧的性能损失，这个会影响普通用户了。
+ 内存双通道比单条性能好
+ 微博 /淘宝搜 萌叔或老牛。都是老牌 diy 商家，装好包好给你发来。
+ 装机帮扶站

 -->


# paper

<!-- 
+ http://users.monash.edu.au/~kailiu/
+ http://www.jiangshanyu.com/
+ https://www.comp.nus.edu.sg/~abhik/
    + research
        + correctness
        + coverage
    + auto
    + manual
    + solidity tools
 -->

## direction
security的论文 实验和出成果都不难, 工业界比学术界领先, 论文还没出工业界就把事情搞出来了(不像密码学,没论文是几乎不可能搞出事情的), 关键在 methodology 和 motivation 要好, __角度要刁钻__(实现不难，然后统计分析一下), 解决实际问题, 

+ https://www.zhihu.com/question/23647187/answer/568803695
    * 模仿, 一开始的时候idea的新颖程度低一些，工作量夯实一些
    * 优先阅读该方向里最近五年的survey
    * 自行整理该方向相关的近三年的顶级会议
        - 关键词搜索出所有的论文
        - 能懂的/和你想做的相关的/热门的论文
    * 找到高年级的学长学姐或者其他小老板讨论，他们可以帮你确定一个小方向
    * 阅读论文，最好能细致一点，把论文之间的引用关系理清楚，把近几年的发展脉络理清楚
    * 如果你不会设计实验/写论文，请模仿和你的工作最相关的论文
    * 逻辑第一
+ 搜索论文用 dblp.org
+ download at https://sci-hub.tw/

## step
+ 先把用学术的语言描述出来
    * 形式化的方式写出来, 比如伪代码或者其他形式
+ 再看这篇文章怎么写
    * scope多大
    * focus在哪
    * contribution在哪
+ 写的过程就能明白其对应的现实环境 也就是security model
+ solution
+ 想完solution就开始实现
+ 写文章
+ 改文章
+ 跑实验画图
+ draft
+ 再拿出去投 边投边改

## reference
+ Mendeley
+ EndNote
+ referencemanager
+ Readcube
+ Zotero
+ NoteExpress
+ papers3
+ Circus Ponies Notebook
+ citavi

## 区块链的文章都发在哪几个会
http://www.conferenceranks.com/ ,randA & rankB

+ 三大密码会Eurocrypt, CRYPTO, Asiacrypt
+ 四大安全会CCS, IEEE S&P, Usenix Security, NDSS
+ 操作系统会OSDI, SOSP
+ 计算机网络的会 NSDI, Infocom, Sigcomm
+ 分布式计算理论的几个会 PODC, DISC, OPODIS
+ 隐私的会议 PETS
+ FC (Financial Cryptography)

