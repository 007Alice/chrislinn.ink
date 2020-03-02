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
        - If your string takes only a few fixed values then consider using integer constants instead
        - If you are storing dates and times as strings, then perhaps parse them and store the date or time as an integer
        - If you fundamentally need to keep hold of a lot of strings
            + if all the string bytes were in a single piece of memory, we could track the strings by offsets to the start and end of each string in this memory. By tracking offsets we no-longer have pointers in our large slice, and the GC is no longer troubled.
            + What we give up by doing this is the ability to free up memory for individual strings, and we’ve added some overhead copying the string bodies into our big byte slice.
            + The principle here is that if you never need to free a string, you can convert it to an index into a larger block of data and avoid having large numbers of pointers. I’ve built a slightly more sophisticated thing that follows this principle here if you are interested.