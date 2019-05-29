# Kerberos

https://www.tarlogic.com/en/blog/how-kerberos-works/
https://www.zhihu.com/question/22177404/answer/492680179

## 总结

* 用户 和 服务 的帐号 （用户名/密码）只有 KDC (Key Distribution Center) 和它自己知道
* KDC 为 用户 和 服务 颁发 ticket.
    - 用户 visit 用户 需要 service ticket
    - 用户 visit 服务 需要 service ticket
    - 服务 visit 服务 需要 service ticket
    - 流程
        + 用户 -(master key 加密一段认证信息)-> KDC
        + KDC (知道用户 master key) 解密信息, 认证成功
* TGT (Ticket Granting Ticket) 是用来申请 TGS (Ticket Granting Service) 的 ticket
+ TGT 是为了避免客户端缓存用户的密钥（master key), 而特别向 KDC 申请的 ticket，这个 TGT  用用户的 master key 加密。用户解密完得到里面的 session key，就会将 master key 从内存删除，__避免 master key 长期滞留在内存而泄漏，更加安全__
    * 我的理解：也有助于 know cipher text attack?  防止密钥多次使用被 side channel
