# Cryptography Interview

## 数论

### 代数结构
具有一个及以上运算的非空集合。

### 群 (group)
只有一个运算（二元操作）的一个（有限或无限）集合。

满足:

+ 封闭性
+ 结合律
+ 单位元
+ 逆元

#### 阿贝尔群 (Abelian group) 
满足交换律，故阿贝尔群又叫交换群 (commutative group)。

#### 循环群 (cyclic group)
存在一个元素  ![](http://latex.codecogs.com/gif.latex?g) (生成元，generator)，群中所有元素都可以由生成元的幂运算获得。

#### 循环群 vs 阿贝尔群
循环群都是阿贝尔群，不是所有的阿贝尔群都是循环群。

#### 子群
一个群的非空子集，若在同样的运算下也构成一个群，则称之为这个群的子群。

### 环 (ring)
具有两个二元操作（加法和乘法）的一个集合。

满足:

+ 加法结合律
+ 加法交换律
+ 加法单位元
+ 加法逆元
+ 乘法结合律
+ 分配率

环在加法操作下是个阿贝尔群。

#### 子环
一个环的非空子集，若在加法和乘法下也构成一个环，则称之为这个环的子环。

例：整数环是有理数域的一个子环，偶数环是整数环的一个子环。

#### 理想 (ideal)
理想是一种特殊的子环。在子环的基础上满足：

+ 如果 ![](http://latex.codecogs.com/gif.latex?B) 是 ![](http://latex.codecogs.com/gif.latex?A) 的一个理想，那么对于任何 ![](http://latex.codecogs.com/gif.latex?a \in A)，![](http://latex.codecogs.com/gif.latex?b \in B)，存在 ![](http://latex.codecogs.com/gif.latex?ab \in B)（左理想），并且 ![](http://latex.codecogs.com/gif.latex?ba \in B)（右理想）。
    * 注意环中乘法不一定可交换，所以 ![](http://latex.codecogs.com/gif.latex?ab) 和 ![](http://latex.codecogs.com/gif.latex?ba) 不同。

每个环至少有两个理想，这两个理想也被称为平凡理想:

+ 单个 0 元所生成的环 (因为任何一个元与0元的乘都为0元)
+ 这个环本身

对于整数环，所有偶数组成的子环是一个理想，因为任何整数和偶数的乘积还是一个偶数。

##### 主理想
略

#### 商环
略

#### 分划 & 类
略

#### 除法代数
略

### 域 (field)
满足以下性质的一个集合:

+ 加法和乘法的结合律
+ 加法和乘法的交换律
+ 加法和乘法的分配律
+ 加法和乘法的单位元
+ 加法和乘法的逆元

实数是域，整数是环。

#### 有限域 (finite field)
集合中元素有限的域。又称为 伽罗瓦域 (Galois field)。

+ 如果域上不要求乘法的交换律，就是除法代数。
+ 域上存在逆元。（域上支持除法运算。）
+ 环不是域。
    * 矩阵环不支持乘法的交换律。
+ 域中所有非零元素的集合是关于乘法的阿贝尔群。
+ 有限域的乘法群是循环群。
+ 有限域中所有非零元素的集合的每个有限子群都是循环群。

## 椭圆曲线
两条平行线有没有交点？黎曼几何里面有：交点在无穷远。

椭圆曲线是一系列满足如下方程的点:

![](http://latex.codecogs.com/gif.latex?y^2 = x^3 + ax + b,\ 4a^3 + 27b^2 \ne 0)

该方程被称作 椭圆曲线的 Weierstrass 方程。

### 基于椭圆曲线的群定义

### 椭圆曲线的加法计算

### 椭圆曲线的标量乘法

#### 对数问题

### 有限域上的椭圆曲线

#### 逆元
扩展欧几里得定理

#### 点加

#### 标量乘法

#### 循环子群的阶
略

#### 寻找生成元
略

#### 离散对数问题


### 为什么说 ECDSA 签名 不是 deterministics 的?
签名算法里面有个 随机数k，每次签出来的名可能不一样

### ECDSA 与 Ed25519 有什么区别与联系?
没有联系，虽然都是椭圆曲线上的。Ed25519 属于 EdDSA，但是 ECDSA 与 EdDSA 也没有关系。ECDSA 是一个又慢又不够安全的过时设计，EdDSA 是一个 更快更安全的现代设计。

### secp256k1 不如 Curve25519 安全，那为什么比特币还用 secp256k1?
因为那时候 Curve25519 还没出世。

## Simulation


## Paillier
Paillier 是一种原生支持加法同态的非对称加密体系。能同时支持 语义安全 (semantically secure) 和 加法同态 (additively homomorphic)；RSA 只能取其一。

## 应用题

### Sharding & Randomness Beacon
Sharding 协议中决定将节点分配至哪个 shard 时可能使用 Randomness Beacon，为什么 Randomness Beacon 需要满足:

+ unbias
    * 无法操纵自己加入哪个shard，否则就可以运行多个节点加入同一个shard，然后51。
+ unpredictable
    * 无法预测分配至哪个shard，否则就可以等到轮到那个shard时再加入，并提前和也加入那个shard的人串谋。

### ETH...

## Coding




<!-- 
zhong guo yu shu?
zhan zhuan xiang chu?
 -->