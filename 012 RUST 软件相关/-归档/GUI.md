

我不想另外学一门语言，至少现在如此。
那么，GUI本来就是一门单独语言吧？？？
rust编程的全栈。
此外，我喜欢web的排版方式。
dioxus/ egui/ slint
生气一样的，选择了slint->slint崩溃了
claude推荐了egui，那我听它的了。
# 试试dioxus的文档吧。


```
cargo install dioxus-cli
```
安装了dioxus的cli工具，毕竟是一个toolchain

```
dx new
```
dx for dioxus开始新项目，...这玩意有749个依赖

全栈和WASM相对立，后者完全在浏览器运行；
WebView即渲染为桌面或移动

dioxus同样失败了。
# tauri呢？
616个依赖项
tauri不好吗？我真的不愿意学习typescript或者JavaScript，因为它们只用在前端，对于极高的内存管理并无要求。
更何况它一次性成功构建了应用。
结论：我使用tauri了