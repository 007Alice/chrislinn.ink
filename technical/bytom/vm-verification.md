# 虚拟机验证

protocol/vm/ 里面即是虚拟机代码



虚拟机结构在 vm.go 中：
```
type virtualMachine struct {
    context *Context

    program      []byte // the program currently executing
    pc, nextPC   uint32
    runLimit     int64
    deferredCost int64

    expansionReserved bool

    // Stores the data parsed out of an opcode. Used as input to
    // data-pushing opcodes.
    data []byte

    // CHECKPREDICATE spawns a child vm with depth+1
    depth int

    // In each of these stacks, stack[len(stack)-1] is the top element.
    dataStack [][]byte
    altStack  [][]byte
}
```