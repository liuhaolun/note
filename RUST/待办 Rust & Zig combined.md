常见的问题，如index out of bounds 应该被编译器提示，除非手动声明如：#[allow(integer_division)], 程序员不被要求成为特别谨慎地遵守格式规范的。=》类似于LaTeX把格式和文字分离。

`free(array) used to free allocated memory areas`
use-after-free`free();array[0]`
double-free`free(array);free(array);`
尤其在分支处显著，zig通过`defer free(array);`搁置`free()`直到全部逻辑完成。
所以：它可以解决一些问题
那么：rust有更强的保证，但safe总是更加困难

问：I don't understand, ..., why people use memory-unsafe language<|Internet comment.
浏览器用JavaScript永远不会有内存问题，从90年代就如此：它是script。
node.js使用了`FFI()`调用外部C文件，引入了内存安全性问题。
	很多语言都是用了`FFI()`，这些语言（python, node.js, haskell）本身安全，dependency不安全
Java, C#,  go, swift, scala, 可以写`unsafe`（屏蔽garba collector）， 可以导入`FFI()`.
它们属于"memory-safety language"
然而，memory unsafety does not suggest an explosion，>遵循某个概念不保证程序质量

##### Why not C?
[useless]

##### Zig tooling
`zig cc` makes cross-compliling
|>目前并没有一种方法，能实现`#[zig]`来直接在rust文件中写zig函数。
然而，现在可以把zig先经过llvm编译后通过rust调用；或者都编译为C-complicable binary调用。

这两种方法都不如直接在#[unsafe]里允许deref等等...#[unsafe]不应该是完全的unsafe
##### 那么，到底是怎样做的？
- zig 经过llvm, 通过rust调用
- c binary
[FFI](https://doc.rust-lang.org/nomicon/ffi.html)