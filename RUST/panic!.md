- Defensive programming, for unexpected conditions, `panic!`
- `panic!`提供模式"unwind"和"abort"，后者立即终止程序
- 当Embedded环境没有操作系统，应该自行处理`#[panic handler]`

```
#[cfg(debug_assertions)]
fn_name1

#[cfg(not(debug_assertions))]
fn_name1

fn main{
#[cfg(debug_assertions)]{
print!("这部分只在debug, print.");
}
}
```
`#[cfg(debug_assertions)]`debug_assertions判断为false，则标记的部分不运行