# Class Group

+ https://www.michaelstraka.com/posts/classgroups/
+ https://hackmd.io/@olivierbbb/r10VpNPZU
+ https://github.com/Chia-Network/vdf-competition/blob/master/classgroups.pdf
+ https://crypto.stanford.edu/pbc/notes/

## class group 作用

（__虚二次数域__, Imaginary Quadratic Number Fields 的）__类群__ 在区块链和零知识证明里是常用工具。

+ 用累加器来替代 merkle tree。可用于高效存储查找删除元素，节省区块体积。
+ 对 __Proof of exponantiation__, __Polynomial commitment__ (zkSNARK Marlin 协议中有使用), __VDF__, __Encryption scheme__ 也有用。

## 为什么代数论会和密码学有关？

在密码学应用中，\textbf{类群}可被用作 \textbf{未知阶的群}(groups of unknown order, GUO)。???



GUO 在构建 VDF 和 密码学累加器 (Cryptographic Accumulators) 时，也有被作为替代考虑方案。
也可以作为 \textbf{多项式承诺}方案 (polynomial commitment schemes) 的 \textbf{目标群} (target groups).

RSA 群 $(\Bbb{Z}/N\Bbb{Z})^\times$
\footnote{$N=pq$，是两个大素数的乘积。}
提供了 GUOs 的一个替代家族
\footnote{计算它们的\textbf{阶}等同于对 $N=pq$ 分解质因数，但这很难。}
。


然而，生成 RSA 模数 (moduli) $N=pq$ 要么需要一个可信方
\footnote{来生成两个大素数 $p,q$, 它们的乘积 $N=pq$ 并且还要相信这个可信方会删除 $p$ 和 $q$.}
,
要么既要进行一个没那么简单的多方计算(multiparty computation)。

\bigskip

生成\textbf{类群}
\footnote{这里指的是\textbf{虚二次数域} $K=\Bbb{Q}(\sqrt{d})$, $d<0$ 的\textbf{类群}}
$\mathcal{Cl}(d)$
则``只需要\footnote{\url{https://mathoverflow.net/questions/16098/complexity-of-testing-integer-square-freeness}}''

## class group 精髓

1