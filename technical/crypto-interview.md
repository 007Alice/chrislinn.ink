# 搞 MPC 和 ZKP 的基础密码学面试

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

### 实数上椭圆曲线的加法计算
计算 ![](http://latex.codecogs.com/gif.latex?P + Q) 的方法: 连接 ![](http://latex.codecogs.com/gif.latex?Q) 和  ![](http://latex.codecogs.com/gif.latex?Q) 画一条直线，与椭圆曲线的另一个交点为 ![](http://latex.codecogs.com/gif.latex?R)，![](http://latex.codecogs.com/gif.latex?P + Q) 的结果就是 ![](http://latex.codecogs.com/gif.latex?R) 的逆。（![](http://latex.codecogs.com/gif.latex?P + Q + R = 0)，![](http://latex.codecogs.com/gif.latex?P + Q = - R)
）

### 椭圆曲线的标量乘法

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
已知两个在 子群 上的点 ![](http://latex.codecogs.com/gif.latex?P) 和 ![](http://latex.codecogs.com/gif.latex?Q = kP)，求解  ![](http://latex.codecogs.com/gif.latex?k) 是非常困难的问题，目前没有多项式时间的求解算法。

### 为什么说 ECDSA 签名 不是 deterministics 的?
签名算法里面有个 随机数k，每次签出来的名可能不一样

### ECDSA 与 Ed25519 有什么区别与联系?
没有联系，虽然都是椭圆曲线上的。Ed25519 属于 EdDSA，但是 ECDSA 与 EdDSA 也没有关系。ECDSA 是一个又慢又不够安全的过时设计，EdDSA 是一个 更快更安全的现代设计。

### secp256k1 不如 Curve25519 安全，那为什么比特币还用 secp256k1?
因为那时候 Curve25519 还没出世。

## Simulation
### proofs based on simulation vs proofs based on games
基于游戏的定义更容易为其编写证明，但基于模拟的定义在获得安全性方面通常更清晰。

哪个更好？取决于实际情况。但模拟能提供组合下的安全性。这些组合定理可让您分析更大的协议，同时将子协议作为理想的功能调用。使问题得到简化。

通用的可组合性及其变体能提供并发组合下的安全性，不仅是同一协议的并发执行（因为在基于游戏的定义中通常也保证了这一点），还包括与其他任意协议的并发执行，而一般基于游戏的定义不能保证这一点。

独立的基于模拟的定义能提供顺序组合下的安全性，所以也很容易通过使用模块化顺序组合定理将协议插入更大的协议来证明大协议的安全性。

## Paillier
Paillier 是一种原生支持加法同态的非对称加密体系。能同时支持 语义安全 (semantically secure) 和 加法同态 (additively homomorphic)；RSA 只能取其一。

## 应用题

### MPC communicarion channel
现有一个 MPC 协议，如果我不想被人窃听，该怎么办？

__答：__ 端到端加密

如果我要证明消息的完整性怎么办？

__答：__ 消息摘要

如果我要证明消息确实是我发的，且没有被篡改，我也不能抵赖说我没有发过呢？

__答：__ 数字签名

现在别人不能伪造消息了，如何防止别人重放呢？

__答：__ 加 nonce


### Sharding & Randomness Beacon
Sharding 协议中决定将节点分配至哪个 shard 时可能使用 Randomness Beacon，为什么 Randomness Beacon 需要满足:

+ unbias
    * 无法操纵自己加入哪个shard，否则就可以运行多个节点加入同一个shard，然后51。
+ unpredictable
    * 无法预测分配至哪个shard，否则就可以等到轮到那个shard时再加入，并提前和也加入那个shard的人串谋。

### ZKP proving time

给区块链设计 ZKP 协议时需要考虑验证的时间成本取舍，指的是什么？只讨论时间成本，不讨论空间和是否交互式等等。

__答：__ 不同算法中在 SNARK 和 虚拟机中 所需的验证时间不一样。比如 SHA256 是 EVM < SNARK，Pedersen Commitment 是 SNARK < EVM。

## Coding

1. 下面这个检查密码是否相等的代码有什么问题？

```c
int
memcmp(const void *s1, const void *s2, size_t n)
{
    if (n != 0) {
        const unsigned char *p1 = s1, *p2 = s2;

        do {
            if (*p1++ != *p2++)
                return (*--p1 - *--p2);
        } while (--n != 0);
    }
    return (0);
}
```

__答：__ 非 constant-time：可通过执行的时间来推测有多少位相同，侧信道攻击。

2. 下面这个 RSA 快速幂取余算法的代码有什么问题？

```c
int PowerMod(int a, int b, int c)
{
    int ans = 1;
    a = a % c;
    while(b>0) {
        if(b % 2 == 1)
            ans = (ans * a) % c;
        b = b/2;
        a = (a * a) % c;
    }
    return ans;
}
```

__答：__ 非 constant-time：当b为奇数时会多执行下面的指令，执行的时间不一样，可被侧信道攻击。

3. 下面这个解密代码有什么问题？

```go
func main() {
    secret := getSecret()
    plainText := decrypt(cipherText, secret)
    fmt.Println(plainText)
}
```

__答：__ 密码使用完后一直放在内存中不主动清除。

在大多数操作系统上，一个进程所拥有的内存可以被另一个进程重用而不会被清除，比如因为第一个进程终止了或将内存退还给系统了。如果内存中包含秘密密钥，则这些密钥可被第二个进程访问到。在多用户系统上，这可以嗅探其他用户的密钥；即使在单用户系统中，也可能带来隐患。
