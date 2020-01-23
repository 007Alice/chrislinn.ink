# Git

https://github.com/tyrchen/book_next/blob/master/src/2019/w48/1-git-pub.md

https://github.com/tyrchen/book_next/blob/master/src/2019/w48/2-git-stash-pub.md

+ TODO
    + packfile 差分编码
    + ...
+ 对象
    * 分类
        - blob （文件对象）
            + 被组织成 merkle tree
        - merkle tree
        - commit
            + 每次 commit 就是根据更改的文件的信息生成新 tree 的过程
            + 新树和老树共享相同的子树，只有变化的部分才会分叉
            + 长此以往，对象数据库中有无数棵树，一起构成了一个 merkle DAG。
            + 通过使用引用（ref），比如 HEAD, heads/master，tags/v0.1， 可以很方便地追踪每一棵树的确切状态
    * 对比 cvs/svn
        - 去中心化
        - cvs/svn 以 diff 为基础，git 认为每个对象 immutable 每次都会生成新对象 （id 为其 sha1 hash）

