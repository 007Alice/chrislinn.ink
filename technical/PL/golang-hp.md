# High-performance GoLang Notes/Skills

在看 https://hackmd.io/@zkteam/goff 的时候 看到了这几个 post:

+ [Avoiding high GC overhead with large heaps](https://blog.gopheracademy.com/advent-2018/avoid-gc-overhead-large-heaps/)
+ [High Performance Go Workshop](https://dave.cheney.net/high-performance-go-workshop/gophercon-2019.html)

讲述了 GoLang 高性能编程的技巧。

---

+ 要避免指针, 不然 GC 影响性能:
    * 什么时候会出现这种情况?
        - 堆上申请了大量内存
        - 就算想不在堆上申请, 把数据挪到堆下 (那就会不可避免的需要很多指针)
    * 什么场景需要警觉?
        - 很多 strings
        - `time.Time`
        - Maps with slice values
        - Maps with string keys
    * 解决办法
        - 如果这个string只可能是几个固定值的其中之一的话，不妨就用整数来表示
        - 如果要用string来存时间，不妨 parse 成整数来存储
        - 如果真的需要存很多字符串
            + 如果在内存中连续，我们可以用 offset 来访问，就不需要 指针了