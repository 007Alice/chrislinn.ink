# 也体验一把密码朋克

<!-- TODO
+ chrome-extension://cdlcdnmhcodhagbmljapgbjdimjckilb/html/options.html
+ https://www.google.com/search?q=pgp%E6%8C%87%E7%BA%B9&oq=pgp%E6%8C%87%E7%BA%B9&aqs=chrome..69i57.746j0j7&sourceid=chrome&ie=UTF-8
+ https://jin-yang.github.io/post/security-pgp-introduce.html
+ https://tomli.blog/pgp
+ http://www.ruanyifeng.com/blog/2013/07/gpg.html
+ https://www.jianshu.com/p/0e1e66423055
+ https://zh.moegirl.org/zh-hant/Help:PGP%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95
+ https://nova.moe/openpgp-best-practices-keyserver-and-configuration/
+ https://wiki.debian.org/Keysigning#Step_5:_Hand_out_your_key.27s_fingerprint
+ https://www.debian.org/events/keysigning
+ http://www.queen.clara.net/pgp/art4.html
 -->

区块链将 密码朋克 这个词带进了普通网民的视线，这篇文章带你也体验一把 密码朋克 (cyber punk) 过过瘾，随便复习一下密码学知识。

使用微信的时候，我们可能会有被监听的顾虑。这时候，PGP 就可以派上用场。

<!-- TODO
你可能也发现了，我在博客首页上贴出了 自己的 PGP Public key。那么，PGP 到底是什么？如何使用呢？下面就来讲解一下 PGP。
 -->

## PGP
PGP (Pretty Good Privacy, "优良保密协议") 本身是用于签名和加解密商业应用程序；OpenPGP 是由 PGP 衍生出的开源规范（RFC 4880），而 GnuPG（简称 GPG）就是遵循 OpenPGP 规范的 GNU 实现。

## 体验

简单的 PGP 体验可以使用 Chrome 插件 PGP ANYWHERE，现在有的邮箱 (Secure Email, https://securegroup.com/secure-email/) 和邮箱插件 (如支持 Gmail 的 FlowCrypt) 也继承了 PGP 功能，邮件加解密、签名更加方便。

进入 PGP Anywhere 插件，在 Options 标签页中 Generate Keys 可以生成一对公私钥，将这把公钥发给别人别人就可以进行加密（从别人那里获取一把公钥就可以加密消息并发给别人）。类似的，也可以使用


<!-- TODO
 -->

## 原理流程

PGP 实现加解密的原理流程图：

![PGP](/img/pgp/PGP_diagram.png)

> 可以看到，OpenPGP 发送方产生一串随机数，作为对称加密密钥，这一随机数 __只对__ 该信息或该会话有效。使用接受者的公钥加密上述的随机数 (密钥)，放置到需要发送消息的开头。然后通过上述产生的随机数加密需要加密的信息（通常会先对信息进行压缩）。

<!-- TODO 

## 别的例子

邮箱

消息签名


## 使用 GPG

-->



## 注意

### 不要依靠 Key ID
Key ID （无论长短）已经 被证明 可以 被碰撞。为了安全应该使用全指纹而非 Key ID。应该在 GPG 配置文件中写上 `keyid-format 0xlong` 和 `with-fingerprint` 来保证所有密钥都是显示 64 位长的 ID 且显示指纹。


### 不要盲目地相信来自密钥服务器的密钥

所有人都可以把自己的密钥上传到密钥服务器上，所以把你不应该仅仅是下载下密钥就盲目地认为就是你需要的那个。

通过线下或者电话的方式向对方确认其密钥的指纹信息后，你可以通过如下指令下载对方的公钥：

```
gpg --recv-key '<fingerprint>'
```

但服务器 还是可能给你一个其他密钥，所以你还是需要确认你下载你的密钥就是你需要的那个，应该在 __导入前验证密钥指纹__ 。（如果你的 GnuPG 版本小于 2.1，那你就需要手动确认你下载到的 Key，如果你的 GnuPG 版本大于 2.1 的话它会自动拒绝来自密钥服务器的不正确的密钥。）

你可以用两种方式来确认密钥指纹：

1. 直接检查密钥指纹
```
gpg --fingerprint '<fingerprint>'
```
2. 尝试在本地用那个指纹给一个密钥签名，`lsign-key` 的 `l` 表示 `local`
```
gpg --lsign-key '<fingerprint>'
```

如果你确定你拿到了那个人的正确的密钥，比较建议的是在本地给那个密钥签名，如果你希望公开的表明你和那个人的联系的话，你可以通过 `--sign-key` 公开给那个密钥签名，并发送出去（比如）。


对于一般的情况（比如在一个网站上面下载了一个密钥），你可以这样在导入前验证密钥指纹：

```
gpg --with-fingerprint <keyfile>
```